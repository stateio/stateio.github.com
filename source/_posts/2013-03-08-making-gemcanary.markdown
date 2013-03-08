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

The Ruby community was recently besieged by a series of devastating security vulnerabilities. As it turns out, everyone used to parse YAML files in a really unsafe way. Mad with power, back in the day folks had parsed all sorts of YAML in all sorts of places and so new vulnerabilities kept popping up at a disheartening pace.

We sighed and scrambled to update our apps. A couple of weeks later, while debugging my personal server, I noticed that I had left a couple of toy apps unattended. I had totally forgotten about them, on account of the fact that I hadn't touched them in well over a year.

After an extremely tense conversation, Max made me wipe the server and redeploy it from scratch. As a security oriented company, it would be *unfortunate* to get owned because we got lazy or distracted.

Yet, being "distracted or lazy" gets a bad rap, and an unwarrented moral connotation, in this scenario because the process *sucks*. 

It's actually really hard to keep up to date with every piece of ruby-related news. Once you've been in this game for five years, like I have, you end up collecting a lot of apps, servers and deployments you're responsible for. And if you work on a small team, depending on your social network to communicate critical patches isn't very tolerant of time spent away from the computer.

And so, we decided to automate it. 

[Gemcanary](https://gemcanary.com) will read your Gemfile.lock and notify you if any of your app's dependencies become vulnerable.

Given that we're a new consultancy, we found ourselves with some time between client projects and just went full blast for two of weeks. It was a lot of fun to work on. Although not every itch is worth investing this much time into, building an app you intend strangers to depend on is a very humbling and educational experience. 

There's a more lot to be said about building apps on Github's API, scratching itches and fixing the Ruby security process &mdash; which through [Rubysec](http://rubysec.github.com) we're trying to be a part of &mdash; and we'll be addressing all of this over the next few weeks.


