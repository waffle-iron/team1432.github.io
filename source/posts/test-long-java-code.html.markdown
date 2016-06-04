---
title: Test Long Java Code
date: 2016-05-11 09:24:44 -0700
author: Caleb Eby
tags: Programming
---
This is a really long piece of code:

READMORE

```java

package circleoptimization;

import circleoptimization.Circle.State;
import gurobi.GRB;
import gurobi.GRBEnv;
import gurobi.GRBException;
import gurobi.GRBExpr;
import gurobi.GRBModel;
import gurobi.GRBLinExpr;
import gurobi.GRBVar;
import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.GridLayout;
import java.awt.Point;
import java.awt.Toolkit;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collection;
import java.util.Random;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.sound.sampled.AudioFormat;
import javax.sound.sampled.AudioSystem;
import javax.sound.sampled.LineUnavailableException;
import javax.sound.sampled.SourceDataLine;
import javax.swing.ButtonGroup;
import javax.swing.JButton;
import javax.swing.JCheckBox;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JRadioButton;
import javax.swing.JRootPane;
import javax.swing.JSlider;
import javax.swing.JTextField;
import javax.swing.WindowConstants;
import javax.swing.event.ChangeEvent;
import javax.swing.event.ChangeListener;
//Note the use of Apache Math Commons v2.2
import org.apache.commons.math.optimization.linear.LinearConstraint;
import org.apache.commons.math.optimization.linear.LinearObjectiveFunction;
import org.apache.commons.math.optimization.linear.Relationship;
import org.apache.commons.math.optimization.linear.SimplexSolver;
import org.apache.commons.math.optimization.GoalType;
import org.apache.commons.math.optimization.OptimizationException;
import org.apache.commons.math.optimization.RealPointValuePair;

/**
 *
 * @author Hank Guss
 */
public class Animation implements ChangeListener, ActionListener, ItemListener, KeyListener {
    
    //Class Variables
    static int MAX_CIRCLES = 6;
    static int MIN_CIRCLES = 6;
    static int NUM_FRAMES = -1;
    //if you want to run for only one frame and save as images the radial and areal sum
    //static int NUM_FRAMES = 1;
    static int TRAIL_FRAME = 5;
    static int WIDTH = 1000;
    static int HEIGHT = 1000;
    static int quadProx = 1;
    static int SLOW = 50;
    static boolean GUROBI = true;
    static boolean fixQuad = true;
    
    //Instance Variables
    GRBEnv env;
    Random rand;
    Circle[] circles;
    Picture canvas;
    LinearObjectiveFunction f;
    Collection constraints;
    SimplexSolver solver;
    int maxVal;
    int lastVal;
    int minVal;
    boolean shadeShapes = false;
    boolean shadeBackground = false;
    private int trailFrames = 0;
    
    //Graphics Variables
    private JFrame frame;
    private JTextField typingArea;
    private JPanel imagePanel;
    private JButton quitButton;
    private JSlider speedSlider;
    private JSlider squareSlider;
    private JSlider proxSlider;
    private JSlider widthSlider;
    private JSlider heightSlider;
    private JSlider trailSlider;
    private JCheckBox shadeShapesBox;
    private JCheckBox shadeBackgroundBox;
    private JCheckBox moveRandomBox;
    private JCheckBox sonificationBox;
    private int numSquares = 20;
    private int shape = 1;
    private int function = 0;
    SourceDataLine line = null;
    private boolean sonification = false;;
    private JRadioButton shapeSquare;
    private JRadioButton shapeCircle;
    private JRadioButton functionLinear;
    private JRadioButton functionQuadratic;
    
    //Main method uses Animation class;
    public static void main(String[] args) {
        //Constructs an instance of the animation and sets it in action
        Animation a = new Animation();
        a.play();
    }
    
    public Animation() {
        //Attempts to initialize Gurobi environment
        try {
            this.env = new GRBEnv();
            env.set(GRB.IntParam.OutputFlag, 0);
        } catch (GRBException ex) {
            Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
        }
        
        //Initialize Instance Variables
        typingArea = new JTextField(20);
        typingArea.addKeyListener(this);
        rand = new Random();
        canvas = new Picture(WIDTH, HEIGHT);
        //To keep track of optimal value range reached so far
        maxVal = 0;
        minVal = WIDTH * HEIGHT;
        
        //Initialize Graphics Variables
        frame = new JFrame("Moving Shape Maximizations");
        frame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());
        frame.setSize(450, 360);
        imagePanel = new JPanel(new BorderLayout());
        //TODO add space for height and width sliders
        JPanel sliders = new JPanel(new GridLayout(3, 1));
        JPanel top = new JPanel(new GridLayout(1, 2));
        JPanel left = new JPanel(new GridLayout(4, 1));
        JPanel right = new JPanel(new GridLayout(4, 1));
        quitButton = new JButton("Quit");
        quitButton.addActionListener(this);
        JPanel speedSliderBox = new JPanel(new BorderLayout());
        JPanel squareSliderBox = new JPanel(new BorderLayout());
        speedSlider = new JSlider(50, 100, 50);
        speedSlider.addChangeListener(this);
        speedSlider.setSnapToTicks(true);
        speedSliderBox.add(speedSlider, BorderLayout.CENTER);
        left.add(new JLabel("     Speed:"));
        right.add(speedSliderBox);
        squareSlider = new JSlider(1, 20, numSquares);
        squareSlider.addChangeListener(this);
        squareSlider.setSnapToTicks(true);
        squareSlider.setMajorTickSpacing(5);
        squareSlider.setMinorTickSpacing(1);
        squareSlider.setPaintTicks(true);
        trailSlider = new JSlider(0, 20, Circle.memory);
        trailSlider.addChangeListener(this);
        trailSlider.setSnapToTicks(true);
        JPanel trailSliderBox = new JPanel(new BorderLayout());
        trailSliderBox.add(trailSlider, BorderLayout.CENTER);
        squareSliderBox.add(squareSlider, BorderLayout.CENTER);
        left.add(new JLabel("     Number of Shapes:"));
        right.add(squareSliderBox);
        Dimension screenRes = Toolkit.getDefaultToolkit().getScreenSize();
        JPanel heightSliderBox = new JPanel (new BorderLayout());
        JPanel widthSliderBox = new JPanel (new BorderLayout());
        heightSlider = new JSlider(100, (int) Math.round(screenRes.getHeight()), HEIGHT);
        widthSlider = new JSlider(100, (int) Math.round(screenRes.getWidth()), WIDTH);
        heightSlider.addChangeListener(this);
        widthSlider.addChangeListener(this);
        heightSlider.setSnapToTicks(true);
        widthSlider.setSnapToTicks(true);
        heightSliderBox.add(new JLabel("Height:"), BorderLayout.WEST);
        widthSliderBox.add(new JLabel("Width:"), BorderLayout.WEST);
        heightSliderBox.add(heightSlider, BorderLayout.CENTER);
        widthSliderBox.add(widthSlider, BorderLayout.CENTER);
        JPanel proxSliderBox = new JPanel (new BorderLayout());
        proxSlider = new JSlider(1, 20, quadProx);
        proxSlider.addChangeListener(this);
        proxSlider.setSnapToTicks(true);
        proxSlider.setMajorTickSpacing(5);
        proxSlider.setMinorTickSpacing(1);
        proxSlider.setPaintTicks(true);
        proxSliderBox.add(proxSlider, BorderLayout.CENTER);
        left.add(new JLabel("     Quadratic Approximation Value:"));
        right.add(proxSliderBox);
        left.add(new JLabel("     Trail Length:"));
        right.add(trailSliderBox);
        top.add(left);
        top.add(right);
        sliders.add(top);
        //TODO add height and width sliders
        //sliders.add(widthSliderBox);
        //sliders.add(heightSliderBox);
        boolean shapeSquareTrue = true;
        if (shape == 1) {
            shapeSquareTrue = false;
        }
        shapeSquare = new JRadioButton("Draw Squares", shapeSquareTrue);
        shapeCircle = new JRadioButton("Draw Circles", shapeSquareTrue);
        ButtonGroup shapeButtons = new ButtonGroup();
        shapeButtons.add(shapeSquare);
        shapeButtons.add(shapeCircle);
        shapeSquare.addActionListener(this);
        shapeCircle.addActionListener(this);
        functionLinear = new JRadioButton("Sum Radii", true);
        functionQuadratic = new JRadioButton("Sum Areas");
        ButtonGroup functionButtons = new ButtonGroup();
        functionButtons.add(functionLinear);
        functionButtons.add(functionQuadratic);
        functionLinear.addActionListener(this);
        functionQuadratic.addActionListener(this);
        JPanel south = new JPanel(new GridLayout(1, 4));
        //south.add(quitButton);
        JPanel shadeBox = new JPanel(new GridLayout(2, 1));
        JPanel optionsBox = new JPanel(new GridLayout(2, 1));
        shadeShapesBox = new JCheckBox("Shade Shapes");
        moveRandomBox = new JCheckBox("Random Movement");
        moveRandomBox.addItemListener(this);
        sonificationBox = new JCheckBox("Sonification");
        sonificationBox.addItemListener(this);
        shadeBackgroundBox = new JCheckBox("Shade Backgrounds");
        shadeShapesBox.addItemListener(this);
        shadeBackgroundBox.addItemListener(this);
        shadeBox.add(shadeShapesBox);
        shadeBox.add(shadeBackgroundBox);
        optionsBox.add(moveRandomBox);
        optionsBox.add(sonificationBox);
        south.add(shadeBox);
        south.add(optionsBox);
        JPanel shapeButtonsPanel = new JPanel(new GridLayout(2, 1));
        JPanel functionButtonsPanel = new JPanel(new GridLayout(2,1));
        shapeButtonsPanel.add(shapeSquare);
        shapeButtonsPanel.add(shapeCircle);
        functionButtonsPanel.add(functionLinear);
        functionButtonsPanel.add(functionQuadratic);
        south.add(shapeButtonsPanel);
        south.add(functionButtonsPanel);
        sliders.add(south);
        frame.add(imagePanel, BorderLayout.NORTH);
        sliders.add(typingArea);
        frame.add(sliders, BorderLayout.SOUTH);
        //Initialize players
        int num_Circles = numSquares;
        if (NUM_FRAMES != 1) {
        circles = new Circle[num_Circles];
        for (int i = 0; i < num_Circles; i++) {
            circles[i] = new Circle(WIDTH, HEIGHT);
        }
        }
        if (NUM_FRAMES == 1) {
            fixQuad = false;
            circles = new Circle[4];
            circles[0] = new Circle(new Point(WIDTH / 5, HEIGHT / 2));
            circles[1] = new Circle(new Point(3 * WIDTH / 5, HEIGHT / 2));
            circles[2] = new Circle(new Point(2 * WIDTH / 6, HEIGHT / 6));
            circles[3] = new Circle(new Point(2 * WIDTH / 6, 5 * HEIGHT / 6));
        }
        if (!GUROBI) {
            double[] coefficients = new double[num_Circles];
            for (int i = 0; i < num_Circles; i++) {
                coefficients[i] = 1;
            }
            f = new LinearObjectiveFunction(coefficients, 0);
            solver = new SimplexSolver();
            RealPointValuePair solution = solve();
            resize(solution);
        }
        else if (GUROBI) {
            GRBModel solution = gurobiSolve();
            gurobiResize(solution);
            //solution.dispose();
        }
        //TODO mainPanel is not being placed in the layout
        frame.add(canvas.mainPanel, BorderLayout.CENTER);
        frame.setVisible(true);
        
    }
    
    private ArrayList<LinearConstraint> generateConstraints(Circle[] circles) {
        ArrayList constraints = new ArrayList();
        int funct = function;
        if (funct == 0) {
            double[] coef = new double[circles.length];
            for (int i = 0; i < circles.length; i++) {
                Circle first = circles[i];
                coef = new double[circles.length];
                for (int k = 0; k < circles.length; k++) {
                    coef[k] = 0;
                }
                coef[i] = 1;
                //These constraints ensure the radius to be less than the distance to each wall
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, circles[i].getX()));
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, circles[i].getY()));
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, WIDTH - circles[i].getX()));
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, HEIGHT - circles[i].getY()));
                for (int j = i + 1; j < circles.length; j++) {
                    Circle second = circles[j];
                    int hor_dist = Math.abs(first.getX() - second.getX());
                    int vert_dist = Math.abs(first.getY() - second.getY());
                    int max_dist = 0;
                    //if shape is square
                    if (shape == 0) {
                        if (vert_dist > hor_dist) {
                            max_dist = vert_dist;
                        }
                        else {
                            max_dist = hor_dist;
                        }
                    }
                    //if shape is circle
                    if (shape == 1) {
                        double dist = Math.sqrt(hor_dist * hor_dist + vert_dist * vert_dist);
                        max_dist = (int) Math.round(dist);
                    }
                    coef[j] = 1;
                    constraints.add(new LinearConstraint(coef, Relationship.LEQ, max_dist));
                    coef[j] = 0;
                    //constraints.add(new LinearConstraint(new double [] {coef[j]}, Relationship.LEQ, hor_dist - coef[i]));
                    //constraints.add(new LinearConstraint(new double[] {coef[j]}, Relationship.LEQ, vert_dist - coef[i]));
                }
            }
        }
        if (funct == 1) {
            int maxRad = getMaxRad();
            double[] coef;
            for (int i = 0; i < circles.length; i++) {
                Circle first = circles[i];
                coef = new double[circles.length * maxRad];
                for (int k = 0; k < circles.length; k++) {
                    for (int j = 0; j < maxRad; j++) {
                        coef[maxRad * k + j] = 0;
                        if (i == k) {
                            coef[maxRad * k + j] = 1;
                        }
                    }
                }
                //These constraints ensure the radius to be less than the distance to each wall
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, circles[i].getX()));
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, circles[i].getY()));
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, WIDTH - circles[i].getX()));
                constraints.add(new LinearConstraint(coef, Relationship.LEQ, HEIGHT - circles[i].getY()));
                for (int j = i + 1; j < circles.length; j++) {
                    Circle second = circles[j];
                    int hor_dist = Math.abs(first.getX() - second.getX());
                    int vert_dist = Math.abs(first.getY() - second.getY());
                    int max_dist = 0;
                    //if shape is square
                    if (shape == 0) {
                        if (vert_dist > hor_dist) {
                            max_dist = vert_dist;
                        }
                        else {
                            max_dist = hor_dist;
                        }
                    }
                    //if shape is circle
                    if (shape == 1) {
                        double dist = Math.sqrt(hor_dist * hor_dist + vert_dist * vert_dist);
                        max_dist = (int) Math.round(dist);
                    }
                    
                    
                    for (int l = 0; l < maxRad; l++) {
                        coef[maxRad * j + l] = 1;
                    }
                    
                    
                    constraints.add(new LinearConstraint(coef, Relationship.LEQ, max_dist));
                    for (int l = 0; l < maxRad; l++) {
                        coef[maxRad * j + l] = 0;
                    }
                    //constraints.add(new LinearConstraint(new double [] {coef[j]}, Relationship.LEQ, hor_dist - coef[i]));
                    //constraints.add(new LinearConstraint(new double[] {coef[j]}, Relationship.LEQ, vert_dist - coef[i]));
                }
            }
        }
        return constraints;
    }
    
    public int getMaxRad() {
        int maxRad = HEIGHT / (2 * quadProx);
        if (WIDTH < HEIGHT) {
            maxRad = WIDTH / (2 * quadProx);
        }
        return maxRad;
    }
    
    //TODO Modify to work with GUROBI as well
    public void changeFunction() {
        maxVal = 0;
        minVal = WIDTH * HEIGHT;
        int num_Circles = numSquares;
        if (fixQuad) {
            quadProx = numSquares;
            proxSlider.setValue(quadProx);
        }
        int funct = function;
        if (!GUROBI) {
            if (funct == 0) {
                double[] coefficients = new double[num_Circles];
                for (int i = 0; i < num_Circles; i++) {
                    coefficients[i] = 1;
                }
                f = new LinearObjectiveFunction(coefficients, 0);
            }
            //TODO WARNING WILL NOT NECESSARILY WORK IF SIZE OF IMAGE IS INCREASED RESIZE
            if (funct == 1) {
                int maxRad = getMaxRad();
                double[] coefficients = new double[num_Circles * maxRad];
                for (int i = 0; i < num_Circles; i++) {
                    for (int j = 0; j < maxRad; j++) {
                        //sets coefficients to be the radius binaries ascending grouped by circle
                        coefficients[j + i * maxRad] = 2 * j + 1;
                    }
                }
                f = new LinearObjectiveFunction(coefficients, 0);
            }
            generateConstraints(circles);
        }
        if (GUROBI) {
            GRBModel m = gurobiSolve();
            gurobiResize(m);
        }
    }
    
    public void play() {
        //Initialize Audio components
        final AudioFormat af = new AudioFormat(Note.SAMPLE_RATE, 8, 1, true, true);
        
        try {
            line = AudioSystem.getSourceDataLine(af);
            line.open(af, Note.SAMPLE_RATE);
        } catch (LineUnavailableException ex) {
            Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
        }
        line.start();
        this.display();
        if (NUM_FRAMES == 1) {
            try {
                Thread.sleep(SLOW);
            } catch (InterruptedException ex) {
                Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
            }
            this.step();
            this.display();
            this.drawCenters();
            canvas.writeFile(System.currentTimeMillis() + "A.png");
            function = 1;
            this.changeFunction();
            System.out.println("Done Printing A");
            GRBModel solution = this.gurobiSolve();
            gurobiResize(solution);
            this.display();
            this.drawCenters();
            line.drain();
            line.close();
            canvas.writeFile(System.currentTimeMillis() + "B.png");
            canvas.display();
            canvas.close();
            System.out.println("Done printing B");
            frame.dispose();
            return;
        }
        for (int i = 0; i < NUM_FRAMES; i++) {
            try {
                Thread.sleep(SLOW);
            } catch (InterruptedException ex) {
                Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
            }
            this.step();
            this.display();
        }
        int i = 0;
        while (NUM_FRAMES == -1) {
            try {
                Thread.sleep(SLOW);
            } catch (InterruptedException ex) {
                Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
            }
            this.step();
            this.display();
            i += 1;
        }
        line.drain();
        line.close();
        canvas.close();
    }
    //Draws centers, used exclusively when NUM_FRAMES == 1;
    private void drawCenters() {
        canvas.setPenColor(Color.YELLOW);
        for (int i = 0; i < circles.length; i++) {
            Circle iC = circles[i];
            canvas.drawCircleFill(iC.getX(), iC.getY(), 10);
        }
    }
    //Removes any information stored in Circle class or instances, and begins animation anew
    public void reset() {
        Circle.scrub();
        maxVal = 0;
        minVal = WIDTH * HEIGHT;
        int num_Circles = numSquares;
        circles = new Circle[num_Circles];
        for (int i = 0; i < num_Circles; i++) {
                circles[i] = new Circle(WIDTH, HEIGHT);
        }
        changeFunction();
        if (!GUROBI) {
            RealPointValuePair solution = solve();
            resize(solution);
        }
        else {
            GRBModel solution = gurobiSolve();
            gurobiResize(solution);
            //solution.dispose();
        }
    }
    
    //Instructs circles to move, then constructs and solves the new linear program
    public void step() {
        for (int i = 0; i < circles.length; i++) {
            if (circles[i] != null) {
                circles[i].move(0, 0, WIDTH - 1, HEIGHT - 1);
            }
        }
        if (!GUROBI) {
            RealPointValuePair solution = this.solve();
            resize(solution);
        }
        if (GUROBI) {
            GRBModel solution = this.gurobiSolve();
            gurobiResize(solution);
            //solution.dispose();
        }
    }
    
    //Takes in solution, modifies class instance circles to have matching radii
    private void resize(RealPointValuePair solution) {
        double[] values = solution.getPoint();
        int length = circles.length;
        lastVal = (int) solution.getValue();
        int funct = function;
        if (lastVal > maxVal) {
            maxVal = lastVal;
        }
        if (lastVal < minVal) {
            minVal = lastVal;
        }
        //TODO circles.length might not be correct for the last value of the solution
        for (int i = 0; i < length; i++) {
            if (i >= values.length) {
                continue;
            }
            double sum = 0;
            if (funct == 0) {
                sum = values[i];
            }
            else if (funct == 1) {
                int maxRad = getMaxRad();
                for (int k = 0; k < maxRad; k++) {
                    sum += values[i * maxRad + k];
                }
            }
            circles[i].setRadius((int) Math.round(sum));
            
        }
    }
    
    private void gurobiResize(GRBModel model) {
        //Grab instance variables
        int funct = function;
        int height = HEIGHT;
        int width = WIDTH;
        int maxRad = getMaxRad();
        Circle[] circs = (Circle[]) circles.clone();
        try {
            //Modify min and max obj.val seen so far
            int optimstatus = model.get(GRB.IntAttr.Status);
            if (optimstatus == GRB.Status.OPTIMAL) {
                double objval = model.get(GRB.DoubleAttr.ObjVal);
            }
            else{ 
                System.out.println("Fuck");
            }
            lastVal = (int) model.get(GRB.DoubleAttr.ObjVal);
            int length = circles.length;
            double[] vals = model.get(GRB.DoubleAttr.X, model.getVars());
            if (vals.length > length * maxRad) {
                return;
            }
            if (funct == 1) {
                //lastVal = lastVal * (int) Math.round(Math.pow(quadProx, length));
                lastVal *= quadProx;
            }
            if (lastVal > maxVal) {
                maxVal = lastVal;
            }
            if (lastVal < minVal) {
                minVal = lastVal;
            }
            //Loop through circles
            for (int i = 0; i < length; i++) {
                if (i >= vals.length) {
                    break;
                }
                double sum = 0;
                if (funct == 0) {
                    sum = vals[i];
                }
                else if (funct == 1) {
                    for (int k = 0; k < maxRad; k++) {
                        if (i * maxRad + k >= vals.length) { continue; }
                        sum += vals[i * maxRad + k];
                    }
                    sum = sum * quadProx;
                }
                Circle circ = circs[i];
                if (circ == null) {
                    continue;
                }
                circ.setRadius((int) Math.round(sum));
            }
        }
        catch (GRBException ex) {
            Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
    
    private RealPointValuePair solve() {
        constraints = generateConstraints(circles);
        RealPointValuePair solution = null;
        try {
            solution = solver.optimize(f, constraints, GoalType.MAXIMIZE, true);
        } catch (OptimizationException ex) {
            Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
        }
        return solution;
    }
    
    private GRBModel gurobiSolve() {
        int funct = function;
        int numCircles = circles.length;
        int height = HEIGHT;
        int width = WIDTH;
        try {
            GRBModel model = new GRBModel(env);
            GRBLinExpr obj = new GRBLinExpr();
            int maxRad = getMaxRad();
            if (funct == 0) {
                int n = numCircles;
                char type = 'C';
                GRBVar[] vars = model.addVars(n, type);
                model.update();
                double[] coef = new double[n];
                Arrays.fill(coef, 1);
                obj.addTerms(coef, vars);
                model.setObjective(obj, GRB.MAXIMIZE);
                model.update();
                gurobiConstraints(model, maxRad, vars);
                model.update();
                model.optimize();
            }
            if (funct == 1) {
                if (fixQuad) {
                if (numCircles >= 3 && quadProx < numCircles) {
                    quadProx = numCircles;
                    proxSlider.setValue(quadProx);
                }
                }
                int n = maxRad * numCircles;
                char type = 'B';
                GRBVar[] vars = model.addVars(n, type);
                model.update();
                double[] coef = new double[n];
                for (int i = 0; i < numCircles; i++) {
                    for (int j = 0; j < maxRad; j++) {
                        //sets coefficients to be the radius binaries ascending grouped by circle
                        coef[j + i * maxRad] = 2 * j + 1;
                    }
                }
                obj.addTerms(coef, vars);
                model.setObjective(obj, GRB.MAXIMIZE);
                model.update();
                gurobiConstraints(model, maxRad, vars);
                model.update();
                model.optimize();
            }
            return model;
        }
        catch (GRBException e) {
            System.out.println("Error code: " + e.getErrorCode() + ". " +
            e.getMessage());
            return null;
        }
    }

    private void gurobiConstraints(GRBModel m, int maxRad, GRBVar[] vars) {
        int funct = function;
        int width = WIDTH;
        int height = HEIGHT;
        int shp = shape;
        Circle[] circs = (Circle[]) circles.clone();
        if (funct == 0) {
            double[] coef = new double[circs.length];
            for (int i = 0; i < circs.length; i++) {
                Circle first = circs[i];
                if (first == null) {
                    continue;
                }
                coef = new double[circs.length];
                for (int k = 0; k < circs.length; k++) {
                    coef[k] = 0;
                }
                coef[i] = 1;
                GRBLinExpr left = new GRBLinExpr();
                try {
                    left.addTerms(coef, vars);
                    //These constraints ensure the radius to be less than the distance to each wall
                    m.addConstr(left, '<', circs[i].getX(), null);
                    m.addConstr(left, '<', circs[i].getY(), null);
                    m.addConstr(left, '<', width - circs[i].getX(), null);
                    m.addConstr(left, '<', height - circs[i].getY(), null);
                    for (int j = i + 1; j < circs.length; j++) {
                        Circle second = circs[j];
                        if (second == null) {
                            continue;
                        }
                        int hor_dist = Math.abs(first.getX() - second.getX());
                        int vert_dist = Math.abs(first.getY() - second.getY());
                        int max_dist = 0;
                        //if shape is square
                        if (shp == 0) {
                            if (vert_dist > hor_dist) {
                                max_dist = vert_dist;
                            } else {
                                max_dist = hor_dist;
                            }
                        }
                        //if shape is circle
                        if (shp == 1) {
                            double dist = Math.sqrt(hor_dist * hor_dist + vert_dist * vert_dist);
                            max_dist = (int) Math.round(dist);
                        }
                        coef[j] = 1;
                        GRBLinExpr left2 = new GRBLinExpr();
                        left2.addTerms(coef, vars);
                        //System.out.println(left);
                        m.addConstr(left2, '<', max_dist, null);
                        m.update();
                        coef[j] = 0;
                    }
                    m.update();
                }
                catch (GRBException ex) {
                    Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
                }
                coef[i] = 0;
            }
        }
        else if (funct == 1) {
            for (int i = 0; i < circs.length; i++) {
                for (int k = 0; k < maxRad - 1; k++) {
                    double[] coef2 = new double[maxRad * circs.length];
                    Arrays.fill(coef2, 0);
                    coef2[maxRad * i + k] = 1;
                    coef2[maxRad * i + k + 1] = -1;
                    GRBLinExpr left = new GRBLinExpr();
                    try {
                        left.addTerms(coef2, vars);
                        m.addConstr(left, '>', -.1, null);
                    } catch (GRBException ex) {
                        Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
                    }
                }
                Circle first = circs[i];
                double[] coef = new double[maxRad * circs.length];
                for (int k = 0; k < circs.length; k++) {
                    for (int j = 0; j < maxRad; j++) {
                        coef[maxRad* k + j] = 0;
                        if (i == k) {
                            coef[maxRad * k + j] = 1;
                        }
                    }
                }
                //These constraints ensure the radius to be less than the distance to each wall
                GRBLinExpr left = new GRBLinExpr();
                try {
                    left.addTerms(coef, vars);
                    m.addConstr(left, '<', circs[i].getX() / quadProx, null);
                    m.addConstr(left, '<', circs[i].getY() / quadProx, null);
                    m.addConstr(left, '<', (width - circs[i].getX()) / quadProx, null);
                    m.addConstr(left, '<', (height - circs[i].getY()) / quadProx, null);
                    
                    for (int j = i + 1; j < circs.length; j++) {
                        Circle second = circs[j];
                        int hor_dist = Math.abs(first.getX() - second.getX());
                        int vert_dist = Math.abs(first.getY() - second.getY());
                        int max_dist = 0;
                        //if shape is square
                        if (shp == 0) {
                            if (vert_dist > hor_dist) {
                                max_dist = vert_dist;
                            } else {
                                max_dist = hor_dist;
                            }
                        }
                        //if shape is circle
                        if (shp == 1) {
                            double dist = Math.sqrt(hor_dist * hor_dist + vert_dist * vert_dist);
                            max_dist = (int) Math.round(dist);
                        }
                        max_dist = max_dist / quadProx;
                        //Mofidy coef to contain variables relating to 'second'
                        for (int l = 0; l < maxRad; l++) {
                            coef[maxRad * j + l] = 1;
                        }
                        GRBLinExpr left2 = new GRBLinExpr();
                        left2.addTerms(coef, vars);
                        m.addConstr(left2, '<', max_dist, null);
                        //Reset coef for next object value of 'decond'
                        for (int l = 0; l < maxRad; l++) {
                            coef[maxRad * j + l] = 0;
                        }
                    }
                } 
                catch (GRBException ex) {
                    Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
                }
            }
        }
    }
    
    private void display() {
        Circle[] circs = circles.clone();
        int funct = function;
        if (minVal == maxVal) {
            minVal -= 1;
        }
        int bckgrnd = Math.round(255 * (lastVal - minVal) / (maxVal - minVal));
        if (bckgrnd < 0) {
            bckgrnd = 0;
        }
        if (bckgrnd > 255) {
            bckgrnd = 255;
        }
        if (!shadeBackground) {
            bckgrnd = 0;
        }
        canvas.setPenColor(bckgrnd, bckgrnd, bckgrnd);
        canvas.drawRectFill(0, 0, WIDTH, HEIGHT);
        int trail = Circle.getMemory();
        for (int i = 0; i < trail; i++) {
            int alpha = 255 * ((i + 1) * (i + 1)) / (trail * trail);
            for (int j = 0; j < circs.length; j++) {
                Circle circ = circs[j];
                ArrayList<State> states = circ.getStates();
                if (states.size() <= i) { continue;}
                State s = states.get(i);
                if (shape == 0) {
                    Color color = s.color;
                    if (!shadeShapes) {
                        color = new Color(255, 255, 255);
                    }
                    color = new Color(color.getRed(), color.getGreen(), color.getBlue(), alpha);
                    canvas.setPenColor(color);
                    canvas.drawRectFill(s.center.x - s.radius, s.center.y - s.radius, s.radius * 2, s.radius * 2);
                    color = new Color(0, 0, 0, alpha);
                    canvas.setPenColor(color);
                    canvas.drawRect(s.center.x - s.radius, s.center.y - s.radius, s.radius * 2, s.radius * 2);
                }
                if (shape == 1) {
                    Color color = s.color;
                    if (!shadeShapes) {
                        color = new Color(255, 255, 255);
                    }
                    color = new Color(color.getRed(), color.getGreen(), color.getBlue(), alpha);
                    canvas.setPenColor(color);
                    canvas.drawCircleFill(s.center.x, s.center.y, s.radius);
                    color = new Color(0, 0, 0, alpha);
                    canvas.setPenColor(color);
                    canvas.drawCircle(s.center.x, s.center.y, s.radius);
                }
            }
        }
        boolean trailed = false;
        for (int i = 0; i < circs.length; i++) {
            Circle a = circs[i];
            if (a == null) {
                continue;
            }
            //Draw squares
            int n2 = 0;
            if (funct == 0) {
                int radialSum = Circle.getRadialSum();
                double n = 0;
                if (radialSum != 0) {
                    n = a.getRadius() * 255 / radialSum;
                }
                n2 = (int) Math.round(n);
            }
            if (funct == 1) {
                int radialSquareSum = Circle.getRadialSquareSum();
                double n = 0;
                if (radialSquareSum != 0) {
                    int rad = a.getRadius();
                    n = rad * rad * 255 / radialSquareSum;
                }
                n2 = (int) Math.round(n);
            }
            if (n2 < 0) {
                n2 = 0;
            }
            if (n2 > 255) {
                n2 = 255;
            }
            //Draw squares
            Color color = null;
            if (shape == 0) {
                color = new Color(0, 32, n2);
                if (!shadeShapes) {
                    color = new Color(255, 255, 255);
                }
                canvas.setPenColor(color);
                canvas.drawRectFill(a.getX() - a.getRadius(), a.getY() - a.getRadius(), a.getRadius() * 2, a.getRadius() * 2);
                canvas.setPenColor(0, 0, 0);
                canvas.drawRect(a.getX() - a.getRadius(), a.getY() - a.getRadius(), a.getRadius() * 2, a.getRadius() * 2);
            }
            //Draw circles
            else if (shape == 1) {
                color = new Color(0, n2, 32);
                if (!shadeShapes) {
                    color = new Color(255, 255, 255);
                }
                canvas.setPenColor(color);
                canvas.drawCircleFill(a.getX(), a.getY(), a.getRadius());
                canvas.setPenColor(0, 0, 0);
                canvas.drawCircle(a.getX(), a.getY(), a.getRadius());
            }
            if (sonification) {
                Audio.play(line, a.note, 500);
            }
            if (trailFrames % TRAIL_FRAME == 0) {
                a.addState(a.getRadius(), new Point(a.getX(), a.getY()), color);
                trailed = true;
            }
        }
        trailFrames = trailFrames + 1;
        if (trailed == true) {
            trailFrames = 1;
        }
        canvas.display();
    }

    @Override
    public void stateChanged(ChangeEvent e) {
        if (e.getSource() == this.speedSlider) {
            //TODO make nonlinear
            SLOW = 100 - 1 * speedSlider.getValue();
        }
        if (e.getSource() == this.proxSlider) {
            quadProx = this.proxSlider.getValue();
        }
        if (e.getSource() == squareSlider) {
            //TODO add or remove squares rather than using reset()
            int val = squareSlider.getValue();
            if (numSquares != val) {
                numSquares = val;
                this.reset();
            }
        }
        if (e.getSource() == widthSlider) {
            int val = widthSlider.getValue();
            if (val != WIDTH) {
                canvas.mainFrame.setVisible(false);
                canvas.close();
                WIDTH = val;
                canvas = new Picture(WIDTH, HEIGHT);
                //TODO don't use reset();
                this.reset();
            }
        }
        if (e.getSource() == heightSlider) {
            int val = heightSlider.getValue();
            if (val != HEIGHT) {
                canvas.mainFrame.setVisible(false);
                canvas.close();
                HEIGHT = val;
                canvas = new Picture(WIDTH, HEIGHT);
                //TODO don't use reset();
                this.reset();
            }
        }
        if (e.getSource() == trailSlider) {
            int val = trailSlider.getValue();
            int temp = Circle.memory;
            Circle.memory = val;
            if (temp > val) {
                for (int j = 0; j < circles.length; j++) {
                    if (val != 0) {circles[j].removeTrail(val); }
                }
            }
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == quitButton) {
            System.exit(0);
        }
        else if (e.getSource() == shapeSquare) {
            shape = 0;
        }
        else if (e.getSource() == shapeCircle) {
            shape = 1;
        }
        else if (e.getSource() == functionLinear) {
            function = 0;
            changeFunction();
        }
        else if (e.getSource() == functionQuadratic) {
            function = 1;
            changeFunction();
        }
    }

    @Override
    public void itemStateChanged(ItemEvent e) {
        Object source = e.getItemSelectable();
        if (source == shadeShapesBox) {
            if (shadeShapes) {
                shadeShapes = false;
            }
            else {
                shadeShapes = true;
            }
        }
        if (source == shadeBackgroundBox) {
            if (shadeBackground) {
                shadeBackground = false;
            }
            else {
                shadeBackground = true;
            }
        }
        if (source == moveRandomBox) {
            Circle.noise = !Circle.noise;
        }
        if (source == sonificationBox) {
            sonification = !sonification;
        }
    }

    @Override
    public void keyTyped(KeyEvent e) {
        displayInfo(e, "KEY TYPED: ");
    }

    @Override
    public void keyPressed(KeyEvent e) {
        displayInfo(e, "KEY PRESSED: ");
    }

    @Override
    public void keyReleased(KeyEvent e) {
        displayInfo(e, "KEY RELEASED: ");
    }
    private void displayInfo(KeyEvent e, String keyStatus){
        
        //You should only rely on the key char if the event
        //is a key typed event.
        int id = e.getID();
        String keyString;
        if (id == KeyEvent.KEY_TYPED) {
            char c = e.getKeyChar();
            keyString = "key character = '" + c + "'";
            if (c == 'w') {
                try {
                    Thread.sleep(1000);
                    canvas.writeFile(System.currentTimeMillis() + ".png");
                    Thread.sleep(1000);
                } catch (InterruptedException ex) {
                    Logger.getLogger(Animation.class.getName()).log(Level.SEVERE, null, ex);
                }
                System.out.println("Image Saved");
            }
        } else {
            int keyCode = e.getKeyCode();
            keyString = "key code = " + keyCode
                    + " ("
                    + KeyEvent.getKeyText(keyCode)
                    + ")";
        }
        
        int modifiersEx = e.getModifiersEx();
        String modString = "extended modifiers = " + modifiersEx;
        String tmpString = KeyEvent.getModifiersExText(modifiersEx);
        if (tmpString.length() > 0) {
            modString += " (" + tmpString + ")";
        } else {
            modString += " (no extended modifiers)";
        }
        
        String actionString = "action key? ";
        if (e.isActionKey()) {
            actionString += "YES";
        } else {
            actionString += "NO";
        }
        
        String locationString = "key location: ";
        int location = e.getKeyLocation();
        if (location == KeyEvent.KEY_LOCATION_STANDARD) {
            locationString += "standard";
        } else if (location == KeyEvent.KEY_LOCATION_LEFT) {
            locationString += "left";
        } else if (location == KeyEvent.KEY_LOCATION_RIGHT) {
            locationString += "right";
        } else if (location == KeyEvent.KEY_LOCATION_NUMPAD) {
            locationString += "numpad";
        } else { // (location == KeyEvent.KEY_LOCATION_UNKNOWN)
            locationString += "unknown";
        }
    }
}
```
