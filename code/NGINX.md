[⌫ back](../README.md)

## Installation
1. Install compiler
```
$ sudo apt-get install build-essential
```
2. [Download](http://nginx.org/en/download.html) NGINX

Run `$ ./configure` in NGINX folder to check dependencies

3. Install useful packages
```
$ sudo apt-get install libpcre3 libpcre3-dev zlib1g-dev libssl-dev
```

4. Configuration options. More [here](http://nginx.org/en/docs/)
```
$ sudo ./configure --help
```

</br>

## Configuration
 - Customize installation
   - Executable file
   - Configuration files
   - Log files
   - Regular expressions
   - Process ID path
     - Server configuration
   - Module flags
     - dynamic
     - http2
     - http_autoindex_module (Remove this default module)
       - *Allows NGINX to respond to a directory request with the content of that directory
```
$ sudo ./configure --sbin-path=/user/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --pid-path=/var/run/nginx.pid --with-http_ssl_module --with-http_image_filter_module=dynamic --modules-path=/etc/nginx/modules --with-http_v2_module --without-http_autoindex_module
```
### Configuration summary
+ using system PCRE library
+ using system OpenSSL library
+ using system zlib library

nginx path prefix: "/usr/local/nginx"\
nginx binary file: "/user/bin/nginx"\
nginx modules path: "/etc/nginx/modules"\
nginx configuration prefix: "/etc/nginx"\
nginx configuration file: "/etc/nginx/nginx.conf"\
nginx pid file: "/var/run/nginx.pid"\
nginx error log file: "/var/nginx/error.log"\
nginx http access log file: "/var/log/nginx/access.log"\
nginx http client request body temporary files: "client_body_temp"\
nginx http proxy temporary files: "proxy_temp"\
nginx http fastcgi temporary files: "fastcgi_temp"\
nginx http uwsgi temporary files: "uwsgi_temp"\
nginx http scgi temporary files: "scgi_temp"


### Init configurations
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

### Libraries
 - GD library
```
$ sudo apt-get install libgd-dev
```

</br>

## systemd [configuration](https://www.nginx.com/resources/wiki/start/topics/examples/systemd/)
   - Configure path to PIDFile
   - Enable startup on boot `$ sudo systemctl enable nginx`


## Commands 
`$ ./configure --help | grep without` *List all default modules*\
`$ nginx -V 2>&1 | tr -- - '\n' | grep module` *List all installed modules*\
`$ sudo systemctl list-units | grep <service name>` *List all systemd services* \
`$ ps aux | grep nginx` *process command: all users and boot processes for nginx*\
`./configure --help` *List all available modules*\
`$ sudo nginx -t` *Test configuration*

</br>

## Tools
### [nghttp2](https://nghttp2.org/documentation/package_README.html)
```
$ sudo apt-get install nghttp2-client
```
Example

`$ nghttp -nysa <url>` \
-n *Discard responses: testing, not saving to disk*\
-y *Ignore selfsigned certificate*\
-s *Print the response*\
-a *Request linked assets for this request*\

### [Siege](https://www.joedog.org/siege-home/)
```
$ sudo apt-get install siege
```
Example

`$ siege -v 2 -c 5 <url>` \
-v *Verbose output*\
-r *Run 2 tests*\
-c *5 concurrent connection*\

### Basic Auth
```
$ sudo apt-get install apache2-utils
```
Example

`$ htpasswd -c <path>/.htpasswd <user>` \
-c *Write password to file*\

</br>

## Let's Encrypt
### Install [Cerbot](https://certbot.eff.org/)

Alt 1

Install certificates where Certbot configures cipher suit and other ssl parameters (Might not be ideal on existing server configuration where edits could cause trouble)

```
$ certbot --nginx
```

Alt 2

Skip above (installation)
```
$ sudo certbot certonly -d <mydomain.com>
```

### Renew certificate
Alt 1

```
$ certbot renew --dry-run
```

Alt 2

```
$ crontab -e
```

Add `@daily cerbot renew`

</br>

## Security
Generate DH (Diffie-Hellman) params

```
$ openssl dhparam -out <path> 2048
```

### Test website [here](https://securityheaders.com)