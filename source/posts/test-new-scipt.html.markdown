---
title: Test New Script
date: 2016-05-01 15:42:03 -0700
author: Caleb
tags: 
---
```ruby
require 'thor'

class Edit < Thor
  include Thor::Actions
  desc 'post', 'edits a post'
  def post
    #_posts = Dir['source/posts/*']
    #for post in _posts
    posts = []
    Dir.entries('source/posts').select {|f| !File.directory? f}.each do |file|
      posts.push file.sub('.html.markdown', '').gsub('-', ' ')
    end
    puts posts
    name = ask 'What is the name of the post you would like to edit?', limited_to: posts
    system "vim #{'source/posts/' + name.gsub(' ', '-') + '.html.markdown'}"
    if yes? 'Would you like to publish your post?'
      invoke :publish
    end
  end
end
```
