# CBS
基于 Chromium 的类似 OBS 的推流软件。需要实现的有：
- 用 ffmpeg 编码 MediaStream 并推出去（目前 Chromium 基于的是）
- 实现 OBS 的游戏窗口捕获

src/third_party/webrtc/pc/mediastreamtrack.h
https://cs.chromium.org/chromium/src/third_party/webrtc/pc/mediastreamtrack.h?q=MediaStreamTrack&sq=package:chromium&dr=CSs&l=28


# 编译 Chrome
中途需要访问很多被 block 的 url，需要开全局 VPN。或者用 Proxifier+shadowsockets，shadowsockets 默认会开一个 SOCKS5 代理，
Proxifier 里面把全局的流量都导向这个 SOCKS5 代理，同时要打开 DNS Resolve 也走代理。

中途也会遇到红色的字提示某个脚本没有数字签名，在 cmd.exe 中键入 `powershell` 打开 Powershell，然后输入 `Unblock-File -Path 某个脚本` 即可。
