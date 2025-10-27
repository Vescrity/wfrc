# wfrc

English | [中文](README-zh_CN.md)

Simple bash script to make recording screen by shortcuts easily. Works on `niri`, `hyprland`, wlroots-based...
because it uses [wf-recorder](https://github.com/ammen99/wf-recorder) or [wl-screenrec](https://github.com/russelltg/wl-screenrec) as backend.

> Since 0.1.2, `wl-screenrec` can be used as backend.
> If you want to use it please set `WFRC_RECORDER=wl-screenrec`

https://github.com/user-attachments/assets/5424662d-9f1e-4302-888f-3cb0a7e98bca

## Features

- [x] Config by environment variables
- [x] Area selection
- [x] Record the audio
- [x] Notification
- [x] Copy to your clipboard

## Install

now in AUR, `wfrc-git`

## Dependencies

- wf-recorder (or wl-screenrec)
- bash
- grep
- slurp
- libnotify
- libpulse
- wl-clipboard

## Usage

- (optional) Add it to your PATH
- (optional) Bind it to a keybind.
- Run it for the first time it will run slurp and get an area to record and the second time it will stop recording and copy the video to your clipboard.

## Config

This script's behavior is controled by environment variables. You can make a script to set them.

```bash
# Use wf-recorder as default recorder.
#WFRC_RECORDER="${WFRC_RECORDER:-wf-recorder}"
# By default it can control where the file will be stored
#WFRC_FOLDER="/tmp/wfrc-$UID"
# By default it can control the title of the notification and the file's name
#SCRIPT_NAME="wfrc"
# Where the lock file
#WFRC_LOCK="$WFRC_FOLDER/WFRCLOCK"
# Set the icon of notification
#WFRC_ICON="record"
# 1 to enable full screen
#WFRC_FULL_SCREEN=0
# 0 to disable notification
#WFRC_NOTIFY=1
# Your audio device
#WFRC_AUDIO_DEV="$(LANG=C pactl get-default-sink).monitor"
#WFRC_FILE_NAME="$WFRC_FOLDER/$SCRIPT_NAME-$(date +%Y-%m-%dT%H-%M-%S).mp4"
# If no wayland, the error msg
#WFRC_NOWAYLAND="No WAYLAND_DISPLAY found. Did you run me on a wayland compositor?"

. /path/to/wfrc "$@"
```

## TODO

## Troubleshooting

### 1. The video has no audio

Check this:
```bash
LANG=C pactl list sources | grep 'Name.*output'
```
  If it got more than one line, it means you have more than one audio output device. Use the config script and change `WFRC_AUDIO_DEV` to your device.

### 2. High CPU usage

It's caused by wf-recorder. If the issue can be resolved by adjusting some command-line options for wf-recorder, please add those options directly to the script, and they will be passed directly to wf-recorder.  

> or use `wl-screenrec` as backend.

See [here](https://github.com/ammen99/wf-recorder?tab=readme-ov-file#usage):

> To use GPU encoding, use a VAAPI codec (for ex. `h264_vaapi`) and specify a GPU device to use with the `-d` option:
> `wf-recorder -f test-vaapi.mkv -c h264_vaapi -d /dev/dri/renderD128` ...

For me, just use 
```bash
wfrc -c h264_vaapi -d /dev/dri/renderD128
```

