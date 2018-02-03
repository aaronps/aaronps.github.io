# ffmpeg

## Linux

May need to build:

* `yum install yasm`
* go `nasm.us` copy the yum repos info you local installation
* `yum install nasm`
* build libx264
```shell
# git clone http://git.videolan.org/git/x264.git
```
* (x264) `./configure --prefix=$HOME/libs --enable-shared --enable-static`
* (x264) `make && make install`
* (ffmpeg) `PKG_CONFIG_PATH="$HOME/libs/lib/pkgconfig" ./configure --prefix=$HOME/libs --enable-shared --disable-doc --enable-gpl --enable-libx264`
* (ffmpeg) `make && make install`

## Windows

Download files from https://ffmpeg.zeranoe.com/builds for your architecture **Dev** and **Shared**.
It is a good idea to also get the source code.

## Basic program

link

* avformat
* avcodec
* avutil
* swscale

dll

* avcodec-57.dll
* avformat-57.dll
* avutil-55.dll
* swresample-2.dll
* swscale-4.dll

_CHECK THE FIFO MUXER, it auto retries_


## Command line

To list direct show devices:

    ffplay -f dshow -list_devices true -i dummy

To play one of the devices:

    ffplay -f dshow -i video="Microsoft Camera Front"

To see some options:

    ffplay -list_options true -f dshow -i video="Microsoft Camera Front"

To see it with more changes:

    ffplay -f dshow -framerate 25 -video_size 1280x720 -i video="Microsoft Camera Front"

Streaming 1

    ffmpeg -f dshow -framerate 15 -video_size 1280x720 -i video="Microsoft Camera Front" -vcodec libx264 -an -vf "format=yuv420p" -profile main -tune zerolatency -f flv rtmp://127.0.0.1/live/cam

    ffmpeg -f dshow -framerate 15 -video_size 640x360 -i video="Microsoft Camera Front" -c:v libx264 -an -vf "format=yuv420p" -profile main -tune zerolatency -me_range 16 -me_method dia -coder ac -threads 4 -rtmp_live live -f flv rtmp://127.0.0.1/live/cam

Stream test pattern:

    ffmpeg -re -f lavfi -i "testsrc2=size=640x360:rate=25" -c:v libx264 -an  -vf "format=yuv420p" -profile main -tune zerolatency -r 25 -f flv rtmp://127.0.0.1/live/test

Reduce start time:

	-analyzeduration 1M
	or 
	-probesize 100000
	
	-fflags +nobuffer

To play the result of some operation directly

    ffmpeg -i … -vcodec rawvideo -pix_fmt yuv420p -f sdl "Maybe window name"


To convert for fast play:

    ffmpeg -i $file -c:v libx264 -g 12 -crf 18 -an -tune zerolatency -f flv file.flv

To capture a window on windows:

    ffmpeg -f gdigrab -framerate 30 -I title="Window Name" <output data>

	ffmpeg -f gdigrab -framerate 25 -offset_x XXX -offset_y YYY -video_size WidthxHeight -i desktop <output data>

Note that for example, chrome uses `<html title> - Google Chrome`

    ffmpeg -f gdigrab -framerate 30 -I title="" -c:v libx264 -an -vf "format=yuv420p" -tune zerolatency output.mp4

Note that zerolantency is not needed if only recording normal video not intended for streaming

To limit recording or video operation by time: `-t <number of seconds>`


## To stich videos with filters (very slow)
```cmd
@echo off

set RTMPBASE=rtmp://192.168.2.173:1554/live

set F1=color=c=black:size=1920x720 [base]
set F2=[5:v] setpts=PTS-STARTPTS, scale=width=640:height=-1 [cam0]
set F3=[0:v] setpts=PTS-STARTPTS, scale=width=640:height=-1 [cam1]
set F4=[1:v] setpts=PTS-STARTPTS, scale=width=640:height=-1 [cam2]
set F5=[2:v] setpts=PTS-STARTPTS, scale=width=640:height=-1 [cam3]
set F6=[3:v] setpts=PTS-STARTPTS, scale=width=640:height=-1 [cam4]
set F7=[4:v] setpts=PTS-STARTPTS, scale=width=640:height=-1 [cam5]
set F8=[base][cam0] overlay=shortest=1:x=0:y=0 [tmp1]
set F9=[tmp1][cam1] overlay=shortest=1:x=640:y=0 [tmp2]
set F10=[tmp2][cam2] overlay=shortest=1:x=1280:y=0 [tmp3]
set F11=[tmp3][cam3] overlay=shortest=1:x=0:y=360 [tmp4]
set F12=[tmp4][cam4] overlay=shortest=1:x=640:y=360 [tmp5]
set F13=[tmp5][cam5] overlay=shortest=1:x=1280:y=360

ffmpeg -probesize 100000 ^
	-i %RTMPBASE%/cam_1_0 ^
	-i %RTMPBASE%/cam_1_1 ^
	-i %RTMPBASE%/cam_1_2 ^
	-i %RTMPBASE%/cam_1_3 ^
	-i %RTMPBASE%/cam_1_4 ^
	-i %RTMPBASE%/cam_1_5 ^
	-filter_complex "%F1%; %F2%; %F3%; %F4%; %F5%; %F6%; %F7%; %F8%; %F9%; %F10%; %F11%; %F12%; %F13%"^
	-an ^
	-vcodec rawvideo -pix_fmt yuv420p -f sdl "SDL_OUTPUT"
```


