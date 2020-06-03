[⌫ back](../README.md)

## Drivers
### List installed packages
```
$ dpkg --get-selections
```

### [X Window server](https://wiki.debian.org/Xorg)
```
$ apt-cache search xserver-xorg
```


### Battery
```
$ sudo add-apt-repository ppa:linrunner/tlp
$ sudo apt-get update
$ sudo apt-get install tlp tlp-rdw
```

### Thinkpad
```
$ sudo apt-get install tp-smapi-dkms acpi-call-dkms
```
- tlp1.
  - Power saving
- tlp-rdw
  - *optional*
  - Radio Device Wizard
- tp-smapi-dkms
  - *ThinkPad* 
  - Provides battery charge thresholds, recalibration and specific status output in tlp-stat for older ThinkPads
- acpi-call-dkms
  - *ThinkPad* 
  - Provides battery charge thresholds and recalibration for newer ThinkPads (X220/T420 and later)

### Touchpad
- Thinkpad Synaptics touchpad drivers
```
$ sudo apt-get install xserver-xorg-input-synaptics-hwe-18.04
```

### MX Master
- Remove Mint default and install blueman
```
$ apt remove blueberry -y && apt install blueman
```

- Connect manually
```
$ bluetoothctl
Controller <MAC ADDR> <HOSTNAME> [default]
[bluetooth]# connect <MAC ADDR>
[bluetooth]# help
```

- Edit device settings
```
$ xinput --list-props <device id>
```

 - Set the minimum/maximum latency for the mouse 
```
$ sudo cat /var/lib/bluetooth/<MAC-addr-adapter>/<MAC-addr-mouse>/info
```


### Codecs
```
$ sudo apt install mint-meta-codecs
```


### Printer
1. Make sure CUPS daemon is running
```
$ lpstat -r
```

2. Start CUPS daemon
   - Default `cups.conf` file
     - cupsd post install copies the file `/usr/share/cups/cupsd.conf.default` to `/etc/cups/cupsd.conf`
```
$ sudo /etc/init.d/cups start
```


</br>


## Settings
### Add aliases & scripts to bashrc
```
if [ -f ~/.bash_aliases ]; then
    source ~/.bash_aliases
fi
```

### Case-insensitive in Bash

- If `~/.inputrc` exist
  - Located in `/etc/inputrc`
```
$ echo 'set completion-ignore-case On' >> ~/.inputrc
```

### `Ctrl+arrow` in terminal
-  In `~/.bashrc`
```
bind '"\e[1;5D" backward-word' 
bind '"\e[1;5C" forward-word'
```

- In `~/.inputrc`
```
"\e[1;5C": forward-word
"\e[1;5D": backward-word
"\e[5C": forward-word
"\e[5D": backward-word
"\e\e[C": forward-word
"\e\e[D": backward-word
```

### User run sudo
- Add user to sudo
```
$ sudo adduser <username> sudo
```

- Execute commands without sudo password
  1. Execute `$ sudo visudo `
  2. Add `<username> ALL=(ALL) NOPASSWD: ALL`
  - [Read more](https://help.ubuntu.com/community/RootSudo#Allowing_other_users_to_run_sudo "ubuntu.com")

### Change hostname
```
$ sudo <text-editor> /etc/hostname
$ sudo <text-editor> /etc/hosts
```

### Bash autocomplete
```
$ sudo apt-get install bash-completion
```

### Increasing the amount of inotify watchers permanently 
```
$ echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
[Read more](https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers#the-technical-details "github.com")


</br>


## Misc
 - Backing up and restoring your Cinnamon settings
   - dconf
     - `dconf dump /org/cinnamon/ > cinnamon_settings`
     - `dconf load /org/cinnamon/ < cinnamon_settings`
   - cp `~/.cinnamon/configs`
   - cp `~/.configs/...`
     - `caja`
     - `gtk-3.0`
     - `qt5ct`
     - `user-dirs.dirs`
 - Settings
   - `$ gsettings list-recursively`
     - Minimize titlebar
       - `$ gsettings set org.cinnamon.desktop.wm.preferences titlebar-font 'Noto Sans Bold 0'`
     - Dual monitor: do not switch workspace
       - `$ gsettings set org.gnome.mutter workspaces-only-on-primary true`
   - Battery
   - Clock
     - `  %V  •  %Y-%m-%d  •  %R:%S`
   - Volume
   - Firewall
   - Menu icon: `menu-none`
   - Default directories
     - `~/.config/user-dirs.dirs`
 - Applets
   - `.local/share/cinnamon/applets/`
   - `this._applet_label.set_style('color: rgba(255, 255, 255, 0.5); font-family: Liberation Mono Regular; font-size: 12px;');`
   - *Simple Memory Monitor*
   - *System Monitor*
 - Extensions
   - *Transparent panels*


</br>


## Apps
### [Spotify](https://www.spotify.com/nl/download/linux/)
```
$ curl -sS https://download.spotify.com/debian/pubkey.gpg | sudo apt-key add - 
$ echo "deb http://repository.spotify.com stable non-free" | sudo tee /etc/apt/sources.list.d/spotify.list
$ sudo apt-get update && sudo apt-get install spotify-client
```

### JSON processor terminal
```
$ sudo apt install jq
```

### Neofetch
```
$ sudo apt-get install neofetch
```
[Read more ](https://github.com/dylanaraps/neofetch "github.com")

### Peek
```
$ sudo add-apt-repository ppa:peek-developers/stable
$ sudo apt update
$ sudo apt-get install peek
```

### Google Translate
- Download
```
$ wget git.io/trans$ sudo apt install peek
$ chmod +x trans
```
- How to use
```
$ ./trans -b -j -lang english -target swedish
```

### Snap
```
$ sudo apt install snapd
```

### Tree
```
$ sudo apt install tree
```

### Dconf-Editor
```
$ sudo apt-get install dconf-tools
$ dconf-editor
```

### Kazam
```
$ sudo apt install kazam
```

### Thunderbird extensions
 - Grammar checker (Swedish)
 - Provider for Google Calendar

### Birdtray
```
sudo add-apt-repository ppa:linuxuprising/apps
sudo apt-get update
sudo apt install birdtray
```
Icons: `/usr/share/icons/Mint-Y/panel/`