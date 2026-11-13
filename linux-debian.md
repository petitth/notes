# Debian

<a name="index"></a>

## Table of contents
1. [ Introduction ](#introduction)
2. [ Usage notes ](#usage-notes)
   - [ Restart with console mode ](#restart-console-mode)
   - [ Rescan audio devices ](#rescan-audio-devices)
   - [ Shortcuts ](#shortcuts)
   - [ Display system information ](#display-system-info)
   - [ Allow SSH connection ](#ssh)
3. [ Installations ](#installations)
   - [ Java JDK ](#install-java-jdk)
   - [ Maven ](#install-maven)
   - [ Firefox ](#install-firefox)
   - [ Chrome ](#install-chrome)
   - [ pdf reader ](#install-pdf-reader)
   - [ SQL Developer ](#install-sql-developer)
   - [ NoSQLBooster ](#install-nosqlbooster)
   - [ Calculator ](#install-calculator)
   - [ Add thumbnails on videos ](#videos-thumbnails)
   - [ Betterlockscreen ](#betterlockscreen)
4. [ Setup ](#setup)
   - [ XFCE : set button at bottom instead of header ](#xfce-button-bottom)
   - [ XFCE : disable windows grouping ](#xfce-window-grouping)
   - [ XFCE : new session starts opening windows from previous one ](#xfce-windows-previous-session)
   - [ XFCE : screen locks when inactive even if disabled ](#xfce-screen-locker)

<a name="introduction"></a>
## Introduction

Debian is a free operating system using the Linux kernel.   
source : www.debian.org

<a name="usage-notes"></a>
## Usage notes

<a name="restart-console-mode"></a>
### Restart with console mode

- when Grub menu is displayed, press "e" to edit it
- add "3" at the end of the line starting with "linux"
- CTRL + X to start

<a href="#index">back to index</a>

<a name="rescan-audio-devices"></a>
### Rescan audio devices

```bash
systemctl --user restart pulseaudio
```

<a href="#index">back to index</a>

<a name="shortcuts"></a>
### Shortcuts

- switch workspace (1 - 4) : CTRL + F1/F2/F3/F4

<a href="#index">back to index</a>

<a name="display-system-info"></a>
### Display system information

HardInfo is a small application that displays information about your hardware and operating system.  
Install it with package manager :
```bash
sudo apt-get update
sudo apt-get install hardinfo
```
Launch it from terminal with ```hardinfo```

<a href="#index">back to index</a>

<a name="ssh"></a>
### Allow SSH connection

- install
```bash
apt install openssh-server
```

- usage
```bash
systemctl status ssh
service ssh stop
service ssh start
```

- turn off SSH indefinitely
```bash
systemctl disable ssh
```

<a href="#index">back to index</a>

<a name="installations"></a>
## Installations

<a name="install-java-jdk"></a>
### Install Java JDK

- download archive from Oracle Java website
- unpack it into ```/usr/lib/jvm/```
- edit ```~/.bashrc``` to declare variables : 
 ```bash
 export JAVA_HOME=/usr/lib/jvm/jdk-11.0.3
 export PATH=$JAVA_HOME/bin:$PATH
 ```
- reload file : ```source ~/.bashrc```
- test it : ```java -version``` 

<a href="#index">back to index</a>


<a name="install-maven"></a>
### Install Maven

- download archive from official website and unpack it where you want
- edit bashrc :
```bash
export MAVEN_HOME=/home/USER/apps/apache-maven-3.6.1/bin
export PATH=$MAVEN_HOME:$PATH
```
- reload it : ```source ~/.bashrc```
- test it : ```mvn -version```

<a href="#index">back to index</a>


<a name="install-firefox"></a>
### Install Firefox

- download Firefox archive on official website
- unpack it somewhere like ```/home/USER/apps/firefox``` with : ```tar -xvjf firefox-8.0.tar.bz2```
- create a launcher on menu, using this command : ```"/home/USER/apps/firefox/firefox" %u```

you can redirect default installation folder to new one, and then launch it from menu (optional) :
```shell script
rm /usr/bin/firefox
ln -s /home/USER/apps/firefox/firefox /usr/bin/firefox
```

<a href="#index">back to index</a>

<a name="install-chrome"></a>
### Install Chrome

Use packet manager : ```apt-get install chromium```

Then, to avoid the message about keyring, launch it with : ```/usr/bin/chromium --password-store=basic %U```

<a href="#index">back to index</a>

<a name="install-pdf-reader"></a>
### Install a pdf reader

Use package manager :
```bash
# okular is a easier to use than evince
apt-get install okular
apt-get install evince
```

<a href="#index">back to index</a>


<a name="install-sql-developer"></a>
### Install SQL Developer

- install alien, OpenJDK 11, and OpenJFX : ```sudo apt install alien openjdk-11-jdk openjfx```
- download SQL Developer : https://www.oracle.com/tools/downloads/sqldev-downloads.html
- install SQL Developer : ```sudo alien -i sqldeveloper-*.rpm```
- open SQL developer in the terminal to set the path to the JDK :
```bash
 $ /usr/local/bin/sqldeveloper 
 Type the full pathname of a JDK installation (or Ctrl-C to quit), the path will be stored in /home/user/.sqldeveloper/19.4.0/product.conf
 /usr/lib/jvm/java-11-openjdk-amd64/
```
source : https://gist.github.com/bmaupin/ad02e5b4a17abfc662d61664bced2773

<a href="#index">back to index</a>

<a name="install-nosqlbooster"></a>
### Install NoSQLBooster

This is a powerful client to query MongoDB.

- download file from official website
- create a shortcut to launch it easily :   
```bash
/home/USER/apps/nosqlbooster4mongo-6.2.13.AppImage --no-sandbox %U
```

source : https://www.nosqlbooster.com

<a href="#index">back to index</a>

<a name="install-calculator"></a>
### Install a calculator

Use package manager :
```bash
# gaclculator has different UI : basic and scientific
apt-get install galculator
# more scientific, nice too
apt-get install speedcrunch
```

<a href="#index">back to index</a>

<a name="videos-thumbnails"></a>
### Add thumbnails on videos

Install the following packages :

```text
tumbler
tumbler-plugins-extra
ffmpegthumbnailer
```

<a href="#index">back to index</a>

<a name="betterlockscreen"></a>
### Betterlockscreen

#### Install dependencies

* ilock

```bash
apt install autoconf gcc make pkg-config libpam0g-dev libcairo2-dev libfontconfig1-dev libxcb-composite0-dev libev-dev libx11-xcb-dev libxcb-xkb-dev libxcb-xinerama0-dev libxcb-randr0-dev libxcb-image0-dev libxcb-util0-dev libxcb-xrm-dev libxkbcommon-dev libxkbcommon-x11-dev libjpeg-dev libgif-dev
```

* ilocker-color

```bash
git clone https://github.com/Raymo111/i3lock-color.git
cd i3lock-color
./install-i3lock-color.sh
```

source : https://github.com/Raymo111/i3lock-color

* ImageMagick

* bc

#### Install and configure it

* install betterlockscreen
 
```bash
wget https://raw.githubusercontent.com/betterlockscreen/betterlockscreen/main/install.sh -O - -q | bash -s user
```

* config : add content to PATH variable

```bash
export PATH=$JAVA_HOME/bin:$MAVEN_HOME/bin:/home/USER/.local/bin:$PATH
```

* set image
 
```bash
betterlockscreen -u "/home/USER/Pictures/something.jpg"
```  

* launch it
 
```bash
betterlockscreen --lock
```

* launch it from a shortcut

```bash
/bin/bash -i -c "betterlockscreen --lock" %f
```

<a href="#index">back to index</a>

<a name="setup"></a>
## Setup

<a name="xfce-button-bottom"></a>
### XFCE : set button at bottom instead of header

- open terminal and launch : ```xfce4-settings-editor```
- in ```xsettings``` : disable ```DialogsUseHeader```

<a href="#index">back to index</a>

<a name="xfce-window-grouping"></a>
### XFCE : disable windows grouping

Go to your panel settings, the one with "Window Buttons" item.  
Click on item and edit it, then switch "Window grouping" to "never" in "Behaviour"

<a href="#index">back to index</a>

<a name="xfce-windows-previous-session"></a>
### XFCE : new session starts opening windows from previous one

Delete ```~/.cache/sessions```

<a href="#index">back to index</a>

<a name="xfce-screen-locker"></a>
### XFCE : screen locks when inactive even if disabled

This issue was met with last version of Debian : even if screensaver is disabled, screen auto locks when inactive for 5 minutes.  

* remove 'light locker' dependency

* install 'xfce4-screensaver' + configure it

* if screen lock doesn't work anymore, install an alternate one : `betterlockscreen` is a good one

<a href="#index">back to index</a>