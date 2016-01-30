---
layout: post
title: avi, wmv, mpeg or anything to flv video
created: 1273746202
comments: true
---
FFmpeg is the Swiss army knife of video and audio conversion tools. It is a rock solid open source product. It is a command line tool that on first glance seems like something only geek heads can use. There are some free FFmpeg GUI applications based on FFmpeg, but theese are calls the command line tool behind the GUI interface.

The rest is the best to use the command line tool to convert something to flv format. FFmpeg is available on linux, bsd's, windows (everywhere where gcc turns around).

Let's convert wmv to flv:
<code>
ffmpeg -i samplevideo.wmv samplevideo.flv
</code>
The video now can be used in flash video players, for example. But what about quality? The following command line parameter will set the sound sample rate to 22050 and video resolution to 320x200.
<code>
ffmpeg -i samplevideo.mpg -ar 22050 -s 320x200 samplevideo.flv
</code>
