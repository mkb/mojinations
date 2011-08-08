---
layout: post
title: "Homebrew?  No thanks."
date: 2011-08-07 20:27
comments: true
categories: mac unix
---
I know lots of smart developers who have switched from MacPorts to Homebrew.  I don't think the switch makes sense.  Let's add up the pros and cons.

## Homebrew uses OS libs instead of building dependencies.

That's not a feature, that's a bug.  Consider the ramifications:

* Relying on OS tools and libraries uses less disk space.  So what?  Disk is essentially free.  Add up the size of those additional installs.  It doesn't amount to much.
* Builds are faster.  That's nice, but how often do you install packages compared to how often you use them?  This is a one-time cost and you can do other work while the builds are running.  Hardware advances keep making builds faster anyway.
* MacPorts controls dependencies, Homebrew does not.  Apple frequently ships old or even buggy libraries and tools.  Moreover these native components can change out from under us when the OS is upgraded or patched.  Why anyone would deliberately choose to cede control of dependencies is a mystery to me.

So using the system's native libraries and tools provides negligible benefit and incurs risk.  Not so good.

## Homebrew lacks GNU Screen

The MacOS built-in `screen` binary is slightly troubled, so I need to install another.  Homebrew can't help me.  If you're perfectly happy with the native screen, then this isn't applicable, but in my case the point goes to MacPorts.

## Homebrew installs packages into their own isolated prefixes and then symlinks into /usr/local.

I've repeatedly seen bright people tout this as an advantage which is befuddling.  Isolated prefix:  good.  Symlinking everything into `/usr/local`:  bad.  This means that to remove Homebrew from a machine, rather than nuking a single directory, I have to deal with symlink turds all over the place.  Brew can clean up the symlinks before I delete the executable, but what if the executable is broken?  Or what if I forget?  Or never knew?  That's a lot of mess to clean up.

Older versions of MacPorts left some residue around the system, but were pretty well behaved.  Current versions are very well behaved.  MacPorts isn't perfect in this regard, but beats Homebrew hands down.

## Homebrew doesn't want to run as root, but then wants to write to /usr/local

Yes, it's worse than just symlinks.  Homebrew wants to take over `/usr/local` which is commonly used for other third-party packages.  Homebrew users are expected to `chown` `/usr/local` to their own ID and troubleshooting advice commonly advises Homebrew users to remove `/usr/local` entirely.  Ceding control of `/usr/local` to Homebrew means trouble when additional packages are installed into `/usr/local` or when the machine has multiple users.

When told that taking over `/usr/local` is a terrible idea, Homebrew apologists respond with "that's only a suggestion.  You are free to use whatever install location you would like."  That's not quite true.  `/usr/local` isn't a suggestion, it's the default.  Customization is nice, but well-behaved software picks reasonable defaults.  The Homebrew installation instructions even go so far as to say "Pick another prefix at your peril!"  That's more than a simple suggestion.

## Way better command line user interface

I'm perfectly content with MacPorts' CLI, but perhaps I don't know what I'm missing.  I'll give the points here to Homebrew.

## Homebrew's recipes are in Ruby, while MacPorts' are in Tcl

I like Ruby, I dislike Tcl.  However, the language used by my ports system is moot.  If I need to know what language my ports system uses, then the ports system has failed in its primary mission. 

## Homebrew and its installation scripts are hosted at Github and have good GitHub integration.

I like GitHub, but how does that affect Homebrew's suitability as a package manager?  Is there some use case I'm missing?

## Pure superficiality

It's petty of me, but I was disappointed to see someone purporting to be Max Howell, author of Homebrew respond to technical and factual criticism by [calling the critic an "ignorant twat."](http://news.ycombinator.com/item?id=1189274)  God knows we've all misbehaved on the internet at some point, but it left a bad taste in my mouth.

## The bottom line

At the end of the day, I need MacPorts for GNU Screen if nothing else.  I'm willing to entertain the idea of running another ports system alongside MacPorts, but I can't come up with a single good reason to give that job to Homebrew.