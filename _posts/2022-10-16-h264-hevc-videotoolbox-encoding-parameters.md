---
layout: post
title: H264/HEVC VideoToolbox encoding parameters
---

This note explains the basic parameters for FFmpeg's `h264_videotoolbox` and `hevc_videotoolbox` codec, exploring its hardware encoding capabilities on Apple silicon.  

Note that this note is drafted in October 2022 against an M1 MacBook. If any of the behaviours mentioned in this note has changed in future version of FFmpeg, VideoToolbox or on newer generation of Apple M series chips, feel free to open a discussion [here](https://github.com/Akatmks/akatmks.github.io/issues). Also, if you find any problems in this note, please let Akatsumekusa know by opening an issue [here](https://github.com/Akatmks/akatmks.github.io/issues). It will be greatly appreciated. Thank you very much.  

## **Ratecontrol**

VideoToolbox in FFmpeg supports both VBR (Variable Bitrate) mode and CQ (Constant Quality) mode.  

### b:v
*Accepts int64 value from 0 to I64_Max. Default: 200k.*  

Set the target bitrate for encoding in VBR mode.  
Akatsumekusa suggests a good starting point for 1080p23.976 video to be around 2000k to 8000k depending on the complexity of the video.  

### q:v
*Accepts float value from 0 to 100. Default: Not Set.*  

Set the target quality for encoding in CQ mode from 0 to 100. Higher numbers produce better quality.  
Setting this option will enable CQ mode and disable `-b:v` setting.  

Constant Quality in VideoToolbox is not optimal. It performs poorly on sections of video with blur effect or detailed texture. The quality of such sections drops far below the target quality, and there is currently no way to encourage the encoder to use more bitrate on these sections.  

Also, despite `-q:v` accept quality values from 0 to 100, it doesn't support that many levels of quality. As an example, values between 53.0 to 54.9 will give the exact same result, so will values between 55.0 to 56.9 and 57.0 to 59.9.  

Apple describes quality value of 25 as low, 50 as normal, 75 as high and 100 as lossless. Akatsumekusa suggests a good starting point to be around 55 to 60.  

### alpha_quality
*\[hevc_videotoolbox only\] Accept double value from 0 to 1. Default: 0 or Not Set.*  

Set the target quality for alpha channel.  
Apple describes 0.0 as the lowest quality and 1.0 as nearly lossless quality.  

### maxrate
*Accepts int64 value from 0 to I64_Max. Default: 0 or Not Set.*  

Set the hard limit on bitrate in VBR and CQ mode.  

VideoToolbox [allows](https://developer.apple.com/documentation/videotoolbox/kvtcompressionpropertykey_dataratelimits) 0, 1 or 2 hard limits, each of which is described by a data size in bytes and a duration in seconds, requiring the total size of compressed data for any contiguous segment of set duration not exceeding the data size. Akatsumekusa didn't find parameters in FFmpeg that fully supports this function. If anyone does have an idea, please let Akatsumekusa know by opening a discussion [here](https://github.com/Akatmks/akatmks.github.io/issues).  

## **Frame configuration**

### g
*Accepts int value from INT_MIN to INT_MAX. Default: 12.*  

Set the interval between I-frames.  

Note that VideoToolbox does not support adaptive I-frame decision. It does [allow](https://developer.apple.com/documentation/videotoolbox/kvtencodeframeoptionkey_forcekeyframe) the user to decide frame by frame if an I-frame should be inserted. However, Akatsumekusa didn't find parameters in FFmpeg related to this function. If anyone does have an idea, please let Akatsumekusa know by opening a discussion [here](https://github.com/Akatmks/akatmks.github.io/issues).  

### bf
*Accepts 0 or 1. Default: 0.*  

Set this to 1 to allow 1 consecutive B-frame. Set this to 0 to disable B-frames.  

A FFmpeg commit [suggests](https://git.ffmpeg.org/gitweb/ffmpeg.git/commit/efece4442f3f583f7d04f98ef5168dfd08eaca5c) that `-b-pyramid` is also supported on hevc_videotoolbox. However, Akatsumekusa failed to enable this options on a Homebrew installation of FFmpeg 5.1.2.  

### coder
*\[h264_videotoolbox only\] Accepts cavlc or vlc as well as cabac or bac. Default: Not Set or Auto.*  

## **Output**

### profile:v
*\[h264_videotoolbox\] Supports baseline, main and high. Default: Not Set or Auto.*  
*\[hevc_videotoolbox\] Supports main and main10. Default: Not Set or Auto.*  

### level
*\[h264_videotoolbox only\] Supports 1.3, 3.0, 3.1, 3.2, 4.0, 4.1, 4.2, 5.0, 5.1 and 5.2. Default: Not Set or Auto.*  

### pix_fmt
*\[h264_videotoolbox\] Supports yuv420p, nv12 and videotoolbox_vld. Default: yuv420p.*  
*\[hevc_videotoolbox\] Supports yuv420p, nv12, p010le, bgra and videotoolbox_vld. Default: yuv420p.*  

### color_primaries
*Supports bt709, bt470m, bt470bg, smpte170m, smpte240m, film, bt2020, smpte428, smpte428_1, smpte431, smpte432 and jedec-p22. Default: Auto.*  

### color_trc
*Supports bt709, gamma22, gamma28, smpte170m, smpte240m, linear, log, log100, log_sqrt, log316, iec61966_2_4, iec61966-2-4, bt1361, bt1361, iec61966_2_1, iec61966-2-1, bt2020_10, bt2020_10bit, bt2020_12, bt2020_12bit, smpte2084, smpte428, smpte428_1 and arib-std-b67. Default: Auto.*  

### colorspace
*Supports bt709, rgb, fcc, bt470bg, smpte170m, smpte240m, ycocg, bt2020nc, bt2020_ncl, bt2020c, bt2020_cl, smpte2085, chroma-derived-nc, chroma-derived-c, ictcp. Default: Auto.*  

### color_range
*Supports tv, mpeg, pc and jpeg. Default: Not Set or Auto.*  

### frame_before
*Accepts boolean value, either true or false. Default: false.*  

Set this value to true if frames compressed in a separate session will be concatenated before the beginning of the current session. Set this value to false if the current session is a standalone session, or if this session will encode the first segment of a multi-segment compression.  

### frame_after
*Accepts boolean value, either true or false. Default: false.*  

Set this value to true if frames compressed in a separate session will be concatenated following the end of the current session. Set this value to false if the current session is a standalone session, or if this session will encode the last segment of a multi-segment compression.  

## **Miscellaneous**

### allow_sw
*Accepts boolean value, either true or false. Default: false.*  

Allow software encoding.  

In Akatsumekusa's test, VideoToolbox didn't use software encoding when encoding a 1080p23.976 video on M1 with this value set to true.  

Also, note that information in this page focuses on VideoToolbox's hardware encoding capacities while completely ignores its software encoding.  

### require_sw
*Accepts boolean value, either true or false. Default: false.*  

Force software encoding.  

### realtime
*Accepts boolean value, either true or false. Default: false.*  

Hint that encoding should happen in realtime if not faster.  

### prio_speed
*Accepts boolean value, either true or false. Default: Auto.*  

Prioritise encoding speed over efficiency.  

In Akatsumekusa's test, VideoToolbox didn't trade efficiency for speed when encoding a 1080p23.976 video on M1 with this value set to true.  

### a53cc
*Accepts boolean value, either true or false. Default: true.*  

Add A53 Closed Captions if they are available.  
