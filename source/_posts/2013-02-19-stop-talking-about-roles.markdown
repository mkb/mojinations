---
layout: post
title: "Stop talking about roles"
date: 2013-02-19 17:52
comments: true
categories: security
---
Stop talking about roles.

Why? Because roles muddle our thinking.

The concept of roles in computer security merges two concepts (facts and
policies) into one--facts about particular people and policies about who can
and cannot access particular resources. The distinction is worth maintaining
because the locus of authority differs between the two.

<!--more-->

For example, HR knows whether or not I am an employee while the design
department knows that only employees should have access to the new prototype.
"Is an employee" is a fact about me, but "employees only" is a policy based on
that fact. Similarly, Legal knows whether or not Alice has an active work
contract, but only you know whether the document you just wrote should be
accessible to both employees and contractors or only employees.

"But wait," I hear you say, "that's just semantics." Yes, but semantics
matter. Semantics matter because they shape the way we think. The concept of
roles drives  us toward a mental model where access control is centralized--
one group or department  is responsible for managing role membership and
policies. The larger your organization, the less this makes sense.
Centralizing access control means that rather than your system receiving
information directly from authorities (HR, Design, Legal, you) each authority
must transmit their information to someone else who takes action on their
behalf. This is slow, expensive, and error prone.

Save yourself some trouble and cut out the middleman as much as you can
manage. Bring the ability to act closer to the people with information. Stop
talking about roles; talk about facts and policies instead.
