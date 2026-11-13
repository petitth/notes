# Java - Misc

<a name="index"></a> 
     
## Table of contents
1. [Compile from command lines](#command-lines-compile)
2. [Visualize all keystores](#view-keystores)
3. [Register a certificate](#register-certificate)
4. [Launch a jar with specific jdk](#force-jdk)

<a name="command-lines-compile"></a>
## Compile from command lines

```java
/*
 * compile from command lines :
 * 1) compile (and create Hello.class) : javac Hello.java
 * 2) launch it : java Hello
 */
public class Hello {
    public static void main(String[] args) {
        System.out.println("hello world");
    }
}
```

<a href="#index">back to index</a>

<a name="view-keystores"></a>
## Visualize all keystores

Use a keystore explorer : https://keystore-explorer.org/

<a href="#index">back to index</a>

<a name="register-certificate"></a>
## Register a certificate

To be able to access some repositories, like Nexus, you may need to register a certificate in your jdk.
- register it to your jdk (pwd = changeit) : 
  ```bash
  keytool -importcert -file /tmp/something.pem -alias something-ca -keystore $JAVA_HOME/jre/lib/security/cacerts
  ```
- other commands :
  ```bash
  keytool -list -v -keystore $JAVA_HOME/jre/lib/security/cacerts
  keytool -delete -alias something-ca -keystore $JAVA_HOME/jre/lib/security/cacerts
  ```

- the exact path of ```cacerts``` may be different from one jdk version to another 
  ```bash
  # open jdk 11
  $JAVA_HOME/lib/security/cacerts
  # oracle jdk 8
  $JAVA_HOME/jre/lib/security/cacerts
  ```

<a href="#index">back to index</a>

<a name="force-jdk"></a>
## Launch a jar with specific jdk

After creating a jar, you may want to launch it with a specific JRE, like Java 8, for example.<br/>
In your system, you need to compare ```JAVA_HOME``` variable and the result of ```java -version``` command.<br/>
If ```JAVA_HOME``` points to Java 8 and the commands brings you Java 11, for example, it means you are launching the jar with Java 11.<br/><br/>
To fix this, and make sure you are using Java 8, use the full path of JRE, like this : <br/>
```bash
$JAVA_HOME/jre/bin/java -jar something.jar
```

<a href="#index">back to index</a>