---
title: Writeup - TryHackMe BankGPT
summary: Writeup of the BankGPT TryHackMe room.
date: 2025-12-02
categories:
  ["writeup"]
tags:
  ["ctf", "pentesting", "TryHackMe", "AI"]
cover:
  image: 2025/bankgpt_writeup/cover.png
  hidden: false
  responsiveImages: true
draft: false
---

> **Description** : Meet BankGPT, a well-mannered digital assistant built to help staff at a busy financial institution. It keeps an eye on sensitive conversations that move through the bank each day. 
> Whenever staff discuss procedures, internal notes, or anything that should stay behind the counter, BankGPT quietly absorbs it all. It isn't supposed to share what it knows, and the system administrators carefully review everything you send to it. Ask the wrong question too bluntly, and it may tighten up or alert the people who monitor it. If you want to coax anything useful out of this assistant, you'll need to take your time, stay subtle, and work around its guardrails.

The room is pretty easy to solve. We have an LLM not well secured, look how I solved it : 

![](/2025/bankgpt_writeup/bankgpt_flag.png)