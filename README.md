# exif
Basic building and usage of exif from libexif

# Repositories
[libexif](https://github.com/libexif/libexif)
[exif](https://github.com/libexif/exif)

# Building
Get your dependencies ([Rocky 9.3](https://rockylinux.org) instructions):
```
yum install tar wget bzip2 gcc popt-devel
```

Download and build [libexif 0.6.24](https://github.com/libexif/libexif/releases/tag/v0.6.24).
```
wget 'https://github.com/libexif/libexif/releases/download/v0.6.24/libexif-0.6.24.tar.bz2'
bzip2 -d libexif-0.6.24.tar.bz2
tar -xf libexif-0.6.24.tar
cd libexif-0.6.24
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

# Time, GPS, Comment

## Time
```
```

## GPD
```
```

## UserComment
```
./exif --tag=0x9286 image.jpg
./exif --ifd=EXIF --tag=0x9286 --set-value="Awesome" image.jpg
```
