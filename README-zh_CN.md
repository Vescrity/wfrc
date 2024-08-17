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

## 行为配置

脚本的行为通过环境变量进行控制。你可以另外编写一个脚本来自定义这些变量。

```bash
# 默认情况下可控制文件存放路径
#WFRC_FOLDER="/tmp"
# 默认情况下可控制通知名称，文件名称等
#SCRIPT_NAME="wfrc"
# 锁文件的路径
#WFRC_LOCK="/tmp/WFRCLOCK"
# 控制通知图标
#WFRC_ICON="record"
# 设为 1 进行全屏录屏
#WFRC_FULL_SCREEN=0
# 设为 0 不进行通知
#WFRC_NOTIFY=1
# 你的音频设备
#WFRC_AUDIO_DEV="$(LANG=C pactl list sources | grep 'Name.*output'|cut -d ' ' -f 2)"
#WFRC_FILE_NAME="$WFRC_FOLDER/$SCRIPT_NAME-$(date -u +%Y-%m-%dT%H-%M-%S).mp4"
# 不在 wayland 上运行时的提示信息
#WFRC_NOWAYLAND="No WAYLAND_DISPLAY found. Did you run me on a wayland compositor?"

exec /path/to/wfrc
```

## TODO

## 疑难解答

### 1. 录屏文件没有捕获到声音

运行:
```bash
LANG=C pactl list sources | grep 'Name.*output'
```
  若输出多行，代表你有多个可用的音频输出设备, 请更改脚本中以 AUDIO_DEV 开头的行, 将其值设定为你的默认音频设备，或通过 `pavucontrol-qt` 等工具禁用掉错误的设备。或者你也可以尝试更改脚本使得脚本能够支持通过命令行选项等方式选择设备。

