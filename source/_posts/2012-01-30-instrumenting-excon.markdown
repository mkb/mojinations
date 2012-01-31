---
layout: post
title: "Instrumenting excon"
date: 2012-01-30 16:12
comments: true
categories:
---

[Excon](https://github.com/geemus/excon) is a Ruby gem for making HTTP requests.  A few features make excon a better choice than Net::HTTP:

* Persistent connections, which allows for much better performance when making many requests to the same host.
* Automatic retries for idempotent requests.
* Easy to stub for your unit tests.

My coworkers and I at Engine Yard needed to measure the performance of calls made with excon so we added one more feature:  instrumentation.
<!--more-->

## Enter ActiveSupport::Notifications

[Larry Diehl](https://twitter.com/larrytheliquid) called my attention to the new-fangled notification API in Rails 3, namely, [ActiveSupport::Notifications](http://api.rubyonrails.org/classes/ActiveSupport/Notifications.html).  Rather than reinvent the wheel, said Larry, it made sense to use an existing API which at least some people would already be familiar with.  ActiveSupport::Notifications is an implementation of our old friend, the publish-subscribe pattern and turns out to be pretty easy to use.

I won't re-create the RDoc here, but the basic idea is you can instrument blocks of code by wrapping them in an #instrument call like so:

{% gist 1707877 %}

Elsewhere in your app you can record those events by subscribing to them:

{% gist 1707917 %}

Now every time the first block is called, we'll be notified with a call to the second block.  Excellent.  Now what?

## With excon

When you include an instrumentor class in the Excon constructor you can then subscribe to excon's events:

{% gist 1707962 %}

If :instrumentor_name is omitted, the base name defaults to "excon".  Excon generates three different events:  excon.request, excon.retry, and excon.error.  Requests and retries each have associated durations, errors do not.  By digging into args we can determine how long the request took along with other assorted goodness.

## Too many gems!

Suppose you don't want to include activesupport in your application.  No problem.  Simply define a class which responds to the #insrument method and record incoming events however you please.

{% gist 1708120 %}

## Go forth and conquer

Hopefully I've given you enough information to start measuring excon for your own purposes.  In a future post I'll discuss how we used this excon feature to make pretty graphs of our calls to AWS.