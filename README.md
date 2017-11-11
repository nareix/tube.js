# tube.js
想做一个基于 Chromium 的 OBS：
- 渲染层使用 Chromium 本身
- video 标签支持 ffmpeg 的 demuxer/codec
- video 标签支持采集源，可以选择某个窗口/全屏/摄像头为采集源，支持类似 OBS 的游戏窗口采集
- 增加推流 API，使用 MediaTrack 作为源编码推流，支持 ffmpeg 的 muxer/codec

# 开发日志
[devlog](devlog.md)
