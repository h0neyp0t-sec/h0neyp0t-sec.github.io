---
title: How To Send Sensitive Files On The Dark Net With OnionShare
summary: "This article explains how to securely send sensitive files over the dark net using OnionShare. Learn step-by-step instructions and understand the importance of privacy and anonymity in digital communications."
url: /2025/onionshare/
date: 2025-10-09
categories: ["privacy"]
tags: ["privacy", "onionshare", "dark net", "file sharing", "anonymity"]
cover:
  image:
  hidden: false
  responsiveImages: true
---
My dear hackers, hello. ✌️

Today I'm going to show you an extraordinary simple and powerful tool leveraging the Tor network to enable anonymous and secure file sharing. The name of this tool : OnionShare. 
# What is OnionShare ? 
**OnionShare** is a free and open-source tool that enables secure and anonymous file sharing using the Tor network. It allows users to send files without revealing their identities, making it ideal for sharing sensitive information securely.

![](https://onionshare.org/design-system/images/cover.png#center)
## Prerequisites
You have to install the **[Tor Browser](https://www.torproject.org/download/)** first, since OnionShare relies on the Tor network.
# Installation
Now you have the Tor Browser installed, you can install OnionShare however it's already preinstalled if you are using : 
- Parrot OS
- Whonix
- Tails
- Qubes OS

If you are using Windows, MacOS, another Linux distribution than the 4 I cited, Android (Even the De-Googled ones) or iOS, go to the [download section](https://onionshare.org/#download) and download the installation file you want. Personally I'm using Debian.
## Alternative Installation (Linux)
The official download section provides two ways to install OnionShare : 
1. [Flatpak]([https://flathub.org/apps/details/org.onionshare.OnionShare](https://flathub.org/apps/details/org.onionshare.OnionShare)) 
2. [Snap](https://snapcraft.io/onionshare)

However, a `.deb` package is available in the Debian package repository : 

```shell-session
sudo apt update && sudo apt install onionshare
```

# Usage
## Send Files
You are Alice and you want to send a sensitive file to Bob, is not complicated at all. 
1. Open the software and click on "**Share Files**" ![](/2025/onionshare/1.png)
2. Here you can drag and drop the files/directories you want to share or click on "**add**" ![](/2025/onionshare/2.png)
3. Once you add the files you want, you can click on "**Start Sharing**" ![](/2025/onionshare/3.png)
4. This is going to generate a temporary **.onion link** and a **private key** that you will have to send to the recipient (ideally via an encrypted messaging app 😉) ![](/2025/onionshare/4.png)
## Receive Files
Bob, the recipient got a link and a private key : 
1. He opens the **Tor Browser** and navigate to the link, and he will be prompted to enter the private key ![](/2025/onionshare/5.png)
2. Bob can now download the file ! ![](/2025/onionshare/6.png)

OnionShare is an amazing tool for securely and anonymously sharing sensitive files. By utilizing the Tor network, it allows users like Alice and Bob to exchange information without fear of surveillance.

If you're looking for a solution to protect your data, OnionShare might be the answer. Don't hesitate to give it a try and see for yourself how it can facilitate secure sharing and remember, never stop learning. 

h0neyp0t