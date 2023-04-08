---
title: mat3uszm HTB:Inject
layout: default
---

# Inject
  
Inject is one of the machines available on Hack The Box service. 

Because it is a machine at the beginning I knew only an address and nothing more about open ports, applications etc.

So my recon I started from checking open ports and paths in web applications with my prepared earlier bash script. 

This script checks all open ports and possible paths when finding a web application on 80 port. 

Beside that it generates reports with paths, open ports and full report from nmap with ports and version of application. 

I found that the 8080 port is open with a web application on it so I could start to do a recon on web application and known issues.

I started to check all possible paths and pressed buttons on the side and found one very interesting route `/upload` .

![screen1](/posts/htb_inject/screen1.png)

Of course I tried to upload their basic webshell file in php but unfortunately this upload form accepts only images.

I also tried some manipulation with a proxy to send malicious images with webshell but without success. 

After opening the image which I sent before I found that the path to the file looked very promising to prepare LFI attack. 

Local File Inclusion(LFI) attack.

I tried to send in the Repeater a few possible paths to /etc/passwd file and I got it. I can make an enumeration on files I got some information from. 

![screen2](/posts/htb_inject/screen2.png)

Of course I tried to get the user flag as fast as possible with LFI but the user.txt file was denied with my permissions. 

So I tried to get as much information from the users directory and about the application as I could.

I found what framework and modules were used in this app. It was spring with spring-cloud.

I found a very interesting file with some credentials in a  frank directory. 

![screen3](/posts/htb_inject/screen3.png)

With that information I could look for some exploit in metasploit. 

I found one exploit for the spring-cloud module and tried to get into the machine. 

![screen4](/posts/htb_inject/screen4.png)

I had to set RHOST and RPORT and easily could get into as a frank but still unfortunately can read user.txt file. 

I found [CVS-2022-22963](https://github.com/J0ey17/CVE-2022-22963_Reverse-Shell-Exploit) in a description of the exploit and tried to use this script to get into bash shell. 

After running the script I tried to login as a phil with password from a file with credential which I found in frank directory.

```
su phil
```

I read the user flag so I could go to the root flag. 

I tried to connect once again and found that I could try to do privilege escalation on bash. 

I tried to use one of the hints from [pentesterlab.blog](https://pentestlab.blog/tag/bash/).

Command was super easy: 

```
bash -p
```

I was a root user and could check everything on this machine, even the root flag. 

Hopefully my walkthrough helps you a little bit to get all the flags.

I'm really sorry for not including screenshots from the attack but I don't want to take your fun.

<style type="text/css">
    img {
        width: 55rem;
    }
</style> 
