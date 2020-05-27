---
layout: post
title:  "First Day with Rust"
date:   2020-05-27 
---

[Yesterday](https://chafey.github.io/2020/05/26/learning-rust-by-implementing-dicom-image-compression.html) 
I began my quest to master Rust by starting to implement
a DICOM RLE Image Compression Codec.  I managed to get a prototype decoder working,
you can find the source for it [here](https://github.com/chafey/dicomrle-rs).
Overall the day went about as expected - a slow day spent wrestling with the compiler
and googling to figure out how to do things in Rust.  I streamed the entire
day on my [twitch.tv channel](https://www.twitch.tv/chafey) and surprisingly
had 10-15 people watching me struggle through it.  Two people even offered some
help via chat which was nice!

I decided right away to try and build the CODEC using [Test Driven Development](https://en.wikipedia.org/wiki/Test-driven_development)
(TDD) so the very first thing I did was create a happy path unit test.  The happy path
unit test would decode an RLE image and compare it with the expected output.  I used
the [DICOM WG04 compression samples](ftp://medical.nema.org/MEDICAL/Dicom/DataSets/WG04)
for this since it has the same images encoded with
multiple transfer syntaxes including RLE and Implement Little Endian (uncompressed).
I used the [dicomParser](https://github.com/cornerstonejs/dicomParser) DICOM Dump with 
Data Dictionary [example](https://rawgit.com/cornerstonejs/dicomParser/master/examples/dumpWithDataDictionary/index.html) 
to extract the raw image frames for each image so I didn't have to pull in a DICOM Parser
dependency to the CODEC.  After I got the initial project up and going with the happy path unit test, I began
implementing the decoder.

A DICOM RLE image consists of a 64 byte header containing 16 32 bit unsigned integers
followed by one or more segments of 8 bit RLE encoded data.  For a color RGB image with
8 bits per channel, there would be three segments, one for each 8 bit color channel.
For 16 bit grayscale data, there would be two segments, one for the most significant 
byte and one for the least significant byte.

I started building the decoder by writing some pseudocode, something like:

``` rust
// Read 64 byte header
// Read number of segments
// Read starting offset of each segment
// Allocate decoded buffer
// Loop over segments and decode RLE stream
```

To read the 32 bit unsigned integers from the header requires pulling in the 
[byteorder crate](https://docs.rs/byteorder/1.3.4/byteorder/) which provides
convenience methods for converting from byte streams and Rust's intrinsic types.
I had hoped to avoid adding a dependency since the codec is so simple to begin with,
but I wanted to make progress so went ahead with it - for now.  I may very well remove
the dependency later since I only use a single function from it which should be easy
to replicate or inline.

After the header was parsed, I began writing the RLE codec itself.  The decoding
logic is very simple, something like this:

``` rust
// read byte from stream into n
// if n < 127
  // literal run case - copy the next n + 1bytes to output buffer
// if n > 128
  // replicated run case - read the next byte b and copy it -n + 1 times into output buffer
// if n == 128
  // continue
```
 
The n == 128 case is strange to me, I am not sure why it just wasn't folded into one of the
other two cases.  I started implementing the logic which ended up taking much longer
than I expected.  I was constantly struggling with the compiler about type issues,
variable naming conventions, unnecessary paranthesis on if conditions and handling all
possible error paths.  I also ran into several DICOM issues including a bug in the CT1
reference image (it overflows the output buffer!) and trying to comprehend an extremely
poorly written [part of the DICOM standard](http://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_G.2.html)
on how 16 bit pixels are RLE encoded.  The type issues and possible error paths was expected and 
welcome as these are the keys to Rust making code safe.  

The type issues mainly had to do with the fact that the 'n' value above was read as a
u8 (unsigned byte) but had to be interpreted as an i8 (signed byte) and usize 
(platform specific sized unsigned integer used for control loops).  Rust has built in
support for casting between types that can be done safely via the as operator:

``` rust
// cast an 8 bit unsigned integer to a 16 bit unsigned integer
let n:u8 = 0; 
let value = n as u16;
```

For types that don't safely convert, a conversion function must be used which will
return an error if the conversion fails:

``` rust
let num_segments:u32 = 0;
let max_segments:usize = usize::try_from(num_segments)?;
```

In this case, usize is a platform specific size and may not be able to hold a u32. 
This is a bit of a surprise since the only situation this could happen is 
when targeting processors with fewer than 32 bits.  I don't keep tabs on the embedded
space at all - but I sort of assumed that everything was 32+ bits by now.  There are
certainly older processors that are < 16 bits, but given that Rust is only 10 years
old, I had assumed that targeting processors with 32 bits and up would have been reasonable.

Error handling in Rust is one of the key features to making it a secure language.
Rust requires that every single control path be handled otherwise it won't compile.  It
accomplishes this by having functions return a Result structure which contains the value
on success or the error on failure and requiring that both paths be handled in the code.
The idea of a returning a Result struct that can contain a success or failure is not new. 
Take the C [atoi()](http://www.cplusplus.com/reference/cstdlib/atoi/) function which converts
a string to an integer or returns 0 if it fails to convert.  This interface is poorly 
designed as 0 is a value you might want to parse but is used to indicate an error.  When an
error occurs, you don't know why - is the number too large for an int?  Is it not a number?
You can use the Rust Result struct approach in C++ to distinguish between a success and a failure:

``` C++
// C++ Code
struct Result {
    int value;
    bool error;
}

Result atoi_improved(const char* str) {
    if(str[0] == '0') { // TODO: Add more robust zero string detection logic
        return Result { .value = 0, .error = false};
    }
    const int value = atoi(string); // NOTE: returns 0 on failure
    if(value == 0) { 
        return Result { .value = 0, .error = true}
    }
    return Result { .value = value, .error = false}
}
```

Rust has adopted this approach by convention in its standard libraries.  This coupled with the
validation done by the compiler ensures that your program never executes a code path that
you didn't anticipate (or actually, it FORCES you to consider all paths your program could 
take - including those you may not have been aware of!).  Using the above C++ code as an example:

```C++
// C++ Code
void add_num(const char* str) {
    static int accumulator = 0;
    accumulator += atoi_improved(str).value; 
    // BUG - the error condition is silently dropped possibly creating a latent
    // bug that only shows up with invalid string data!  Rust automatically
    // detects this situation forcing you to deal with it early in the design
    // stage
}
```

Overall I feel a storng sense of accomplishment after my first day and am looking forward to continuing
on today with learning more!