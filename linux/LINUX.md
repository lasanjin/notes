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
`$ upower -i upower -e | grep 'BAT'` *List battery info*


<br/>


# Shortcuts
`Ctrl`+`Alt`+`Arrow` *Move to other desktop*\
`Super`+`Shift`+`Alt`+`Arrow` *Move window to other desktop*\
`Super`+`Shift`+`Arrow` *Move window to other monitor*\
`Shift`+`PrtCn` *Select area to grab for screenshot*

More shortcuts [here](https://community.linuxmint.com/tutorial/view/244)


<br/>


# Misc
### chmod
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