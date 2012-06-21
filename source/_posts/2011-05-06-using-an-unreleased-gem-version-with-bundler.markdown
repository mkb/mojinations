---
layout: post
title: "Using an unreleased gem version with Bundler"
date: 2011-05-06 23:06
comments: true
categories: ruby bundler
---
## TL;DR

Do this:

``` ruby
    gem 'nokogiri'
    gem 'w3c_validators', "1.1.1", :git => 'git://github.com/alexdunae/w3c_validators.git'
```
<!--more-->
## Why?

After experiencing trouble with the w3c_validators gem, I discovered that the problem was a [known bug](https://github.com/alexdunae/w3c_validators/issues/3).  A fix has been committed, but not yet released in gem form.  I could have gone with an older version of the gem, but instead tweaked my Gemfile (ie, Bundler) to use the latest and greatest code.

I added a :git argument to my Gemfile, instructing bundler to grab the w3c_validators gem straight from the GitHub repo, bypassing rubygems.org.

``` ruby
    gem 'w3c_validators', :git => 'git://github.com/alexdunae/w3c_validators.git'
```

This turned out not to work:

    Could not find gem 'w3c_validators (>= 0, runtime)' in 
    git://github.com/alexdunae/w3c_validators.git (at master).
    Source does not contain any versions of 'w3c_validators (>= 0, runtime)'

The code was present, but the latest revision lacks a gemspec.  To work around this, Bundler needs to know what version number to use for the code it pulls down:

``` ruby
    gem 'w3c_validators', "1.1.1", :git => 'git://github.com/alexdunae/w3c_validators.git'
```

KABLAM.  Still not quite right.  Without a gemspec, Bundler has no way of knowing that w3c_validators depends on nokogiri, so we must state the dependency explicitly:

``` ruby
    gem 'nokogiri'
    gem 'w3c_validators', "1.1.1", :git => 'git://github.com/alexdunae/w3c_validators.git'
```

And then there was much rejoicing throughout the land.

## RTFM

[http://gembundler.com/git.html](http://gembundler.com/git.html)
