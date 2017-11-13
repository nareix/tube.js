# 编译 Chrome
中途需要访问很多被 block 的 url，需要开全局 VPN。或者用 Proxifier+shadowsockets，shadowsockets 默认会开一个 SOCKS5 代理，
Proxifier 里面把全局的流量都导向这个 SOCKS5 代理，同时要打开 DNS Resolve 也走代理。

中途也会遇到红色的字提示某个脚本没有数字签名，在 cmd.exe 中键入 `powershell` 打开 Powershell，然后输入 `Unblock-File -Path 某个脚本` 即可。

# 分析

extension 里面有 tabCapture 的功能。

https://cs.chromium.org/chromium/src/chrome/browser/extensions/api/tab_capture/tab_capture_api.h?q=tabCapture&sq=package:chromium&dr=CSs&l=25

使用了一个特殊的 url（以 web-contents-media-stream:// 开头）。这个 url 在
`src/content/browser/media/capture/web_contents_video_capture_device.cc`
中被识别。

MediaStream 的双向的使用姿势：
- 可以把 MediaStream 交给 video 标签播放
- 也可以把 video 标签或者其他 capture 来源变成一个 MediaStream

MediaStream 里面有多个 [MediaStreamTrack](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack)。

MediaStreamTrack：

- [webrtc/pc/mediastreamtrack.h](https://cs.chromium.org/chromium/src/third_party/webrtc/pc/mediastreamtrack.h?type=cs&q=webrtc/pc/medi&sq=package:chromium&l=1)

  - MediaTrack<T> 这个类是模板类，用来套 AudioTrackInterface/VideoTrackInterface

- [webrtc/api/mediastreaminterface.h](https://cs.chromium.org/chromium/src/third_party/webrtc/api/mediastreaminterface.h?type=cs&q=VideoTrackInterface&sq=package:chromium&l=158)

  - 包含对 AudioTrackInterface/VideoTrackInterface 这两个 class 可以获取 VideoFrame
  
- [webrtc/pc/videotrack.h](https://cs.chromium.org/chromium/src/third_party/webrtc/pc/videotrack.h?type=cs&sq=package:chromium)

  - VideoTrack 继承了 MediaStreamTrack<VideoTrackInterface>

- [content/renderer/media/media_stream_track.h](https://cs.chromium.org/chromium/src/content/renderer/media/media_stream_track.h?q=+content/renderer/media/media_stream_track.h&sq=package:chromium&dr)

  - MediaStreamTrack is a Chrome representation of blink::WebMediaStreamTrack. It is owned by blink::WebMediaStreamTrack as blink::WebMediaStreamTrack::ExtraData.

- [Webkit/Source/modules/mediastream/MediaStreamTrack.h](https://cs.chromium.org/chromium/src/third_party/WebKit/Source/modules/mediastream/MediaStreamTrack.h?type=cs&q=Webkit/Source/modules/mediastream/MediaStreamTrack.h&sq=package:chromium&l=1)

  - modules 目录下的都是一些 js/v8 层的接口（IDL）

## blink::WebMediaStream -> WebRTC 内部的过程

https://cs.chromium.org/chromium/src/media/capture/video_capturer_source.h?type=cs&sq=package:chromium&l=44

https://cs.chromium.org/chromium/src/content/renderer/media/media_stream_video_track.cc?type=cs&sq=package:chromium&l=310

https://cs.chromium.org/chromium/src/content/renderer/media/webrtc/media_stream_video_webrtc_sink.cc?type=cs&sq=package:chromium&l=260

https://cs.chromium.org/chromium/src/content/renderer/media/webrtc/media_stream_video_webrtc_sink.h?type=cs&sq=package:chromium&l=30

https://cs.chromium.org/chromium/src/content/renderer/media/webrtc/webrtc_media_stream_track_adapter.cc?type=cs&sq=package:chromium&l=16

https://cs.chromium.org/chromium/src/content/renderer/media/webrtc/webrtc_media_stream_track_adapter_map.cc?type=cs&sq=package:chromium&l=99

https://cs.chromium.org/chromium/src/content/renderer/media/webrtc/webrtc_media_stream_adapter.cc?type=cs&sq=package:chromium&l=121

https://cs.chromium.org/chromium/src/content/renderer/media/webrtc/webrtc_media_stream_adapter.h?type=cs&sq=package:chromium&l=35

https://cs.chromium.org/chromium/src/content/renderer/media/rtc_peer_connection_handler.h?type=cs&sq=package:chromium&l=144

https://cs.chromium.org/chromium/src/content/renderer/media/webrtc/peer_connection_dependency_factory.cc?type=cs&sq=package:chromium&l=132

https://cs.chromium.org/chromium/src/content/renderer/renderer_blink_platform_impl.cc?type=cs&sq=package:chromium&l=903

https://cs.chromium.org/chromium/src/third_party/WebKit/Source/modules/peerconnection/RTCPeerConnection.cpp?type=cs&sq=package:chromium&l=511
