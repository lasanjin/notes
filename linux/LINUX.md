[⌫ back](../README.md)

## Random commands
`$ cal` *CLI calendar*\
`$ rev <input>` *Reverse input* \
`$ inxi -Fxz` *System info*\
`$ sudo service /etc/init.d/dns-clean start` *Flush DNS Cache*\
`$ locale -a` *List the installed locales which will work for the LC_ALL variable*\
`$ LC_TIME=sv_SE.utf-8 date '+%A'` *Get today in Swedish*\
`$ sudo dpkg -r <package>` *Install debian package*\
`$ sudo apt-add-repository --remove <ppa>` *Remove ppa*\
`$ tput cols` *Number of columns*\
`$ tput lines` *Number of rows*\
`$ xrandr -qt` *State of system display*\
`$ xrandr --output <monitor> --brightness 0.5` *Adjust brightness*\
`$ gsettings get/set org.blueman.transfer shared-path` *get/set path for directory for incoming files*\
`$ upower -i upower -e | grep 'BAT'` *List battery info*\
`$ tail -n 1 <file>` *Check 1 line at the end of file*\
`$ lsb_release -a` *Debian/Ubuntu version*\
`$ lscpu` *CPU info*\
`$ ulimit -n` *How many files can be open at once for each CPU core*\
`$ dpkg --list | grep compiler` *List compilers*\
`$ cat $(ps aux | grep '[/]var/lib/NetworkManager/\S*.lease') | grep dhcp-server-identifier` *DHCP address*\
`$ echo "example.com" | cut -f1 -d"."` *Cut everything after '.'*\
`$ ps aux | grep -ie <process name> | awk '{print $2}'` *list pid of process*\
`$ xinput --list --short` *List connected devices*\
`$ sudo pkill -f <pattern>` *Kill all occurences of pattern*\
`$ mkdir -p folder/{1..100}/{sub1,sub2,sub3}` *Creates combinatorial folder nest. Works with ranges {1..100}*\
`$ disown -a` *Exit terminal & leave processes running*\
`if ls -U <files> &>/dev/null; then` *files do exist: ls returns non-zero when the files do not exist* \
`$ sudo lshw -class disk` *list all disks on the system* \
`$ ls -l /lib/modules/$(uname -r)/` *List Linux modules* \
`$ lsmod` *List loaded Linux modules* \
`$ cat /proc/acpi/button/lid/LID/state` *Check if lid is open/closed* \
`$ sudo rtcwake -m no -l -t "$(date --date='60 seconds' +%s)"` *Schedule system wakeup 1min from now* \
`$ hostnamectl` *Check Linux version*

</br>

## SSH
### Tunnel
```
$ ssh -L <local port>:<remote host IP>:<remote host port> root@<host> -N
```

### Connect to VM
1. Generate SSH key-pair
```
$ sudo ssh-keygen -t rsa
```

1. Get public-key
```
$ cat ~/.ssh/id_rsa.pub
```
   
1. Connect
```
$ ssh username@hostname
```

### Commands
Copy files
```
$ scp -r <file> username@ip:~/.
```

