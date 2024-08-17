# wfrc

English | [中文](README-zh_CN.md)

Simple bash script to record screen on wlroots-based compositors using wf-recorder

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

. /path/to/wfrc
```

## TODO

## Troubleshooting

### 1. The video has no audio

Check this:
```bash
LANG=C pactl list sources | grep 'Name.*output'
```
  If it got more than one line, change the line in the script which started with AUDIO_DEV, change it to your default audio device, or disable the wrong device through tools like pavucontrol-qt. Or try to make the script can support the option to decide which device to use.
