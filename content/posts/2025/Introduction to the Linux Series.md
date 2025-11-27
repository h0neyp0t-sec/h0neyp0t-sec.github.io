---
title: Introduction to the Linux Series
summary: Dive into the awesome world of Linux with this first article in our series designed to help you get to know this powerful operating system.
date: 2025-11-01
categories:
  ["linux"]
tags:
  ["linux", "sysadministration", "cybersecurity", "devops"]
cover:
  image: 2025/introlinuxseries/cover.png
  hidden: false
  responsiveImages: true
draft: false
---

Hello everyone, today is a great day. You are going to learn about the operating system that runs the world. 

I am starting a Linux series to help you understand how this amazing OS works and this is the first article of this series.
We are going to see enough things just to spark your curiosity but we are not going to deep dive too far into the details (I'm leaving this for the rest of the series). 

We are going to focus on the desktop AND the server side.
# Linux History
## UNIX
To better understand Linux, you have to know about the History of Unix. Linux and Unix sound a little bit familiar and Linux is a Unix-like but what is Unix ? 

The Unix history is huge but to be concise this is what you should know.

To really grasp Linux, you’ve got to take a look at UNIX. Developed back in the 1960s and 1970s at AT&T, UNIX was one of the first operating systems that could handle multiple tasks and users at the same time. Its design laid down some important ideas that shaped many systems that came after, including Linux.

UNIX introduced key concepts like process management, a hierarchical file system, and the command-line interface that we still use today. The simple and efficient philosophy behind UNIX was picked up by the creators of Linux, which is why these two systems share so much in common.

But it doesn't stop there ! UNIX has also influenced a variety of other systems. For example, the Berkeley Software Distribution (BSD) has its roots in UNIX and has contributed to several systems, including macOS (yes), which is built on a UNIX foundation. So, when you think about Linux, keep in mind that UNIX is a big part of the story.

![](/2025/introlinuxseries/macosunix.png)
> Apple UNIX Ad from the 2000. 
## GNU
When we talk about Linux, we actually talk about an operating system, but Linux itself is NOT an operating system. Linux is a **kernel**, this is the central part of an operating system that manages the computer's hardware and resources. It acts as a bridge between applications and the hardware, ensuring they communicate effectively. The kernel handles tasks such as running programs, managing memory, and controlling devices like printers and storage. 
Linux cannot work on its own without the support of additional software and tools, this is where **GNU** comes in. 

GNU (GNU's Not Unix) is a collection of free software and its story is deeply rooted to the hacker culture and the hacker community. The main goal of Richard Stallman (the creator of GNU) was to provide a free alternative to proprietary software and to encourage the free software philosophy. Actually when we talk about Linux as an operating system, we talk about **GNU/Linux**, but it's simpler to say Linux. Because of the GNU project, we have tools and commands like `ls` (to list a directory content), `cp` (to copy a file) or `rm` (to delete a file). This is not even 1% of what GNU offers. 

# Linux Distributions 
One of the biggest strength (but also a weakness in my opinion) of Linux is the number of distributions available. Debian, Ubuntu, Arch Linux, Kali Linux, Red Hat Enterprise Linux etc... There are more than 600 Linux distros. Yes, 600. But worry about this number, you don't have to know about 600 distributions to know about Linux, most of them are similar. 

A Linux distribution is made of: 
- The Linux kernel
- A package management system
- The User Space Utilities
- A desktop environment (optional)
- Some system libraries
- Some pre-Installed programs
- Configuration Files
- A documentation 
- An update/upgrade cycle
If you add all of these layers by yourself, you can create your own distribution it can be a fun project. 

The 600 Linux distros available are not all very different, most of them are based on already existing distro. For example Kali Linux, Ubuntu, Linux Mint Debian Edition and Parrot OS are all based on Debian. Look at [this list](https://upload.wikimedia.org/wikipedia/commons/1/1b/Linux_Distribution_Timeline.svg) of distributions. This is huge but like I said, don't worry about this number. 

![](/2025/introlinuxseries/redstaros.png)
> Red Star OS, the North-Korean Linux OS.

# Desktop Environment 
The Desktop Environment or the DE is basically what you see when you are using a Linux distro as well as the applications that come with certain DEs. Desktop Environments can be highly customizable, in the Linux community we are talking about "**ricing**". This is one of the aspects loved by many Linux users, if you want to see some beautiful rices, [Reddit](https://www.reddit.com/r/unixporn/) is where you want to look.
You can use 2 totally different Linux distributions but using the same DE and you can use 2 differents DEs on the same distribution. Some beginners don't understand that and they end up jumping from distro to distro (aka the "**distro hopping**") only to have a different look.

I am going to show you the most popular DEs, all on the same distribution (Debian) :
## GNOME
A modern and user-friendly environment that emphasizes simplicity and accessibility.
![](/2025/introlinuxseries/debian_gnome.png)
## KDE Plasma
Known for its rich features and high level of customization.
![](/2025/introlinuxseries/debian_kde.png)
## XFCE
A lightweight desktop environment aimed at being fast and low on system resources. 
![](/2025/introlinuxseries/debian_xfce.png)

There are more desktop environments but these three are the most commonly used.

# Linux Today
Here are some facts to understand how big Linux is: 
- Linux powers all top 500 supercomputers
- There are over 600 active Linux distributions
- 90% of cloud infrastructure relies on Linux
- You are reading this article hosted on a Linux server
- If you are reading this on an Android device, you're reading on a Linux device

# Conclusion
I tried to keep this article light, but don't worry it's just a small introduction, more things are coming next. You can reach me on Instagram if you want to give me some suggestions. 