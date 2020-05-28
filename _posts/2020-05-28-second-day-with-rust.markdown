---
layout: post
title:  "Second Day with Rust"
date:   2020-05-28 
---

[Yesterday](https://chafey.github.io/2020/05/27/first-day-with-rust.html)
I continued on my learning Rust quest by focusing on cleaning up
the prototype DICOM RLE Decoder I created on the first day.  Overall the day
went much smoother - I have become familiar with Rust enough to make changes
with confidence.  The prototype was developed using a 16 bit grayscale
image but I hadn't tested 8 bit grayscale or 8 bit RGB so those were the
first things I wanted to do.  

I immediately ran into the buffer overflow issue with the US1 image that 
I encounted on day 1 with the CT1 image.  Since I already knew the 16 bit 
grayscale path was working, I decided to get CT1 working first as I
figured it would probably fix the issue with the US1 RGB image.
This is an interesting issue as the bug is technically in the RLE encoder,
but it is possible for the RLE decoder to detect and compensate for this
bug without too much cost.  I added some logic to prevent overwriting the
decoded buffer and got CT1 working.  

As expected, this allowed US1 to decode, but the decoded buffer did not
match the RAW US1 buffer indicating some kind of issue.  It turns out
that the byte swapping logic required for 16 bit grayscale unfortunately
does not work with the RGB data.  This is disappointing from a DICOM
specification point of view as it seems like it could have been
generalized.  I don't know the history of the 16 bit grayscale RLE
encoding specification work, but I suspect that there was a preference
for big endian CPU architectures which were popular in the early days of
DICOM on workstations.  Intel won the CPU war and is little endian so
I rarely deal with any kind of endianess issues nowadays.  It does bring
back some fond memories of the early days of computing though. 

I proceeded to add logic to handle the 8 bit case and the unit test passed.
The DICOM WG04 samples unfortunately didn't include an 8 bit grayscale image
so I had to create one myself.  For this scenario, I found an RF image
I had in my DICOM image collection and converted it to RLE using DCMTK.
The 8 bit image decoded properly so I moved on to adding unit tests for
various failure conditions and more cleanup.


Currently the code has been cleaned up a bit (at least as best as I know how
to with my current level of Rust knowledge), 11 passing unit tests and documentation 
both in the source and the README.  Feel free to take a look at the 
code [here](https://github.com/chafey/dicomrle-rs).  Today I will be working
on creating a WebAssembly build, JavaScript bridge code and examples of using
it from both NodeJS and the browser.





