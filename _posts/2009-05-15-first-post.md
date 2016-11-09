---
title: The First Post
tags: [meta]
---

I've been noticing for a while that in a conversation, I sometimes tend to stop my thoughts mid-sentence, leaving the listener wondering about the points I'm trying to make. These interruptions of speech are caused by nothing but a lack of vocabulary---stopping to search for an appropriate word, but effectively ending the conversation instead.

So here I go. I'm starting a blog. To improve my communication skills.

As far as I can tell, most personal blogs revolve around the blogger's life, which I find creepy and uninteresting. I won't blog about my life. Instead, I will put down some notes about projects I'm currently working on. I will mention things that I've encountered and found interesting. I will sometimes rant about technologies that are hyped too much and other people's opinions about those technologies. And I will rant about those people.

I've set myself a goal to publish at least one article a week. Let's see how it turns out.

## Blogging software
Since I've spent the last three days searching for the proper software to run the blog with, I think it's appropriate to give proper credit to the one that I've eventually chosen.

I've started with a pretty simple set of requirements that I wanted the blogging software to satisfy.

  1. The articles should be input in a simple wiki-like markup languages (Markdown for example).
  2. The software should turn three consecutive hyphens into an [em-dash][wiki-dash].
  3. It should be possible to add labels (tags) to the articles so as to make searching easier.
  4. The software should allow me to preview the article before I publish it.
  5. It should have a decent-looking theme.

I went through the [list of weblog software][wiki-weblog] on Wikipedia and tried to choose the best one. I would have preferred to use a hosted blog, as it would have saved me the trouble of hosting it myself. Unfortunately, hosted blogs tend to only support filtered HTML input.

I've tried most of the blogging packages I found on the aforementioned Wikipedia page, before I finally bumped onto [Drupal][drupal]. Through plugins it supports Markdown, a wiki-like language filter, and Typogrify, a filter that turns ASCII sequences to their proper Unicode equivalents (save for dashes, it can also turn straight quotes to curly ones, which I consider to be a nice bonus). It supports tags natively, although they must be explicitly enabled in configuration. Lastly, Drupal supports article preview (surprisingly, there are blogging packages that don't).

I'm very excited about Drupal and, hopefully, it will turn out to be as good as I feel it is.

  [wiki-dash]: http://en.wikipedia.org/wiki/Dash
  [wiki-weblog]: http://en.wikipedia.org/wiki/Weblog_software#Examples
  [drupal]: http://drupal.org/
