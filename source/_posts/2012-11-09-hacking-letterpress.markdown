---
layout: post
title: "Hacking Letterpress"
date: 2012-11-09 14:38
comments: true
categories: 
author: Max Veytsman
---

[Letterpress](http://www.atebits.com/letterpress/) is an iPhone game
that came out a few weeks ago and quickly became popular enough to
take down Apple's GameCenter. It's a cross between Boggle and Go. The
game is played on a board of 25 letters. Players take turns building
words in order to capture the letters they use. 

I found myself addicted to Letterpress until I figured out how to win consistently.

![winning](/assets/images/letterpress/winning.png)

As it turns out, Letterpress's dictionary is stored locally on the
phone. By adding words to Letterpress's dictionary, you can play any
combination of letters as word. For instance, you can play a word made
up of all the letters on the board like in the above screenshot.  

The dictionary is stored in a
series of text files in `/<Letterpress
folder>/Letterpress.app/o/[aa-zz].txt`. For instance, `ab.txt`
contains all the words that begin with aa, and so forth.

Modifying the dictionary makes it easy to cheat at letterpress, and I decided to
write a tool to help me do it.

First of all, it's a common misconception that you need to jailbreak a
phone to access an individual app's files. Actually, iOS application
directories are accessible on non-jailbroken phones. Tools like
[iExplorer](http://www.macroplant.com/iexplorer/) allow you to, among
other things, access an app's directory and modify the files there. I
believe they are using undocumented calls in the library iTunes uses for syncing to
do this.

Since I didn't want to rely on a third party paid tool like iExplorer,
I decided to use libimobiledevice.
[libimobiledevice](http://www.libimobiledevice.org/). Libimobiledevice
is an open-source library for talking to iDevices over USB. It
supports filesystem access, access to iPhone internals, and much more.
Libimobiledevice supports both OS X and Linux.

I wrote a ruby gem that acts as an adapter for
libimobiledevice and exposes some of the API calls in an object-oriented way. It's available on github as
[imobiledevice](https://github.com/stateio/imobiledevice). So far, I have
implemented only the small subset of libimobiledevice that I need,
but I certainly welcome pull reqests.

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