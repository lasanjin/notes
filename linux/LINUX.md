# Programming
### Node & NPM
```
$ sudo apt-get install curl python-software-properties
$ curl -sL https://deb.nodesource.com/setup_11.x | sudo bash -
$ sudo apt-get install nodejs
```

### Git
Save username
```
$ git config --global user.name <username>
$ git config --global user.email <email>
```

Save login
```
$ git config credential.helper store
$ git config --list
```

Enable credential caching for 7200sec (2h)
```
$ git config --global credential.helper 'cache --timeout 7200'
```


# TODOs after installation
## Drivers

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
```
Thinkpad Synaptics touchpad drivers:

$ xserver-xorg-input-synaptics
```

### MX Master 2S
```
$ apt remove blueberry -y && apt install blueman
```

### Codecs
```
$ sudo apt install mint-meta-codecs
```


## Applications
### Spotify
```
$ sudo apt-get install spotify-client
```

### JSON processor terminal
```
$ sudo apt install jq
```


## Additional settings
 - Activate Redshift
 - Show battery percentage
 - Setup volume
 - Enable Firewall
   - Mint default is Utf (Uncomplicated Firewall)
 - Applets
   - *Simple Memory Monitor*
   - *System Monitor*

### Put all your additions into a separate file
```
if [ -f ~/.bash_aliases ]; then
    source ~/.bash_aliases
fi
```


# Misc
### Allowing user to run sudo
Alt 1
```
$ sudo adduser <username> sudo
```
Alt 2
```
$ sudo visudo 

Add <username> ALL=(ALL) NOPASSWD: ALL
```
[Read more](https://help.ubuntu.com/community/RootSudo#Allowing_other_users_to_run_sudo)

### Increasing the amount of inotify watchers permanently 
```
$ echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
[Read more](https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers#the-technical-details)

### chmod
<img src="fp.png" width="300"/>

```
rwx rwx rwx = 111 111 111
rw- rw- rw- = 110 110 110
rwx --- --- = 111 000 000
```
```
rwx = 111 in binary = 7
rw- = 110 in binary = 6
r-x = 101 in binary = 5
r-- = 100 in binary = 4
```

### Save and restore session
```
$ sudo apt-get install dconf-tools
$ dconf-editor
```
org -> gnome -> gnome-session -> auto-save-session




# Fun & useful
`$ cal` *Calendar*\
`$ rev <input>` *Reverse input* \
`$ inxi -Fxz` *System info*\
`$ sudo service /etc/init.d/dns-clean start` *Flush DNS Cache*\
`$ locale -a` *List the installed locales which will work for the LC_ALL variable*\
`LC_TIME=sv_SE.utf-8 date '+%A'` *Get today in Swedish*

### Change hostname
```
$ sudo nano /etc/hostname
$ sudo nano /etc/hosts
```

### Tree
```
$ apt install tree
```