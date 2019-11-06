[⌫ back](../README.md)

# Java
## JDK

1. Remove default OpenJDK
```
$ sudo apt-get --purge remove openjdk* && sudo apt-get autoremove && sudo apt-get clean
```
2. Download
- Oracle [here](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
- OpenJDK [here](https://jdk.java.net/)

### [Oracle-JDK vs OpenJDK](https://www.baeldung.com/oracle-jdk-vs-openjdk)

Alt 1

1. Extract, create as root directory
```
$ sudo tar -zxvf jdk-* && sudo mkdir -p /opt/java && sudo mv jdk-* /opt/java
```

2. Install
```
$ sudo update-alternatives --install "/usr/bin/java" "java" "/opt/java/jdk<version>/bin/java" 1
```

3. Make system default
```
$ sudo update-alternatives --set java /opt/java/jdk<version>/bin/java
```

Alt 2

1. Extract
```
$ sudo tar -zxvf jdk-*
```

2. Set PATH in /etc/enviroment
```
JAVA_HOME=<path>
export JAVA_HOME
```


3. Reload the system PATH
```
$ . /etc/enviroment
```

Alt 3

1. Install
```
$ sudo add-apt-repository ppa:openjdk-r/ppa
$ sudo apt-get update
$ sudo apt-get install openjdk-XX-jdk
```
2. Set PATH (/usr/lib/jvm/java-XX-openjdk-amd64/)


### List Java versions
```
$ sudo update-alternatives --config java
```

### Set default Java version
1. Type in a number to select a Java version
```
$ sudo update-alternatives --config java
```

2. Set default Java compiler by running
```
$ sudo update-alternatives --config java
```

## Maven
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

### Maven Intellij conf
1. Get path to Maven 
```
$ maven -v
```
2. Change home directory

Settings -> Build, Execution, Deployment -> Maven -> Maven home directory

3. Set permission on project
```
$ sudo chmod -R 777 <folder>
```

4. Build project manually
```
$ sudo mvn clean install
```

## Spring Boot
 - Init project [here](https://start.spring.io/)


</br>


# JavaScript
## Node & NPM
```
$ sudo apt-get install curl python-software-properties
$ curl -sL https://deb.nodesource.com/setup_11.x | sudo bash -
$ sudo apt-get install nodejs
```

## Vue
1. Install
```
$ sudo apt-get install @vue/cli
```

2. Init
```
$ vue init <template> <name>
```

4. Packages
```
$ npm add axios 
$ npm install vue-router
$ npm add vuejs-logger
$ npm run serve
```
 - axios makes HTTP requests to server 
 - vuejs-logger is a logging framework


<br/>


# Python
Find path(s)
```
$ which -a <python/pip>
```

Install pip
```
$ sudo apt install python-pip
```

## Python 2
```
$ sudo apt-get install python-all-dev python-wheel python-setuptools
```

 - Formatter
```
$ python2.7 -m pip install autopep8
```

- VSCode
```
$ sudo apt-get install python3-pip
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


# Haskell
- [Downloads](https://www.haskell.org/downloads/) 
- [Mint](https://www.haskell.org/platform/linux.html#linux-mint)
  - GHC (compiler), Cabal (build tool), and some other tools, along with a starter set of libraries in a global location on your system.
```
$ sudo apt-get install haskell-platform
```

- GHCi: GHC’s interactive environment
```
$ ghci 
```

- The Haskell Tool [Stack](https://docs.haskellstack.org/en/stable/README/)

- [Formatter](https://hackernoon.com/keeping-it-clean-haskell-code-formatters-32ca25c59c70)
```
$ stack install stylish-haskell 
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
### Settings json
`$HOME/.config/Code/User/settings.json`

### List all extensions
```
$ code --list-extensions | xargs -L 1 echo code --install-extension
```

### Install current extensions
```
code --install-extension 2gua.rainbow-brackets
code --install-extension esbenp.prettier-vscode
code --install-extension foxundermoon.shell-format
code --install-extension justusadam.language-haskell
code --install-extension lunaryorn.hlint
code --install-extension ms-azuretools.vscode-docker
code --install-extension ms-python.python
code --install-extension ms-vscode.cpptools
code --install-extension ms-vsliveshare.vsliveshare
code --install-extension octref.vetur
code --install-extension rafaelmaiolla.remote-vscode
code --install-extension raynigon.nginx-formatter
code --install-extension redhat.java
code --install-extension ritwickdey.LiveServer
code --install-extension shanoor.vscode-nginx
code --install-extension shd101wyy.markdown-preview-enhanced
code --install-extension streetsidesoftware.code-spell-checker
code --install-extension vigoo.stylish-haskell
code --install-extension VisualStudioExptTeam.vscodeintellicode
code --install-extension vscjava.vscode-java-debug
code --install-extension vscjava.vscode-java-dependency
code --install-extension vscjava.vscode-java-pack
code --install-extension vscjava.vscode-java-test
code --install-extension vscjava.vscode-maven
code --install-extension william-voyek.vscode-nginx
code --install-extension yzhang.markdown-all-in-one
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
$ sudo wget https://raw.github.com/aurora/rmate/master/rmate
$ sudo chmod a+x rmate
```
3. Export PATH in VM
4. In VSCode do Ctrl+P and execute the `>Remote: Start Server`
5. Create an ssh tunnel
```
$ ssh -R $PORT:localhost:$PORT VIRTUAL_MACHINE_IP_ADDRESS
```
6. Open file from VM in localhost `$ rmate -p $PORT file <filename>`

The -R option sets up a reverse tunnel. The first $PORT names a port on
the remote. It will be connected to localhost:$PORT or the same port on
the connecting box. That port number is the default for TextMate 2 and
rmate.

- Enable opening multiple files in different VSCode tabs

Uncheck `VSCode -> Settings -> Workbench>Editor:Enable Preview`


<br/>


# GCP
### Cloud SDK
[Guide](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)

### GKE
[Guide](https://kubernetes.io/docs/setup/production-environment/turnkey/gce/)


</br>


# Minikube
### Installation
[Guide](https://github.com/kubernetes/minikube/releases)


</br>


# Git
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



# Mininet
1. `$ sudo apt-get install mininet`
    - `$ mn --version`
2. `$ sudo apt-get install openvswitch-testcontroller`
    - "This controller enables OpenFlow switches that connect to it to act as MAC-learning Ethernet switches. It can be used for initial testing of OpenFlow networks. It is not a necessary or desirable part of a production OpenFlow deployment." 
3. sudo apt-get install openvswitch-switch
    - `$ ovs-vswitchd --version` 