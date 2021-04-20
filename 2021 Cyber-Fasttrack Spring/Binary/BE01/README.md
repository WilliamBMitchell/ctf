# BE01
## BRIEFING
Download the file and find a way to get the flag.

Contents: chicken.pdf

The provided `chicken.pdf` was a `pdf` that simply contained an image of a chicken

<img src="chicken.png" width="400">

Suspecting that the file included more than met the eye, I ran `binwalk` against it. Binwalk is a tool for searching a given binary image for embedded files and executable code. Specifically, it is designed for identifying files and code embedded inside of firmware images. Sure enough:

```console
root@osboxes:~/Downloads/be01# binwalk chicken.pdf 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PDF document, version: "1.4"
72            0x48            Zip archive data, at least v1.0 to extract, compressed size: 550522, uncompressed size: 550522, name: egg.zip
550609        0x866D1         End of Zip archive, footer length: 22
551319        0x86997         Zlib compressed data, default compression
6478358       0x62DA16        Zlib compressed data, default compression
6478601       0x62DB09        End of Zip archive, footer length: 22
```

The `pdf` had a zip file named `egg.zip` embedded within it. I extracted the embedded files with the command:

```console
root@osboxes:~/Downloads/be01# binwalk --dd='.*' chicken.pdf 
```
I then ran `file` against the extracted documents:

```console
root@osboxes:~/Downloads/be01/_chicken.pdf.extracted# file *
0:           PDF document, version 1.4
48:          Zip archive data, at least v1.0 to extract
62DA16:      ASCII text
62DA16.zlib: zlib compressed data
62DB09:      Zip archive data (empty)
866D1:       Zip archive data (empty)
86997:       data
86997.zlib:  zlib compressed data
```

This told me that the file named `48` was `egg.zip`. I renamed it to `egg.zip` for ease of identification. First I tried to unzip using `unzip` but got the following:

```console
root@osboxes:~/Downloads/be01/_chicken.pdf.extracted# unzip egg.zip 
Archive:  egg.zip
error [egg.zip]:  missing 72 bytes in zipfile
  (attempting to process anyway)
error: invalid zip file with overlapped components (possible zip bomb)
```

I then tried using `7zip` which was able to repair the zip archive:

```console
root@osboxes:~/Downloads/be01/_chicken.pdf.extracted# 7z x egg.zip 

7-Zip [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,2 CPUs Intel(R) Core(TM) i7-8750H CPU @ 2.20GHz (906EA),ASM,AES-NI)

Scanning the drive for archives:
1 file, 6478870 bytes (6328 KiB)

Extracting archive: egg.zip

WARNINGS:
There are data after the end of archive

--
Path = egg.zip
Type = zip
WARNINGS:
There are data after the end of archive
Offset = -72
Physical Size = 6478623
Tail Size = 319
Embedded Stub Size = 72

    
Would you like to replace the existing file:
  Path:     ./egg.zip
  Size:     6478870 bytes (6328 KiB)
  Modified: 2021-04-19 21:39:10
with the file from archive:
  Path:     egg.zip
  Size:     550522 bytes (538 KiB)
  Modified: 2020-10-26 11:45:31
? (Y)es / (N)o / (A)lways / (S)kip all / A(u)to rename all / (Q)uit? y

Everything is Ok

Archives with Warnings: 1

Warnings: 1
Size:       550522
Compressed: 6478870
```

I could then use `unzip` successfully:
```console
root@osboxes:~/Downloads/be01/_chicken.pdf.extracted# unzip egg.zip 
Archive:  egg.zip
 extracting: chicken.zip   
 ```
 It became apparent that there were a couple nested layers of zipping:
 
 ```console
 root@osboxes:~/Downloads/be01/_chicken.pdf.extracted/egg# unzip chicken.zip 
Archive:  chicken.zip
 extracting: egg.zip  
 ```
 ```console
 root@osboxes:~/Downloads/be01/_chicken.pdf.extracted/egg/chicken# unzip egg.zip 
Archive:  egg.zip
 extracting: chicken.zip  
 ```
 But before long I unzipped a `chicken.zip` to get `egg.pdf`:
 
 ```console
 root@osboxes:~/Downloads/be01/_chicken.pdf.extracted/egg/chicken/egg# unzip chicken.zip 
Archive:  chicken.zip
  inflating: egg.pdf  
  ```
  
  This `pdf` contained the flag:
  
  
  The flag is **wh1ch_came_f1rst?**.
