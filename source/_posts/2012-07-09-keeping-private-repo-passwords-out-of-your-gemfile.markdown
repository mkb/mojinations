---
layout: post
title: "Keeping private repo passwords out of your Gemfile"
date: 2012-07-09 11:25
comments: true
categories: ruby, bundler, secuirty
---
Suppose you've got your own private gem repository for your company's internal
rubygems.  The Gemfile for one of your internal projects might look like this:

``` ruby Gemfile
source :rubygems
source 'http://dhh:wizard@rubygems.mycorp.us/'

gem 'sinatra'
gem 'haml'

gem 'awesome_secret_gem'
gem 'sooper_s3kr1t'
```

Now you can freely access both public and private gems.  This arrangement
works nicely but the login to your repository is now checked into version
control.  That's a no-no, right?  The whole idea is to keep your internal gems
private but you've made it easier to accidentally share them with the world.

So long as you don't need to check your Gemfile.lock into source control as
well, there's a straightforward way to get that private repo information
out of your Gemfile.
<!--more-->
## Ruby to the rescue

Fortunately for us, Gemfiles are just Ruby code.  Rather than hard coding the
login to our private repository, we can grab it some other way.  Environment
variables are an obvious choice:

``` ruby Gemfile
source :rubygems
source ENV['PRIVATE_REPO_URL']

gem 'sinatra'
gem 'haml'

gem 'awesome_secret_gem'
gem 'sooper_s3kr1t'
```

Environment variables work, but they aren't ideal.  Passing one or two
parameters around via the environment can be really handy, but the approach
does not scale well.  As environment variables proliferate, life gets messy.

If you're like me and you keep your dotfiles under source control, then moving
the pivate repo details into an environment varable just moves the problem to
a different codebase without solving it.  Worse still, environment variables
are passed all over the place which provides even more opportunity to leak
your secrets.  Not so good.

## Bundler badassery

Bundler's built-in configuration mechanism has us covered.  You may already
have a Bundler config in your home directory.  Mine looks like this:

``` yaml ~/.bundle/config
---
BUNDLE_PATH: vendor/bundle
BUNDLE_DISABLE_SHARED_GEMS: '1'
```

Let's add our private repo to the config:

``` sh
% bundle config private_gem_server 'http://dhh:wizard@rubygems.mycorp.us/'
```

Now the Bundler config looks like this:

``` yaml ~/.bundle/config
---
BUNDLE_PATH: vendor/bundle
BUNDLE_DISABLE_SHARED_GEMS: '1'
BUNDLE_PRIVATE_GEM_SERVER: http://dhh:wizard@rubygems.mycorp.us/
```

## Tying it all together

We use Bundler.settings to access the newly added repo URL:

``` ruby Gemfile
source :rubygems
source Bundler.settings['private_gem_server']

gem 'sinatra'
gem 'haml'

gem 'awesome_secret_gem'
gem 'sooper_s3kr1t'
```

Voilà!

## The bad news

As I mentioned at the outset, this does not work if you also need to check
your Gemfile.lock into source control-- or rather, it still works, but there
is little point.  Bundler sees fit to add the private repo URL to
Gemfile.lock.  So very  sad.

When I find a good workaround, I'll let you know.  If you know of one, please share!


