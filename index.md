# oneliners.sh
Life is too short to fumble with bash commands

Share with your pals 🤝🔗: [oneliners.sh](http://oneliners.sh)

Merge in your own slick lines 📋✍️: [oneliners GitHub repo](https://github.com/mkrupczak3/oneliners.github.io)

---
# Disk/IO

Oneliners for disks and files

## find
start search from current directory (.)

Find where a file of a certain name (case insensitive) is located:
```bash
find . -type f -iname init.el
```

Find where a directory of a certain name (case insensitive) is located:
```bash
find . -type d -iname ".emacs.d"
```

Find files of multiple extentions:
```bash
find . -type f -iname "*.cpp" -o -iname "*.txt" 
```
_where `-o` acts as a logical OR operator_

## grep
requires the [`grep`](https://en.wikipedia.org/wiki/Grep) package

a powerful tool for searching plain-text

Find occurences of a phrase in all files in current directory
```bash
grep -iIn "kind: ConfigMap" *
```
_where * is a wildcard_

Find occurences of a phrase in all files in current directory and its subdirectories:
```bash
grep -iIrn "kind: ReplicaSet"
```

## ag
requires [`the_silver_searcher`](https://github.com/ggreer/the_silver_searcher) (_silversearcher-ag_) package

much faster than `grep`, useful for searching through large directory

Search occurences of a phrase in all files in current directory and its subdirectories:
```bash
ag "kind: ReplicaSet"
```

## permissions (`chown` and `chmod`)

change ownership of a file `file1.txt` to user `root`:
```bash
sudo chown root file1.txt
```

change group ownership of a file `file1.txt` to group `sudo`:
```bash
sudo chown :sudo file1.txt
```

do both of the above at once:
```bash
sudo chown root:sudo file1.txt
```

add execute permissions to a file:
```bash
sudo chmod ugo+x gettools.sh
```

remove execute permissions from a file:
```bash
sudo chmod ugo-x gettols.sh
```

remove `group` and `others` ability to write a file:
```bash
sudo chmod go-w file1.txt
```

allow `group` and `others` to read a file:
```bash
sudo chmod go+r file1.txt
```

## tar 

Like .zip files, but for Linux

### make a tar
Copy current folder into a `gzip`-compressed `tar` archive

```bash
tar zcvf test.tgz .
```

### extract a tar
Extract the contents of a tar into current directory

```bash
tar -xzf test.tgz
```

## zip

For all the times you can't just use `tar`

### make a zipped folder
requires the [`zip`](https://linux.die.net/man/1/zip) package

```bash
zip sampleZipFile.zip ExampleFile.txt ExampleFile1.txt
```

### extract a zipped folder
requires the [`unzip`](https://linux.die.net/man/1/unzip) package

```bash
unzip sampleZipFile.zip
```

## ln
Linux create a filesystem link


### hard link
creates another inode to the data on hard disk,

completely transparent to underlying software

```bash 
ln SOURCE TARGET
```

### soft link
creates a symbolic link to point to another file

some software doesn't play nice

```bash
ln -s SOURCE TARGET
```

## List Disks:

### df
`df` is a [`coreutil`](https://en.wikipedia.org/wiki/GNU_Core_Utilities) and is almost certainly on your system
```bash
df -h
```
### parted
[`parted`](https://en.wikipedia.org/wiki/GNU_Parted) is a useful tool for disks

May not be on your system

Has great GUI's available like [`GParted`](https://en.wikipedia.org/wiki/GParted)
```bash
parted -l
```

## mount
From a given disk device, [`mount`](https://linux.die.net/man/8/mount) it to filesystem:

_mount [OPTION/S] DEVICE_NAME DIRECTORY_NAME_

```bash
mount -t ext4 /dev/sdb1 /mnt/media
```

You can un-mount with [`umount`](https://linux.die.net/man/8/umount):
```bash
sudo umount /dev/sdc1
```
## ncdu
requres [`ncdu`](https://en.wikipedia.org/wiki/Ncdu) package

Look through a filesystem and see where space is being used:
```bash
sudo ncdu /
```

## DD:

Use DD to **make** a backup image of a physical disk device:
```bash
sudo dd bs=4M if=/dev/sdc of=/home/urname/pi_BKP.img status=progress
```

Use DD to **restore** a backup image of a drive onto a disk
```bash
sudo dd bs=4M if=/home/urname/pi_BKP.img of=/dev/sdc status=progress
```

Use DD to **duplicate** one drive onto another:
```bash
dd if=/dev/sda of=/dev/sdb bs=4M conv=notrunc,noerror status=progress
```

## S.M.A.R.T. Hard Drive health

[`smartctl`](https://linux.die.net/man/8/smartctl) is part of the [`smartmontools`](https://en.wikipedia.org/wiki/Smartmontools) package

useful for maintaining spinning-disk Hard drives

can detect some fatal errors before data loss

### Run a short test on a disk
```bash
smartctl -t short /dev/sda
```

### Human-readable health of a disk
```bash
smartctl -H /dev/sda 
```

### All info of a disk, including results of all tests
```bash
smartctl -a /dev/sda
```


## mkdosfs
Format a whole thumbdrive as FAT32, without a partition table

part of the [`dosfstools`](https://github.com/dosfstools/dosfstools) package

useful for many legacy hardware devices:
```bash
sudo umount /dev/sdc1 && sudo mkdosfs -F 32 -I /dev/sdc1
```

can be used for formatting FAT16 or FAT12 as well:
```bash
sudo umount /dev/sdc1 && sudo mkdosfs -F 16 -I /dev/sdc1
```

---
# Network and remote backup

Oneliners for network troubleshooting, downloading, and remote backup

## rsync
requires [`rsync`](https://en.wikipedia.org/wiki/Rsync) package

Remotely SYNC two directories

smarter than `scp` and easier than creating a tar:

```bash
rsync -rtvP --bwlimit=512 [source] [destination] 
```

```
# [source] [destination] e.g. myuser@remotemachine.com:/mixtapes /home/localuser/mixtapebkp
# Where
#
# -r is recursive
# -t retains the files' modification time
# -v to show what is going on
# -P shows the progress
# --bwlimit=512 limits the upload to 512Kb/sec, ~= 4.1 mbps
# 
# If interrupted, just run the command again and it will pickup where it left off
```

## ping

check if a server is reachable, AKA an echo request:
```bash
ping google.com
```

simple way to check if if internet connection is good, even if DNS isn't:
```bash
ping 1.1.1.1
```

## whois

check if a website is legit, and/or how long it has been registered:
```bash
whois example.com
```

## what's my IP
Figure out what your IP looks like to other computers on internet:
```bash
curl ifconfig.me
```

## nslookup

check if system [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) is working
```bash
nslookup pets.com
```

## speedtest
requires [`speedtest-cli`](https://pypi.org/project/speedtest-cli/) package

test your connection speed to the internet:
```bash
speedtest-cli
```


## nmap
requires [`nmap`](https://en.wikipedia.org/wiki/Nmap) package

Check if a specific port or range of ports (TCP and or UDP) is/are open using `nmap`:
```bash
nmap -p U:53,111,137,T:21-25,80,139,8080 1.1.1.1 example.com
```

## telnet
requires [`telnetd`](https://manpages.ubuntu.com/manpages/trusty/en/man8/telnetd.8.html) package

simple way to test if a TCP session can be initiated to a host at a port:
```bash
telnet 1.1.1.1 80
```

press ctrl+] then ctrl+d to exit (US keyboard) 

---
# Downloading
Oneliners for downloading things

## wget
requires [`wget`](https://en.wikipedia.org/wiki/Wget) package

Download a file from a website over http:
```bash
wget example.com/funnymeme.jpg
```

Download all URL's listed in a text file:
```bash
wget -i example.com/fire_mixtape.m3u
```

## aria2c
requires [`aria2`](https://aria2.github.io/) package

Download a file faster using multiple TCP connections:
```bash
aria2c -x 16 -s 16 example.com/funnyvideo.mp4
#          |    |
#          |    |
#          |    |
#          ---------> the number of connections here
```

## youtube-dl
requires [`youtube-dl`](https://github.com/ytdl-org/youtube-dl) package

works on many different video and audio sites, including YouTube!

may not work right unless you update it

Download a YouTube video:
```bash
youtube-dl https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

Download a YouTube video and convert to mp3:
```bash
youtube-dl --restrict-filenames --ignore-errors -x --audio-format mp3 https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

Download an album from bandcamp as mp3 files:
```bash
youtube-dl https://example.bandcamp.com/album/an-album -x --audio-format best
```

Download a video from any other website via m3u8 manifest

(Chrome) Right Click -> Inspect -> Network -> search "m3u8" -> refresh page -> right click -> Copy -> link address

download via m3u8:
```bash
youtube-dl https://ga.video.cdn.pbs.org/videos/frontline/279e5586-ffca-4870-a56f-f6452b06aafd/2000244604/hd-16x9-mezzanine-1080p/00004005-hls-16x9-1080p.m3u8
```

## eyeD3
requires `eyeD3` package

label all .mp3 files in current directory with Artist and Album name:
```bash
eyeD3 -a "Example Artist" -A "Example Album" *.mp3
```

set track number for an .mp3 file
```bash
eyeD3 -n 2 song.mp3
```

---
# FFMPEG
Oneliners for [`FFMPEG`](https://en.wikipedia.org/wiki/FFmpeg), the powerful video manipulation tool

All commands require [`ffmpeg`](https://en.wikipedia.org/wiki/FFmpeg) package

## convert a video (any format) for Twitter
max length 2min 20sec, any video after will be cut off

convert a video for posting on Twitter:
```bash
ffmpeg -i in.mov -strict -2 -max_muxing_queue_size 9999 -vcodec libx264 -vf "pad=ceil(iw/2)*2:ceil(ih/2)*2" -pix_fmt yuv420p -strict experimental -r 30 -t 2:20 -acodec aac -vb 1024k -minrate 1024k -maxrate 1024k -bufsize 1024k -ar 44100 -ac 2 out.mp4
```

## create video of music over a static image
e.g. for uploading a song to YouTube

use:
```bash
ffmpeg -loop 1 -r 1 -i pic.jpg -i audio.mp3 -c:a copy -shortest -c:v libx264 output.mp4
```
where `audio.mp3` is music, `pic.jpg` is album cover, `output.mp4` is output filename


## heavily compress a video
ideal for spotty networks, like w/ cell phones

set `-threads` to number of physical CPU cores on computer

compress a 16:9 video to very low resolution (768x432):
```bash
ffmpeg -i input.mp4 -strict -2 -max_muxing_queue_size 9999 -threads 4 -vf scale=768x432:flags=lanczos -c:v libx264 -preset slow -crf 21 output_compress_480p.mp4
```

## upscale a video
ideal for uploading low resolution video to youtube while perserving bitrate (quality)

upscale a video:
```bash
ffmpeg -i input.mp4 -strict -2 -max_muxing_queue_size 9999 -threads 4 -vf scale=1920x1080:flags=lanczos -c:v libx264 -preset slow -crf 21 output_compress_1080p.mp4
```
---
# ImageMagick
Oneliners for [`ImageMagick`](https://imagemagick.org/index.php), the powerful photo manipulation tool

credit [Drew Lustro](https://drewlustro.com/blog/batch-convert-raw-images-to-jpeg-with-imagemagick-in-parallel)

All commands require [`ImageMagick`](https://imagemagick.org/index.php) package and associated pre-requisites

Display info on an image:
```bash
magick identify portrait.jpeg 
```

set `-P` to number of physical CPU cores on computer

Convert raw images, commonly used in photography, web-safe JPEG:
```bash
find . -type f \( -iname \*.CR2 -o -iname \*.ARW -o -iname \*.3fr -o -iname \*.ari -o -iname \*.arw -o -iname \*.bay -o -iname \*.braw -o -iname \*.crw -o -iname \*.cr2 -o -iname \*.cr3 -o -iname \*.cap -o -iname \*.data -o -iname \*.dcs -o -iname \*.dcr -o -iname \*.dng -o -iname \*.drf -o -iname \*.eip -o -iname \*.erf -o -iname \*.fff -o -iname \*.gpr -o -iname \*.iiq -o -iname \*.k25 -o -iname \*.kdc -o -iname \*.mdc -o -iname \*.mef -o -iname \*.mos -o -iname \*.mrw -o -iname \*.nef -o -iname \*.nrw -o -iname \*.obm -o -iname \*.orf -o -iname \*.pef -o -iname \*.ptx -o -iname \*.pxn -o -iname \*.r3d -o -iname \*.raf -o -iname \*.raw -o -iname \*.rwl -o -iname \*.rw2 -o -iname \*.rwz -o -iname \*.sr2 -o -iname \*.srf -o -iname \*.srw -o -iname \*.tif -o -iname \*.x3f \) -print0 | xargs -0 -n 1 -P 4 -I {} convert -verbose -units PixelsPerInch {} -colorspace sRGB -resize 2560x2650 -set filename:new '%t-%wx%h' -density 72 -format JPG -quality 80 '%[filename:new].jpg'
```

**modify for:**

* potato CPU: `...xargs -0 -n 1 -P 1 -I {}...`
* NASA CPU: `...xargs -0 -n 1 -P 64 -I {}...`
* Max Twitter resolution:  `...-resize 4096x4096...` (preserves aspect ratio, scales by maximum of h and w) 
* higher quality JPEG: `...-format JPG -quality 97...`


set `-quality` to desired level of compression, from 1-100

Compress a JPEG to smaller file size:
```bash
convert image.jpg -quality 75 output_file.jpg
```

Convert an image to a PDF:
```bash
convert vaccine_card.jpg vaccine_card.pdf
```

Convert video to GIF animated image:
```bash
convert -quiet -delay 1 a.avi a.gif
```

# Ghostscript (PDF tool)

Merge PDF files:
```bash
gs -dBATCH -dNOPAUSE -q -sDEVICE=pdfwrite -sOutputFile=vaccine_combined.pdf vaccine_card.pdf booster.pdf
```

---
# Get Tools
## Install Matthew's favorite tools
Uses Matthew's [gettools.sh](https://github.com/mkrupczak3/gettools) BASH script

works on MacOS, Debian (e.g. Ubuntu), Fedora (e.g. RHEL), and Arch (e.g. Manjaro) based systems

careful, downloads lots of stuff

Don't do this for scripts you don't trust! (Also, why do you trust me?):
```bash
sudo wget -O - https://raw.github.com/mkrupczak3/gettools/master/gettools.sh | sudo bash
```
