# Hyprland Configuration

A personal Hyprland setup featuring a cmatrix background terminal, blur/shadow decorations, and a minimal Wayland stack.

---

## Dependencies

| Tool | Role |
|---|---|
| [Hyprland](https://hyprland.org/) | Wayland compositor |
| [Hyprpaper](https://github.com/hyprwm/hyprpaper) | Wallpaper daemon |
| [Waybar](https://github.com/Alexays/Waybar) | Status bar (see Waybar section for full deps) |
| [Kitty](https://sw.kovidgoyal.net/kitty/) | Terminal emulator |
| [Wofi](https://hg.sr.ht/~scoopta/wofi) | App launcher |
| [Nemo](https://github.com/linuxmint/nemo) | File manager |
| [Brave](https://brave.com/) | Browser |
| [VS Code](https://code.visualstudio.com/) | Code editor (`code` command) |
| [cmatrix](https://github.com/abishekvashok/cmatrix) | Matrix rain terminal effect |
| [wpctl](https://pipewire.pages.freedesktop.org/wireplumber/) | Audio volume control (WirePlumber) |
| [hyprshutdown](https://github.com/hyprwm/hyprland-contrib) | Graceful exit helper (optional) |

---

## File Structure

```
~/.config/hypr/
├── hyprland.conf        # Main compositor config
└── hyprpaper.conf       # Wallpaper config

~/.config/waybar/
├── config.jsonc         # Bar layout & module list
├── style.css            # Root stylesheet (imports tokens)
├── modules/
│   ├── audio.jsonc
│   ├── battery.jsonc
│   ├── clock.jsonc
│   ├── connections.jsonc
│   ├── distro.jsonc
│   ├── groups.jsonc
│   ├── idle-ihibitor.jsonc
│   ├── power-profiles-daemon.jsonc
│   ├── storage.jsonc
│   ├── system.jsonc
│   ├── tray-notif.jsonc
│   └── workspace.jsonc
└── tokens/
    ├── colors.css       # Matugen color palette
    ├── batt-clock.css
    ├── slider.css
    ├── state.css
    ├── widget.css
    └── workspace.css
```

---

## hyprpaper.conf

Controls the desktop wallpaper via the Hyprpaper daemon.

```ini
wallpaper {
    monitor  =                                    # Empty = all monitors
    path     = /home/popopopo/wallpapers/wall3.jpg
    fit_mode = cover                              # Scale to fill the screen
}
splash = false                                    # Disable Hyprpaper splash text
```

**To change the wallpaper** — update the `path` to point to your image. Supported formats: JPEG, PNG, WebP.

The commented-out lines at the top (`#preload`, `#wallpaper`) are the legacy flat syntax and are unused. The block syntax (`wallpaper { }`) is the current approach.

---

## hyprland.conf

### Variables

```ini
$mainMod    = SUPER        # Windows / Meta key
$terminal   = kitty
$fileManager = nemo
$menu       = wofi
```

These are used throughout the keybind section so you only need to change an app in one place.

---

### Autostart (`exec-once`)

These run once when Hyprland starts:

| Command | Purpose |
|---|---|
| `hyprpaper` | Loads the wallpaper |
| `waybar` | Starts the status bar |
| `kitty --class cmatrix_term -e cmatrix` | Spawns a cmatrix terminal pinned to workspace 2 |

---

### Workspaces

```ini
workspace = 1, persistent:true
workspace = 2, persistent:true
workspace = 3, persistent:true
```

Workspaces 1–3 are always kept alive even when empty. Workspace 2 is where the cmatrix terminal lives.

---

### Animations

```ini
animations {
    enabled = true
    animation = workspaces, 1, 4, default, slidevert
}
```

Workspace switching uses a vertical slide animation at speed 4.

---

### Decorations

#### Rounding
```ini
rounding = 10   # 10px rounded corners on all windows
```

#### Shadow
```ini
shadow {
    enabled      = true
    range        = 20           # Shadow spread in pixels
    render_power = 3            # Shadow falloff sharpness
    color        = rgba(000000aa)  # Black at ~67% opacity
}
```

#### Blur
```ini
blur {
    enabled = true
    size    = 6    # Blur kernel size
    passes  = 3    # More passes = stronger blur
}
```

---

### Window Rules

| Rule | Matches | Effect |
|---|---|---|
| `cmatrix-opacity` | class `cmatrix_term` | 70% opacity |
| `kitty-opacity` | class `kitty` | 65% opacity |
| `cmatrix-workspace` | class `cmatrix_term` | Auto-moves to workspace 2 |

The cmatrix terminal is intentionally semi-transparent so it can serve as a living wallpaper effect behind other windows on workspace 2.

---

### Keybindings

#### Applications

| Shortcut | Action |
|---|---|
| `Super + Q` | Open terminal (Kitty) |
| `Super + E` | Open file manager (Nemo) |
| `Super + R` | Open app launcher (Wofi) |
| `Super + B` | Open browser (Brave) |
| `Super + V` | Open VS Code |

#### Window Management

| Shortcut | Action |
|---|---|
| `Super + C` | Close active window |
| `Super + Space` | Toggle floating |
| `Super + F` | Fullscreen (maximized, not true fullscreen) |
| `Super + M` | Exit Hyprland (uses `hyprshutdown` if available, falls back to `hyprctl dispatch exit`) |

#### Focus Navigation

| Shortcut | Action |
|---|---|
| `Super + ←` | Focus left |
| `Super + →` | Focus right |
| `Super + ↑` | Focus up |
| `Super + ↓` | Focus down |

#### Workspace Switching

| Shortcut | Action |
|---|---|
| `Super + Shift + →` or `↑` | Previous workspace |
| `Super + Shift + ←` or `↓` | Next workspace |

#### Audio

| Shortcut | Action |
|---|---|
| `Super + Ctrl + ↑` | Volume +5% |
| `Super + Ctrl + ↓` | Volume −5% |

#### Other

| Shortcut | Action |
|---|---|
| `Shift + C` | Run `clear` in shell |

---

---

## Waybar

### Dependencies

| Tool | Role |
|---|---|
| [Waybar](https://github.com/Alexays/Waybar) | Bar itself |
| [SwayNC](https://github.com/ErikReider/SwayNotificationCenter) | Notification daemon (`swaync-client`) |
| [PulseAudio / PipeWire](https://pipewire.org/) | Audio backend |
| [NetworkManager](https://networkmanager.dev/) | Network (`nm-applet`) |
| [Blueman](https://github.com/blueman-project/blueman) | Bluetooth manager |
| [pavucontrol](https://freedesktop.org/software/pulseaudio/pavucontrol/) | Audio GUI |
| [power-profiles-daemon](https://gitlab.freedesktop.org/upower/power-profiles-daemon) | Power profile switching |
| [Tela-circle-dark icons](https://github.com/vinceliuice/Tela-circle-icon-theme) | Tray icon overrides |
| [Inter](https://rsms.me/inter/) + [JetBrainsMono Nerd Font](https://www.nerdfonts.com/) | Bar fonts |
| [Matugen](https://github.com/InioX/matugen) | Color token generator (used to produce `colors.css`) |

---

### Bar Layout (`config.jsonc`)

The bar sits at the top of the screen at 42px tall and is divided into three zones:

**Left** — distro launcher drawer, storage group, system group

**Center** — power profile indicator, workspace switcher, idle inhibitor

**Right** — audio group (with slider), connections group, battery, clock, tray group

All module definitions live in separate files under `modules/` and are pulled in via the `include` array, keeping `config.jsonc` clean.

---

### Modules

#### Distro Group (`distro.jsonc` + `groups.jsonc`)

A drawer that slides open left-to-right (650ms transition) revealing quick-launch icons. Click the distro icon to expand it.

| Icon | App launched |
|---|---|
| Distro logo | — (static, no action) |
| Terminal | `kitty` |
| Code | `code` (VS Code) |
| Obsidian | `obsidian` |
| Office | `onlyoffice-desktopeditors` |
| Files | `thunar` |
| Spotify | `spotify` |
| Browser | `firefox` |

#### Storage Group (`storage.jsonc`)

Shows disk and RAM usage as icons only. Click either icon to toggle an inline text readout of current usage.

| Module | Warning | Critical |
|---|---|---|
| Disk (`/`) | 80% | 90% |
| Memory | 75% | 85% |

Disk usage refreshes every hour; memory every 15 seconds.

#### System Group (`system.jsonc`)

Shows CPU and temperature as icons. Click to toggle percentage/°C readout.

| Module | Warning | Critical |
|---|---|---|
| CPU | 75% | 90% |
| Temperature (zone 8) | — | 80°C |

> **Note:** `thermal-zone: 8` may need to be adjusted for your hardware. Run `cat /sys/class/thermal/thermal_zone*/type` to find the right zone index.

#### Power Profiles Daemon (`power-profiles-daemon.jsonc`)

Displays the active power profile as an icon. Tooltip shows the profile name and driver.

| Profile | Icon color |
|---|---|
| Power-saver | Green |
| Balanced | Default |
| Performance | Red |

#### Workspaces (`workspace.jsonc`)

Displays 5 persistent workspaces on all outputs. Each workspace shows an icon for the active power profile and icons for every open window inside it, using a rich rewrite map:

| App class | Icon |
|---|---|
| `kitty` | Terminal |
| `code` | VS Code |
| `firefox` | Firefox |
| `obsidian` | Obsidian |
| `spotify` | Spotify |
| `discord` | Discord |
| `vlc` / `mpv` | Video player |
| `blueman-manager` | Bluetooth |
| `pavucontrol` | Audio |
| and many more... | — |

Click a workspace button to switch to it. Special scratchpad workspace is also shown.

#### Idle Inhibitor (`idle-ihibitor.jsonc`)

Prevents the system from going idle/sleeping when activated. Toggled by clicking the icon in the center of the bar. Tooltip reads "System Focused" when active.

#### Audio Group (`audio.jsonc`)

A drawer that slides open right-to-left (650ms) revealing a volume slider and microphone indicator alongside the speaker icon.

- **Speaker icon** — click to open `pavucontrol`. Shows headphone/headset icon when those are the active output.
- **Volume slider** — horizontal, 0–100%.
- **Microphone icon** — shows a muted icon when the mic is off.

#### Connections Group (`connections.jsonc`)

A drawer (500ms, right-to-left) with network and Bluetooth.

**Network:**
- Scroll up/down on the icon to toggle Wi-Fi on/off via `nmcli`
- Left-click opens `nm-applet`; right-click kills it
- Tooltip shows SSID, signal strength, frequency, and live bandwidth

**Bluetooth:**
- Left-click opens `blueman-manager`
- Right-click toggles Bluetooth with `rfkill`
- Tooltip lists connected devices with battery percentages when available

#### Battery (`battery.jsonc`)

Refreshes every 60 seconds. Shows a dynamic battery icon + percentage. Switches to a charging icon while plugged in and charging, and a plug icon when fully charged and plugged in. Alt-click shows remaining time instead of percentage.

| State | Warning | Critical |
|---|---|---|
| Battery % | 30% | 15% |

Tooltip includes power draw (W), battery health (%), and total charge cycles.

#### Clock (`clock.jsonc`)

Displays time in `HH.MM` format (24h), timezone set to **Asia/Jakarta (WIB, UTC+7)**. Click to toggle to a full date view (`DD Month YYYY`). Hover for a full calendar tooltip.

> To change timezone — edit the `"timezone"` field to your own, e.g. `"Europe/Paris"` or `"America/New_York"`.

#### Tray Group (`tray-notif.jsonc`)

A drawer (500ms, right-to-left) with the system tray and a SwayNC notification button.

The `custom/tray-arrow` button integrates with SwayNC:
- **Left-click** — open/close the notification panel
- **Right-click** — toggle Do Not Disturb mode
- Icon changes to reflect DND state and whether notifications are pending

Tray icon overrides use the **Tela-circle-dark** icon theme for Blueman, Discord, Steam, and Spotify.

---

### Styling

#### Color System (`tokens/colors.css`)

All colors are generated by [Matugen](https://github.com/InioX/matugen) from a seed color (`#25738B`, a teal-blue). The palette follows Material You conventions — do not edit this file manually; regenerate it via Matugen when you want a new palette.

Key color roles used throughout the bar:

| Variable | Usage |
|---|---|
| `@primary` | Active workspace, slider thumb, icons |
| `@surface_container` | Module pill backgrounds |
| `@secondary_fixed_dim` | Default foreground text/icons |
| `@outline_variant` | Empty workspace indicators, slider trough |
| `@tertiary` | Inactive workspace buttons |

#### State Colors (`tokens/state.css`)

Overrides icon color based on module state:

| Color | States |
|---|---|
| Green | Charging, power-saver, idle inhibitor active, no notifications |
| Yellow | Warning thresholds (CPU/disk/memory/battery), pending notifications |
| Red | Critical thresholds, network disabled, performance mode, DND+inhibited |

#### Module Pills (`tokens/widget.css`)

Every module group is styled as a rounded pill (`border-radius: 20px`) with `@surface_container` background, providing a floating capsule look against the transparent bar background.

Each quick-launch icon in the distro drawer has its own accent color (green for terminal, blue for VS Code, purple for Obsidian, etc.).

#### Workspace Buttons (`tokens/workspace.css`)

- Inactive buttons — tertiary color, transparent background
- Active button — expands with padding, filled with `@primary`, white label
- Empty buttons — dimmed with `@outline_variant`
- Hover — highlights with `@surface_container_highest`
- Smooth transitions on all state changes (400ms cubic-bezier)

#### Volume Slider (`tokens/slider.css`)

- Trough: 10px tall, rounded, `@outline_variant` color
- Fill: `@secondary_fixed_dim`
- Thumb: 12px circle, `@primary` color

---

## Customization Tips

**Waybar timezone** — Edit `"timezone"` in `modules/clock.jsonc`.

**Change the browser in the distro drawer** — Edit `"on-click"` in `custom/browser` inside `modules/distro.jsonc`. Also update the color in `tokens/widget.css` under `#custom-browser` if desired.

**Regenerate the color palette** — Run `matugen image /path/to/wallpaper` (or `matugen color hex '#RRGGBB'`) and point the output to `tokens/colors.css`. All module colors will update automatically.

**Adjust the thermal zone** — Find your CPU thermal zone with:
```bash
paste <(cat /sys/class/thermal/thermal_zone*/type) \
      <(cat /sys/class/thermal/thermal_zone*/temp) \
| column -t
```
Then update `"thermal-zone"` in `modules/system.jsonc` to the correct index.

**Disable a drawer module** — Remove it from its group's `"modules"` array in `modules/groups.jsonc`.

**Add a new quick-launch icon** — Add a `custom/myapp` entry to `modules/distro.jsonc`, add it to the `group/distro-group` modules list in `groups.jsonc`, and add a color rule for it in `tokens/widget.css`.

**Change username paths** — Search and replace `/home/popopopo/` with your actual home directory in `hyprpaper.conf`.

**Add more workspaces** — Duplicate the `workspace = N, persistent:true` lines.

**Disable cmatrix autostart** — Remove or comment the `exec-once = kitty --class cmatrix_term -e cmatrix` line and its associated window rules.

**Adjust blur strength** — Increase `passes` for a stronger effect; decrease `size` for a tighter, less spread blur.

**Remap a key** — All keybinds follow the pattern:
```ini
bind = MODIFIER, KEY, action, [args]
```
