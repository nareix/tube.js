# tube.js
我想做一个基于 Chromium 的 OBS：
- 渲染层使用 Chromium 本身
- video 标签支持 ffmpeg 的 demuxer/codec。比如可以 `video.src = "rtmp://..."`
- 增加推流 API，可以选择某个 DOM Element 作为视频源编码推流，支持 ffmpeg 的 muxer/codec
- 增加视频采集 API，可以选择某个窗口/全屏/摄像头作为采集源（貌似 Chromium 已经支持了）

# 进度
把过程记录下来
