
# Maven

## Commands

| Command                                                   | Detail                                            |
| ----------------------------------------------------------|---------------------------------------------------|
| ```mvn --version```                                       | installed version of Maven                        |
| ```mvn dependency:tree```                                 | check dependencies for all libraries of pom.xml   |
| ```mvn spring-boot:run```                                 | launch spring boot project                        |
| ```mvn spring-boot:run -Dspring-boot.run.profiles=dev```  | launch spring boot project with a profile         |
| ```mvn clean package```                                   | build project                                     |
| ```mvn clean package -DskipTests```                       | build project without launching tests             |
| ```mvn clean test```                                      | launch unit tests                                 |
| ```mvn test -Dtest=SomeTestClass.java```                  | launch only a specific test class                 |
| ```mvn test -Dtest=SomeTestClass.java#methodName```       | launch only a specific method from a test class   |

## Flags

| Flag | Example                                         | Detail                              |
| -----|-------------------------------------------------|-------------------------------------|
| -D   | ```mvn clean package -Dfile.encoding=UTF-8 ```  | define a system property            |
| -P   | ```mvn clean package -PDEV```                   | use a profile (named DEV)           |
| -e   |                                                 | produce execution error messages    |      
| -X   |                                                 | produce execution debug output      |      

more examples :
- skip security : ```-Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true```

## Build phases

Each of these build lifecycles is defined by a different list of build phases, wherein a build phase represents a stage in the lifecycle.   
For example, the default lifecycle comprises the following phases :
- ```validate``` validate the project is correct and all necessary information is available
- ```compile``` compile the source code of the project
- ```test``` test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
- ```package``` take the compiled code and package it in its distributable format, such as a JAR.
- ```integration-test``` process and deploy the package if necessary into an environment where integration tests can be run
- ```verify``` run any checks to verify the package is valid and meets quality criteria
- ```install``` install the package into the local repository, for use as a dependency in other projects locally
- ```deploy``` done in an integration or release environment, copies the final package to the remote repository for sharing with other developers and projects.

There are two other Maven lifecycles of note beyond the default list above :
- ```clean``` cleans up artifacts created by prior builds
- ```site``` generates site documentation for this project

**When a phase is given, Maven will execute every phase in the sequence up to and including the one defined.**


sources :  
https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html  
https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html

## Examples of pom.xml usage

### Call a command

This plugin will be called just after ```maven-clean-plugin```, since its phase is ```generate-sources```.

```xml
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-antrun-plugin</artifactId>
                <version>3.0.0</version>
                <executions>
                    <execution>
                        <id>display-message-task</id>
                        <goals>
                            <goal>run</goal>
                        </goals>
                        <phase>generate-sources</phase>
                        <configuration>
                            <target>
                                <echo message="base dir = ${project.basedir}}" />
                            </target>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
```

### Filter Spring Boot output jar

This block of code is added in ```build``` section, before the plugins.  
The purpose of this filtering is to take only the content of a subfolder, containing web resources.

```xml
<resources>
    <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
        <excludes>
            <exclude>static/foo/</exclude>
            <exclude>static/bar/</exclude>
            <exclude>static/index.html</exclude>
        </excludes>
    </resource>
    <resource>
        <directory>src/main/resources/static/foo</directory>
        <targetPath>BOOT-INF/classes/static</targetPath>
        <filtering>true</filtering>
        <includes>
            <include>**/**</include>
        </includes>
    </resource>
</resources>
```

## Misc

### Use multiple versions of jdk with Maven

Creating aliases is a good way to deal with multiple versions of jdk.
```bash
alias mvn8='JAVA_HOME=/usr/lib/jvm/some-jdk-8 && mvn'
alias mvn11='JAVA_HOME=/usr/lib/jvm/some-jdk-11 && mvn'
```

## Setup

### Basics

- download archive from official website and unpack it where you want
- create a link for this folder : ```sudo ln -s /home/USER/apache-maven-3.6.0 /home/USER/maven```
remark : this way, you don't need to update path when upgrading your version of maven 
- edit `.bashrc` :
```bash
export MAVEN_HOME=/home/USER/maven
export PATH=$MAVEN_HOME/bin:$PATH
```
- reload it : ```source ~/.bashrc```
- test it : ```mvn -version```

remark : add this link in case `settings.xml` is not found when building : <br/>
  ```ln -s /home/USER/maven/conf/settings.xml /home/USER/.m2/settings.xml```

### Encrypt passwords

First use non encrypted passwords in `settings.xml`, to confirm the access to repository is working.  
Then follow the procedure from Maven website to encrypt password.  
This will include adding `settings-security.xml`.

remark : use single quotes to encrypt passwords

```bash
mvn --encrypt-master-password 'password-clean-value'
mvn --encrypt-password 'password-clean-value'
ln -s <maven path>/conf/settings.xml /home/<user>/.m2/settings.xml
ln -s <maven path>/conf/settings-security.xml /home/<user>/.m2/settings-security.xml
```

source : https://maven.apache.org/guides/mini/guide-encryption.html