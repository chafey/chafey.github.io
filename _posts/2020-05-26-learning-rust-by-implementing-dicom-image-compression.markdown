---
layout: post
title:  "Learning Rust by implementing DICOM Image Compression"
date:   2020-05-26 
---

I am a big fan of the [Rust language](https://www.rust-lang.org/) but so far haven't done
more than read a few books and write some simple hello world applications.  I want to become proficient in Rust
because it has first class support for [WebAssembly](https://webassembly.org/),
runs as fast as C++ but is much safer.  I am a big believer in the future of WebAssembly and Rust
seems like the best language for building WebAssembly.  Rust does have a large learning curve, but
those that make it over the hump absolutely [love it](https://www.zdnet.com/article/developers-love-rust-programming-language-heres-why/).

I have been looking forward to really getting into Rust and now have a project in
mind.  The project is to create a DICOM RLE CODEC in Rust as part of my effort to
update the [CornerstoneJS](https://cornerstonejs.org/) codecs.  RLE is currently
[implemented in CornerstoneJS](https://github.com/cornerstonejs/cornerstoneWADOImageLoader/blob/master/src/shared/decoders/decodeRLE.js) 
in native JavaScript, but I would like to have WebAssembly versions of each codec 
to deliver maximum performance and be used by any language that supports WebAssembly.
While I did do a quick [C++ Implementation](https://github.com/chafey/modern-cpp-lib/blob/master/src/lib.cpp)
as part of [another experiment](https://chafey.github.io/wasm/dicom/compression/codec/2020/04/16/dicom-image-decoding-with-webassembly.html)
which could be built in WebAssembly via EMSCRIPTEN, it is not production ready and 
I would rather spend my time learning productizing a Rust version rather than a
C++ version.

I am going to be streaming my development of the Rust RLE Codec on my [twitch.tv channel](https://www.twitch.tv/chafey/),
feel free to join in and say hi on chat!  Although the DICOM RLE algorithm is simple, 
I am expecting this to be painfully slow due to me being new to the Rust language. 