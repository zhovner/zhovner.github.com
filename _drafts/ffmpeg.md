---
layout: post
category: tools
tags:
excerpt:
title: ffmpeg
---

### convert any video to mp4 (h264, AAC) and adopt it for playing in browser  

`ffmpeg -i input.avi -movflags faststart output.mp4`

Option `-movflags faststart` moovs atom at the front of the file to allow video play in streaming mode  

### convert any video to mp4 and compreess it 

`ffmpeg -i input.avi -movflags faststart -crf 30 output.mp4`

`-crf 23` — default compression  
`-crf 25` — middle compression  
`-crf 35` — high compression  

### just remove audio and keep video untoched

`ffmpeg -i input.avi -vcodec copy -an output.mp4`
