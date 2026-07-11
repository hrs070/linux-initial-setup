# linux-initial-setup
This repo contains initial setup guide to make linux work and look like macos.


## Optimize DNF Package Manager
`sudo nano /etc/dnf/dnf.conf`
```
max_parallel_downloads=10
fastestmirror=True
```

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

## Setup GPU
`sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda`
`sudo dnf install libva-nvidia-driver libva-utils`
`sudo dnf config-manager setopt cuda-fedora43-$(uname -m).exclude=nvidia-driver,nvidia-modprobe,nvidia-persistenced,nvidia-settings,nvidia-libXNVCtrl,nvidia-xconfig`
`sudo dnf config-manager setopt cuda-fedora43-$(uname -m).exclude=nvidia-driver,nvidia-modprobe,nvidia-persistenced,nvidia-settings,nvidia-libXNVCtrl,nvidia-xconfig`
`sudo dnf clean all`
`sudo dnf install cuda-toolkit`
`echo 'export PATH=/usr/local/cuda-13.2/bin${PATH:+:${PATH}}' >> ~/.bashrc`
`echo 'export LD_LIBRARY_PATH=/usr/local/cuda-13.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc`
`source ~/.bashrc`
`nvcc --version`

## Install Multimedia Codecs
`sudo dnf swap ffmpeg-free ffmpeg --allowerasing`
`sudo dnf upgrade @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin`
`sudo dnf groupupdate sound-and-video`

# Setup Terminal
[Link for steps](https://linuxcapable.com/how-to-install-zsh-on-fedora-linux/)

`sudo dnf install`
`nano ~/.zshrc`
`git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"`
`zsh`
`chsh -s "$(command -v zsh)"`
`
if [ -f ~/.shell_aliases ]; then
  source ~/.shell_aliases
fi
`
`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
`test -d ~/.oh-my-zsh && echo "Oh My Zsh installed"`
`git clone --depth 1 https://github.com/zsh-users/zsh-autosuggestions "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions"`
`git clone --depth 1 https://github.com/zsh-users/zsh-syntax-highlighting "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting"`
In ~/.zshrc, add this `plugins=(git zsh-autosuggestions zsh-syntax-highlighting)`
`ZSH_THEME="powerlevel10k/powerlevel10k"`
`source ~/.zshrc`
`p10k configure`
`echo 'export PATH=/usr/local/cuda-13.2/bin${PATH:+:${PATH}}' >> ~/.zshrc`
`echo 'export LD_LIBRARY_PATH=/usr/local/cuda-13.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.zshrc`


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

## Fix Video preview
`sudo mkdir -p /etc/environment.d/`
`sudo nano /etc/environment.d/99-gtk-opengl.conf`
Add this `GDK_GL=gles`
Save file

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

## AI Tools and repos
-  Video2QS

## ZRAM configure
-  `swapon --show`
-  `zramctl`
-  `free -h`
-  `sudo swapoff /dev/zram0`  # Instantly stop the active zRAM swap partition in memory
-  `sudo systemctl stop systemd-zram-setup@zram0.service` # Stop and disable the background service that creates it
-  `sudo systemctl start systemd-zram-setup@zram0.service` # Start the generator service to rebuild the drive
-  `sudo nano /usr/lib/systemd/zram-generator.conf`
-
```
   [zram0]
# TRY VARIATION A: Set it to 100% of your RAM (32GB)
zram-size = ram

# TRY VARIATION B: Hardcode it to a specific size (e.g., 4G, 8G, 16G)
# zram-size = 16384

# TRY VARIATION C: Match your current stock setup (8GB)
# zram-size = 8192

# THE ALGORITHM DIAL: Test 'zstd' (tight compression) vs 'lzo-rle' (raw speed)
compression-algorithm = zstd
```
- `sudo systemctl daemon-reload` # Force systemd to read the updated text file
- `sudo systemctl restart systemd-zram-setup@zram0.service` # Restart the service to recreate the virtual drive with your new sizes



## systemctl commands
-  `systemctl status bluetooth` (any service name)
-  `sudo systemctl stop bluetooth`    # Kills the bluetooth service immediately
-  `sudo systemctl start bluetooth`   # Launches it back up right now
-  `sudo systemctl restart bluetooth` # Hard stop and fresh reboot of the service
-  `sudo systemctl disable bluetooth`  # Prevents it from launching at next boot
-  `sudo systemctl enable bluetooth`   # Tells systemd to auto-launch it at next boot
-  `systemctl list-unit-files --type=service --state=enabled`
-  `systemctl list-units --type=service --state=running`
-  `systemctl --failed`
-  `systemctl list-units --type=service --state=not-found`
