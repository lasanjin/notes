[⌫ back](../README.md)

# Drivers
### Battery
```
$ sudo add-apt-repository ppa:linrunner/tlp
$ sudo apt-get update
$ sudo apt-get install tlp tlp-rdw

Thinkpad specific:
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
$ sudo apt-get install xserver-xorg-input-synaptics
```

### MX Master 2S
```
$ apt remove blueberry -y && apt install blueman
```

### Codecs
```
$ sudo apt install mint-meta-codecs
```


<br/>


# Settings
### Add aliases in bashrc
```
if [ -f ~/.bash_aliases ]; then
    source ~/.bash_aliases
fi
```

### Tab auto-complete case-insensitive in Bash

If ~/.inputrc doesn't exist yet
```
$ touch ~/.inputrc
$ echo 'set completion-ignore-case On' >> ~/.inputrc
```
~/.inputrc is located in `/etc/inputrc`

### Ctrl + arrow in terminal
1. In ~/.bashrc
```
bind '"\e[1;5D" backward-word' 
bind '"\e[1;5C" forward-word'
```

2. In ~/.inputrc
```
"\e[1;5C": forward-word
"\e[1;5D": backward-word
"\e[5C": forward-word
"\e[5D": backward-word
"\e\e[C": forward-word
"\e\e[D": backward-word
```

### User run sudo
Alt 1
```
$ sudo adduser <username> sudo
```
Alt 2
```
$ sudo visudo 

Add <username> ALL=(ALL) NOPASSWD: ALL
```
[Read more](https://help.ubuntu.com/community/RootSudo#Allowing_other_users_to_run_sudo "ubuntu.com")

### Change hostname
```
$ sudo <text-editor> /etc/hostname
$ sudo <text-editor> /etc/hosts
```

### Save and store sessions
```
org -> gnome -> gnome-session -> auto-save-session
```

### Auto-complete sudo-apt get
```
$ sudo apt-get install bash-completion
```

### Increasing the amount of inotify watchers permanently 
```
$ echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
[Read more](https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers#the-technical-details "github.com")


<br/>


# Apps
### Spotify
```
$ sudo apt-get install spotify-client
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
$ sudo apt install peek
```

### Google Translate
Alt 1 
- Install
```
$ sudo apt-get install translate-shell
```
Alt 2 
- Download
```
$ wget git.io/trans$ sudo apt install peek
$ chmod +x trans
```
- How to use example
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


<br/>


# Others
 - Settings
   - Battery
   - Clock
     - `%V  %Y-%m-%d  %R`
   - Volume
   - Firewall
     - Mint default is Utf (Uncomplicated Firewall)
 - Applets
   - *Simple Memory Monitor*
   - *System Monitor*
 - Extensions
   - gTile
   - Transparent panels