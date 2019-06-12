[⌫ back](../README.md)

# Tools
## JDK
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

## Apache Maven
Alt 1
```
$ sudo apt install maven
```

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

## Node & NPM
```
$ sudo apt-get install curl python-software-properties
$ curl -sL https://deb.nodesource.com/setup_11.x | sudo bash -
$ sudo apt-get install nodejs
```

## Git
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


<br/>


# Python
Find path(s)
```
$ which -a <python/pip>
```

## Python 2
```
$ sudo apt-get install python-all-dev python-wheel python-setuptools
```

 - Formatter
```
$ python2.7 -m pip install autopep8
```

## Python 3
```
$ sudo apt-get install python3-all-dev python3-wheel python3-setuptools
```

## Libraries

### jq
```
$ sudo python2.7 -m pip install pyjq
```
Installation requires programs required to build jq. This includes:
 - Autoreconf
 - C compiler toolchain (gcc, make)
 - libtool
 - Python headers
```
$ sudo install autoconf automake libtool python
```


<br/>


# IDE
## Intellij
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

## VS Code
### List all extensions
```
$ code --list-extensions | xargs -L 1 echo code --install-extension
```

### Install current extensions
```
$ code --install-extension 2gua.rainbow-brackets
$ code --install-extension dbaeumer.vscode-eslint
$ code --install-extension dsznajder.es7-react-js-snippets
$ code --install-extension foxundermoon.shell-format
$ code --install-extension JuanBlanco.solidity
$ code --install-extension ms-python.python
$ code --install-extension ms-vsliveshare.vsliveshare
$ code --install-extension shd101wyy.markdown-preview-enhanced
$ code --install-extension yzhang.markdown-all-in-one
```

**OBS** shell-format requires golang and shfmt installed
```
$ sudo snap install shfmt
$ sudo snap install --classic go
```

### Remote VSCode
1. Install the extension and re-launch VSCode
2. In VM install rmate
```
$ sudo wget -O /usr/local/bin/rmate https://raw.github.com/aurora/rmate/master/rmate
$ sudo chmod a+x /usr/local/bin/rmate
```
3. In VSCode do Ctrl+P and execute the `>Remote: Start Server`
4. Connect to VM
```
$ ssh -R 52698:localhost:52698 VIRTUAL_MACHINE_IP_ADDRESS
```
5. Open file from VM in localhost `$ rmate <filename>`
