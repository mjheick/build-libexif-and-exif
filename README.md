# exif
I got this little project to take my huge EMPIRE OF DIGITAL IMAGES and make sure everything has the necessary minimums.

Also called "Basic building and usage of exif from libexif"

# Repositories
- [libexif](https://github.com/libexif/libexif)
- [exif](https://github.com/libexif/exif)

# Building
Gave this a try across a couple platforms from minimal installs, just 'cuz.

## Ubuntu 20.04 Desktop
Get your dependencies:
```
apt install gcc make libpopt-dev
```
## Rocky 9.3
Get your dependencies:
```
yum install tar wget bzip2 gcc popt-devel
```

## Build Instructions
Download and build [libexif 0.6.25](https://github.com/libexif/libexif/releases/tag/v0.6.25).
```
wget 'https://github.com/libexif/libexif/releases/download/v0.6.25/libexif-0.6.25.tar.bz2'
bzip2 -d libexif-0.6.25.tar.bz2
tar -xf libexif-0.6.25.tar
cd libexif-0.6.25/
./configure
make
sudo make install
```

Download and build [exif 0.6.22](https://github.com/libexif/exif/releases/tag/exif-0_6_22-release).
```
wget 'https://github.com/libexif/exif/releases/download/exif-0_6_22-release/exif-0.6.22.tar.gz'
tar -xzf exif-0.6.22.tar.gz
cd exif-0.6.22
./configure
make
```
Final file is in repository/exif/exif

# Smallest JPEG

At 141 bytes, and from [reddit](https://www.reddit.com/r/programming/comments/4tqtva/1_pixel_in_various_image_formats_png_gif_webp_bpg/):

```
$ hexdump -Cv ./smallest-jpeg.jpg
00000000  ff d8 ff db 00 43 00 ff  ff ff ff ff ff ff ff ff  |.....C..........|
00000010  ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff  |................|
00000020  ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff  |................|
00000030  ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff  |................|
00000040  ff ff ff ff ff ff ff ff  c0 00 0b 08 00 01 00 01  |................|
00000050  01 01 11 00 ff c4 00 14  00 01 00 00 00 00 00 00  |................|
00000060  00 00 00 00 00 00 00 00  00 03 ff c4 00 14 10 01  |................|
00000070  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000080  ff da 00 08 01 01 00 00  3f 00 37 ff d9           |........?.7..|
0000008d
```

And since it doesn't have any metadata we manually have it be created:

```
$ exif -c smallest-jpeg.jpg
Wrote file 'smallest-jpeg.jpg.modified.jpeg'.
```

# Date/Time, GPS, ImageDescription, UserComment

## Date/Time
```
$ exif --ifd=0 --tag=0x0132 image.jpg # 'Date and Time'
$ exif --ifd=0 --tag=0x0132 --set-value="2010:10:10 10:10:10" image.jpg

$ exif --tag=0x9003 image.jpg # 'Date and Time (Original)'
$ exif --ifd=EXIF --tag=0x9003 --set-value="2010:10:10 10:10:10" image.jpg

$ exif --tag=0x9004 image.jpg # 'Date and Time (Digitized)'
$ exif --ifd=EXIF --tag=0x9004 --set-value="2010:10:10 10:10:10" image.jpg
```

## GPS
```
$ exif --ifd=GPS --tag=0x0001 image.jpg # 'North or South Latitude'
$ exif --ifd=GPS --tag=0x0001 --set-value="S" image.jpg

$ exif --ifd=GPS --tag=0x0002 image.jpg # 'Latitude'
$ exif --ifd=GPS --tag=0x0002 --set-value="10 1 10 2 10 4" image.jpg

$ exif --ifd=GPS --tag=0x0003 image.jpg # 'East or West Longitude'
$ exif --ifd=GPS --tag=0x0003 --set-value="E" image.jpg

$ exif --ifd=GPS --tag=0x0004 image.jpg # 'Longitude'
$ exif --ifd=GPS --tag=0x0004 --set-value="10 1 10 2 10 4"  image.jpg
```
Note: GPS rational-type (fractional) data is in the format:
```
A B C D E F
```
where A/B C/D E/F

## ImageDescription
```
$ exif --ifd=0 --tag=0x010e image.jpg # 'User Comment'
$ exif --ifd=0 --tag=0x010e --set-value="Awesome" image.jpg
```

## UserComment
```
$ exif --ifd=EXIF --tag=0x9286 image.jpg # 'User Comment'
$ exif --ifd=EXIF --tag=0x9286 --set-value="Awesome" image.jpg
```

# Final hexdump

When incorporating all the above modifications and substituting json data for ImageDescription and UserComment we get:

```
$ hexdump -Cv ./smallest-jpeg.jpeg
00000000  ff d8 ff e1 01 d0 45 78  69 66 00 00 4d 4d 00 2a  |......Exif..MM.*|
00000010  00 00 00 08 00 08 01 0e  00 02 00 00 00 17 00 00  |................|
00000020  00 92 01 1a 00 05 00 00  00 01 00 00 00 6e 01 1b  |.............n..|
00000030  00 05 00 00 00 01 00 00  00 76 01 28 00 03 00 00  |.........v.(....|
00000040  00 01 00 02 00 00 01 32  00 02 00 00 00 14 00 00  |.......2........|
00000050  00 7e 02 13 00 03 00 00  00 01 00 01 00 00 87 69  |.~.............i|
00000060  00 04 00 00 00 01 00 00  00 aa 88 25 00 04 00 00  |...........%....|
00000070  00 01 00 00 01 62 00 00  00 00 00 00 00 48 00 00  |.....b.......H..|
00000080  00 01 00 00 00 48 00 00  00 01 32 30 31 30 3a 31  |.....H....2010:1|
00000090  30 3a 31 30 20 31 30 3a  31 30 3a 31 30 00 7b 22  |0:10 10:10:10.{"|
000000a0  69 6d 61 67 65 22 3a 22  69 73 20 41 77 65 73 6f  |image":"is Aweso|
000000b0  6d 65 22 7d 00 00 00 09  90 00 00 07 00 00 00 04  |me"}............|
000000c0  30 32 31 30 90 03 00 02  00 00 00 14 00 00 01 1c  |0210............|
000000d0  90 04 00 02 00 00 00 14  00 00 01 30 91 01 00 07  |...........0....|
000000e0  00 00 00 04 01 02 03 00  92 86 00 07 00 00 00 1d  |................|
000000f0  00 00 01 44 a0 00 00 07  00 00 00 04 30 31 30 30  |...D........0100|
00000100  a0 01 00 03 00 00 00 01  ff ff 00 00 a0 02 00 04  |................|
00000110  00 00 00 01 00 00 00 00  a0 03 00 04 00 00 00 01  |................|
00000120  00 00 00 00 00 00 00 00  32 30 31 30 3a 31 30 3a  |........2010:10:|
00000130  31 30 20 31 30 3a 31 30  3a 31 30 00 32 30 31 30  |10 10:10:10.2010|
00000140  3a 31 30 3a 31 30 20 31  30 3a 31 30 3a 31 30 00  |:10:10 10:10:10.|
00000150  41 53 43 49 49 00 00 00  7b 22 75 73 65 72 22 3a  |ASCII...{"user":|
00000160  22 69 73 20 41 77 65 73  6f 6d 65 22 7d 00 00 04  |"is Awesome"}...|
00000170  00 01 00 02 00 00 00 02  53 00 00 00 00 02 00 05  |........S.......|
00000180  00 00 00 03 00 00 01 98  00 03 00 02 00 00 00 02  |................|
00000190  45 00 00 00 00 04 00 05  00 00 00 03 00 00 01 b0  |E...............|
000001a0  00 00 00 00 00 00 00 0a  00 00 00 01 00 00 00 0a  |................|
000001b0  00 00 00 02 00 00 00 0a  00 00 00 04 00 00 00 0a  |................|
000001c0  00 00 00 01 00 00 00 0a  00 00 00 02 00 00 00 0a  |................|
000001d0  00 00 00 04 ff db 00 43  00 ff ff ff ff ff ff ff  |.......C........|
000001e0  ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff  |................|
000001f0  ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff  |................|
00000200  ff ff ff ff ff ff ff ff  ff ff ff ff ff ff ff ff  |................|
00000210  ff ff ff ff ff ff ff ff  ff ff c0 00 0b 08 00 01  |................|
00000220  00 01 01 01 11 00 ff c4  00 14 00 01 00 00 00 00  |................|
00000230  00 00 00 00 00 00 00 00  00 00 00 03 ff c4 00 14  |................|
00000240  10 01 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000250  00 00 ff da 00 08 01 01  00 00 3f 00 37 ff d9     |..........?.7..|
0000025f
```
