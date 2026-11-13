# Windows 11 tips

## Add variable in path

- windows + X / system
- 'advanced system settings' link
- 'advanced' tab
- environment variables
(close Power Shell / reopen it to get updated variables)

## Power Shell commands

Create variable for JAVA_HOME, MAVEN_HOME and add them to PATH

```text
%JAVA_HOME%\bin
%MAVEN_HOME%\bin
```

Check variables :

```bash
echo $env:PATH
echo $env:MAVEN_HOME
```

## Java
Install jdk, define variable and check with a terminal :

```bash
echo $env:JAVA_HOME
java -version
```
