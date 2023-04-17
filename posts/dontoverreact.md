---
title: mat3uszm HTB:Don't overreact
layout: default
---

# Don't Overreact walkthrough

Don't Overreact is a CTF which you can find in Hack The Box mobile challenges.

It's a really good mobile challenge for beginners in mobile application hacking. 

With this challenge you can learn how to do reverse engineering and how to check which technology was used to build application.

To start doing Don't Overreact CTF you need to download necessary files from Hack The Box site and unzip it.

After that you find an android application file.

![Apkfile](/posts/dontoverreact/photo1.png)

At first we should run it on some device to check how it works. Remember recon is very important during hacking applications.

Unfortunately the application just displays the Hack The Box logo.

After that we can try to reverse this application.

To reverse engineering android applications we use d2j-dex2jar and apktool.

Let's start from d2j-dex2jar because with that tool you can read the code of the application. 

To use d2j-dex2jar you need to use 

```
d2j-dex2jar app_name
```

To read the code I used the jd-gui tool.

![jd-giu](/posts/dontoverreact/photo2.png)

From the code I can tell that the application is written in React Native. React Native is a framework to build mobile applications with Javascript.  

So in the next step I need to check how javascript code looks like. To find code I need to reverse the application with apktool and find the bundle file in assets directory.

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

In t variable you can find a debug property that looks very similar to base64 code. It's really worth checking if it is a flag.

```
var t = {
        importantData: "baNaNa".toLowerCase(),
        apiUrl: 'https://www.hackthebox.eu/',
        debug: 'SFRCezIzbTQxbl9jNDFtXzRuZF9kMG43XzB2MzIyMzRjN30='
    };
```

To check it you can try to decode it with some web services to decode base64 codes.

After checking it I can say that was the right secret code.  

Nice, flag was found.