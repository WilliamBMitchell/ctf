# FM01
## BRIEFING
Download the file and find a way to get the flag.

Contents: fm01.jpg

## Solution

We are presented with the image:

![fm01](fm01.jpg)

The first thing I did was take a look at the image metadata by using exiftool. Exchangeable Image File Format (EXIF) data is a standard that defines specific information related to an image or other media captured by a digital camera. It is capable of storing such information as camera exposure, date/time the image was captured, and even GPS location. Sometimes information can be hidden with EXIF data. Exiftool provides a command line application for reading metadata. I used it on this image and found the flag:

`exiftool fm01.jpg`

```console
ExifTool Version Number         : 12.13
File Name                       : fm01.jpg
Directory                       : .
File Size                       : 3.1 MiB
File Modification Date/Time     : 2021:03:12 09:03:30-05:00
File Access Date/Time           : 2021:04:17 13:27:12-04:00
File Inode Change Date/Time     : 2021:04:05 14:38:20-04:00
File Permissions                : rw-rw-r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : inches
Current IPTC Digest             : e8f15cf32fc118a1a27b67adc564d5ba
Application Record Version      : 0
IPTC Digest                     : e8f15cf32fc118a1a27b67adc564d5ba
X Resolution                    : 72
Displayed Units X               : inches
Y Resolution                    : 72
Displayed Units Y               : inches
Print Style                     : Centered
Print Position                  : 0 0
Print Scale                     : 1
Global Angle                    : 30
Global Altitude                 : 30
URL List                        : 
Slices Group Name               : painting-3135875_1920
Num Slices                      : 1
Pixel Aspect Ratio              : 1
Photoshop Thumbnail             : (Binary data 10277 bytes, use -b option to extract)
Has Real Merged Data            : Yes
Writer Name                     : Adobe Photoshop
Reader Name                     : Adobe Photoshop 2021
Photoshop Quality               : 12
Photoshop Format                : Standard
Progressive Scans               : 3 Scans
XMP Toolkit                     : Adobe XMP Core 6.0-c006 79.164648, 2021/01/12-15:52:29
Document ID                     : adobe:docid:photoshop:c62cc249-d917-484b-aaf9-af0b3df2ace8
Instance ID                     : xmp.iid:434d9271-6b85-624d-8de6-7dc17c1a43a4
Original Document ID            : E25BCF5D355B2F2CE5EB55EC6B67C7AF
Format                          : image/jpeg
Color Mode                      : RGB
ICC Profile Name                : 
Create Date                     : 2021:03:12 11:34:40Z
Modify Date                     : 2021:03:12 13:28:07Z
Metadata Date                   : 2021:03:12 13:28:07Z
History Action                  : saved, saved
History Instance ID             : xmp.iid:57de9171-9b29-714c-b64f-0791ca77a2bf, xmp.iid:434d9271-6b85-624d-8de6-7dc17c1a43a4
History When                    : 2021:03:12 13:28:07Z, 2021:03:12 13:28:07Z
History Software Agent          : Adobe Photoshop 22.2 (Windows), Adobe Photoshop 22.2 (Windows)
History Changed                 : /, /
Text Layer Name                 : flag: tr4il3r_p4rk
Text Layer Text                 : flag: tr4il3r_p4rk
Document Ancestors              : E25BCF5D355B2F2CE5EB55EC6B67C7AF
Image Width                     : 1920
Image Height                    : 1295
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 1920x1295
Megapixels                      : 2.5
```

The flag is: **tr4il3r_p4rk**.


