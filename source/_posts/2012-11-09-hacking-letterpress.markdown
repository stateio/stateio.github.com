---
layout: post
title: "Hacking Letterpress"
date: 2012-11-09 14:38
comments: true
categories: 
author: <a href="http://twitter.com/mveytsman">Max Veytsman</a>
---

[Letterpress](http://www.atebits.com/letterpress/) is an iOS game
that came out a few weeks ago and immediately became popular enough to [take down Apple's GameCenter](https://twitter.com/marcoarment/statuses/261337316268859392). It's a cross between [Scrabble and Go](http://daringfireball.net/linked/2012/10/24/letterpress). The game is played on a board made out of 25 letters and players take turns building words in order to capture the letters they use. 

I was hopelessly addicted to Letterpress until I figured out how to win consistently.

![winning](/assets/images/letterpress/winning.png)

As it turns out, Letterpress's dictionary and word check is stored and performed locally on your
phone. By simply adding words to Letterpress's dictionary, you can register any
combination of letters as a valid word.

Inside your iPhone, the dictionary is spread across a
series of text files located in `/<Letterpress
folder>/Letterpress.app/o/[ab-zz].txt`. For instance, `ab.txt`
contains all the words that begin with aa, and so on and so forth.

However, digging around a bunch of text files is no fun, and so I decided to
write a tool to help me out.

## Automating Letterpress cheating

First of all, it's a common misconception that you need to jailbreak a
phone in order to access an individual app's files. 
Tools like
[iExplorer](http://www.macroplant.com/iexplorer/) allow you to, among
other things, access an app's directory and modify the files there on any iPhone. (It's my understanding that they're using undocumented calls in the library iTunes uses for syncing in order to
pull this off.)

Since I didn't want to rely on a third party paid tool like iExplorer,
I decided to use libimobiledevice.
[libimobiledevice](http://www.libimobiledevice.org/). Libimobiledevice
is an open-source library for talking to iDevices over USB. It's capable of providing
access to the filesystem, the iPhone internals, and much more. 
Libimobiledevice supports both OS X and Linux.

So, I wrote a ruby gem that acts as an adapter for
libimobiledevice and exposes some of the API calls in an object-oriented way. It's available on github as
[imobiledevice](https://github.com/stateio/imobiledevice). So far, I have
implemented only the small subset of libimobiledevice that I need,
but I definitely welcome pull requests.

Once I had a ruby wrapper for for libimobiledevice, it was simple to
write an app that adds arbitrary words to the Letterpress dictionary.
I call it
[letterpress-lexicographer](https://github.com/stateio/letterpress-lexicographer).
Here's how it works:

## Is this a word?

![Before](/assets/images/letterpress/before.png)

## Shucks!

```
./letterpress-lexicographer.rb
 [*] Connecting to iDevice
 [*] Connected to iDevice. UDID:REDACTED
 [*] Accessing Letterpress app
 [*] Accessed Letterpress app
 [?] What word would you like to add? (/q to quit)
  -> szug
 [+] Reading word-list /Letterpress.app/o/sz.txt
 [+] Inserting word szug into word-list /Letterpress.app/o/sz.txt
 [+] Successfully added word szug.  You can play it now!
```
## Let's try again
![After](/assets/images/letterpress/after.png)

You can find the app [here](https://github.com/stateio/letterpress-lexicographer).  Please enjoy responsibly.
