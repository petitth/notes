# Linux commands

<a name="index"></a>

## Table of contents
1. [File handling](#file-handling)
2. [Search data](#search-data)
3. [Archives](#archives)
4. [RPM packages](#rpm-packages)
5. [DEB packages](#deb-packages)
6. [Network](#network)
7. [Hard disk](#harddisk)
8. [Misc](#misc)
9. [Users](#users)
10. [Variables](#variables)


<a name="file-handling"></a>
## File handling

| Command                                         | Detail                                     |
| ------------------------------------------------|--------------------------------------------|
| ```cat file```                                  | display file content                       |
| ```cat -n file```                               | display file content with line numbers     |
| ```tail -n 5 file```                            | consult last 5 lines (default is 10)       |
| ```tail -f file```                              | keep showing file                          |
| ```rm -rf folder```                             | remove a non empty folder                  |
| ```chmod 777 file```                            | set 'rwx rwx rwx' permission on file       | 
| ```chown -R user:group directory```             | change ownership in all directory          |
| ```ln -s source target```                       | create link (use full paths !)             |
| ```ls -lt```                                    | list with descending sort                  |
| ```ls -rtl```                                   | list with ascending sort                   |
| ```ls -l --block-size=MB``` **OR** ```ls -lh``` | list with file size in megabytes           |
| ```cp -a /source/. /dest/```                    | copy folder including hidden files         |

<a href="#index">back to index</a>

<a name="search-data"></a>
## Search data

| Command                                                          | Detail                                       |
|------------------------------------------------------------------|----------------------------------------------|
| ```grep -Ri word_to_search path```                               | indexed search                               |
| ```find . -type f -exec du -Sh {} + \| sort -rh \| head -n 15``` | 15 biggest files in current path             |
| ```find . -name "repository"```                                  | find by name                                 |
| ```grep --color=auto -iR 'getChar();' *.c```                     | find by content (no case + color for result) |
| ```grep --color=auto -rnw '/some/path' -e 'searched'```          | find by content (recursive)                  |


<a href="#index">back to index</a>

<a name="archives"></a>
## Archives

| Command                                         | Detail                                 |
| ------------------------------------------------|----------------------------------------|
| ```zip -r dir.zip dir```                        | create recursive zip                   |
| ```zip something.zip file1 file2 file3```       | create zip from specific files         |
| ```unzip something.zip```                       | unpack zip in local directory          |
| ```unzip -p file.war WEB-INF/views/error.jsp``` | view one file in archive               |
| ```zipinfo file.zip```                          | provides infos about zip content       |
| ```tar -zxvf fileName.tgz```                    | unpack tgz                             |
| ```rar a -v2048000k file.mkv```                 | create rar archive with limited size   |
| ```jar -tf file```                              | list files from jar/war file           |  

<a href="#index">back to index</a>

<a name="rpm-packages"></a>
## RPM packages (Red Hat)

| Command                          | Detail                                 |
|----------------------------------|----------------------------------------|
| ```rpm -qa \| grep mc```         | search for 'mc' package                |
| ```rpm -e mc-4.5.51.18.rpm```    | uninstall 'mc' package                 |
| ```rpm -ivh mc-4.5.51.18.rpm```  | install 'mc' package                   |
| ```rpm -ql mc-4.5.51.18.rpm```   | check where 'mc' package is installed  |

<a href="#index">back to index</a>

<a name="deb-packages"></a>
## DEB packages (Debian)

| Command                                 | Detail                                           |
|-----------------------------------------|--------------------------------------------------|
| ```dpkg -l```                           | list installed packages                          |
| ```dpkg -i <package-name.deb>```        | install package                                  |
| ```apt install <package-name.deb>```    | install with dependencies                        |
| ```dpkg --remove <package-name.deb>```  | remove package binaries                          |
| ```dpkg --purge <package-name.deb>```   | remove package binaries and configuration files  |

<a href="#index">back to index</a>

<a name="network"></a>
## Network

| Command                                      | Detail                                |
| ---------------------------------------------|---------------------------------------|
| ```wget url```                               | download a file from an url           |
| ```scp user@host:foobar.txt /directory```    | get a file from external host         |
| ```curl url```                               | launch query on an url                |
| ```hostname```                               | machine name                          |
| ```hostnamectl```                            | get info about system (dist. version) | 
| ```ip a```                                   | ip address                            |

<a href="#index">back to index</a>

<a name="harddisk"></a>
## Hard disk

| Command                                             | Detail                                          |
| ----------------------------------------------------|-------------------------------------------------|
| ```fdisk -l```                                      | list partitions                                 |
| ```df -h```                                         | list partitions with disk usage                 |
| ```du -shx */```                                    | list subfolders with disk usage in current dir  |
| ```du -h .```                                       | disk usage of current folder + folder size      |
| ```du -h folder1 folder2```                         | disk usage of multiples folders at once         |
| ```mount -t ext4 /dev/sda1 /mnt/something```        | mount partition                                 |
| ```mount -o loop -t iso9660 fichier.iso /mnt/iso``` | mount iso file                                  |
| ```dd if=/dev/zero of=/dev/sda bs=446 count=1```    | format just MBR on /dev/sda                     |
| ```dd if=/dev/zero of=/dev/sda bs=512 count=1```    | format MBR and partition table                  |
| ```dd if=image.iso of=/dev/sdx```                   | create bootable usb                             |
| ```dd if=/dev/cdrom of=/tmp/disk-image.iso```       | create disk image                               |

remark : sdx is the volume, not the partition (which contains a number)

<a href="#index">back to index</a>

<a name="misc"></a>
## Misc

| Command                             | Detail                                      |
|-------------------------------------|---------------------------------------------|
| ```ls -1 \| wc -l```                | count files                                 |
| ```which echo```                    | search command path                         |
| ```bc```                            | command line calculator ('quit' at the end) |
| ```date```                          | current time on machine                     |
| ```crontab -l```                    | cron jobs                                   |
| ```rm -rf ~/.local/share/Trash/*``` | clear trash                                 |
| ```cat /etc/os-release```           | current distribution                        |
| ```shutdown now```                  | shutdown machine                            |
| ```shutdown -r now```               | restart machine                             |


<a href="#index">back to index</a>

<a name="users"></a>
## Users

| Command                                         | Detail                                          |
|-------------------------------------------------|-------------------------------------------------|
| ```useradd -m <user>```                         | create user with home directory                 |
| ```usermod -aG sudo <user>```                   | allow sudo to user                              |
| ```useradd -s `which bash` -m -G sudo <user>``` | full : create user with bash, sudo and home dir |
| ```su <user>```                                 | switch user                                     |
| ```groups <user>```                             | see user's groups                               |
| ```deluser <user>```                            | delete user                                     |
| ```passwd``` **OR** ```passwd <user>```         | change your password or someone's else          |
| ```sudo chage --list <user>```                  | check expiration infos                          |
| ```sudo chage --expiredate -1 <user>```<br>```sudo chage --maxdays -1 <user>```     | remove expiration date for login                |

<a href="#index">back to index</a>

<a name="variables"></a>
## Variables

| Command                  | Detail                            |
| -------------------------|-----------------------------------|
| ```export FOO=bar```     | define variable                   |
| ```echo $FOO```          | display variable                  |
| ```unset FOO```          | remove variable                   |
| ```source ~/.bashrc```   | reload profile                    |
| ```env```                | list all variables                |

<a href="#index">back to index</a>