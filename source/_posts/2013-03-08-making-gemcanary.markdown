---
layout: post
title: "What an MVP looks like: making gemcanary"
date: 2013-03-08 00:40
comments: true
categories: 
author: <a href="http://twitter.com/phillmv">Phillip Mendonça-Vieira</a>
meta: true
---

Here at [State Machinery](http://state.io) we're big fans of [lolcommits](https://github.com/mroth/lolcommits) and we set it up on every project we work on. [Gemcanary](https://gemcanary.com) was no different:

<iframe width="480" height="360" src="http://www.youtube.com/embed/R7pgAlCB3kU?rel=0" frameborder="0" allowfullscreen></iframe>

Scratching an itch
------------------

The Ruby community was recently besieged by a series of devastating security vulnerabilities. These things happen. It's hard to get everything right all the time and, if anything, for an ecosystem everyone loves to hate it's a little surprising it took this long for something this serious to emerge.

We sighed and scrambled to update our apps in production. 

However, a couple of weeks later while debugging my personal server, I noticed that I had left a couple of toy apps unattended. I had totally forgotten about them, on account of the fact that I hadn't touched them in well over a year. No one had visited these urls in forever and nothing important was stored on that machine, but we wiped the server as part of our standard operating procedure.

After spending a couple of hours putting everything that wasn't in a chef script back together, I felt really annoyed. The whole security process as we experience it *sucks*. 

It's actually really hard to keep up to date with every single piece of ruby-related news. If you work on a small team, depending on your social network to communicate critical patches totally falls apart if you ever plan on spending time away from the computer. And once you've been a web developer for a few years you end up collecting a lot of apps, repositories and deployments you're responsible for.

So, we decided to automate it. 

[Gemcanary](https://gemcanary.com) will read your Gemfile.lock and notify you if any of your app's dependencies become vulnerable.

Given that we're a new consultancy, we found ourselves with some time between client projects and just went full blast for two weeks. It was a lot of fun to work on. Although not every itch is worth investing this much time into, building an app you intend strangers to depend on is a very humbling and educational experience. 

There's a more lot to be said about building apps on Github's API, scratching itches and fixing the Ruby security process &mdash; which, through [Rubysec](http://rubysec.github.com) we're trying to be a part of &mdash; and this is something we'll be exploring more of over the next few weeks.