### Keygen
 - [ssh-keygen man page](https://linux.die.net/man/1/ssh-keygen)

### Troubleshooting
 -  "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED"
```
$ ssh-keygen -R <host>
```
`-R <host>` removes all keys belonging to hostname from a known_hosts file. This option is useful to delete hashed hosts (see the -H option).

</br>

## Encryption
### GPG
Generate PGP key-pair
```
$ sudo gpg --gen-key
```

Export public key
```
$ sudo gpg --armor --export <email@address.com> > public-key.asc
```

Import public key
```
$ sudo gpg --import <public-key-file>
```

Encrypt message
```
$ sudo gpg --encrypt --armor --recipient <email@address.com> <message-file>
```

Decrypt message
```
$ sudo gpg <encrypted-message.asc>
```

List keys
```
$ sudo gpg --list-keys
```

Delete key
```
$ sudo gpg --delete-key <pub>
```

Change password to key
```
$ sudo gpg --edit-key <key-ID>
$ gpg> passwd
$ gpg> save
```

</br>

## WC
```
$ wc filename
```
Outputs
- number of lines
- number of words
- number of bytes

### Commands
`wc -l` *Number of lines* \
`wc -w` *Number of words* \
`wc -c` *Count of bytes* \
`wc -m` *Print the count of characters* \
`wc -L` *Prints only the length of the longest line* 

</br>

## sed
- Remove everything except digits 
```
$ sed 's/[^0-9]*//g' <file>
```

 - Cut everything before last occurence of pattern
```
$ sed 's/<pattern>.*//'
```

- Read part of file
```
$ sed -n '<from>,<to>p' <file>
```

 - Delete everything before last pattern
```
$ sed 's@.*<pattern>@@'
```

</br>

## awk
- Ad number of line on each row of document
```
$ awk '{printf("%10d %s\n", NR, $0)}' <document>`
```
- Cut everything before second occurence of pattern in file
```
$ awk -F '<pattern>' '{print $2}' <file>
```

</br>

## grep
### Find word in file(s)
```
$ grep -rnwl '<path>' -e '<pattern>'
```
- `r` is recursive,
- `n` is line number
- `w` is whole word
- `l` output just the file name of matching files
- `--include=\*.{c,h}` will search through files with `.c` or `.h` extensions
- `--exclude=*.o` will exclude searching all files with `.o` extension:
- `--exclude-dir={dir1,dir2,*.dst}` will exclude all directories dir1, dir2 and all them matching `.dst`

### OR
```
$ grep "<pattern1>\|<pattern2>" FILE
$ grep -e <pattern1> -e <pattern2> FILE
```

### AND
```
$ grep -E '<pattern1>.*<pattern2>' FILE
```

</br>

## find
- Save names of directories to file
```
$ find . -type d -printf '%f\n' > <file>
```

- Copy all occurences of pattern to path
```
$ find . -iname "*<pattern>*" -exec cp {} <path> \;
```

 - Delete all occurences of pattern
```
$ find -iname "*<pattern>*" -delete
```

</br>

## regex
`^[0-9]+$` *Unsigned numbers* \
`^[+-]?[0-9]+([.][0-9]+)?$` *Number with signs* \
`^[0-9]+([.][0-9]+)?$` *Float numbers*

</br>

## sort 
 - sort files in order
   - n: sorts numerically
   - t: field separator '-'
   - k: sort on second field (the numbers after the first '-') 
```
$ ls data-* | sort -n -t - -k 2
```

</br>

## Nano
### Commands
`Shift+Insert` *Paste text copied outside of nano* 

</br>

## [udev](https://en.wikipedia.org/wiki/Udev)
 - `udev` is a device manager for the Linux kernel, primarily managing device nodes in the /dev directory.  It also handles all user space events raised when hardware devices are added into the system or removed from it, including firmware loading as required by certain devices.
 - `udevadm` is udev management tool.


### Commands
 - List system rules
```
$ ls /lib/udev/rules.d/
```

 - Reload rules
```
$ sudo udevadm control --reload-rules
```

 - List `card0` device attributes
```
$ udevadm info -a -p $(udevadm info -q path -n /dev/dri/card0)
```

 - Query `AC` device info stored in the udev database and properties of a device from its sysfs representation
```
$ udevadm info /sys/class/power_supply/AC
```

 - List `AC` device attributes
```
$ udevadm info -a -p $(udevadm info -q path /sys/class/power_supply/AC)
```


### rules
 1. List connected `usb` device attributes and properties
```
$ udevadm info -q all -n /dev/sdb | grep -E -i -w '.*VENDOR_ID.*|.*MODEL_ID.*'
```

 2. Create rule and place in `/etc/udev/rules.d/`
```
ACTION=="add", ATTRS{idVendor}=="<VENDOR_ID>", ATTRS{idProduct}=="<MODEL_ID", RUN+="<path>/script"
```

 3. Create `script`
```
#!/bin/bash

<do something>
```

 1. Make is executable and place in `<path>`
```
$ chmod +x script
```

 5. Reload rules
```
$ udevadm control --reload-rules
```


</br>


## VirtualBox 6.0 on Linux Mint 19
1. Import public key
```
$ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
```

2. Add repository
```
$ echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian bionic contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
```

3. Update apt cache & install
```
$ sudo apt-get update && sudo apt-get install -y virtualbox-6.0
```


</br>


## Ghostscript
 - Merge pdf
   - Preserve hyperlinks ( [Markdown + CSS $\rightarrow$ PDF](../code/TOOLS#Markdown "Markdown") ) 
     - `a:link { text-decoration: none; }`
```
$ ghostscript \
      -sDEVICE=pdfwrite \
      -dPrinted=false \
      -dNOPAUSE -dQUIET -dBATCH \
      -sOutputFile=combined.pdf \
      file1.pdf \
      file2.pdf
```


</br>


## Misc
### Create RAM disk
1. Create directory for disk
```
$ mkdir -p <directory>
```

2. Mount the RAM disk
```
$ mount -t tmpfs tmpfs <directory> -o size=8192M
```

### Permissions
<img src="fp.png" width="350">

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

 - Change the user owning file or directory
```
$ sudo chown <username>: <directory>
```

 - Add write permission to username user
```
$ sudo chmod u+w <files>
```



