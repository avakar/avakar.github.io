---
title: Creole Wiki Markup
tags: [creole, wiki markup, bitbucket]
---

I've been using [bitbucket][1] for some time now to host my [hgdeps][2] Mercurial extension. I've never used any of the many features that bitbucket offers, including issue tracking and wiki hosting.

  [1]: http://bitbucket.org
  [2]: http://bitbucket.org/avakar/hgdeps


That changed today when I opened bitbucket and created a new repository, hoping to write down design notes regarding a software that I'm attempting to build. (I've made a simple "oscilloscope" -- an analog converter that would sample a signal at 160kHz and send the samples through an Ethernet connection. I need a software to display and manipulate the trace. But I digress.)

The wiki engine on bitbucket uses [Creole][3] markup. Since I really hate how every wiki engine uses a different markup, I was quite pleased by the attempt at standardization, even if somewhat informal. My initial enthusiasm, however, quickly disappeared when I saw the results that the labor yielded.

*Who in their right state of mind would use a double slash to indicate emphasis?* Actually, the so-called specification furthers its shameful act by making the double slash result in italics, thus dealing a hurtful blow to all of us that firmly believe in the semantic web. Please people, use [Markdown][4].

  [3]: http://www.wikicreole.org/wiki/Home
  [4]: http://en.wikipedia.org/wiki/Markdown
