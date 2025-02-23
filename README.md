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

# Date/Time, GPS, UserComment

## Date/Time
```
./exif --ifd=0 --tag=0x0132 image.jpg # 'Date and Time'
./exif --ifd=0 --tag=0x0132 --set-value="2010:10:10 10:10:10" image.jpg

./exif --tag=0x9003 image.jpg # 'Date and Time (Original)'
./exif --ifd=EXIF --tag=0x9003 --set-value="2010:10:10 10:10:10" image.jpg

./exif --tag=0x9004 image.jpg # 'Date and Time (Digitized)'
./exif --ifd=EXIF --tag=0x9004 --set-value="2010:10:10 10:10:10" image.jpg
```

## GPS
```
./exif --ifd=GPS --tag=0x0001 image.jpg # 'North or South Latitude'
./exif --ifd=GPS --tag=0x0001 --set-value="S" image.jpg

./exif --ifd=GPS --tag=0x0002 image.jpg # 'Latitude'
./exif --ifd=GPS --tag=0x0002 --set-value="10 1 10 2 10 4" image.jpg

./exif --ifd=GPS --tag=0x0003 image.jpg # 'East or West Longitude'
./exif --ifd=GPS --tag=0x0003 --set-value="E" image.jpg

./exif --ifd=GPS --tag=0x0004 image.jpg # 'Longitude'
./exif --ifd=GPS --tag=0x0004 --set-value="10 1 10 2 10 4"  image.jpg
```
Note: GPS rational-type (fractional) data is in the format:
```
A B C D E F
```
where A/B C/D E/F

## UserComment
```
./exif --ifd=EXIF --tag=0x9286 image.jpg # 'User Comment'
./exif --ifd=EXIF --tag=0x9286 --set-value="Awesome" image.jpg
```
