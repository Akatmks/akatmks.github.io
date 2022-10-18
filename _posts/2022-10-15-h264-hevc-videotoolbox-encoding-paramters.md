---
layout: post
title: H264/HEVC VideoToolbox Encoding Parameters
---

This page explains the parameters of FFmpeg's `h264_videotoolbox` and `hevc_videotoolbox` codec. It will focus on VideoToolbox's hardware encoding capabilities on Apple silicon.  

Note that this page is drafted in October 2022 against an M1 MacBook. If any of the behaviours mentioned in the page has changed in future version of VideoToolbox or on newer generation of Apple M series chips, feel free to open a discussion [here](https://github.com/Akatmks/akatmks.github.io/issues). Also, if you find any problems within the page, please let Akatsumekusa know by opening a issue [here](https://github.com/Akatmks/akatmks.github.io/issues). It will be greatly appreciated. Thank you very much.  

### Ratecontrol

VideoToolbox in FFmpeg supports both VBR (Variable Bitrate) mode and CQ (Constant Quality) mode.  

#### b:v
*Accepts int64 value from 0 to I64_Max. Default: 200k.*  

Set the target bitrate for encoding in VBR mode.  
A good starting point for 1080p video is around 3000k to 6000k.  

#### q:v
*Accepts float value from 0 to 100. Default: Not Set.*  

Set the target quality for encoding in CQ mode from 0 to 100. Higher numbers produce better quality.  
Setting this option will enable CQ mode and disable `-b:v` setting.  

Constant Quality in VideoToolbox is not optimal. It performs badly on sections of video with blur effect or detailed texture. The quality of such sections drops far below the target quality, and there is currently no way to encourage the encoder to use more bitrate on these sections.  

Also, despite `-q:v` accept quality values from 0 to 100, it doesn't support that many levels of quality. As an example, values between 53.0 to 54.9 will yield the exact same result, so do 55.0 to 56.9 and 57.0 to 59.9.  

A good starting point for `-q:v`  is around 55 to 60.  


