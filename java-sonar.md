# Checking the quality of a maven java application with Sonar on Linux

## What is Sonar ?

Sonar is a tool to check to quality of an application, which provides :
- the percentage of code which is covered by tests, with line by line detail on each class
- quality information on the syntax of the code
- information related to security on the code and project dependencies

## Install a local Sonar for a maven project

- unpack sonarqube 7.8 <br>
https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.8.zip

- edit bashrc then reload it with : source ~/.bashrc
```bash
    export SONAR_HOME=$HOME/sonarqube-7.8/bin/linux-x86-64
    export PATH=$PATH:$SONAR_HOME
    alias sonar_rebuild='mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent test -Dmaven.test.failure.ignore=true'
    alias sonar_refresh='mvn sonar:sonar -Dsonar.host=http://localhost:9000'
```
    
- launch sonar
```bash
    cd $SONAR_HOME
    sonar.sh start
```
    
- import your custom rules (xml), if you have, in sonar page 
    - login as admin/admin first with http://localhost:9000
    - header \ quality profiles
    - right corner : 'restore' button
    - then select the xml with the rules    
    
- build report from your project with maven, using defined aliases **sonar_rebuild** and **sonar_refresh**  

- consult the results in local sonar : http://localhost:9000  
remark : there's a small loading time between the moment you launch Sonar and the moment you can access its web interface

## Use a Docker version of Sonar

```yaml
#
# Launch it with : docker-compose -f docker-compose-sonar.yaml up -d
# Access local sonar with : http://localhost:9000
#
# To get your project metrics in sonar :
# 1) build metrics : mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent test -Dmaven.test.failure.ignore=true
# 2) refresh sonar : mvn sonar:sonar -Dsonar.host=http://localhost:9000
# remark : create aliases for these commands to make your life easier
#
version: "3.8"

services:
  sonar:
    image: "sonarqube:7.8-community"
    container_name: my-sonar
    ports:
      - 9000:9000 
```

You can create a bash script to load quality profiles from an xml :

```bash
#!/bin/bash
#
# Apply custom config to an existing running sonar container.
#
# read me
# =======
# - make sure the script is executable : chmod +x ./sonar-apply-rules
# - launch it : ./sonar-apply-rules
#

# use ansi escape codes for colors
NOCOLOR='\033[0m'
CYAN='\033[0;36m'

echo -e "${CYAN}--- load quality profiles ---${NOCOLOR}"
curl -v POST -u admin:admin "http://localhost:9000/api/qualityprofiles/restore" --form backup=@`pwd`/sonar-rules.xml
echo " "
echo -e "${CYAN}--- done loading quality profiles ---${NOCOLOR}"
exit 0
```

## Additional infos

### Change default port of Sonar

- edit sonar.propperties
```bash
    cd $SONAR_HOME
    cd ../../conf/
    vi sonar.properties
```

- uncomment the line "sonar.web.port" and update it

- restart sonar
```bash
    cd $SONAR_HOME
    sonar.sh stop
    sonar.sh start
```
- adapt 'sonar_refresh' alias in your bashrc

### Remove a project 

- authenticate as admin 
- go to your project
- in menu : administration / deletion

### Exclude classes from Sonar processing

add properties in your pom.xml :
```xml
<properties>
  <sonar.exclusions>**/foo/bar/**</sonar.exclusions>
  <sonar.coverage.exclusions>**/foo/bar/**</sonar.coverage.exclusions>
</properties>
```