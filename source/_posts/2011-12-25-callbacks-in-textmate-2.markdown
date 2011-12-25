---
layout: post
title: "Callbacks in TextMate 2"
date: 2011-12-25 12:52
comments: true
categories: textmate, mac, editors
---
The [TextMate 2 alpha](http://blog.macromates.com/2011/textmate-2-0-alpha/) now supports callbacks in bundles. The callbacks are implemented as "semantic classes" in TextMate-speak. Our pal Alan shared a list of the new classes in a message to the TM mailing list which I have reproduced here:
<!--more-->

>* `callback.document.did-save`
* `callback.document.binary-import`
* `callback.document.binary-export`
* `callback.document.import`
* `callback.document.export`
* `callback.file-browser.action-menu`
* `callback.mouse-click`

>For the import/export callbacks input/output must be document/replace document. Binary means before/after TextMate converts to/from UTF-8/LF.

>For the mouse-click callback you can limit this to a modifier by using any of the following in the scope:

>* `dyn.modifier.shift`
* `dyn.modifier.control`
* `dyn.modifier.option`
* `dyn.modifier.command`

>The above should be considered subject to change.
