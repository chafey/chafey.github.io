---
layout: post
title:  "DICOM Image Decoding with WebAssembly"
date:   2020-04-16 
categories: wasm dicom compression codec 
---

One of the biggest challenges developers face while building web based medical imaging applications is fast image loading. The main issue here is that medical images are typically stored on the server using an image compression format such as JPEG-2000 or JPEG-LS that web browsers don't know how to decode. There are several strategies to handle this:

1) Have the server transcode the image into an uncompressed image format. This eliminates the need to decode the image on the client, but requires that the server have the appropriate hardware and architecture to support this at scale. This strategy will not work well in low bandwidth scenarios since uncompressed images require more bandwidth than compressed images.

2) Decode the images in the browser using a decoder written in JavaScript. During the early days of [CornerstoneJS](https://cornerstonejs.org) development, I found JavaScript decoders for JPEG, JPEG Lossless and JPEG2000. The JPEG and JPEG Lossless decoders were ports from the original C code and generally work. The JPEG2000 decoder was extracted from the Mozilla PDF.js project but didn't support all JPEG2000 features.

3) Cross compile C/C++ libraries into JavaScript using EMSCRIPTEN. Cornerstone uses this approach to add support for JPEG-LS (via CharLS) and JPEG 2000 (via OpenJPEG)

Since I originally created the EMSCRIPTEN builds of CharLS and OpenJPEG 4 years ago, a lot has changed. New major versions of each library are available which may provide faster decode speeds. WebAssembly is now widely available in Web Browsers and brings many benefits compared to the JavaScript based EMSCRIPTEN output. Specifically:

* EMSCRIPTEN WebAssembly output is typically smaller than JavaScript output. This means that page load speeds are improved due to less code being downloaded in the first place and faster parse times due to WebAssembly being easier to parse than JavaScript.
* WebAssembly is capable of running faster than JavaScript. The general feeling is that JavaScript will at best be half the speed of native code. WebAssembly on the other hand was designed to run at 90% of the speed of native code.

I recently started working on updating the CornerstoneJS codecs and was curious to see how WebAssembly performance compared to native and EMSCRIPTEN JS builds. I ended up implementing a DICOM RLE Decoder in C++ and creating a JS/WASM build from it. You can find the code for this here: https://github.com/chafey/modern-cpp-lib-js

Here are the performance numbers I measured for decoding a 512x512 16 bit CT image 1000 times on the same hardware (2014 27" iMac Retina Intel i7-4790k @ 4.4GHz processor):

### Linux Timings (Ubuntu 19.10)

* Native C++: .313
* Node 13.13.0 WebAssembly: .572
* Google Chrome 81.0 WebAssembly: 1.162
* Firefox 75.0 WebAssembly: 1.679

### Linux + Docker Timings (Ubuntu 19.10 + Docker 19.03.06)

* Native C++: .378
* Node 13.13.0 WebAssembly: .558
* Node 12.16.2 WebAssembly: .703
* Node 10.20.1 WebAssembly: .722
* Node 8.9.1 WebAssembly: .759

### Mac OS X Timings (macOS Catalina 10.15.4)

* Native C++: .387
* Node 13.13.0: .566
* Firefox 75.0 WebAssembly: 626 ms
* Google Chrome 81.0 WebAssembly: 628 ms
* Firefox 75.0 WebAssembly: 626 ms
* Safari 13.1: 699 ms
* Node 12.6.2: .729

### Mac OS X + Docker Timings (macOS Catalina 10.15.4 + Docker 19.03.8)

* Native C++ in Docker Container: .342
* Node 8.9.1 WebAssembly in Docker Container: .904

A few conclusions from this testing:

1) WebAssembly in node is improving every version with the latest version being less < 2x slower that native

2) WebAssembly performance in Mac OS X popular browsers is about 2x slower than native code

3) WebAssembly in Linux browsers is much slower than Mac OS X browsers

Given these results, it seems that WebAssembly is a good option for medical image decoding in web browsers today and will likely continue to improve in the future. Feel free to try out how your machine and OS performs by opening the test here: https://chafey.github.io/modern-cpp-lib-js/test/browser/