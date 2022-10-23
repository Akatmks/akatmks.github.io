---
layout: post
title: Akatsumekusa's Note on VideoToolbox Encoders
---

*If you are here looking for a general guide on FFmpeg's VideoToolbox codecs, please refer to [this]({% post_url 2022-10-16-h264-hevc-videotoolbox-encoding-parameters %}) post.*  

Akatsumekusa is encoding WebRip with x264 on a 2000 series mobile Ryzen CPUs. Even with fast settings like `--bframes 8 --ref 4` and `--me hex --merange 16 --subme 7`, Akatsumekusa can hardly get a speed faster than 0.65x or 18 fps.  
That's why Akatsumekusa is looking for an alternative solution that offers a quality similar to x264 `-crf 22 --psy-rd 0.6:0.1` but with faster encoding speed, maybe at the expense of a 10% to 15% filesize increase.  

Sadly, after some testing, VideoToolbox is considered completely unusable despite its super fast encoding speed.  

There are a few big dealbreakers:  
- VideoToolbox's terrible Constant Quality mode. The Constant Quality mode performs poorly on sections of video with blur or fast movements. The quality of these sections drops far below the target quality, and there is no way to encourage the encoder to use more bitrate on these sections.  
- VideoToolbox's inability to perform adaptive I-frame decision. Using a fixed I-frame interval like 72 or 120 will increase the result filesize by as much as 80% comparing to an encoding with only one I-frame at the start.  
- VideoToolbox's terrible encoding efficiency. At similar bitrate, the edge produced by VideoToolbox is considerably worse than that of x264 faster. It also frequently chooses to ignore the source texture and replace anything with a flat, untextured surface. This is unacceptable considering that a lot of recent anime prefer either very noisy or detailed backgrounds.  

There are also some other problems, such as the rather big gap between nearby quality levels and VideoToolbox's bad b-frame execuation.  

All in all, Akatsumekusa would considered VideoToolbox to be completely unusable for WebRip encoding.  
