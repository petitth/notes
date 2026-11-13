# Java - Spring Boot

<a name="index"></a>

## Table of contents
1. [Environment variables](#environment-variables)
2. [Actuators](#actuators)
3. [application.yml and profiles](#application-yaml)


<a name="environment-variables"></a>
## Environment variables

With Spring Boot, any property can also be declared with an environment variable.  
If both the property and the environment variable are present, only the environment variable will be used.  
On a Linux system, declare environment variables in ```~/.bashrc``` and reload it with ```source ~/.bashrc``` :

```bash
export USER_LOGON="user-logon-in-clear"
export USER_PWD="some-password-in-clear"
export SOME_DATASOURCES_0_PWD="some-password-in-clear-for-first-datasource"
export SOME_DATASOURCES_1_PWD="some-password-in-clear-for-second-datasource"
```

will override :

```properties
user.logon=something
user.pwd=something
some.dataSources[0].pwd=something
some.dataSources[1].pwd=something
```

remark : on Linux, every usage of the dollar character needs to be escaped

<a href="#index">back to index</a>


<a name="actuators"></a>
## Actuators

Spring Boot includes a number of additional features to help you monitor and manage your application when you push it to production.  
You can choose to manage and monitor your application by using HTTP endpoints or with JMX. Auditing, health, and metrics gathering can also be automatically applied to your application.  
link : https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html

### Settings

- add dependency

```xml
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
 </dependency>
```

- set your actuators endpoints

```yaml
management:
  endpoint:
    health:
      enabled: true
    logfile:
      enabled: true
      external-file: /tmp/application.log
    loggers:
      enabled: true
  endpoints:
    web:
      exposure:
        include: health,logfile,loggers
```

### Usage

- health
```bash
curl -X GET http://localhost:8080/actuator/health
```

which brings

```json
{"status":"UP"}
```

- loggers

```bash
# list all loggers, including those used by the libraries 
curl -X GET http://localhost:8080/actuator/loggers

# get a specific log level
curl -X GET http://localhost:8080/actuator/loggers/com.foo.bar.controller

# change a log level
curl -X POST -H 'Content-Type: application/json' -d '{"configuredLevel": "TRACE"}' http://localhost:8080/actuator/loggers/com.foo.bar.controller
```

- logfile

```bash
# get log file content
curl -X GET http://localhost:8080/actuator/logfile
```

remark : this log file will be the one set in application.yml

<a href="#index">back to index</a>


<a name="application-yaml"></a>
## application.yml

* set it on command line
```bash
--spring.config.location=$HOME/some-application.yml
```

OR use a profile :

```bash
# application-dev.yaml will be used
mvn spring-boot:run -Dspring-boot.run.profiles=dev
```

You can also associate a junit class with application-dev.yaml
```java
@ActiveProfiles("dev")
```

With Intellij Community Edition, you can set profile with `environment variables` field in the launch window : 
`spring.profiles.active=local`

* set application port

```yaml
server:
  port: 8085
```

* define logging configuration : doing this in ```application.yml``` is nice if you have only one appender, otherwise it's better to use ```logback.xml```

```yaml
logging:
  pattern:
    console: "%d{HH:mm:ss.SSS} %mdc - %-5level %logger{36} - %msg %n"
  level:
    edu.foo.bar: DEBUG
    ROOT: ERROR
```

* define a log file appender with ```application.yml``` :
 
```yaml
logging:
  file:
    name: "/tmp/foobar.log"
  pattern:
    console:
    file: "%d{HH:mm:ss.SSS} - %-5level %logger{36} - %msg %n"
  level:
    edu: TRACE
    ROOT: ERROR
```

remark 1 : deactivate console logging by removing the pattern

remark 2 : tested with Spring Boot 2.5.3, previous versions were using ```logging.file```

more infos:  https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.logging

<a href="#index">back to index</a>
