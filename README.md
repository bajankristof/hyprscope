# hyprscope

A Hyprland based wrapper for [Gamescope](https://github.com/ValveSoftware/gamescope)
to run apps and games in a nested Wayland compositor with various enhancements.

## Features
- Automatically detects the width, height and refresh rate of the focused monitor to pass them to Gamescope.
- Supports Gamescope (and internal) flags from a per-user configuration file (under `~/.config/hypr/hyprscope.conf`).
- Can enable HDR in Gamescope only if the focused monitor supports it.
- Can enable SDR to HDR inverse tone mapping only if the focused monitor supports HDR.
- Can run Gamescope in GameMode with a simple flag (less typing... I'm lazy).

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

You can pass in the `--dry-run` flag to see the generated Gamescope command without executing it.

### Example
`~/.config/hypr/hyprscope.conf`:
```bash
# Enable GameMode
-G
# Enable adaptive sync
--adaptive-sync
```

#### 1440p 165Hz SDR monitor
```bash
hyprscope -e --hdr -- steam -gamepadui
# gamemoderun gamescope --adaptive-sync -e -W 2560 -H 1440 -r 165 -- steam -gamepadui
```

#### 4K 120Hz HDR monitor
```bash
hyprscope -e --hdr -- steam -gamepadui
# gamemoderun gamescope --adaptive-sync -e -W 3840 -H 2160 -r 120 --hdr-enabled -- steam -gamepadui
```
