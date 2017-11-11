# 编译 Chrome
中途需要访问很多被 block 的 url，需要开全局 VPN。或者用 Proxifier+shadowsockets，shadowsockets 默认会开一个 SOCKS5 代理，
Proxifier 里面把全局的流量都导向这个 SOCKS5 代理，同时要打开 DNS Resolve 也走代理。

中途也会遇到红色的字提示某个脚本没有数字签名，在 cmd.exe 中键入 `powershell` 打开 Powershell，然后输入 `Unblock-File -Path 某个脚本` 即可。

# 分析

## 获取窗口的 MediaStream
extension 里面有 tabCapture 的功能。

https://cs.chromium.org/chromium/src/chrome/browser/extensions/api/tab_capture/tab_capture_api.h?q=tabCapture&sq=package:chromium&dr=CSs&l=25

使用了一个特殊的 url（以 web-contents-media-stream:// 开头）。这个 url 在
`src/content/browser/media/capture/web_contents_video_capture_device.cc`
中被识别。


## 从 MediaTrack 到获取 VideoFrame 的过程

MediaStreamTrack：
- webrtc/pc/mediatrack.h
- content/renderer/media/media_stream_track.h
- Webkit/Source/modules/mediastream/MediaStreamTrack.h
 - modules 目录下的都是一些 js/v8 层的接口（IDL）
