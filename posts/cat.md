---
title: mat3uszm HTB:Don't overreact
layout: default
---

# Cat

Cat is a mobile challenge from Hack The Box. To start playing with this challenge I needed to download files and unzip it.

After unzipping I found an android backup file. So I guess to resolve this challenge and find a flag I need to extract all files from the backup file.

![photo1](/posts/cat/photo1.png)

To extract android backup file I used command:

```
( printf "\x1f\x8b\x08\x00\x00\x00\x00\x00" ; tail -c +25 cat.ab ) |  tar xfvz -
```

After extracting I got two directories shared and apps.

![photo2](/posts/cat//photo2.png)

I tried to find some strings in these files with some keywords like for example secret or HTB but I didn't find anything.

```
grep -R "key_word" *
```

![photo3](/posts/cat/photo3.png)

So I needed to do manual research to check if something would be visible on other types of files like for example photos.

I went to the shared directory and checked what I could find in photos. The DCIM directory was empty so I checked the Pictures directory.

I found a few pictures with cats and one with a guy who keeps some notes in hand. After zooming in on this picture I found a flag at the bottom of a visible piece of paper.

![photo4](/posts/cat/photo4.png)

Flag was found. 


