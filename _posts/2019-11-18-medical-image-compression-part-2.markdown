---
layout: post
title:  "Medical Image Compression Part 2 - Deflate"
date:   2019-11-18 01:00:00 +0000
---

In the previous article, we explored entropy encoders and how they work. We learned that there are just a few entropy encoding algorithms and data transformations are the main difference between different compression algorithms. This post will explore the deflate compression algorithm which utilizes an entropy encoder as well as two data transformations – run length encoding (RLE) and pattern matching. Deflate is perhaps the most popular compression algorithm in the world and is used by ZIP, gzip and even inside of the PNG image compression algorithm. RLE and pattern matching are foundational data transformations and are often used in other compression algorithms.

Run length encoding (RLE) replaces a sequence of identical symbols (a run) with just a few bytes (the symbol in the run and length of the run). In medical imaging, we can see long runs of the same value – specifically in Ultrasound Single Frame images. 


![](rle-ultrasound.png)

**Figure 1: Example ultrasound single frame image which has many runs of black pixels**


![](rle-data.png)

**Figure 2: RLE Data Encoding a run of 12 symbols of value 0 with 2 symbols**

RLE can provide improved compression ratios for some image types (like US) but not all – in many cases it will actually result in something larger than the original uncompressed data stream! RLE is a poor choice when used on its own but is often used as one of the transformations in compression algorithms. If a compression algorithm can somehow transform the input data such there are more runs of a given value that exist in the input data stream, RLE can be used to increasing the compression ratio.  

Pattern matching is another basic data transformation technique that scans the data looking for patterns of data. For example, scanning English text data will reveal that some patterns of letters (words) are common (e.g. the, of, and, to, in, is, etc). Once a pattern is detected, the pattern could be captured as a unique symbol and mapped to an entry in the entropy encoder dictionary. Another option is to encode the first pattern match as you normally would but replace future pattern matches with a back pointer pointing to the first pattern match. The back pattern could simply be the offset in the bit stream where the first pattern is located which can be represented in just a few bytes. 

Pattern matching works best when the patterns are very frequent (like English) and long. Unfortunately, patterns of pixels in medical images are infrequent so little benefit is realized with pattern matching - that is unless a data transformation can somehow create repeating patterns.

One of the most popular compression algorithms used today is deflate (also known as gzip or ZIP) which combines Huffman encoding, run length encoding and pattern matching to provide lossless compression. Deflate was original created as part of the PKZIP software in 1989 and quickly gained ubiquity throughout the computing world due to being very fast and provided excellent compression ratios for text and good compression ratios for binary data (like executables).  Most data sent over the web today is compressed using deflate (via gzip) and the popular PNG image format also uses deflate.  Deflate is still known for being a good general-purpose compression algorithm as it strikes a good balance between compression ratio, encode speed and decode speed on most data types. There are other algorithms that are faster (with lower compression ratio) and slower (with higher compression ratios).

Given the popularity of deflate, you may wonder how well it does on a typical CT image. DICOM actually has a transfer syntax for deflate (1.2.840.10008.1.2.1.99) but it was added specifically to support text-based SOP Classes such as SR and GSPS. The deflate transfer syntax is applied to all attributes in the SOP Instances which is different than the other transfer syntaxes which are applied to the pixel data only. Here is the same DICOM SOP Instance of a CT image compressed with different algorithms:

| Compression   | Size (bytes) | Ratio |
| ------------- | ------------ | ----- |
| Uncompressed  | 526,982      | 1.0   |
| Deflate       | 295,271      | 1.78  |
| JPEG-LS       | 197,454      | 2.66  |
| JPEG-2000     | 192,144      | 2.74  |


**Figure 3: Compression ratios of different compression algorithms on the same image**

Deflate certainly provides some compression but performs significantly worse than JPEG-LS and JPEG-2000 and only moderately better than the bit packing strategy we explored in the first article. Why is JPEG-LS and JPEG-2000 so much better than deflate? The reason is that JPEG image compression utilizes data transformations which produce data streams which compress better than the original pixel data. There are hundreds of different data transformation algorithms each of which have different tradeoffs. An image compression algorithm may use several transformation algorithms chained together in a pipeline to achieve a specific goal. These goals vary but are usually focused on one or more of the following: compression ratio, speed to encode, speed to decode, lossy vs lossless and progressive decoding. We will dig into data transformations that work well in images in a future post. 