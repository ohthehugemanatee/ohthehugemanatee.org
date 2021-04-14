---
title: "How to Format Video for Fast Playback on the Web"
date: 2020-11-11T20:47:39+02:00
lastmod: 2021-04-14T20:47:39+02:00
categories : [ "video", "media" ]
---

It's a pain in the ass to get your video optimized for web. Not only do you have to work out codecs and their support across browsers, but even within codecs there are tricks to help it stream more easily. Here's the short version.

In terms of format, there are some really great options if you only care about compatibility with the most popular browsers (ahem - chrome). webm/VP8 is the way to go here, or webm/VP9 if you really only care about chrome. You'll get a very small filesize and high quality, with hardware playback on the latest devices. But if you want to include Safari and others, or if you care about non-flagsip devices or ones more than a couple of years old, it has to be MP4/h.264 . Your filesize won't be as small, but it will play with hardware acceleration on any device since about 2011.

One common opimization is to use `@media` queries to dfault to webm/VP9 and fallback to other formats based on the browser capabilities. Do that if you want, but for my use cases I prefer simplicity over bandwidth savings. 

I use [ffmpeg](https://ffmpeg.org/), because it does everything except my laundry (and I'm pretty sure that's because I don't know th right flags for "spin cycle"). To convert one video into a format that is universally compatible:

``` bash
ffmpeg  -i input.mp4 \
  -c:v libx264 # h264 encoder \
  -profile:v main # h264 options to make available. for >5 year old phones, use "baseline". \
  -preset veryslow # slowest option, best compression \
  -s hd720 # rarely need higher resolution than this \
  -b:v 1.5M # sets the video bitrate \
  -an # no audio track; this is for a background video \
  -movflags +faststart # allows playing while the file is downloading \
  output.mp4
```
  
The non-obvious options:

- `-profile:v main` sets which features of h264 to use. The standard evolved over time, and depending on just how new your playback devices are, there improvements available. Most of the time `main` is the right choice, but if you want to target older devices use `baseline`.
- `-b:v 1.5M` sets a 1.5 Megabit bitrate. You should experiment with this number to find the right tradeoff between quality and filesize. You can find out the bitrate of your original source with `ffinfo`, or with the output of this `ffmpeg` command.
- `-an` puts no audio in the finished video. I worked this out while making a background video for a site, so ths was appropriate. Side benefit that it simplifies this blog post. :) If you want audio, you could replace this with `-codec:a aac` is a very compatible option.
- `movflags +faststart` is critical if you want the video to play before being fully downloaded. Video files contain multiple streams of data (at least one stream of video and one of audio). Usually the metadata about each stream is at the _end_ of the filee, meaning that a player has to load the whole file before it knows how to play the data. Faststart breaks the file up into chunks and puts the metadata in with each chunk, so your player can start playing the video as soon as the first bit is loaded.

That's all you really need to make good looking video files, optimized for web, which play before they're fully downloaded, and work on every device. Have fun!
