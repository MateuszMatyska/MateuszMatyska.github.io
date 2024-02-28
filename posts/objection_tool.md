---
title: mat3uszm Objection
layout: default
---

# Objection

As documentation said, Objection is “ a runtime mobile exploration toolkit, powered by Frida, built to help you assess the security posture of your mobile applications, without needing a jailbreak.” Link to github project site [Objection GitHub Site](https://github.com/sensepost/objection). 

## How to install

Installation is super easy because to install Objection you just need to use pip - python package manager. Command to install Objection below:

```
pip3 install objection
```

It would be better to consider if installation Objection in virtual env be better idea because with python venv you receive extra secure layer when it comes to your OS and sensitive data on it. 


## How to use it with root device 

Natural environment for mobile application security testing is rooted/jailbroken devices.
To run Objection with an application installed on a rooted device at first you should remember about running frida-server on your device. Please remember to connect your device and check with adb if it detected. About Frida and scripts I'll write in next blogpost. 
In the next step you should check what id has got your tested application.
To check it enter in your terminal

```
firda-ps -U
```
Last step is running objection with gadget

```
objection -g "Application_Id" explore 
```

With that we can start testing applications on rooted device.

## How to use it without root device

On devices without root the situation looks different. You don't have to install and run frida server but patch your application.
To patch applications you need to install aapt, adb, jarsigner, apktool before patching.
More information about it you can find here [Application Patching](https://github.com/sensepost/objection/wiki/Patching-Android-Applications). 
With that tools patching application required just to enter one command to your terminal 

```
objection patchapk --source app-release.apk
```

After that install patched application and run objection with 

```
objection explore 
```

## What Objection can do

Objection looks like a swiss butterfly knife in cyber security. You can test almost everything in application. 
You can check all files and check stores. Check what envs contain and what activities and classes the application contains.

## The most important objection tools
### envs

In environment variables you can find a lot of interesting informations for example passwords or paths.
To get all variables with paths from environments you have to enter the env command.

![screen1](/posts/objection/screen1.png)

### file download

To download file which you find in directories structure use 

```
file download file_name
```

### Memory

Next very helpful tool included in Objection is the memory tool. This tool allows you to check the list of loaded modules in the current process or all exports in the module. Memory tool allows you also to make a memory dump of the current application state.  

![screen2](/posts/objection/screen2.png)

### Keystore

Objection with all tools allows you also to check keystore and intents. Exists a huge possibility that in the keystore you find a lot of information about applications or users of application. To list all Aliases with keys and certificates you have to use keystore list options.

![screen3](/posts/objection/screen3.png)

### Hooking

Hooking is a command which helps you to get information about activities, services, classes and methods in class. Hooking command has got a few additional options:   
* list
* get
* search
* set

These few options can provide you with a lot of information about application. List option allows you to list all activities or classes that are included in the application.

![screen4](/posts/objection/screen4.png)

Get options allows you to get information about current activity. 

![screen5](/posts/objection/screen5.png)

Of course you can search classes or methods which are included in the tested application. The command to search items looks similar to the commands above. You need to insert what would you like to search and name for example: 

![screen6](/posts/objection/screen6.png)

The last option which I would like to present is the set option. Set is very helpful when you have to return always the same value from the method. With this option you can hack all badly secured forms and validations in application. Example of set use case

![screen7](/posts/objection/screen7.png)

### Inject Frida script

With Objection you can easily inject Frida scripts. More about Frida and scripts I will write in my next blogpost. 
Objection allows you to inject scripts only with one command, what makes injectioning faster and more comfortable.

![screen8](/posts/objection/screen8.png)

## Conclusion

I warmly recommend using objection in the recon phase and for penetration testing. There are a lot of tools which help testers better understand application and test all aspects of application. 

### Wish you a happy hacking and testing Objection tool.

<style type="text/css">
    img {
        width: 55rem;
    }
</style> 
