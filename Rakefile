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
  deploy_branch = 'gh-pages'
  if repo.match(/github\.com\.git$/)
    deploy_branch = 'master'
  end
  system "git remote set-url --push origin #{repo}"
  system "git remote set-branches --add origin #{deploy_branch}"
  system 'git fetch -q'
  system "git config user.name '#{ENV['GIT_NAME']}'"
  system "git config user.email '#{ENV['GIT_EMAIL']}'"
  system 'git config credential.helper "store --file=.git/credentials"'
  File.open('.git/credentials', 'w') do |f|
    f.write("https://#{ENV['GH_TOKEN']}:x-oauth-basic@github.com")
  end
  print 'building'
  system 'bundle exec middleman build'
  print 'here\'s git\'s config:'
  print ENV['GIT_NAME']
  system 'git config user.name'
  system 'git config user.email'
  print 'commiting'
  system 'git commit -am "travis built"'
  print 'listing branches'
  system 'git branch'
  print 'branching'
  system "git branch #{deploy_branch} origin/#{deploy_branch}"
  File.delete '.git/credentials'
end
