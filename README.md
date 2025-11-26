# hyprscope

A Hyprland based wrapper for [Gamescope](https://github.com/ValveSoftware/gamescope)
to run apps and games in a nested Wayland compositor with various enhancements.

## Features
- Detection of width (`-W`) and height (`-H`) based on the focused monitor.
- Detection of refresh rate (`-r`) based on the focused monitor.
- Global Gamescope flags from a configuration file (`$HOME/.config/hypr/hyprscope.conf`).
- Enable HDR in Gamescope only if the monitor supports HDR (`--hdr`).
- Enable SDR to HDR inverse tone mapping in only if the monitor supports HDR (`--hdr-itm`).
- Run Gamescope in GameMode (`-G`).

## Requirements
- [Hyprland](https://hypr.land)
- [Gamescope](https://github.com/ValveSoftware/gamescope)
- [Bash](https://gnu.org/software/bash)
- [jq](https://github.com/jqlang/jq)
- (Optional) [GameMode](https://github.com/FeralInteractive/gamemode)

### Arch
```bash
sudo pacman -S bash gamemode gamescope hyprland jq
```

## Installation
1. Clone the repository.
2. Link `hyprscope` to a directory in your `$PATH`.

(If you have `make` installed, you can run `make install` to link the script to `/usr/local/bin/hyprscope`.)

## Usage
```bash
hyprscope -- %command%
# gamescope <global_flags> -W <width> -H <height> -r <refresh_rate> -- %command%
```

### Example
~/.config/hypr/hyprscope.conf:
```bash
# Enable GameMode
-G
# Enable adaptive sync
--adaptive-sync
```

With an SDR monitor that has a resolution of 2560x1440 at 165Hz:
```bash
hyprscope -e --hdr -- steam -gamepadui
# gamescope -G --adaptive-sync -W 2560 -H 1440 -r 165 -- steam -gamepadui
```

With an HDR monitor that has a resolution of 3840x2160 at 120Hz:
```bash
hyprscope -e --hdr -- steam -gamepadui
# gamescope -G --adaptive-sync --hdr-enabled -W 3840 -H 2160 -r 120 -- steam -gamepadui
```
