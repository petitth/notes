# Ubuntu

## Introduction

Ubuntu is a Linux distribution based on Debian. <br/>
link : https://ubuntu.com

## Installations

### Set custom environment variables

Creating a separated file is a good option when bashrc may be edited by external process.  
To do so, add a sh file to your home folder, and reference it from `~/.profile`
```shell
if [ -f "$HOME/env.sh" ]; then
  . "$HOME/env.sh"
fi
```

example of `env.sh`
```shell
export FOO=bar
export PATH=<blabla>:$PATH
```


### Install a different desktop

Install the package, then restart the machine.

- XFCE : ```sudo apt-get install xubuntu-desktop```
- KDE : ```sudo apt-get install kubuntu-desktop```

### Install OpenJDK 8

- install it with package manager : ```sudo apt install openjdk-8-jdk```
- add variables in ```env.sh``` :
```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
PATH="<existing content>:$JAVA_HOME/bin"
```
- reload it : ```source ~/env.sh```
- try it : ```java -version``` 

### Install Maven

- download archive from official website and unpack it in your home folder
- create a link for this folder : ```sudo ln -s /home/USER/apache-maven-3.6.0 /home/USER/maven```
- add variables to ```~/env.sh``` :
```
export MAVEN_HOME=/home/USER/maven
export PATH=$MAVEN_HOME/bin:$PATH
```
- reload it : ```source ~/env.sh```  
- try it : ```mvn -version```

source : https://linuxize.com/post/how-to-install-apache-maven-on-ubuntu-20-04/

remark : add this link in case your settings.xml is not found when building : <br/>
```ln -s /home/USER/maven/conf/settings.xml /home/USER/.m2/settings.xml```

## FAQ

### Fix error 'system program problem detected' on startup

Clear crash reports : ```sudo rm /var/crash/*``` <br/><br/>
source : https://itsfoss.com/how-to-fix-system-program-problem-detected-ubuntu/

### Session doesn't start after authenticating from UI

- try to use console : from UI prompt, press CTRL + ALT + F3
- if path is messed up : ```export PATH="/usr/bin:$PATH"```