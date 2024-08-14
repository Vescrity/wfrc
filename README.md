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

## TODO

- Support for command option to control notification or not, area or fullscreen, system-audio or microphone or no-sound ...

## Troubleshooting

### 1. The video has no audio

Check this:
```bash
LANG=C pactl list sources | grep 'Name.*output'
```
  If it got more than one line, change the line in the script which started with AUDIO_DEV, change it to your default audio device, or disable the wrong device through tools like pavucontrol-qt. Or try to make the script can support the option to decide which device to use.
