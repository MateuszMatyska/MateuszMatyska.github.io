---
title: mat3uszm HTB:Don't overreact
layout: default
---

# Hack The Box: Don't Overreact walkthrough

Dont Overreact is a CTF which you can find in Hack The Box mobile challenges. 

It's really good mobile challenge for begginiers in mobile application hacking. 

With this challenge you can learn how to do reverse engineering and how to check which technology was used to build application.

To start doing Dont Overreact CTF you need to download necessery files from Hack The Box site and unzip it.

After that you find an android application file.

![Apkfile](/posts/dontoverreact/photo1.png)

At first we should run it on some device to check how it works. Remember recon is very importand during hacking applications.

Unfortunately application just display Hack The Box logo.

After that we can try to reverse this applicataion. 

To reverse engineering android application we use d2j-dex2jar and apktool.

Let's start from d2j-dex2jar becaue with that tool you can read code of application. 

To use d2j-dex2jar you need to use 

```
d2j-dex2jar app_name
```

To read code I used jd-gui tool.

![jd-giu](/posts/dontoverreact/photo2.png)

From code I can tell that applcation is written in React Native. React Native is a framework to build mobile application with Javascript. 

So in next step I need to check how javascript code looks like. To find code I need to reverse application with apktool and find bundle file in assets directory.

```
apktool d app_name
```

Code is super unreadable.

![code](/posts/dontoverreact/photo3.png)

To make it more clear and readable I used [js-beautify](https://github.com/beautify-web/js-beautify)

```
js-beautify index.android.bundle -o new_file_name.js 
```

Now looks better

![beautify](/posts/dontoverreact/photo4.png)

In t variable you can find debug property what looks very similar to base64 code. It's really worth to check if it is a flag.

```
var t = {
        importantData: "baNaNa".toLowerCase(),
        apiUrl: 'https://www.hackthebox.eu/',
        debug: 'SFRCezIzbTQxbl9jNDFtXzRuZF9kMG43XzB2MzIyMzRjN30='
    };
```

To check it you can try to decode it with some webservices to decode base64 codes.

After check it I can say that was a right secret code.  

Nice, flag was found.