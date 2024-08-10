# wfrc
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
- Bind it to a keybind. Run it for the first time it will run slurp and get an area to record and the second time it will stop recording and copy it to your clipboard.

## TODO

- It seems to have some bugs on nixOS. It can run on my Arch Linux anyway.
- Support for command option to control notification or not, area or fullscreen, system-audio or microphone or no-sound ...
