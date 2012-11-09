---
layout: post
title: "Hacking Letterpress"
date: 2012-11-09 14:38
comments: true
categories: 
author: Max Veytsman
---

[Letterpress](http://www.atebits.com/letterpress/) is an iphone game
that came out a few weeks ago and quickly became popular enough to
take down Apple's GameCenter. It's basically a cross between Boggle
and Go. Players take turns building words from a board of 25 letters.
Using a letter captures it, and your goal is to score points by
capturing your oppenents letters.

I got pretty addicted to it for a time, but I found that figuring out how to consistently win cured me of my addiction.

![winning](/assets/images/letterpress/winning.png)

As it turns out, the dictionary is stored locally. So, if you can add
words to Letterpress' dictionary, you can play any word you want. The
dictionary is stored in a series of text files in `/<Letterpress
folder>/Letterpress.app/o/[aa-zz].txt`. For instance, `ab.txt` contains all the words
that begin with aa, and so forth.  It's thus pretty easy to cheat at letterpress, and I decided to write an app to help me do it.

It's a common misconception that you need to jailbreak a phone to
access an individual app's files. iOS application directories are
actually accessible on non-jailbroken phones through the API that
iTunes uses to sync to your phone. Tools like
[iExplorer](http://www.macroplant.com/iexplorer/) allow you to, among
other things, access an app's directory and modify the files there. I
imagine they are using undocumented calls in the iTunes libraries to
do this.

I didn't want to rely on a 3rd party paid app like iExplorer, so I
used libimobiledevice.
[libimobiledevice](http://www.libimobiledevice.org/) is an open-source
library that allows you to talk to iOS devices, and it includes the
API. It works on OS X and Linux.

I ended up writing a ruby gem that acts as an adapter for
libimobiledevice. It's available on github as
[imobiledevice](https://github.com/stateio/imobiledevice). So far, I
implemented only the small subset of libimobiledevice that I needed,
but I certainly welcome pull reqests. It's very cool to be able to
talk to your iPhone from ruby.

Once I had a ruby wrapper for for libimobiledevice, it was very easy to write an app that adds any word you want to the dictionary.  I call it [letterpress-lexicographer](https://github.com/stateio/letterpress-lexicographer).  Here's how it works:

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