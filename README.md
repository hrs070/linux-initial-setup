# fedora-quick-setup
This repo is meant to help me setup fedora quickly


## Optimize DNF Package Manager
`sudo nano /etc/dnf/dnf.conf`
```
max_parallel_downloads=10
fastestmirror=True
```

## Update the system
-  `sudo dnf update`
-  `sudo dnf upgrade`

## Setup Terminal
[Link for steps](https://linuxcapable.com/how-to-install-zsh-on-fedora-linux/)

-  `sudo dnf install zsh`
-  `nano ~/.zshrc`
-  `git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"`
-  `zsh`
-  `chsh -s "$(command -v zsh)"`
-
  `
if [ -f ~/.shell_aliases ]; then
  source ~/.shell_aliases
fi
`
-  `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
-  `test -d ~/.oh-my-zsh && echo "Oh My Zsh installed"`
-  `git clone --depth 1 https://github.com/zsh-users/zsh-autosuggestions "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions"`
-  `git clone --depth 1 https://github.com/zsh-users/zsh-syntax-highlighting "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting"`
-  In ~/.zshrc, add this `plugins=(git zsh-autosuggestions zsh-syntax-highlighting)`
-  In ~/.zshrc, add this `ZSH_THEME="powerlevel10k/powerlevel10k"`
-  `source ~/.zshrc`
-  `p10k configure`

## Backup
-  `sudo dnf install timeshift`
-  Launch timeshift GUI
-  Select rsync
-  Select auto backup schedule or uncheck for manual
-  Exclude all user directories
-  Finish

## Enable rpm fusion repo
-  `sudo dnf install https://rpmfusion.org(rpm -E %fedora).noarch.rpm https://rpmfusion.org(rpm -E %fedora).noarch.rpm`

## Setup NVIDIA GPU
-  `sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda`
-  `sudo dnf install libva-nvidia-driver libva-utils`
-  `sudo dnf config-manager setopt cuda-fedora43-$(uname -m).exclude=nvidia-driver,nvidia-modprobe,nvidia-persistenced,nvidia-settings,nvidia-libXNVCtrl,nvidia-xconfig`
-  `sudo dnf clean all`
-  `sudo dnf install cuda-toolkit`
-  `echo 'export PATH=/usr/local/cuda-13.2/bin${PATH:+:${PATH}}' >> ~/.zshrc`
-  `echo 'export LD_LIBRARY_PATH=/usr/local/cuda-13.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.zshrc`
-  `source ~/.bashrc`
-  `nvcc --version`

## Install Multimedia Codecs
-  `sudo dnf swap ffmpeg-free ffmpeg --allowerasing`
-  `sudo dnf upgrade @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin`
-  `sudo dnf groupupdate sound-and-video`

## Fix Video preview
-  `sudo mkdir -p /etc/environment.d/`
-  `sudo nano /etc/environment.d/99-gtk-opengl.conf`
-  Add this `GDK_GL=gles`
-  Save file

## Keymap
-  `sudo dnf copr enable alternateved/keyd`
-  `sudo dnf install keyd`
-  `sudo dnf copr disable alternateved/keyd`
-  `sudo nano /etc/keyd/default.conf`
-  
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
-  `sudo keyd reload`

-  `gsettings set org.gnome.shell.keybindings toggle-overview "['<Alt>Up']"`
-  `gsettings set org.gnome.desktop.wm.keybindings panel-main-menu "['<Control>space']"`
-  `gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-left "['<Alt>Left']"`
-  `gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-right "['<Alt>Right']"`
-  `gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-left "['<Alt><Shift>Left']"`
-  `gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-right "['<Alt><Shift>Right']"`

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
-  Trayscale
-  Firewall configuration
-  Steam
-  Lutris
-  Bottles
-  Winboat
-  Sunshine/moonlight
-  Proton vpn
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

## SSD swapfile
1.  Allocate a fixed, unfragmented file on your SSD (e.g., 64 Gigabytes)
  `sudo fallocate -l 64G /swapfile`
2. Lock permissions so only the root system can read/write to it (Crucial for security)
  `sudo chmod 600 /swapfile`
3. Format the blank file into a Linux Swap filesystem
  `sudo mkswap /swapfile`
4. Instantly activate it alongside your existing memory layout
  `sudo swapon /swapfile`
5. Verify It Is Working
   `swapon --show`
6. Make It Permanent
   `sudo bash -c 'echo "/swapfile none swap defaults 0 0" >> /etc/fstab'`
7. Safely deactivate the swapfile (this moves any cached data back to RAM)
   `sudo swapoff /swapfile`
8. Delete the physical file from your SSD
   `sudo rm /swapfile`
9. Clean the registry file
    `sudo nano /etc/fstab`
10. Find the line and completely delete that single line that says
    `/swapfile none swap defaults 0 0`


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


## //TODO
-  gsconnect / kdeconnect
-  steam setup
-  gui for systemctl, firewall: gufw ?
-  rdp
