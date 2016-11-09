---
title: The Site's New Layout
tags: meta
---

I've had a headache the whole day today. I couldn't really concentrate on programming, so after I got home from school I've decided to kill some time before the dose of ibuprofen would kick in, and post an article here. Unfortunately, when I looked at the state the site was in, I've decided to fix it instead. I've spent the rest of the afternoon relaying and reconfiguring the site.

First a little background. Originally, I wanted to keep the blog separate from main site. The idea was to have a set of static pages served by my quickbook parser (cue a link to the future article about the parser here) on the main site, featuring the projects I've worked on and some stuff about me (a CV perhaps). The blog was to be a leisure-time activity, a second-class citizen if you will.

About a half a year and three blog posts later, the blog already had ten times the content of the main site. (Haha, I could be a comedy writer. Yes, there were two sentences on the main site.) Given the data, a major restructuring was in order.

First, I've merged the two sites together. Although still accessible from both blog.ratatanek.cz and ratatanek.cz (the former of which also served as the name of the blog), only plain ratatanek.cz is now considered to be the proper name of the site. I will be removing the former name from Google's webmaster's utils shortly. (The [contents of the original site][1] is still available for the curious.)

The link to my source repository -- the only real contribution of the main site apart from the link to the blog -- is now available in the form of a button at the top of the page. Since I'm ultimately planning to add some static pages to the site as well, it seems logical to use a tab-like interface to switch between different modes of the site (a homepage, a blog, a repository browser, etc.).

The other thing I felt an intense desire to do was to get rid of the irritating blue look that I selected for the blog six months ago. I spent over an hour browsing through the list of Drupal's themes. The list is quite long, but you can actually scan through it pretty fast, simply by skipping over those templates that are overly colorful or have a ridiculously off-topic image in their header. I eventually settled on [Zero Point][2], a theme, which, while featuring some minor quirks, was actually decent-looking.

Having solved the important problems to my satisfaction, I went on to tackle with what seems to be a modern hype -- copyright issues. I read up on Creative Commons licenses and chosen the least restrictive one. The license text is now included at the bottom of every page. Surprisingly, there is [no need for a copyright notice][3] (i.e. a text of the form "Copyright Year Author"), the copyright automatically protects the author of the original work. I can't blame Zero Point for trying to be helpful and including it anyway. I do blame it for using the site's name as the copyright holder (in my case ratatanek.cz). I've removed the notice completely.

There are few other enhancements, the search box, the working [cron job][4]; the most important important one, however, is the feel the site now evokes (at least as far as I'm concerned). I'm hoping the new design will make me a little more motivated to post articles. I'm keeping my fingers crossed.

  [1]: http://technika.junior.cz/~avakar/
  [2]: http://drupal.org/project/zeropoint
  [3]: http://en.wikipedia.org/wiki/Berne_Convention_for_the_Protection_of_Literary_and_Artistic_Works
  [4]: http://drupal.org/project/poormanscron
