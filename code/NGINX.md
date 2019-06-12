[⌫ back](../README.md)

# NGINX
## Commands
`$ ps aux | grep nginx` *process command: all users and boot processes for nginx*\
` `

## Installation
1. Install compiler
```
$ sudo apt-get install build-essential
```
2. [Download](http://nginx.org/en/download.html) NGINX

Run `$ ./configure` in NGINX folder to check dependencies

3. Install useful packages
```
$ sudo apt-install libpcre3 libpcre3-dev zlib1g-dev libssl-dev
```

4. Configuration options. More [here](http://nginx.org/en/docs/)
```
$ sudo ./configure --help
```

## Configuration
 - Customize installation
   - Executable file
   - Configuration files
   - Log files
   - Regular expressions
   - Process ID path
     - Server configuration
```
$ sudo ./configure --sbin-path=/user/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --pid-path=/var/run/nginx.pid --with-http_ssl_module
```

 - Compile configuration source
```
$ sudo make
```

 - Install compiled source
```
$ sudo make install 
```

 - Configuration files
```
$ ls -l /etc/nginx
```

 - systemd [configuration](https://www.nginx.com/resources/wiki/start/topics/examples/systemd/)
   - Configure path to PIDFile
   - Enable startup on boot `$ sudo systemctl enable nginx`
```

```

## Commands
 - `$ sudo nginx -t` *Test configuration*
 