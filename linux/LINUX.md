# Programming
### JDK
[Oracle-JDK vs OpenJDK](https://www.baeldung.com/oracle-jdk-vs-openjdk)

1. Remove OpenJDK
```
$ sudo apt-get --purge remove openjdk* && sudo apt-get autoremove && sudo apt-get clean
```

2. Download [here](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

3. Extract, create as root directory
```
$ sudo tar -zxvf jdk-* && sudo mkdir -p /opt/java && sudo mv jdk-* /opt/java
```

4. Install
```
$ sudo update-alternatives --install "/usr/bin/java" "java" "/opt/java/jdk<version>/bin/java" 1
```

5. Make system default
```
$ sudo update-alternatives --set java /opt/java/jdk<version>/bin/java
```

### Node & NPM
```
$ sudo apt-get install curl python-software-properties
$ curl -sL https://deb.nodesource.com/setup_11.x | sudo bash -
$ sudo apt-get install nodejs
```

### Apache Maven
Alt 1
```
$ sudo apt install maven
```
<br/>

Alt 2

1. Download [here](https://maven.apache.org/download.cgi)

2. Extract
```
$ sudo tar xf /tmp/apache-maven-*.tar.gz -C /opt
```

3. Setup enviromental variables
```
$ sudo <text-editor> /etc/profile.d/maven.sh
```

4. Configuration
```
export JAVA_HOME=/usr/bin/java
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
```

5. Make the script executable
```
$ sudo chmod +x /etc/profile.d/maven.sh && . /.maven.sh
```



### Git
1. Save username
```
$ git config --global user.name <username>
$ git config --global user.email <email>
```

2. Save login
```
$ git config credential.helper store
$ git config --list
```

3. Enable credential caching for 7200sec (2h)
```
$ git config --global credential.helper 'cache --timeout 7200'
```

### Intellij
- Default directory
```
File -> Settings -> Appearance & Behavior -> System Settings -> Default directory
```

- Set JDK
```
File -> Project Structure -> SDKs
```

- Switch JDK
```
Help -> Find Action -> Swtich Boot JDK
```

### VS Code
- List all extensions
```
$ code --list-extensions | xargs -L 1 echo code --install-extension
```

- Install current extensions
```
code --install-extension 2gua.rainbow-brackets
code --install-extension dbaeumer.vscode-eslint
code --install-extension dsznajder.es7-react-js-snippets
code --install-extension foxundermoon.shell-format
code --install-extension JuanBlanco.solidity
code --install-extension ms-python.python
code --install-extension shd101wyy.markdown-preview-enhanced
code --install-extension yzhang.markdown-all-in-one
```

**OBS** shell-format requires golang and shfmt installed
```
$ sudo snap install shfmt
$ sudo snap install --classic go
```
**OBS** requires [snap](#Snap) to be installed


<br/>
<br/>


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
- Thinkpad Synaptics touchpad drivers
```
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

<br/>

## Applications
### Spotify
```
$ sudo apt-get install spotify-client
```

### JSON processor terminal
```
$ sudo apt install jq
```

<br/>

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


<br/>
<br/>


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
<div style="text-align:center"><img src="fp.png" width="350"></div>

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


<br/>
<br/>


# Fun & useful
`$ cal` *Calendar*\
`$ rev <input>` *Reverse input* \
`$ inxi -Fxz` *System info*\
`$ sudo service /etc/init.d/dns-clean start` *Flush DNS Cache*\
`$ locale -a` *List the installed locales which will work for the LC_ALL variable*\
`LC_TIME=sv_SE.utf-8 date '+%A'` *Get today in Swedish*\
`sudo dpkg -r <package-name>` *Install package*

<br/>

## Shortcuts
`Ctrl`+`Alt`+`Arrow` *Move to other desktop*\
`Super`+`Shift`+`Alt`+`Arrow` *Move window to other desktop*\
`Super`+`Shift`+`Arrow` *Move window to other monitor*

<br/>

### Snap
```
$ sudo apt install snapd
```

### Change hostname
```
$ sudo <text-editor> /etc/hostname
$ sudo <text-editor> /etc/hosts
```

### Tree
```
$ apt install tree
```

### Auto-complete sudo-apt get
```
$ sudo apt-get install bash-completion
```

### Neofetch
```
$ sudo apt-get install neofetch
```
[Read more](https://github.com/dylanaraps/neofetch)