---
title: Minix reloaded
tags: [minix]
---

We had a class reunion last week and I had a chance to talk to one of my former classmates, who is currently studying computer science in Prague. What's interesting is that he somehow got involved with [Minix][1] project.

  [1]: http://www.minix3.org/

I vaguely recall that Minix was originally intended to be a teaching tool written by a professor somewhere in Netherlands. It turns out that it made some interesting leaps forward. In fact, version 3 of the system is actually meant to be of practical use on embedded memory-restricted devices or devices requiring high reliability.

I slightly disagree with the amount of separation the Minix imposes on its device drivers. While I fully support memory separation between drivers, the system process and user processes, I feel that they should at least be allowed access to I/O space. At the moment, each I/O access (8-bit access, mind you) executes about 200 lines of C code, and performs two context switches (to and from the system process).

Save for that, the system seems to be on its way to reach its goals. Let's see how far it gets.
