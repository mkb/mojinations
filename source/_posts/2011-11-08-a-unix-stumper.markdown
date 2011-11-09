---
layout: post
title: "A Unix stumper"
date: 2011-11-08 10:38
comments: true
categories: 
---

>**EDIT:** Problem solved.  [Noah](https://twitter.com/#!/bleah) and [Dan](http://twitter.com/#!/dpiddee) hit the nail
on the head.  Updating the God config to explicitly use the correct gid rather than relying
on picking up additional groups from /etc/group did the trick.  I still wish that wan't
necessary, but a working setup is good enough for me.

If you have a good understanding of Unix process spawning and group affiliation, I could use your help.
<!--more-->

I'm seeing some unexpected behavior on Gentoo boxen.  I've got a user called "deploy"
who belongs to two groups:  deploy and rvm.  When I ssh into the box I can see that
deploy is in the correct groups:  

{% codeblock %}
deploy@[redacted] ~ $ id
uid=1000(deploy) gid=1000(deploy) groups=1000(deploy),1003(rvm)
deploy@[redacted] ~ $ groups
deploy rvm
{% endcodeblock %}

However, when I invoke a script from a [Resque](https://github.com/defunkt/resque) worker running as that user, I only
see one of the two groups:

{% codeblock %}
+ id
uid=1000(deploy) gid=1000(deploy) groups=1000(deploy)
+ groups
deploy
{% endcodeblock %}

Resque is spawned by [God](http://god.rubyforge.org/), which is spawned by inittab.

Of course the first thing I did was bounce Resque, then re-bounce after bouncing
the God process invoking Resque.  No love.  I've seen the same behavior on every
box I've tested on-- more than a dozen at this point.  

Help me, Obi Wan Kenobi, you're my only hope.
