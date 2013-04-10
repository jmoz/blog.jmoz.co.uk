---
layout: post
title: "How to strip exif data using Imagick"
date: 2011-08-24 12:00
comments: true
categories: [php, imagick]
published: true
---
Today I spent a good amount of time trying to figure out how to **strip exif data** from an image using **Imagick**.  The first port of call was the (pathetic) [documentation](http://php.net/manual/en/book.imagick.php) at php.net.  I searched for 'exif' but found nothing.  Google isn't too helpful either.  There's a method `Imagick::setImageProperty()` but that didn't seem to save the data correctly.  There's also an exif extension but it only reads the exif data, not writes it.

I was tipped off by a colleague to the method [`Imagick::stripImage()`](http://php.net/manual/en/function.imagick-stripimage.php) which apparently did what I wanted.  The only mention of exif is in a rather helpful comment at the bottom of the page.

To remove the exif data you need to call `Imagick::stripImage()` on the populated Imagick object and then call `Imagick::writeImage()` to save your changes.  I've written a test script below that uses a sample image with exif data from the exif website.

{% img http://www.exif.org/samples/canon-ixus.jpg %}

Here's the script:

{% gist 1169074 %}

And here's the output:

```
moz on mz-904 in ~ 
 $ php imagick.php
Array
(   
    [exif:ApertureValue] => 262144/65536
    [exif:ColorSpace] => 1
    [exif:ComponentsConfiguration] => 1, 2, 3, 0
    [exif:CompressedBitsPerPixel] => 3/1
    [exif:Compression] => 6
    [exif:DateTime] => 2001:06:09 15:17:32
    [exif:DateTimeDigitized] => 2001:06:09 15:17:32
    [exif:DateTimeOriginal] => 2001:06:09 15:17:32
    [exif:ExifImageLength] => 480
    [exif:ExifImageWidth] => 640
    [exif:ExifOffset] => 184
    [exif:ExifVersion] => 48, 50, 49, 48
    [exif:ExposureBiasValue] => 0/3
    [exif:ExposureTime] => 1/350
    [exif:FileSource] => 3
    [exif:Flash] => 0
    [exif:FlashPixVersion] => 48, 49, 48, 48
    [exif:FNumber] => 40/10
    [exif:FocalLength] => 346/32
    [exif:FocalPlaneResolutionUnit] => 2
    [exif:FocalPlaneXResolution] => 640000/206
    [exif:FocalPlaneYResolution] => 480000/155
    [exif:InteroperabilityIndex] => R98
    [exif:InteroperabilityOffset] => 1088
    [exif:InteroperabilityVersion] => 48, 49, 48, 48
    [exif:JPEGInterchangeFormat] => 1524
    [exif:JPEGInterchangeFormatLength] => 5342
    [exif:Make] => Canon
    [exif:MakerNote] => 10, 0, 1, 0, 3, 0, 19, 0, 0, 0, 120, 3, 0, 0, 2, 0, 3, 0, 4, 0, 0, 0, 158, 3, 0, 0, 3, 0, 3, 0, 4, 0, 0, 0, 166, 3, 0, 0, 4, 0, 3, 0, 15, 0, 0, 0, 174, 3, 0, 0, 0, 0, 3, 0, 6, 0, 0, 0, 204, 3, 0, 0, 6, 0, 2, 0, 32, 0, 0, 0, 216, 3, 0, 0, 7, 0, 2, 0, 24, 0, 0, 0, 248, 3, 0, 0, 8, 0, 4, 0, 1, 0, 0, 0, 243, 105, 15, 0, 9, 0, 2, 0, 32, 0, 0, 0, 16, 4, 0, 0, 16, 0, 4, 0, 1, 0, 0, 0, 0, 0, 4, 6, 0, 0, 0, 0, 38, 0, 2, 0, 0, 0, 3, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 2, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 2, 0, 90, 1, 211, 0, 158, 0, 0, 0, 0, 0, 0, 0, 0, 0, 30, 0, 0, 0, 140, 0, 2, 1, 128, 0, 14, 1, 0, 0, 0, 0, 0, 0, 1, 0, 4, 0, 0, 0, 0, 0, 0, 0, 2, 48, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 73, 77, 71, 58, 74, 80, 69, 71, 32, 102, 105, 108, 101, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 70, 105, 114, 109, 119, 97, 114, 101, 32, 86, 101, 114, 115, 105, 111, 110, 32, 49, 46, 48, 0, 0, 0, 0, 84, 111, 109, 32, 82, 111, 119, 97, 110, 32, 97, 110, 100, 32, 83, 97, 114, 97, 104, 32, 67, 108, 105, 102, 116, 111, 110, 0, 0, 0, 0, 0
    [exif:MaxApertureValue] => 194698/65536
    [exif:MeteringMode] => 2
    [exif:Model] => Canon DIGITAL IXUS
    [exif:Orientation] => 1
    [exif:RelatedImageLength] => 640
    [exif:RelatedImageWidth] => 480
    [exif:ResolutionUnit] => 2
    [exif:SensingMethod] => 2
    [exif:ShutterSpeedValue] => 553859/65536
    [exif:SubjectDistance] => 3750/1000
    [exif:UserComment] => 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
    [exif:XResolution] => 180/1
    [exif:YCbCrPositioning] => 1
    [exif:YResolution] => 180/1
)
*** stripImage()
Array
(
)
```