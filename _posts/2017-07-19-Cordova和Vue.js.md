---
layout:     post
title:      "初入Cordova 和 Vue.js碰的部题（Unable to start the daemon process.）"
date:       2017-07-19 19:00:00
author:     "Chen"
header-img: "img/cordova/cordova.jpg"
tags:
    - Cordova
    - Vue.js
---

问题：（Unable to start the daemon process.）
-------------------
> 列了几点希望可以帮到你们。

### input:  
  > λ cordova build android

### output:    
```
 ANDROID_HOME=C:\Users\Administrator\AppData\Local\Android\sdk
        JAVA_HOME=C:\Program Files (x86)\Java\jdk1.8.0_131
        Subproject Path: CordovaLib
        Starting a Gradle Daemon (subsequent builds will be faster)

        FAILURE: Build failed with an exception.

        * What went wrong:
        Unable to start the daemon process.
        This problem might be caused by incorrect configuration of the daemon.
        For example, an unrecognized jvm option is used.
        Please refer to the user guide chapter on the daemon at https://docs.gradle.org/3.3/usergu
        ide/gradle_daemon.html
        Please read the following process output to find out more:
        -----------------------
        Error occurred during initialization of VM
        Could not reserve enough space for 2097152KB object heap


        * Try:
        Run with --stacktrace option to get the stack trace. Run with --info or --debug option to
        get more log output.
        Error: cmd: Command failed with exit code 1 Error output:
        FAILURE: Build failed with an exception.

        * What went wrong:
        Unable to start the daemon process.
        This problem might be caused by incorrect configuration of the daemon.
        For example, an unrecognized jvm option is used.
        Please refer to the user guide chapter on the daemon at https://docs.gradle.org/3.3/usergu
        ide/gradle_daemon.html
        Please read the following process output to find out more:
        -----------------------
        Error occurred during initialization of VM
        Could not reserve enough space for 2097152KB object heap


        * Try:
        Run with --stacktrace option to get the stack trace. Run with --info or --debug option to
        get more log output.

```
### **so: input '  Unable to start the daemon process.' in  [StackOverFlow](https://stackoverflow.com/)**

### **大概有这么几个回答：**
  1. > Try deleting your .gradle from C:\Users\<username> directory and try again.

    ![对象指针](/img/cordova/cordovaanswer0.png)
  2. > Open your project gradle.properties file and make changes in heap size.

      > XX:MaxPermSize=256m is default value and make it XX:MaxPermSize=1024m

      > gradle.properties

      ![one](/img/cordova/one.png)
      ![two](/img/cordova/two.png)
  3. > C:\Users\Administrator\.gradle change  gradle.properties file content .

  ![对象指针](/img/cordova/userGradle.png)
  ![对象指针](/img/cordova/three.png)
  4. > I got the solution by adding a new system variable name : _JAVA_OPTIONS and value : -Xmx512M

  ![对象指针](/img/cordova/four.png)

### 第4个对我起作用了.so:

![对象指针](/img/cordova/cordovaSolve.png)

   
   
   
   
   
