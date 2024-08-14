# wfrc

[English](README.md) | 中文

简易的 wf-recorder 的 bash 脚本封装，实现在基于 wlroots 的 wayland 混成器上进行选区录屏。

## 依赖

- wf-recorder
- bash
- grep
- slurp
- libnotify
- libpulse
- wl-copy

## 使用方法

- (可选) 添加到 PATH
- (可选) 将脚本绑定到快捷键。
- 第一次执行将启动 `slurp` 以获取录屏区域，并随后执行录屏。再次运行则将停止录屏并将文件复制到剪贴板。

## TODO

- 支持命令行参数控制是否开启通知，是否全屏，声音来源取自系统输出还是麦克风或是静音...

## 疑难解答

### 1. 录屏文件没有捕获到声音

运行:
```bash
LANG=C pactl list sources | grep 'Name.*output'
```
  若输出多行，代表你有多个可用的音频输出设备, 请更改脚本中以 AUDIO_DEV 开头的行, 将其值设定为你的默认音频设备，或通过 `pavucontrol-qt` 等工具禁用掉错误的设备。或者你也可以尝试更改脚本使得脚本能够支持通过命令行选项等方式选择设备。

