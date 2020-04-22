---
layout: post
title:  "New Linux Workstation"
date:   2020-04-22 15:40:00 +0000
categories: linux desktop workstation development
---

One of the reasons I left NucleusHealth was so I could spend more time
doing open source development.  This naturally lead me to question whether
or not I could switch away from Mac OS X to Linux.  This is a big deal
for me as I have been a major Apple fanboy since I bought my first
iMac in 2005.

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
about two weeks and have decided to go all in and switch from Mac OS X
to Linux.

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

My productivity on Linux is much higher than it is on Mac OS X.  There are 
three main reasons for this.  The first is that Linux just feels faster than
Mac OS X doing the exact same kinds of things.  I don't have any objective
measurements on this, but switching windows, compiling, web surfing all 
just feel snappy.  

The second is that Docker runs significantly faster on
Linux that it does on Mac OS X.  Not only is it faster, but it makes better
use of the system resources.  In Mac OS X, you have to allocate memory and
CPU cores for docker to use.  These allocated resources are taken away from
Mac OS X and all containers have to compete with each other for them.  The
net effect is that under heavy use, you don't use your system resources 
optimally and end up wasting time.  In Linux, all memory and CPU cores are 
automatically shared between the host OS and all docker containers 
resulting in maximum efficiency.  The other benefit of Docker on LInux is that shared
volume mounts on Linux run at full speed while they run at a significantly
[reduced speed on Mac OS X](https://docs.docker.com/docker-for-mac/osxfs-caching/).

The third reason is that the Pop!_OS desktop environment is extremely fast and
has many hotkeys that help me work quickly.  The Pop!_OS desktop environment 
is based on Gnome but comes with some great default settings

Overall I am amazed at the state of Linux on the desktop.  It is easy to install,
fast, works with most hardware and is easier to use than Mac OS X.  If you 
haven't tried Linux on the desktop recently, give Pop!_OS a shot! 
