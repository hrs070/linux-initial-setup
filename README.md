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

## SMB setup
  1. Returns your local network IP (e.g., 192.168.1.15) for home devices `ip route get 1.1.1.1 | awk '{print $7}'`
  2. ip route get 1.1.1.1 | awk '{print $7}' `tailscale ip -4`
  3. Returns your exact local Wi-Fi interface name if needed for troubleshooting `ip route show default | awk '{print $5}'`
  4. Creates the dedicated directory you want to share (Change "SharedFolder" to your liking) `mkdir -p /home/$USER/SharedFolder`
  5. Installs the clean, required Samba binaries `sudo dnf install samba samba-client -y`
  6. Create a dedicated system access group for sharing `sudo groupadd smbgroup`
  7. Add your primary account to the sharing group `sudo usermod -aG smbgroup $USER`
  8. Create a second account without local desktop login privileges (Change "shareuser" to your liking) `sudo useradd -M -s /sbin/nologin shareuser`
  9. Add the second account to the sharing group (Match the username chosen in the step above) `sudo usermod -aG smbgroup shareuser`
  10. Sets folder owner to you and group to smbgroup (Match your folder name) `sudo chown -R ${USER}:smbgroup /home/$USER/SharedFolder`
  11. Restricts access exclusively to you and group members (Match your folder name) `sudo chmod -R 2770 /home/$USER/SharedFolder`
  12. Grants parent directory traversal access so the Samba service can pass through the home root gate `sudo chmod o+x /home/${USER}`
  13. Adds a persistent rule to the central SELinux database allowing Samba to access this folder tree `sudo semanage fcontext -a -t samba_share_t "/home/${USER}/SharedFolder(/.*)?"`
  14. Actively applies the new security context label to the file system `sudo restorecon -R -v /home/${USER}/SharedFolder`
  15. Set the network connection password for your primary account `sudo smbpasswd -a $USER`
  16. Set the network connection password for the secondary account (Match your secondary username) `sudo smbpasswd -a shareuser`
  17. `sudo nano /etc/samba/smb.conf`
  18. 
```
[global]
    # Default Windows network grouping profile
    workgroup = WORKGROUP
    
    # Visual name displayed on network browsers
    server string = Fedora Samba Server
    
    # Forces credential-based authentication instead of anonymous guest access
    security = user
    
    # Required so Samba adapts to Tailscale's dynamic virtual interfaces
    bind interfaces only = no

    # Protocol enforcement for modern security (Blocks legacy, vulnerable SMBv1/v2)
    server min protocol = SMB3
    server max protocol = SMB3

    # Local IP & Tailscale IP security layer (Defense-in-Depth)
    # Adjust 192.168.1.0/24 if your local router uses a different subnet (e.g., 192.168.0.0/24)
    hosts allow = 127.0.0.1 192.168.1.0/24 100.64.0.0/10
    hosts deny = ALL

[SecureShare]
    # Description of the specific share
    comment = Isolated Secure Shared Folder
    
    # Absolute target path of the shared folder
    path = /home/yourusername/SharedFolder
    
    # Allows connected users to write and modify files
    writable = yes
    
    # Visible to network exploration tools on authorized devices
    browseable = yes
    
    # Strips anonymous, passwordless connections completely
    guest ok = no
    
    # Permits everyone in smbgroup and shareuser explicitly
    valid users = @smbgroup shareuser
    
    # New files receive read/write permissions for user and group only
    create mask = 0660
    
    # New folders receive read/write/execute permissions for user and group only
    directory mask = 0770
```
  19. Permanently opens SMB ports in Fedora's firewalld `sudo firewall-cmd --permanent --add-service=samba`
  20. Applies the firewall policy alterations instantly `sudo firewall-cmd --reload`
  21. Forces the Samba services to start now and run automatically at every boot `sudo systemctl enable --now smb nmb`
  22. Verifies the syntax integrity of your smb.conf file to check for configuration typos `testparm`
  23. Forces a structural restart of the servers to read any profile changes `sudo systemctl restart smb nmb`
  24. Check the active status of the server if troubleshooting execution `sudo systemctl status smb`

## Remote Login
  1. Enable from settings.
  2. Create a username,password
  3. On Client (Windows App), enter the IP of linux machine.
  4. When entering username, enter `.\{username}` and `password`
  5. If there is still issue, Right-click or hold the PC tile inside the Windows App and select Export. Save the .rdp configuration file anywhere on your Mac/device.
  6. Open that exported .rdp file using TextEdit or any text editor.
  7. Locate this exact line: `use redirection server name:i:0`
  8. Change the 0 to a 1 so it reads exactly like this: `use redirection server name:i:1`
  9. Save the file and close the text editor.

## SSH
  1. Enable SSH from settings.
  2. To login, use user@ip and password and add ssh config

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
