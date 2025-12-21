---
title: TryHackMe Advent of Cyber 2025 Day 11
summary: Walkthrough of the TryHackMe Advent of Cyber 2025 Day 11 - XSS - Merry XSSMas
date: 2025-12-14T14:00:00
categories:
  - walkthrough
tags:
  - web hacking
  - TryHackMe
  - ctf
  - AdventOfCyber2025
cover:
  image: /2025/aoc_2025_day_11/Advent_of_Cyber_2025_Day_11_cover.png
  hidden: false
  responsiveImages: true
draft: false
---
# Introduction
## The Story

>After last year's automation and tech modernisation, Santa's workshop got a new makeover. McSkidy has a secure message portal where you can contact her directly with any questions or concerns. However, lately, the logs have been lighting up with unusual activity, ranging from odd messages to suspicious search terms. Even Santa's letters appear to be scripts or random code. Your mission, should you choose to accept it: dig through the logs, uncover the mischief, and figure out who's trying to mess with McSkidy.

## Learning Objectives
- Understand how XSS works
- Learn to prevent XSS attacks

# Leave the Cookies, Take the Payload

>For today's room we will be using a the web app found under http://10.82.140.138. You can use the browser of your AttackBox to navigate to it. You will see a page as shown below:

![Portal page](https://tryhackme-images.s3.amazonaws.com/user-uploads/6890a24ce8a6d10e9ad22d67/room-content/6890a24ce8a6d10e9ad22d67-1761292275782.png)

>Let's review some key material regarding potential attacks on websites like this portal, specifically [Cross-Site Scripting (XSS)](https://tryhackme.com/room/axss). 

>XSS is a web application vulnerability that lets attackers (or evil bunnies) inject malicious code (usually JavaScript) into input fields that reflect content viewed by other users (e.g., a form or a comment in a blog). When an application doesn't properly validate or escape user input, that input can be interpreted as code rather than harmless text. This results in malicious code that can steal credentials, deface pages, or impersonate users. Depending on the result, there are various types of XSS.  In today’s task, we focus on **Reflected XSS** and **Stored XSS**.

## Reflected XSS

>You see reflected variants when the injection is immediately projected in a response. Imagine a toy search function in an online toy store, you search via:

`https://trygiftme.thm/search?term=gift`

>But imagine you send this to your friend who is looking for a gift for their nephew (please don't do this):

`https://trygiftme.thm/search?term=<script>alert( atob("VEhNe0V2aWxfQnVubnl9") )</script>`

>If your friend clicks on the link, it will execute code instead.

**Impact**

>You could act, view information, or modify information that your friend or any user could do, view, or access. It's usually exploited via phishing to trick users into clicking a link with malicious code injected.

## Stored XSS

>A Stored XSS attack occurs when malicious script is saved on the server and then loaded for every user who views the affected page. Unlike Reflected XSS, which targets individual victims, Stored XSS becomes a "set-and-forget" attack, anyone who loads the page runs the attacker’s script.

>To understand how this works, let’s use the example of a simple blog where users can submit comments that get displayed below each post.

## Normal Comment Submission

```
POST /post/comment HTTP/1.1 
Host: tgm.review-your-gifts.thm
postId=3
name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=This gift set my carpet on fire but my kid loved it!
```        

>The server stores this information and displays it whenever someone visits that blog post.

## Malicious Comment Submission (Stored XSS Example)

>If the application does not sanitize or filter input, an attacker can submit JavaScript instead of a comment:

```
POST /post/comment HTTP/1.1 
Host: tgm.review-your-gifts.thm  

postId=3 name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script> + "This gift set my carpet on fire but my kid loved it!"
```

>Because the comment is saved in the database, every user who opens that blog post will automatically trigger the script.

>This lets the attacker run code as if they were the victim in order to perform malicious actions such as: 
>- Steal session cookies
>- Trigger fake login popups
>- Deface the page

## Protecting against XSS

>Each service is different, and requires a well-thought-out, secure design and implementation plan, but key practices you can implement are:
>- **Disable dangerous rendering raths:** Instead of using the `innerHTML` property, which lets you inject any content directly into HTML, use the `textContent` property instead, it treats input as text and parses it for HTML.
>- **Make cookies inaccessible to JS:** Set session cookies with the [HttpOnly](https://owasp.org/www-community/HttpOnly), [Secure](https://owasp.org/www-community/controls/SecureCookieAttribute), and [SameSite](https://owasp.org/www-community/SameSite) attributes to reduce the impact of XSS attacks.
>- **Sanitise input/output and encode:**
>- In some situations, applications may need to accept limited HTML input—for example, to allow users to include safe links or basic formatting. However it's critical to sanitize and encode all user-supplied data to prevent security vulnerabilities. Sanitising and encoding removes or escapes any elements that could be interpreted as executable code, such as scripts, event handlers, or JavaScript URLs while preserving safe formatting.

>To exploit XSS vulnerabilities, we need some type of input field to inject code. There is a search section, let's start there.

## Exploiting Reflected XSS

>To exploit reflected XSS, we can use test payloads to check if the app runs the code injected. If you want to test more advanced payloads, there are [cheat sheets](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) online that you can use to craft them. For now, we'll pick the following payload:

`<script>alert('Reflected Meow Meow')</script>`

>Inject the code by adding the payload to the search bar and clicking " **Search Messages**". If it shows the alert text, we have confirmed reflected XSS. So, what happened?
>- The search input is reflected directly in the results without encoding
>- The browser interprets your HTML/JavaScript as executable code
>- An alert box appeared, demonstrating successful XSS execution

>You can track the behaviour and how the system interprets your actions by checking the " **System Logs**" tab at the bottom of the page:

![System logs](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/61a7523c029d1c004fac97b3-1760342225658.png)

>Now that we have confirmed reflected XSS, let's investigate if it's susceptible to stored XSS. This vector must be different, as it needs to be persisted. Looking at the website, we can see that you are able to send messages, which are stored on the server for McSkidy to view later (as opposed to searching, which is stored temporarily on the client side).

>Navigate to the message form, and enter the malicious payload we used before (others work too): 

`<script>alert('Stored Meow Meow')</script>`

>Click the " **Send Message**" button. Because messages are stored on the server, every time you navigate to the site or reload, the alert will display.

## Wrapping Up

>So it's confirmed! The site is vulnerable to XSS; it's no wonder that unusual payloads have been detected in the logs. The team will now harden the site to prevent future malicious code from being injected.

# Questions

## Which type of XSS attack requires payloads to be persisted on the backend?

Stored - To remember that, think about a database, the purpose is to **store** data. 

## What's the reflected XSS flag?
`THM{Evil******}` - Copy and paste the first payload into the Search Previous Messages field.

## What's the stored XSS flag?
`THM{Evil_**********}` - Copy and paste the second payload into the Send a Message to McSkiddy field

# Conclusion

We have learned about XSS and how to exploit a basic reflected (non persistent) and a stored (persistent) XSS vulnerability. You can also complete the [Intro to Cross-site Scripting](https://tryhackme.com/room/xss) room for more !

Never stop learning. 

[h0neyp0t](https://instagram.com/h0neyp0t.sec)