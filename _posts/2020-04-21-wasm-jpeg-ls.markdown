---
layout: post
title:  "WASM JPEG-LS Codec Performance"
date:   2020-04-21 18:21:59 +0000
categories: wasm dicom compression codec
---

Some numbers on the updates to the [CornerstoneJS](https://cornerstonejs.org)
JPEG-LS codec:

* Hardware: Intel i7-4790k (2014 iMac 5k)
* Test Images: ftp://medical.nema.org/MEDICAL/Dicom/DataSets/WG04/compsamples_jpegls.tar

### Linux (Ubuntu 19.10)

#### JLSL/CT2_JLSL - 512x512 16 bit lossless CT Image

Decode 
* Native C++: 4.311 ms
* NodeJS 14.0.0: 6.903 ms
* Firefox 75.0: 7.2 ms
* Google Chrome 81.0: 7.6 ms 

Encode 
* Native C++ Encode: 5.507 ms
* NodeJS 14.0.0 Encode: 8.526 ms

#### JLSL/MG1_JLSL - 3064x4664 16 bit lossless MG Image

Decode 
* Native C++: 236.189 ms
* NodeJS 14.0.0: 455.83 ms
* Firefox 75.0: 453.6 ms
* Google Chrome 81.0: 501.597 ms 

Encode:
* Native C++: 363.734 ms
* NodeJS 14.0.0: 555.262 ms



### Download Size
* charls-js.js - 68,077 bytes
* charls-js.wasm - 195,952 bytes