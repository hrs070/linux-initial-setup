# linux-initial-setup
This repo contains initial setup guide to make linux work and look like macos.


## Update the system
`sudo dnf update`
`sudo dnf upgrade`

## Backup
`sudo dnf install timeshift`
Launch timeshift GUI
Select rsync
Select auto backup schedule or uncheck for manual
Exclude all user directories
Finish

## Tailscale
//todo

## Enable rpm fusion repo
`sudo dnf install https://rpmfusion.org(rpm -E %fedora).noarch.rpm https://rpmfusion.org(rpm -E %fedora).noarch.rpm`

## Install Multimedia Codecs
`sudo dnf swap ffmpeg-free ffmpeg --allowerasing`
`sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin`
`sudo dnf groupupdate sound-and-video`

## Optimize DNF Package Manager
`sudo nano /etc/dnf/dnf.conf`
```
max_parallel_downloads=10
fastestmirror=True
```

## Keymap
`sudo dnf copr enable alternateved/keyd`
`sudo dnf install keyd`
`sudo dnf copr disable alternateved/keyd`
`sudo nano /etc/keyd/default.conf`

```
[ids]
*

[main]
# 1. First button: Map physical [Ctrl] to act as the hardware Left Alt key
leftcontrol = leftalt

# 2. Second button: Map physical [Win/Start] to act as macOS Option
leftmeta = layer(macos_opt)

# 3. Third button: Map physical [Alt] to act as macOS Command
leftalt = layer(macos_cmd)

[macos_cmd:c]
# Basic Editing (Physical Alt + Key)
c = C-c
v = C-v
x = C-x
z = C-z
y = C-y
a = C-a
f = C-f
s = C-s
o = C-o
n = C-n
w = C-w
q = C-q
t = C-t
r = C-r

# Cursor Navigation (Physical Alt + Arrows)
left = home
right = end
up = C-home
down = C-end
backspace = C-backspace

[macos_opt]
# Word-by-word jumping (Physical Win/Start + Arrows)
left = C-left
right = C-right
backspace = C-backspace
```

`sudo keyd reload`

`gsettings set org.gnome.shell.keybindings toggle-overview "['<Alt>Up']"`
`gsettings set org.gnome.desktop.wm.keybindings panel-main-menu "['<Control>space']"`
`gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-left "['<Alt>Left']"`
`gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-right "['<Alt>Right']"`
`gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-left "['<Alt><Shift>Left']"`
`gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-right "['<Alt><Shift>Right']"`

## Theme
-  `https://github.com/vinceliuice/WhiteSur-gtk-theme`
-  `https://github.com/vinceliuice/WhiteSur-icon-theme`

## Extensions
-  Extension manager - mathew jakeman
-  Refine
-  Tweaks
-  AppIndicator and KStatusNotifierItem Support
-  Blur my Shell
-  Caffeine
-  Copyous
-  Dash to Dock
-  Just Perfection
-  Logo Menu
-  Search Light
-  User Themes

## Apps
-  Flatseal
-  Tailscale
-  Steam
-  Lutris
-  Bottles
-  Winboat
-  Sunshine/moonlight
-  mozilla vpn
-  bit torrent
-  vlc
  

## ZRAM configure
