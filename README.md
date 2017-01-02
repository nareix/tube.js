# tube.js
我想做一个基于 Chromium 的 OBS：
- 渲染层使用 Chromium 本身
- video 标签支持 ffmpeg 的 demuxer/codec
- video 标签支持采集源，可以选择某个窗口/全屏/摄像头为采集源
- 增加推流 API，可以选择某个 DOM Element 作为视频源编码推流，支持 ffmpeg 的 muxer/codec

# 开发日志
[编译 Chromium](compile-chrome.md)
