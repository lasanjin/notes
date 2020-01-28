[⌫ back](../README.md)

# Java
## OpenJDK

1. Install
```
$ sudo add-apt-repository ppa:openjdk-r/ppa
$ sudo apt-get update
$ sudo apt install openjdk-<version>-jdk
```

## Set Java versions
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
$ sudo tar xf apache-maven-*.tar.gz -C /opt
```

3. Setup enviromental variables in `/etc/profile.d/maven.sh`
```
export JAVA_HOME=/usr/bin/java
export M2_HOME=/opt/maven
export MAVEN_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
```

4. Make the script executable
```
$ sudo chmod +x /etc/profile.d/maven.sh
```

### Maven Intellij configurations
1. Get path to Maven 
```
$ maven -v
```
2. Change home directory
```
Settings -> Build, Execution, Deployment -> Maven -> Maven home directory
```

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

2. Init and run
```
$ vue init <template> <name>
$ $ npm run serve
```

- Packages
  - vue-router
  - axios
  - vuejs-logger


<br/>


# Python
Find path
```
$ which -a <python/pip>
```

## Python 2
- modules
```
$ sudo apt-get install python-all-dev python-wheel python-setuptools
```

- pip
```
$ sudo apt install python-pip
```

- autopep8
```
$ python2.7 -m pip install autopep8
```


## Python 3
- pip modules
```
$ sudo apt-get install python3-all-dev python3-wheel python3-setuptools
```

- pip3
```
$ sudo apt-get install python3-pip
```


## Handle versions
 - List versions
```
$ ls /usr/bin/python*
```

### Change python version system-wide
 1. Update alternatives table and include several python versions
    - `--install` option takes multiple arguments and creates a symbolic link
    - `1` and `2` specifies priority, i.e. if no manual alternative selection is made the alternative with the highest priority number is set
```
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
```

 2. Set version
```
$ update-alternatives --config python
```



## Libraries

### [jq](https://pypi.org/project/jq/)
```
$ sudo python2.7 -m pip install pyjq
```

 - Installation requires programs required to build jq. This includes:
   - Autoreconf
   - C compiler toolchain (gcc, make)
   - libtool
   - Python headers
```
$ sudo install autoconf automake libtool python
```

### [mpmath](http://mpmath.org/doc/current/setup.html#download-and-installation)
```
$ pip install mpmath
```


<br/>


# Haskell
- [Downloads](https://www.haskell.org/downloads/) 
- [Mint](https://www.haskell.org/platform/linux.html#linux-mint)
  - GHC (compiler)
  - Cabal (build tool)
  - Some other tools along with a starter set of libraries in a global location on your system
```
$ sudo apt-get install haskell-platform
```

- GHCi
  - GHC’s interactive environment
```
$ ghci 
```

- Compile & run
```
$ ghc --make <filename> && ./filename
```

- Optional 
  - The Haskell Tool [Stack](https://docs.haskellstack.org/en/stable/README/)

- [Formatter](https://hackernoon.com/keeping-it-clean-haskell-code-formatters-32ca25c59c70)
  - `$ apt-get install stylish-haskell` (recommended)
  - `$ stack install stylish-haskell`
- [Hlint](https://hackage.haskell.org/package/hlint)
  1. `$ cabal update`
  2. `$ cabal install hlint`


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
code --list-extensions | xargs -L 1 echo code --install-extension
```

### Extensions
- Theme
```
code --install-extension Equinusocio.vsc-material-theme
```

- Haskell
```
code --install-extension justusadam.language-haskell
code --install-extension vigoo.stylish-haskell
```

- Python
```
code --install-extension ms-python.python
```

- Shell
```
code --install-extension foxundermoon.shell-format
```

- Java
```
code --install-extension redhat.java
code --install-extension vscjava.vscode-java-debug
code --install-extension vscjava.vscode-java-dependency
code --install-extension vscjava.vscode-java-pack
code --install-extension vscjava.vscode-java-test
code --install-extension vscjava.vscode-maven
```

- Nginx
```
code --install-extension shanoor.vscode-nginx
code --install-extension william-voyek.vscode-nginx
code --install-extension raynigon.nginx-formatter
```

- Vue
```
code --install-extension octref.vetur
```

