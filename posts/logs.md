---
title: mat3uszm Logs in Mobile Apps
layout: default
---

# Logs

Sometimes the smallest things can give us huge results and lots of fun. Like for example raspberry pi computers on your side or college project. The same situation is with logs from applications. Small things but can give you huge results. 

Logs can contain a lot of informations. Sometimes in logs you can find sensitive informations like keys, unique ids and credentials. It is very important from a developer perspective to not allow to keep sensitive data in logs but as a pentester or bug bounty hunter you should remember that logs are a treasury of knowledge. 

Checking logs on android is very easy but it gives you a lot of information. You need to plug your device to the computer, turn on transfer file mode and then open the terminal on your computer. In terminal at first you should check connection between device and computer you have to enter command

```
adb devices -l 
```

![screen1](/posts/logs/photo1.png)

It checks all connected devices and asks the device for permission to read data. Remember to accept reading data permission.
In the next step you have to open the android shell in your computer terminal.
To open it use adb. How to install it you can find there.
After adb installation you have to enter 

```
adb shell 
```

command.
To read all logs you need to run the logcat tool.

```
logcat 
```

![screen2](/posts/logs/photo2.png)

Now you can see all logs from your device. I know it is really hard to read logs from one specific application. Don’t worry there is a solution.
As you probably know Android has got a Linux kernel. So some of the cli tools and commands also work on android. To display logs from specific application you need to use grep command and | symbol to transfer all logs to grep. Your command in shell should looks like 

```
logcat | grep “app_name”
```

![screen3](/posts/logs/photo3.png)

Right now reading logs is very easy and you can find a lot of interesting informations about an application.

Please don’t forget to check it because lots of informations leaks from logs.

Hopefully this post helps you to get more informations about the application which you test and make it more secure. 

### Happy hacking and happy reading logs… and blog posts. 

<style type="text/css">
    img {
        width: 55rem;
    }
</style> 
