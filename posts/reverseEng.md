---
title: mat3uszm Reverse engineering
layout: default
---

# Reverse engineering

Reverse engineering is a process which helps to understand how an application works and how an application is built.

It is a reverse process to the development process because from compiled or build application receive code.  

Of course processes and tools for reverse engineering are different for iOS and Android platforms.

## Android

Let’s start from the Android platform.

To reverse applications and get information you can use a few tools. 

In this article I would like to describe two of them: Dex2Jar and ApkTool.

### dex2jar

[dex2jar](https://github.com/pxb1988/dex2jar/tree/2.x) as you can read in github repo dex2jar is a tool which work with android dex and java class files.

dex2jar decompiles an android application and gives you a jar application file.

You can open a jar file with [jd-gui](https://github.com/java-decompiler/jd-gui) and receive application code and structure. 

With code and structure you can do a static analysis and understand how the application works. 

You can follow all important methods and functions and try to find some vulnerabilities which you can use to get sensitive information from the application. 

It is always worth it to check if developers left some hardcoded values like for example credentials. 

Definitely hardcoded values are huge issues when it comes to mobile application security.

To reverse android application with dex2jar you have to use 

```
d2j-dex2jar application_name.apk
``` 

![screen1](/posts/reverseEng/ph1.png)

You can find the dex2jar tool in ParrotOs Security as a pre-installed tool.

### Apktool

The next Android platform tool is [ApkTool](https://ibotpeaches.github.io/Apktool/).

Apktool is a tool which decodes resources to nearly original. 

After the reverse process with Apktool you can receive smali files, AndroidManifest and all resources from applications like for example strings.

This type of reversing application doesn't give you a structure of project but directory structure.

With this structure you can also get a lot of information.

For example with apktool you can check javascript or typescript code from React Native applications.

React part of code you can find in assets directory with index.bundle name.

Example of that case you can find in one of my previous blog posts [Hack The Box: Don’t Overreact walkthrough](https://mateuszmatyska.github.io/posts/dontoverreact.html).

To reverse application with apktool you have to use command:

```
apktool d application_name.apk
```  

![screen2](/posts/reverseEng/ph2.png)

## iOS

On iOS, the reverse engineering process looks a little bit harder than on Android. 

Because iOS applications are compiled to bitcode, you have to read an assembly code. 

Of course you can treat iOS code like android and check hardcoded values and look for some credentials.  

Because of assembly code following methods and functions sometimes could be harder but of course you can do everything if you want to. 

To reverse iOS applications you can use for example [Hopper](https://www.hopperapp.com/) application.

![screen3](/posts/reverseEng/ph3.png)

It is an application with a user-friendly graphical interface so you don’t need any commands to reverse the application.

Of course it is better to have a macbook device to reverse iOS applications because of limitations.

## Summarise 

Reverse engineering is a very powerful step when it comes to checking mobile application security. 

It should be one of the first steps in application recon. 

You can get a lot of information about applications with this process.

There are a lot of tools which reverse applications for you and help you to understand how it works. 

In the end I would like to say that there is  one universal application for android and ios applications which is called [Ghidra](https://ghidra-sre.org/) but I would like to describe this tool in one of my next blog posts. 


### Wish you a happy hacking and happy reverse engineering.    

<style type="text/css">
    img {
        width: 55rem;
    }
</style> 
