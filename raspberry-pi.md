# Raspberry Pi

<a name="index"></a>
## Table of contents
1. [ Install Raspbian ](#raspbian-install)
2. [ Backup / restore sd card ](#backup-restore)
3. [ Raspbian usage ](#raspbian-usage)
4. [ Raspbian known issues ](#raspbian-known-issues)
5. [ Install applications ](#install-applications)
    - [ Text editor ](#install-text-editor)
    - [ GIT ](#install-git)
    - [ Firefox ](#install-firefox)
    - [ Java JDK ](#install-java)
    - [ Libre Office ](#install-libre-office)
6. [ Printer setup ](#printer-setup)
7. [ Qemu ](#qemu)
    - [ Build Qemu from source ](#qemu-build)
    - [ Qemu usage examples ](#qemu-usage)

<a name="raspbian-install"></a>
## Install Raspbian

Install Rapberry Pi Imager to prepare the memory card.
After that, launch the Imager and set the card to use Raspbian. <br>
Then you can boot your Raspberry Pi : default user is ```pi```  and his password is ```raspberry```. <br/>
remark : change the default password ASAP

sources :  
https://www.raspberrypi.org/downloads/  
https://www.raspberrypi.org/documentation/installation/installing-images/README.md

<a href="#index">back to index</a>

---

<a name="backup-restore"></a>
## Backup / restore sd card

* create backup of sd card (same size as card !) : <br/>```sudo dd bs=4M if=/dev/sdb of=PiOS.img```
* create compressed backup of sd card : <br/>```sudo dd bs=4M if=/dev/sdb | gzip > PiOS.img.gz```
* restore sd card from image file : <br/>```sudo dd bs=4M if=PiOS.img of=/dev/sdb```
* restore sd card from compressed image file : <br/> ```gunzip --stdout PiOS.img.gz | sudo dd bs=4M of=/dev/sdb```

<a href="#index">back to index</a>

---

<a name="raspbian-usage"></a>
## Raspbian usage

- change password : ```passwd```
- switch to root account : ```sudo su```
- add show desktop shortcut : right click on taskbar \ panel settings \ 'Panel Applets' tab \ add 'Minimize all windows'
- add workspace icon : similar as previously, add 'Desktop Pager' and then right click on it to configure more than one
- search packages containing a name : ```sudo apt-cache pkgnames | grep <name>```

<a href="#index">back to index</a>

---

<a name="raspbian-known-issues"></a>
## Raspbian known issues

### No sound using audio jack and an hdmi to vga connection
- on desktop : right click on sound icon : check 'analog' instead of HDMI

<a href="#index">back to index</a>

---

<a name="install-applications"></a>
## Install applications

<a name="install-text-editor"></a>
### Text editors

The list of supported text editors :  
https://www.raspberrypi.org/documentation/linux/usage/text-editors.md

<a href="#index">back to index</a>

<a name="install-git"></a>
### GIT

- check if it exists : ```git --version```

- if not :
```bash
sudo apt update
sudo apt install git 
```

then use command lines to interact with a git repository

<a href="#index">back to index</a>

<a name="install-firefox"></a>
### Firefox

```bash
sudo apt update
sudo apt-get install firefox-esr
```

<a href="#index">back to index</a>

<a name="install-java"></a>
### Java JDK

- install last version of open jdk (11)

```bash
sudo apt update
sudo apt install default-jdk
```

- alternative : install open jdk 8
```bash
sudo apt update
sudo apt install openjdk-8-jdk
```

- check default jdk : ```java -version```

- set JAVA_HOME variable
  ```sudo update-alternatives --config java```

alternative : edit ```~/.bashrc``` to declare one jdk

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-armhf
or 
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-armhf
``` 

then reload file with ```source ~/.bashrc```

<a href="#index">back to index</a>

<a name="install-libre-office"></a>
### Libre Office

```bash
sudo apt update
sudo apt-get install libreoffice
```

<a href="#index">back to index</a>

---

<a name="printer-setup"></a>
## Setup a printer with CUPS (Common Unix Printing System)

* get last updates first
```bash
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

* install cups : ```sudo apt-get install cups```

* configure cups : ```sudo vi /etc/cups/cupsd.conf```
```text
# listen on all interfaces
#Listen localhost:631
# Restrict access to the server...
<Location />
  Order allow,deny
  Allow @Local
</Location>

# Restrict access to the admin pages...
<Location /admin>
  Order allow,deny
  Allow @Local
</Location>

# Restrict access to configuration files...
<Location /admin/conf>
  AuthType Default
  Require user @SYSTEM
  Order allow,deny
  Allow @Local
</Location>
```

* User & Network Access Settings
    - add the Pi user to the lpadmin group, gives the Raspberry Pi the ability to perform administrative functions of CUPS without necessarily being a super user :
      ```bash
      sudo usermod -a -G lpadmin pi
      ```
    - make sure that CUPS is accessible across entire network
      ```bash
      sudo cupsctl --remote-any
      ```
    - restart CUPS to effect changes
      ```bash
      sudo /etc/init.d/cups restart
      ```

* install brlaser : ```sudo apt-get install printer-driver-brlaser```

* restart cups : ```sudo service cups restart```

* add printer
    - turn printer on
    - launch ```127.0.0.1:631``` in a browser
    - logon is pi/usual pwd
    - go to ```administration | add printer```
    - select your printer and check 'share this printer'

source :   
https://medium.com/@alexanderbelov/how-to-use-your-brother-printer-with-cups-on-raspberry-pi-5b712cc2b4e6

<a href="#index">back to index</a>

---

<a name="qemu"></a>
## Qemu

QEMU is a generic and open source machine emulator and virtualizer.

links :  
https://www.qemu.org/  
https://blog.yucas.net/2020/10/14/installing-windows-xp-on-qemu/  
https://www.chrisrcook.com/projects/retro/building-qemu-5.0-on-raspbian/

<a name="qemu-build"></a>
### Build Qemu

Using Qemu after an installation with packet manager doesn't work on Raspberry Pi. <br/>
The alternative is to build it from sources.

* clone source
```bash
git clone https://github.com/qemu/qemu.git
# or
git clone https://gitlab.com/qemu-project/qemu.git
git clone https://git.qemu.org/git/qemu.git
```

* install dependencies before starting the build
```bash
sudo apt-get install ninja-build
sudo apt-get install libglib2.0-dev
sudo apt-get install libpixman-1-dev
sudo apt-get install libfdt-dev
# adviced but not mandatory
sudo apt-get install git-email
sudo apt-get install libaio-dev libbluetooth-dev libbrlapi-dev libbz2-dev
sudo apt-get install libcap-dev libcap-ng-dev libcurl4-gnutls-dev libgtk-3-dev
sudo apt-get install libibverbs-dev libjpeg8-dev libncurses5-dev libnuma-dev
sudo apt-get install librbd-dev librdmacm-dev
sudo apt-get install libsasl2-dev libsdl1.2-dev libseccomp-dev libsnappy-dev libssh2-1-dev
sudo apt-get install libvde-dev libvdeplug-dev libvte-2.90-dev libxen-dev liblzo2-dev
sudo apt-get install valgrind xfslibs-dev 
sudo apt-get install libnfs-dev libiscsi-dev
```

* start the build
```bash
cd qemu
git submodule init
git submodule update --recursive
./configure
# build using all cpus
sudo make -j4
```

* edit ```~/.bashrc``` to add qemu in your path : ```export PATH=/home/USER/git/gitlab/qemu/build:$PATH``` <br/>
  then reload it with : ```source ~/.bashrc```

<a href="#index">back to index</a>

<a name="qemu-usage"></a>
### Qemu usage examples

#### Install Windows XP

* create vm with 10G max size <br/>
  ```qemu-img create -f qcow2 -o size=10G /home/USER/vm-xp.img```

* start vm mapping iso image to cdrom and boot from cd (-boot d) <br/>
  ```qemu-system-i386 -L . -hda /home/USER/vm-xp.img -net nic,model=pcnet -net user -cpu pentium -soundhw all -vga cirrus -m 1G -cdrom /home/USER/iso/win-xp.iso -boot d```

* start vm with 2G RAM (-m), boot from hd (-boot c), belgian keyboard <br/>
  ```qemu-system-i386 -m 2047 -hda /home/USER/vm-xp.img -boot c -k be -usb -full-screen```

remark : at this stage, the vm is too slow to be usable

<a href="#index">back to index</a>

---  

**TO DO**

- configure scanner
- Wine + Word
