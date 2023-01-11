---
title: mat3uszm HTB:APKey
layout: default
---

# APKey
Apkey is the next challenge from Hack The Box service. As with all mobile challenges you need to download files before playing with this challenge. In downloaded file you can find android application file APKey.apk

On the running application I saw there is a simple login form with two inputs and a button.

![screen](/posts/apkey/APKey.jpg)

Probably after login I can read a flag. So let's start with a classic recon.
 
At first reverse engineering. It is always better to know how an application works. Unfortunately the application is obfuscated and I couldn't read anything from the code. 

![screen1](/posts/apkey/screen1.png)

To be honest I didn't know what I should do in the next step. I didn't know any vector of attacks. I tried the sql injection command but it didn't work. 
So I tried to use MobSF to scan applications for vulnerabilities and found some weak cryptography.

![screen2](/posts/apkey/screen2.png)

MobSF is a really powerful tool. I found some vulnerability and code with this issue even with obfuscated code.

![screen3](/posts/apkey/screen3.png)

I analyzed code and found that checking code uses some cryptography and password from other classes in MainActivity.

![screen4](/posts/apkey/screen4.png)

So I could say that I crack this app because with this information I can use frida and read flag without login to the app. More about Frida you can read here. I prepared a frida script and injected it into the app.

```
Java.perform(function () {
	const cbab = Java.use('c.b.a.b')
	const cbag = Java.use('c.b.a.g')
	
	console.log(cbab.a(cbag.a()))
});
```

And voila I found a flag I could read it from my console.
 

<style type="text/css">
    img {
        width: 55rem;
    }
    img[alt="screen"] {
        width: 25rem;
    }
</style> 

