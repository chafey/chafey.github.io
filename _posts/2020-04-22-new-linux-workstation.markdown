---
layout: post
title:  "My New Linux Workstation"
date:   2020-04-22 15:40:00 +0000
categories: linux desktop workstation development
---

I bought my first iMac in 2005 and have been an Apple fanboy ever since.  
One of the reasons I left NucleusHealth was so I could spend more time
doing open source development.  This naturally lead me to question whether
or not I could switch away from Mac OS X to Linux.  

I first became aware of [Linux Mint](https://www.linuxmint.com/) about 
18 months ago and was impressed with how well it worked.  It really felt 
like a modern operating system and was almost as easy to use as Mac OS X.
I decided to install Linux Mint on my 2014 27" iMac and was surprised to 
find that it installed cleanly and most of the hardware was properly
detected and just worked.  I used it for a few days and then discovered
[Pop!_OS](https://system76.com/pop), which was getting rave reviews.  I
decided to give it a shot and was absolutely blown away.

The installation process was a breeze - it was very clean and easy to do.  
The desktop environment is absolutely amazing - I quickly found that I 
preferred it over Mac OS X!  I have now been using Pop!_OS full time for
about two weeks and have decided to go all in and ditch Mac OS X.

My 2014 iMac was working well, but it had a few issues which were bugging me.
The first issue is with thermal management.  My 2014 iMac had the best CPU
you could get at the time - an i7 4790k which was the fastest processor in the
Intel Haswell architecture.  This means it generates a ton of heat and that
requires extra cooling.  Linux actually has good thermal management tools, but
I was unable to find a configuration that would keep things snappy while also
keeping the fans quite.  The fans would often get quite loud even when doing
basic things like web surfing.  I found the fan noise to be quite distracting -
any time they would turn on I would look to see what was going on and start
messing with the performance monitor.  Not good for productivity.

The second issue is that Linux does not support the iMac's 5k display
and instead runs it at 4k resolution.  The text still looked pretty clear
thanks to antialiasing (and probably degradation of eyesite due to age) but
it was frustrating to have hardware that wasn't being used.

The third issue is that it would hang/freeze on startup if a second monitor was
attached (it worked fine with just one monitor).  I eventually worked around
this by [setting a kernel parameter](https://www.reddit.com/r/pop_os/comments/fzyddv/gui_freezes_when_second_display_is_attached/)
but this also caused strange video artifacts to appear.  It is possible this
kernel parameter contributed to the thermal management issues as it may
have disabled GPU acceleration for video playback.

These issues plus the fact that current hardware is much better lead me to 
buy a new workstation specifically for Linux.  Today I picked up a new
Intel i9-9900K liquid cooled system and did a fresh install of Pop!_OS.  
The liquid cooling keeps the system cool and most importantly - quiet!  
The i9-9900K is quite fast and the power management works much better - it
often just runs at 0.8GHz during normal use and instantly jumps up to
4.9 GHz when it needs to.