- cpp
```
code --install-extension ms-vscode.cpptools
```

- Docker
```
code --install-extension ms-azuretools.vscode-docker
```

- Markdown
```
code --install-extension yzhang.markdown-all-in-one
```

- Misc
```
code --install-extension 2gua.rainbow-brackets
code --install-extension esbenp.prettier-vscode
code --install-extension lunaryorn.hlint
code --install-extension ms-vsliveshare.vsliveshare
code --install-extension rafaelmaiolla.remote-vscode
code --install-extension ritwickdey.LiveServer
code --install-extension streetsidesoftware.code-spell-checker
code --install-extension Gruntfuggly.triggertaskonsave
```

- `shell-format` requires golang and shfmt installed
```
$ sudo snap install shfmt
$ sudo snap install --classic go
```

### Remote VSCode ([rmate](https://github.com/aurora/rmate))
1. Install the extension and re-launch VSCode
2. Install rmate in VM
```
$ sudo wget https://raw.github.com/aurora/rmate/master/rmate
$ sudo chmod a+x rmate
```
3. Export PATH in VM
4. In VSCode do `Ctrl+P` and execute `>Remote: Start Server`
5. Create an ssh tunnel
   - `-R` option sets up a reverse tunnel
   - The first `$PORT` names a port on the remote
     - It will be connected to `localhost:$PORT` or the same port on the connecting box
       - That port number is the default for TextMate 2 and rmate
```
$ ssh -R $PORT:localhost:$PORT VIRTUAL_MACHINE_IP_ADDRESS
```
1. Open file from VM in localhost `$ rmate -p $PORT file <filename>`



2. Enable opening multiple files in different VSCode tabs
   - Uncheck `VSCode -> Settings -> Workbench>Editor:Enable Preview`


### Formatting
- Turn off formatting for part of Java code
```java
// @formatter:off 
int[][] = {
    { 1, 2, 3 }
    { 4, 5, 6 }
    { 7, 8, 9 }
}
// @formatter:on 
```

- Curly braces on same line in C/C++
   1. Preferences -> Settings
   2. Search for `C_Cpp.clang_format_fallbackStyle`
   3. Change to `{ BasedOnStyle: Google, IndentWidth: 4, ColumnLimit: 0 }`


<br/>


# Markdown
## Compiling Markdown with CSS into PDF
1. Install [Pandoc](https://pandoc.org/installing.html) and [wkhtmltopdf](https://wkhtmltopdf.org/)
```
$ sudo apt install pandoc
$ sudo apt install wkhtmltopdf
```

2. Execute
```
$ pandoc -t html5 -c <file>.css -o <file>.pdf <file>.md
```
- [Variables](https://pandoc.org/MANUAL.html#variables-for-wkhtmltopdf) for wkhtmltopdf


## [VSCode](https://code.visualstudio.com/Docs/languages/markdown)
1. Generates a `tasks.json` file
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build-pdf",
            "type": "shell",
            "command": "pandoc -t html5 -c *.css -o *.pdf *.md",
            "group": "build",
            "problemMatcher": [],
        }
    ],
}
```
2. Place `tasks.json` in a folder `.vscode` in your workspace
3. Run the build task
   - Press `Ctrl+Shift+B` -> `Run Build Task`


### Trigger task on save
```json
"triggerTaskOnSave.tasks": {
    "build-pdf": [
        "**/*.md"
    ]
}
```

## [Syntax](https://daringfireball.net/projects/markdown/syntax#html)
- Additional space
```
&nbsp;
&ensp;
&emsp;
```


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


</br>


# Mininet
1. `$ sudo apt-get install mininet`
    - `$ mn --version`
2. `$ sudo apt-get install openvswitch-testcontroller`
    - "This controller enables OpenFlow switches that connect to it to act as MAC-learning Ethernet switches. It can be used for initial testing of OpenFlow networks. It is not a necessary or desirable part of a production OpenFlow deployment." 
3. `$ sudo apt-get install openvswitch-switch`
    - `$ ovs-vswitchd --version` 


<br/>


# [GCP](kubernetes/GCP)


</br>


# Minikube
### Installation
[Guide](https://github.com/kubernetes/minikube/releases)