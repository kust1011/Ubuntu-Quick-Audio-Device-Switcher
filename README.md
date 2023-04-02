# Ubuntu Quick Audio Device Switcher

This script allows you to quickly switch between available audio devices on Ubuntu using PulseAudio.

## Prerequisites

- Ubuntu (or another Debian-based Linux distribution)
- PulseAudio (which is already installed by default on Ubuntu and flavors)

## Usage

1. Clone this repository or download the `audio-switcher.sh` script.
2. Open a terminal and navigate to the directory where you cloned the repository.
3. Run the following command to make the script executable: `chmod +x audio-switcher.sh`.
3. Run the script using the following command: `./audio-switcher.sh`.
4. The script will switch to the next available audio output device and move all inputs to it.
4. You can assign a keyboard shortcut to run the script using the instructions below.

## Assigning a Keyboard Shortcut

- Open `Settings` -> `Keyboard` ->  `View and Customize Shortcuts`
- Click `Customize Shortcuts` then click the plus sign to add a new shortcut
- Enter "Switch Audio Output" or any other name you prefer in the Name field
- In the Command/Script field, enter the full path to the script. For example, `/path/to/audio-switcher.sh.`
- Click Add and then choose a suitable key combination for your shortcut.

## Version Differences

There are two versions of the script provided in this repository. The only difference between the two is that one version skips output devices with a specific `device.description`, such as "GP107GL High Definition Audio Controller Digital Stereo (HDMI)". This is useful if you want to exclude certain output devices from being cycled through by the script.

The version that skips specific output devices is named `audio-switcher-exclude.sh`. The other version is named `audio-switcher.sh`.

## Implementation
The script works by using the `pacmd` command to interact with the PulseAudio server. It first gets a list of all available audio sinks (output devices) and their indexes using the `pacmd list-sinks` command. It then creates an array of all available sinks, and finds the index of the active sink.

If there are no other available sinks, the script will keep on using current active sink. Otherwise, it will switch to the next sink in the array and move all inputs to it using the `pacmd set-default-sink` and `pacmd move-sink-input` commands.

## Notes

- This script only switches audio output devices, not input devices or recording sources.
- If you encounter the error "No PulseAudio daemon running, or not running as session daemon" when running the script with `sudo`, try running it without `sudo`. Alternatively, you can run the script as a non-root user by adding your user to the `pulse-access` group using the following command: `sudo usermod -aG pulse-access your_username`.
- The script `audio-switcher-exclude.sh` skips over sinks with a specific device description. If you have a different sink that you want to exclude, you can modify `exclude_dev` on line 3 of the script accordingly. You can also use the following command: `pacmd list-sinks` to find the device description of the sink you want to exclude.
- This script was tested on Ubuntu 22.04.2 LTS.