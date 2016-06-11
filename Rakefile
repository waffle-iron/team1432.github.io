#require 'middleman-gh-pages'
#ENV["BRANCH_NAME"] = 'master'

require 'rubygems'

desc 'Generate site from Travis CI and publish site to GitHub Pages'
task :travis do
  # if this is a pull request, do a simple build of the site and stop
  if ENV['TRAVIS_PULL_REQUEST'].to_s.to_i > 0
    puts 'Pull request detected. Executing build only.'
    system 'bundle exec middleman build'
    next
  end

  repo = %x(git config remote.origin.url).gsub(/^git:/, 'https:')
  deploy_branch = 'master'
  if repo.match(/github\.com\.git$/)
    deploy_branch = 'master'
  end
  system "git remote set-url --push origin #{repo}"
  system "git remote set-branches --add origin #{deploy_branch}"
  system 'git fetch -q'
  system "git config user.name 'ALIEN FROM MARS'"
  system "git config user.email '#{ENV['COMMIT_AUTHOR_EMAIL']}'"
  system 'git config credential.helper "store --file=.git/credentials"'
  File.open('.git/credentials', 'w') do |f|
    f.write("https://#{ENV['GH_TOKEN']}:x-oauth-basic@github.com")
  end
  puts 'gh-token'
  puts ENV['GH_TOKEN']
  system 'git pull origin source'
  print 'building'
  system 'bundle exec middleman build'
  puts '$ mv build ../'
  system 'mv build ../'
  #puts 'commiting'
  #system 'git commit -am "travis built"'
  puts '$ pwd'
  system 'pwd'
  print 'listing branches'
  system 'git branch'
  print 'branching'
  system "git branch #{deploy_branch} origin/#{deploy_branch}"
  puts 'branch:'
  system 'git branch'
  puts '$ mv ../build .'
  system 'mv ../build .'
  File.delete '.git/credentials'
end
