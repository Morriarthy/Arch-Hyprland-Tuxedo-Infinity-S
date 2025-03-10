# Arch-Hyprland-Tuxedo-Infinity-S
This will become an Installation Script for Arch Linux to install on a Tuxedo Infinity-S Gen-1 with Nvidia RTX 4060 Ti


Packages which need to be installed after installing Arch Linux.

- git // to download YAY
- yay // to install programms from the AUR
- linux-headers // for some reason it is no longer installed but needed since the beginning of 2023
- linux-firmware
- nvidia-dkms
- xdg-desktop-portal-hyprland //for Cosmic-Files
- xdg-desktop-portal-cosmic
- xdg-user-dirs
- qt5-wayland
- qt5ct
- libva
- hyprland
- xwaylandvideobridge // for screensharing (see tules.conf in hypr)
- hyprpaper // for wallpapers
- hyprutils
- hyprlang
- hyprcursor
- polkit (-hyprland)
- waybar
- wofi
- gtklock // to lock the screen, I am currently testing also hypridle options 
- bluez //for Bluetooth support
- cups // for printer support
- networkmanager // to connect easily to different networks
- pavucontrol // gives you easy control over all audio devices
- kitty // my prefered terminal
### needed for Screenshots
- grim
- slurp
- swappy
### additionally useful packages
- ntfs-g3 // well, you know usb-drives with ntfs-format
- yazi // filemanager in the terminal
- stow // to make easily link dotfiles from a specific folder to ~/.config/
- cosmic-files / graphical filemanager
- btop
- fastfetch
- vim //my editor of choise
- bat // like cat bat better
- tmux // sometimes it is usefull to use a terminal multiplexer

# More will follow while I am tinkering around and installing.
> [!IMPORTANT]
> Install script will auto-detect nvidia card and install nvidia-dkms drivers for your kernel.
> So please ensure that your Nvidia card supports [dkms](https://wiki.archlinux.org/title/NVIDIA) drivers and hyprland.

# How to get Hyprland to possibly work on Nvidia
Install the `nvidia-dkms` driver and add it to your initramfs & kernel parameters.
For people using systemd-boot you can do this adding `nvidia_drm.modeset=1` to the end of `/boot/loader/entries/arch.conf`. For people using grub you can do this by adding `nvidia_drm.modeset=1` to the end of `GRUB_CMDLINE_LINUX_DEFAULT=` in `/etc/default/grub`, then run `# grub-mkconfig -o /boot/grub/grub.cfg` For others check out kernel parameters and how to add `nvidia_drm.modeset=1` to your specific bootloader.

in `/etc/mkinitcpio.conf` add `nvidia nvidia_modeset nvidia_uvm nvidia_drm` to your MODULES

`run # mkinitcpio --config /etc/mkinitcpio.conf --generate /boot/initramfs-custom.img` (make sure you have the linux-headers package installed first)

add a new line to /etc/modprobe.d/nvidia.conf (make it if it does not exist) and add the line options nvidia-drm modeset=1

More information is available here: [Source - Arch Wiki](https://wiki.archlinux.org/title/NVIDIA#DRM_kernel_mode_setting)

Export these variables in your hyprland config:

`env = LIBVA_DRIVER_NAME,nvidia)`
`env = XDG_SESSION_TYPE,wayland`
`env = GBM_BACKEND,nvidia-drm`
`env = __GLX_VENDOR_LIBRARY_NAME,nvidia`
`env = WLR_NO_HARDWARE_CURSORS,1`

Install `qt5-wayland`, `qt5ct` and `libva`. Additionally `libva-nvidia-driver-git` (AUR) to fix crashes in some Electron-based applications, such as Unity Hub.

Reboot your computer

Launch Hyprland.

