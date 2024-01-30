---
title: mat3uszm Static analysis
layout: default
---

# Static analysis

Recon is the one of the most important parts of every application security tests. Penetration testers have to know what needs to be tested. 
There are a lot of methods to get information about applications. For web applications there are a lot of scanners and plugins to web browsers for gathering information about applications. 
For mobile applications there are also a lot of scanners and tools to get more details about applications but I would like to describe one of my favorite tool MobSF. 

![screen1](/posts/static_analysis/screen1.png)

## What MobSF is ?

MobSF, or Mobile Security Framework, stands as a pivotal tool for security professionals. 
This open-source solution specializes in analyzing the security of mobile applications, offering both static and dynamic analysis for Android and iOS apps. With its robust features, MobSF enables the identification of vulnerabilities, empowering security experts to fortify mobile applications against potential threats.
Its versatility and effectiveness make it an invaluable asset in ensuring the security integrity of mobile applications in today's dynamic cybersecurity landscape.

## How to install ?

My recommendation is to use MobSF in docker. It always gives a fresh instance of a tool. Installation is very easy you just to need run script: 

### for static analysis:

```
docker run -it --rm -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest

``` 

### for dynamic analysis:

```
docker run -it --rm -p 8000:8000 -p 1337:1337 -e MOBSF_ANALYZER_IDENTIFIER=<adb device identifier> opensecurity/mobile-security-framework-mobsf:latest

```
but with dynamic analysis this https://mobsf.github.io/docs/#/dynamic_analyzer documents is worth to read. 

## What can you find in raptors ?

MobSF provides you a huge amount of information about applications. There is everything that testers need.
From basic information to in-depth insights.
At the beginning of the report you can find all basic information like name, security score, app size or amount of activities or services. 

![screen2](/posts/static_analysis/screen2.png)

In the next section you can find more interesting information. The most important of them are: 

* Decompiled code - MobSF lets testers easily read application code without any extra actions or tools. It is very important to understand how an application works. 

![screen3](/posts/static_analysis/screen3.png)

* Application permissions - MobSF displays all permission which application is required to work. It helps to prepare targeted attacks on applications. 

![screen4](/posts/static_analysis/screen4.png)
   
* Network Security - MobSF checked all included domains in application if domains are secured if not display information which of them are not secured enough.

![screen5](/posts/static_analysis/screen5.png)

* Code analysis - MobSF delivers information about all vulnerabilities included in application code base. In this part of the report tester can find not only information about what and where is vulnerabled but also CWE number if that vulnerability is well known. Very important to check this information twice because sometimes it is a false positive result. 

![screen6](/posts/static_analysis/screen6.png)

* Firebase database - MobSF can check if firebase databases are exposed publicly.

![screen7](/posts/static_analysis/screen7.png)

* Possible hardcoded secrets - MobSF report contains all possible hardcoded secrets.  

![screen8](/posts/static_analysis/screen8.png)

## Generate report. 

MobSF also contains a feature to generate a report from all analyses. Testers don’t have to worry about it. 
They can press one button and get ready to share a report in a pdf file.

## Summarise 

MobSF is a really powerful tool. You can easily read reversed code, get all information about certificates, and read leaked secrets.
MobSF does automatic code scanning and displays results in reports. I recommend using it during penetration testing of mobile applications.

### Wish you a happy hacking and happy recon. 

<style type="text/css">
    img {
        width: 55rem;
    }
</style> 
