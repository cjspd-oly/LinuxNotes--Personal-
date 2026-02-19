# Portable OS
- Freshly install Fedora OS on External Disk (Luckily, Faced no issue of any bootloader corruption)
- On first login, pick same username
- Install all the required packages
- Using rsync reply `/home/pd` folder from primary system disk content.
- everything should be back to normal

## Commands to copy/update `/home/pd/` on portable os
```bash
sudo mount /dev/nvme0n1p8 /mnt/old/

sudo rsync -aAXv /mnt/old/home/pd/ /home/pd/

sudo chown -R pd:pd /home/pd

sudo restorecon -R -v /home/pd
```

# Issues - one time
## Setup dnf repo + dnf & flatpak packages
```bash
# MUST
sudo dnf install fedora-workstation-repositories -y

sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

sudo dnf install dnf-plugins-core -y

# GNOME (MUST)
sudo dnf install gnome-extensions-app -y
flatpak install flathub com.mattjakeman.ExtensionManager

# DDCUTIL (MUST)
sudo dnf copr enable rockowitz/ddcutil
sudo dnf install ddcutil -y

# MUST
sudo dnf install dnfdragora -y
sudo dnf install gparted -y
sudo dnf install btop -y
sudo dnf install ark -y
sudo dnf install vlc -y
sudo dnf install video-downloader -y
flatpak install flathub org.bluesabre.MenuLibre
flatpak install flathub io.github.flattool.Warehouse
flatpak install flathub it.mijorus.gearlever # Gear Level - Appimage Utility
flatpak install flathub be.alexandervanhee.gradia # Gradia
flatpak install flathub io.missioncenter.MissionCenter
flatpak install flathub io.github.peazip.PeaZip
flatpak install flathub org.telegram.desktop

# flatpak install flathub org.gnome.World.PikaBackup # Be Careful with Backup Data & Location

# BROWSERS
sudo dnf config-manager addrepo --from-repofile=https://brave-browser-rpm-release.s3.brave.com/brave-browser.repo
sudo dnf install brave-browser -y

sudo dnf config-manager setopt google-chrome.enabled=1

sudo dnf install google-chrome-stable -y

sudo dnf install chromium -y

# DISCORD
sudo dnf install discord -y

# GEDIT
sudo dnf install gedit -y

# VS CODE
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc &&
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
dnf check-update && sudo dnf install code -y

# GITHUB
flatpak install flathub io.github.shiftey.Desktop

# SUBLIME TEXT
sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg
sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

# QBITTORRENT
sudo dnf install qbittorrent -y

# DOWNLOAD MANUALLY
# Proton Authenticator: https://proton.me/authenticator
# Bleachbit: https://www.bleachbit.org/download/linux
```

## Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## C++ Boost library
```bash
sudo dnf install gcc-c++ python3 bzip2-devel zlib-devel libicu-devel
```
## Pihole
```bash
1) Switch to root user, set SELinux on permissive and reboot:
sudo -i
sed -i 's/^SELINUX=enforcing/SELINUX=permissive/g' /etc/selinux/config
reboot

2) Switch to root user and install Pi-Hole:
sudo -i
curl -sSL https://install.pi-hole.net | bash

3) Set a blank password
sudo pihole setpassword # leave it blank

4) copy /etc/pihole/ from old disk to new disk
sudo rm -rf /etc/pihole/*
sudo cp -r /mnt/old/etc/pihole/ /etc/pihole/
```

## NVIDIA GPU - prefer to not install unless you reach a static system temporarily

# Issues - For Each time i copy home
## Chrome - will freeze a lot
```bash
chrome-browser --disable-gpu # edit .desktop file
```
## pyenv
```bash
rm -rf ~/.pyenv/
curl -fsSL https://pyenv.run | bash
```