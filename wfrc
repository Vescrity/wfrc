#!/usr/bin/env bash
# v0.1.0
XDG_RUNTIME_DIR="${XDG_RUNTIME_DIR:-/run/user/$UID}"
WFRC_FOLDER="${WFRC_FOLDER:-$XDG_RUNTIME_DIR/wfrc}"
SCRIPT_NAME="${SCRIPT_NAME:-wfrc}"
WFRC_LOCK="${WFRC_LOCK:-$WFRC_FOLDER/WFRCLOCK}"
# Set the icon of notification
WFRC_ICON="${WFRC_ICON:-record}"
mkdir -p "$WFRC_FOLDER"
if ! [ -n "$WAYLAND_DISPLAY" ]; then
    WFRC_NOWAYLAND="${WFRC_NOWAYLAND:-No WAYLAND_DISPLAY found. Did you run me on a wayland compositor?}"
    echo "$WFRC_NOWAYLAND" >&2
    notify-send --app-name=$SCRIPT_NAME 'Error' "$WFRC_NOWAYLAND" --icon="$WFRC_ICON"
    exit 1
fi

# 1 to enable full screen
WFRC_FULL_SCREEN="${WFRC_FULL_SCREEN:-0}"
# 0 to disable notification
WFRC_NOTIFY="${WFRC_NOTIFY:-1}"
if [ -f "$WFRC_LOCK" ]; then
    kill $(cat "$WFRC_LOCK")
    rm "$WFRC_LOCK"
    exit
fi
echo $$ > "$WFRC_LOCK"

kill_wfrc() {
    kill -TERM $wf_recorder_pid
    rm $WFRC_LOCK
    echo -n "file://$WFRC_FILE_NAME" | wl-copy -t 'text/uri-list'
    if [ $WFRC_NOTIFY -eq 1 ]; then
        SIZE="$(ls -lh $WFRC_FILE_NAME | tr -s ' ' | cut -d ' ' -f 5)"
        ENDTIME=$(date +%s)
        duration=$((ENDTIME - STARTTIME))
        if [ $WFRC_FULL_SCREEN -eq 0 ]; then
            resolution=$(echo $output | cut -d' ' -f2)
        else
            resolution='Full screen'
        fi
        minutes=$((duration / 60))
        seconds=$((duration % 60))
        if [ $minutes -gt 0 ]; then
            formatted_duration="${minutes}m ${seconds}s"
        else
            formatted_duration="${seconds}s"
        fi
        notify-send -t 1000 --app-name=$SCRIPT_NAME 'Finished.' \
            "$formatted_duration | $SIZE | $resolution" --icon="$WFRC_ICON"
    fi
}

trap 'kill_wfrc $$' SIGINT SIGTERM 

if [ $WFRC_FULL_SCREEN -eq 0 ]; then
    output=$(slurp)
    result=$?
    if ! [ $result -eq 0 ]; then
        rm "$WFRC_LOCK"
        if [ $WFRC_NOTIFY -eq 1 ]; then
            notify-send --app-name=$SCRIPT_NAME 'Canceled.' --icon="$WFRC_ICON"
        fi
        exit
    fi
fi

if [ $WFRC_NOTIFY -eq 1 ]; then
    notify-send -t 1000 --app-name=$SCRIPT_NAME 'Rec.' --icon="$WFRC_ICON"
fi
WFRC_AUDIO_DEV="${WFRC_AUDIO_DEV:-$(LANG=C pactl list sources | grep 'Name.*output'|cut -d ' ' -f 2)}"
WFRC_FILE_NAME="${WFRC_FILE_NAME:-$WFRC_FOLDER/$SCRIPT_NAME-$(date +%Y-%m-%dT%H-%M-%S).mp4}"
if [ $WFRC_FULL_SCREEN -eq 0 ]; then
    wf-recorder -f $WFRC_FILE_NAME -g "$output" --audio=$WFRC_AUDIO_DEV "$@" &
else
    wf-recorder -f $WFRC_FILE_NAME --audio=$WFRC_AUDIO_DEV "$@" &
fi
STARTTIME=$(date +%s)
wf_recorder_pid=$!
wait $wf_recorder_pid
