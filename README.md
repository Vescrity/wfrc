# wfrc

English | [中文](README-zh_CN.md)

Simple bash script to make recording screen by shortcuts easily. Only work on wlroots based wayland compositors because it uses [wf-recorder](https://github.com/ammen99/wf-recorder) as backend.

## Features

- [x] Config by environment variables
- [x] Area selection
- [x] Record the audio
- [x] Notification
- [x] Copy to your clipboard

## Dependencies

- wf-recorder
- bash
- grep
- slurp
- libnotify
- libpulse
- wl-copy

## Usage

- (optional) Add it to your PATH
- (optional) Bind it to a keybind.
- Run it for the first time it will run slurp and get an area to record and the second time it will stop recording and copy the video to your clipboard.

## Config

This script's behavior is controled by environment variables. You can make a script to set them.

```bash
# By default it can control where the file will be stored
#WFRC_FOLDER="/tmp"
# By default it can control the title of the notification and the file's name
#SCRIPT_NAME="wfrc"
# Where the lock file
#WFRC_LOCK="/tmp/WFRCLOCK"
# Set the icon of notification
#WFRC_ICON="record"
# 1 to enable full screen
#WFRC_FULL_SCREEN=0
# 0 to disable notification
#WFRC_NOTIFY=1
# Your audio device
#WFRC_AUDIO_DEV="$(LANG=C pactl list sources | grep 'Name.*output'|cut -d ' ' -f 2)"
#WFRC_FILE_NAME="$WFRC_FOLDER/$SCRIPT_NAME-$(date -u +%Y-%m-%dT%H-%M-%S).mp4"
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
  If it got more than one line, change the line in the script which started with AUDIO_DEV, change it to your default audio device, or disable the wrong device through tools like pavucontrol-qt. Or try to make the script can support the option to decide which device to use.

### 2. High CPU usage

It's caused by wf-recorder. If the issue can be resolved by adjusting some command-line options for wf-recorder, please add those options directly to the script, and they will be passed directly to wf-recorder.  

See [here](https://github.com/ammen99/wf-recorder?tab=readme-ov-file#usage):

> To use GPU encoding, use a VAAPI codec (for ex. `h264_vaapi`) and specify a GPU device to use with the `-d` option:
> `wf-recorder -f test-vaapi.mkv -c h264_vaapi -d /dev/dri/renderD128` ...
