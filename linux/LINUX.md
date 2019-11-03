[⌫ back](../README.md)

# Commands
`$ cal` *CLI calendar*\
`$ rev <input>` *Reverse input* \
`$ inxi -Fxz` *System info*\
`$ sudo service /etc/init.d/dns-clean start` *Flush DNS Cache*\
`$ locale -a` *List the installed locales which will work for the LC_ALL variable*\
`$ LC_TIME=sv_SE.utf-8 date '+%A'` *Get today in Swedish*\
`$ sudo dpkg -r <package>` *Install debian package*\
`$ tput cols` *Number of columns*\
`$ tput lines` *Number of rows*\
`$ xrandr -q` *State of system display*\
`$ xrandr --output <monitor> --brightness 0.5` *Adjust brightness*\
`$ gsettings get org.blueman.transfer shared-path` *get path for directory for incoming files*\
`$ gsettings set org.blueman.transfer shared-path '<path>'` *edit path for incoming files*\
`$ upower -i upower -e | grep 'BAT'` *List battery info*\
`$ tail -n 1 <file>` *Check 1 line of the end of file*\
`$ lsb_release -a` *Debian/Ubuntu version*\
`$ lscpu` *CPU info*\
`$ ulimit -n` *How many files can be open at once for each CPU core*\
`$ dpkg --list | grep compiler` *List compilers*\
`$ find . -mindepth 2 -maxdepth 2 -type d -printf '%f\n' > <filename>` *Save names of subdirectories to file*\
`$  cat $(ps aux | grep '[/]var/lib/NetworkManager/\S*.lease') | grep dhcp-server-identifier` *DHCP address*\
`$ awk -F 'example' '{print $2}' h1_to_h2.txt | sed 's/[^0-9]*//g' > h1_to_h2_clean.txt` *Cut everything after 'example' and filter only numbers*


<br/>


# Shortcuts
`Ctrl`+`Alt`+`Arrow` *Move to other desktop*\
`Super`+`Shift`+`Alt`+`Arrow` *Move window to other desktop*\
`Super`+`Shift`+`Arrow` *Move window to other monitor*\
`Shift`+`PrtCn` *Select area to grab for screenshot*

More shortcuts [here](https://community.linuxmint.com/tutorial/view/244)


<br/>


# SSH
## Connect to VM
1. Generate SSH key-pair
```
$ sudo ssh-keygen -t rsa
```

2. Save public-key to VM instance
```
$ cat ~/.ssh/id_rsa.pub
```
   
3. Connect to VM
```
$ ssh username@hostname
```

## Commands
Copy files to VM
```
$ scp -r <file> username@ip:~/.
```

## Keygen
Read more [here](https://linux.die.net/man/1/ssh-keygen)

## Warnings
 -  "WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED"
```
$ ssh-keygen -R <host>
```
`-R <host>` removes all keys belonging to hostname from a known_hosts file. This option is useful to delete hashed hosts (see the -H option).


<br/>


# Crypto
## GPG
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

<br/>


# WC
Syntax 
```
$ wc [options] filename
```
No parameters

```
$ wc filename
```
Outputs
- number of lines
- number of words
- number of bytes

## Args
`wc -l` *Number of lines* \
`wc -w` *Number of words* \
`wc -c` *Count of bytes* \
`wc -m` *Print the count of characters* \
`wc -L` *Prints only the length of the longest line* \


<br/>


# Nano
## Commands
`Shift` + `Insert` *Paste text copied outside of nano* \


<br/>


# sed
- Remove All Except Digits (Numbers) From Input
```
$ sed 's/[^0-9]*//g' input.txt > output.txt
$ sed -i 's/[^0-9]*//g' input.txt
```


</br>


# awk
- Ad number of line on each row of document
```
$ awk '{printf("%10d %s\n", NR, $0)}' <document>`
```


</br>


# grep
- Find word in file(s)
```
$ grep -rnwl '/path/to/somewhere/' -e 'pattern'
```
- `r` is recursive,
- `n` is line number
- `w` is whole word
- `l` output just the file name of matching files
- `--include=\*.{c,h}` will search through those files which have .c or .h extensions
- `--exclude=*.o`
- `--exclude-dir={dir1,dir2,*.dst}`


</br>


# Misc
## chmod
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