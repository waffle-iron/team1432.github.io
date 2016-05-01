---
title: This is a Test
date: 2016-04-23 17:41 UTC
author: caleb e
tags: something, other, test
---
this is some text with formatting like **bold** and *italics* This is the really long summary with some text.

READMORE

this is a [test](https://google.com)

>block
>quote

Our `Java` encoder code from our 2016 bot:

```java
package org.usfirst.frc.team1432.robot;

import edu.wpi.first.wpilibj.*;
import java.util.concurrent.locks.*;

public class Encoder extends Thread {
  AnalogInput input;
  double current;
  double ago1;
  double rotations;
  double resetValue;
  public static double degreesPerRotation = 360;
  public static double inchesPerRotation = 0.88;
  public static double centemetersPerRotation = 2.24;
  private ReentrantLock lock;
  private Thread thread; 
  private Boolean cont;
  
  
  public Encoder(int port) {
    cont = false;
    input = new AnalogInput(port);
    current = 0;
    ago1 = 0;
    rotations = 0;
    lock = new ReentrantLock();
    reset();
  }
    
  public void reset(){
    lock.lock();
    resetValue = input.getAverageVoltage()/5;
    rotations = 0;
    current = 0;
    ago1 = 0;
    lock.unlock();
  }
  
  public double round(double value){
    return Math.round((value) * 100d) / 100d;
  }
  public double roundlong(double value){
    return Math.round((value) * 10000d) / 10000d;
  }
  
  public void print(String string) {
      System.out.println(string);
    }
    
    public void print(double Double){
      System.out.println(round(Double));
    }
    
    public void print(int Int) {
      System.out.println(round(Int));
    }
    
  /*
   * Gets double value from input 
   */
  public double getValue(){
    lock.lock();
    double value = current;
    lock.unlock();
    return value;
  }
    
    public double getRotations(){
      lock.lock();
      double value =  rotations + current;
      lock.unlock();
      return value;
    }
  public double getDegrees(){
    lock.lock();
    double value = (current)*degreesPerRotation;
    lock.unlock();
    return value;
  }
  public double getCM(){
    lock.lock();
    double value = (current+rotations)*centemetersPerRotation;
    lock.unlock();
    return value;
  }
  public double getInches(){
    lock.lock();
    double value = (current+rotations)*inchesPerRotation;
    lock.unlock();
    return value;
  }
    
  @Override
  public void run() {
    Boolean running = cont;
    while(running) {
      lock.lock();
      ago1 = current;
      current = (input.getVoltage()/5)-resetValue;
      if (current - ago1 < -.8) {
        rotations++;
      }
      if (current - ago1 > .8) {
        rotations--;
      }
      running = cont;
      lock.unlock();
      Timer.delay(.02);
    }
  }
  
  public void start() {
    if(thread == null) {
      cont=true;
      thread = new Thread(this);
      thread.start();
    } else {
      cont=true;
      thread = null;
      thread = new Thread(this);
      thread.start();
    }
  }
  public void stoprun() {
    print("stoprun");
    // TODO Auto-generated method stub
    lock.lock();
    cont = false;
    lock.unlock();
  }
}
```

See the rest of our code on [GitHub](https://github.com/team1432/FRC-2016)

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec egestas elementum sodales. Phasellus sagittis felis non sem efficitur, ut tristique magna ullamcorper. Quisque pellentesque a lectus eu facilisis. Nunc risus eros, sodales eget dignissim ultrices, porta sed ligula. Suspendisse sit amet odio quis dui vehicula interdum id sed ligula. Nam augue velit, aliquet eget lobortis quis, posuere in erat. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Vestibulum a tristique tellus.

Quisque eget nibh hendrerit, hendrerit erat sit amet, egestas nisl. Vivamus laoreet efficitur molestie. Phasellus eget tortor et est varius finibus eleifend sed nulla. Quisque ut eros ac tellus commodo iaculis. Pellentesque vitae maximus nisl. Nullam a enim tempor, laoreet dolor non, feugiat mi. Pellentesque gravida ante ut tempor accumsan. Integer volutpat sodales interdum. Donec nisl ligula, placerat eget tincidunt vel, laoreet ac nisl. In ullamcorper augue lobortis elit faucibus luctus. Praesent sit amet vulputate ante. Pellentesque nec nibh et ligula semper vulputate. Integer volutpat massa volutpat interdum placerat. Proin turpis ligula, fermentum vitae porta in, ornare ut neque.

Nulla facilisi. Quisque eu erat vel nisl mollis rutrum. Duis auctor eget enim sollicitudin tempus. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed in malesuada odio. Nunc et euismod nunc, non egestas ex. Nulla egestas est vitae pellentesque tempor. Vivamus pellentesque vitae erat at aliquam. Duis id lacinia nibh, quis ornare leo. Pellentesque nunc sapien, ornare ut neque non, bibendum feugiat diam.

Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Nulla posuere a nisi a ullamcorper. Vivamus rhoncus, ex et rhoncus tincidunt, ante libero tincidunt velit, in pharetra dui magna non risus. Vestibulum ullamcorper, mauris ac viverra facilisis, erat urna dapibus ex, vel semper lorem ante congue orci. Nunc elit felis, tempor non molestie et, tincidunt nec dui. Suspendisse nunc mauris, placerat vitae nulla at, porttitor elementum justo. Aenean fringilla sollicitudin tristique. Donec tincidunt ultricies placerat.

Nulla sit amet feugiat nunc. Phasellus eget turpis ac nunc efficitur vehicula tempor quis mi. Sed mattis convallis nisi id suscipit. Nunc neque lorem, ornare non ante vel, convallis cursus tellus. In in lacinia mauris. Curabitur ac venenatis velit, in aliquam massa. Maecenas a neque a diam bibendum porttitor sagittis viverra lacus. Quisque id eleifend nisl. Morbi feugiat rutrum euismod. Vivamus at eros posuere, elementum neque non, vestibulum orci.
