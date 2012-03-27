---
layout: post
title: "Data integrity desiderata"
date: 2012-03-26 11:45
comments: true
categories:
---

https://gist.github.com/003fde2b9ae76aeeab01

I'm thinking out loud here and I'd like your help.

How do we maintain data across distributed systems without breaking
everything? Here are a few thoughts. Please let me hear your thoughts as well.

## Each datum must have a single, unambiguous, authoritative representation.

Yes, it's our old friend the DRY principle. This does not mean data cannot
appear in more than one place, but rather that only one place is
authoritative. Sometimes replication is a necessary evil.

That said:

### Avoid caching until you are sure you need it.

Caches are one of [the two hard things](http://martinfowler.com/bliki/TwoHardThings.html)
in computer science.  Go ahead and make repeated queries to the remote system
until you have a measurable problem.

### Encapsulate local representations of remote data.

When you store more than one datum from an external system, pull them out into
a separate model object.

## Divide responsibilities logically and clearly

### Re-visit these divisions as your understanding matures.
### Place data where they are used most.
### Strive for orthogonality.

#### Draw lines of responsibly to minimize types of interaction.

...or alternately...

#### Draw lines of responsibly to minimize quantity of interaction.

Which of the two is more important?  Why?

## Connections between components should be self-maintaining.

If you need a relationship between your data and a remote system, be prepared
to create or repair that relationship whenever you need it.

### Assume that the other system will occasionally go down.  What will you do?

In particular, the remote system might go down while you are initializing
data, preventing you from creating a remote record you will need later.

### Recognize which changes may happen outside of your control.

Once you have nice, consistent data, people can pull the rug out from under
you. If people can modify the remote system in ways you don't expect, they
will do so.

### Implicit data

A reference to a record in an external system is an assumption that the record
actually exists. The inverse is also true. If the external system is the
single point of truth (and it should be) then you must be prepared for these
assumptions to be false and to correct the inconsistency.

## Insist on consistent naming.

### Find good naming conventions.
### Don't let name inconsistencies linger.  Fix broken windows.
### When name inconsistencies are outside your control make the adapter clear and explicit.






