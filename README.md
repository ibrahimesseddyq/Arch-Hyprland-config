# Arch Hyprland Config

[![Hyprland](https://img.shields.io/badge/desktop-Hyprland-blue?logo=linux)](https://github.com/hyprwm/Hyprland)

This is my custom configuration for [Hyprland](https://github.com/hyprwm/Hyprland) — a dynamic tiling Wayland compositor — on Arch Linux.  
The setup focuses on workflow efficiency, keyboard navigation, and a minimalist look.

---

##  Features

- Keyboard-driven operation, minimal mouse usage required  
- Custom workspace and window management
- Handy keybindings for utilities, apps, and system controls
- Automatically starts wallpaper daemon and status bar

---

##  Installation

```sh
git clone https://github.com/ibrahimesseddyq/Arch-Hyprland-config.git
mv ~/.config/hypr ~/.config/hypr.bak # Optional: backup your existing config
cp -r Arch-Hyprland-config/hypr ~/.config/
```
Launch or reload Hyprland for changes to take effect.

---

## Keyboard Shortcuts

All keybindings are set in [hypr/hyprland.conf](./hypr/hyprland.conf):

| Shortcut                                      | Action                       |
|-----------------------------------------------|------------------------------|
| <kbd>SUPER</kbd> + <kbd>Q</kbd>              | Open terminal (`kitty`)      |
| <kbd>SUPER</kbd> + <kbd>C</kbd>              | Close active window          |
| <kbd>SUPER</kbd> + <kbd>M</kbd>              | Shutdown/exit Hyprland       |
| <kbd>SUPER</kbd> + <kbd>E</kbd>              | Open file manager (`nemo`)   |
| <kbd>SUPER</kbd> + <kbd>SPACE</kbd>          | Toggle floating mode         |
| <kbd>SUPER</kbd> + <kbd>V</kbd>              | Open VS Code                 |
| <kbd>SUPER</kbd> + <kbd>F</kbd>              | Fullscreen window            |
| <kbd>SUPER</kbd> + <kbd>R</kbd>              | App launcher (`wofi`)        |
| <kbd>SUPER</kbd> + <kbd>B</kbd>              | Open Brave browser           |
| <kbd>SUPER</kbd> + <kbd>←/→/↑/↓</kbd>        | Change window focus direction|
| <kbd>SUPER</kbd> + <kbd>SHIFT</kbd> + <kbd>→/↓</kbd> | Next workspace      |
| <kbd>SUPER</kbd> + <kbd>SHIFT</kbd> + <kbd>←/↑</kbd> | Previous workspace  |
| <kbd>SUPER</kbd> + <kbd>CTRL</kbd> + <kbd>↑</kbd>    | Volume up (+5%)     |
| <kbd>SUPER</kbd> + <kbd>CTRL</kbd> + <kbd>↓</kbd>    | Volume down (-5%)   |
| <kbd>SHIFT</kbd> + <kbd>C</kbd>                      | Run `clear` command |

**Notes:**  
- <kbd>SUPER</kbd> is typically your <kbd>Windows</kbd> key.
- All programs (e.g., terminal, file manager, launcher) are configurable in the `.conf`.


---

## 🖼️ Wallpaper Setup

Set your wallpaper in `hypr/hyprpaper.conf`:
```
wallpaper {
    monitor = 
    path = /home/popopopo/wallpapers/wall3.jpg
    fit_mode = cover
}
```


- [Waybar](https://github.com/Alexays/Waybar) for the status bar
- [wofi](https://hg.sr.ht/~scoopta/wofi) for the app launcher

---

Enjoy your personalized Arch Hyprland desktop!
