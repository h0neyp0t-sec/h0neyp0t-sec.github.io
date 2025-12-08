---
title: TryHackMe Advent of Cyber 2025 Day 4
summary: Walkthrough of the TryHackMe Advent of Cyber 2025 Day 4 - AI in Security - old sAInt nick
date: 2025-12-07
categories:
  - walkthrough
tags:
  - ai
  - TryHackMe
  - ctf
  - AdventOfCyber2025
cover:
  image: /2025/aoc_2025_day_4/Advent_of_Cyber_2025_Day_4_cover.png
  hidden: false
  responsiveImages: true
draft: false
---

# Introduction

## The Story

> The lights glimmer and servers hum blissfully at The Best Festival Company (TBFC), melting the snow surrounding the data centre. TBFC has continued its pursuit of AI excellence. After the past two years, they realise that Van Chatty, their in-house chatbot, wasn’t quite meeting their standards. 
> 
> Unfortunately for the elves at TBFC, they are also not immune to performance metrics. The elves aim to find ways of increasing their velocity; something to manage the tedious, distracting tasks, which allows the elves to do the real magic. 
> 
> TBFC, adventurous as ever, is trialling their brand new cyber security AI assistant, Van SolveIT, which is capable of helping the elves with all their defensive, offensive, and software needs. They decide to put this flashy technology to use as Christmas approaches, to identify, confirm, and resolve any potential vulnerabilities, before any nay-sayers can.

## Learning Objectives

- How AI can be used as an assistant in cyber security for a variety of roles, domains and tasks
- Using an AI assistant to solve various tasks within cyber security
- Some of the considerations, particularly in cyber security, surrounding the use of AI

# AI for Cyber Security Showcase 

> Ah, yes, artificial intelligence, that buzzword that seems here to stay. Who would have thought? Today’s room will highlight some ways AI is utilised in cyber security, along with important considerations to bear in mind when deploying AI for such tasks.
> 
> Particularly at the time of writing, AI is increasingly seen as a tool to boost speed by handling often tedious, time-consuming tasks, allowing humans to perform the real magic. Organisations want to see experience, not avoidance, in how these tools are operated.
> 
> GPT this, GPT that, we’ve all heard it often. And it’s likely to persist. As AI’s capabilities expand daily, we’ve observed a shift from AI being just “something to ask because you were too lazy to Google” (a mistake I’ve made myself). Now, AI is being embedded into everyday workflows, transforming how tasks are done and boosting productivity like never before. 
> 
> With that said, let’s begin today’s room!

I would like to add something, this might be obvious for a lot of people but not for everyone : AI is just a tool, not a replacement. It's a tool to handle time-consuming tasks, a tool to learn things, a tool to do research but AI should NOT do your work. 
Overuse of LLMs is linked to lower critical thinking abilities and a weaker ability to learn. This is why you should use AI correctly and understand in which cases you can best take advantage of this **tool**. 

I am not anti-AI, I use it myself almost every day but only for the good reasons. 

## AI in Cyber Security

> The use of artificial intelligence has seen a significant boost in cyber security. Visit almost any vendor, and they'll now have some form of AI powering a solution somewhere. Why? Well, it's not just because they're capitalising on the buzzword (although that's certainly _a part of it_), but rather, the benefits from artificial intelligence really do apply here. Let's explore some of these in the table below:

| **Features of AI**               | **Cyber Security Relevance**                                                                                      |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Processing large amounts of data | Analysing vast data from multiple types of sources. For example, system and network logs together.                |
| Behaviour analysis               | Tracking normal behaviour and activities over a period of time and flagging anything that is out of the ordinary. |
| Generative AI                    | Summarising or providing context behind a series of events.                                                       |

## Defensive Security 

> AI agents are being used in blue teaming to speed up detection, investigation, and response, making them quicker and more dependable. Acting like automated assistants, these agents continuously process telemetry (logs, network flows, endpoint signals) and add context to alerts. Furthermore, we are witnessing the integration of AI into vendor appliances—such as AI-assisted firewalling and intrusion detection systems
> 
> Beyond just detecting threats, AI can also assist in automating responses. Picture your system automatically isolating an infected device, blocking a suspicious email, or flagging an unusual login attempt — all in real time.

## Offensive Security 

> AI agents have made a notable impact on offensive security by automating and handling the often very labourious and time-consuming tasks that a pentester might traditionally undertake. 
> 
> For example, AI can be a powerful tool in a penetration tester's workflow for reconnaissance and information gathering, from OSINT to analysing noisy scanner outputs and mapping attack surfaces. This allows the pentester to spend more time on the crucial tasks that require a human touch.

## Software 

> AI-driven software development, rightfully, sounds a bit frightening. Isn't that so? Well, you wouldn't be wrong to feel this way; we've all heard about the popularity of vibe coding and the vulnerabilities introduced by AI.
> 
> However, AI has proven to be a valuable addition to the software development process in several ways. One example is a virtual "colleague" to bounce ideas off while writing the code itself. More importantly, it is used as a SAST/DAST scanner. These scanners audit and analyse written code and applications for potential vulnerabilities. 
> 
> Yes, it's somewhat ironic. AI agents can be great at identifying vulnerabilities, but are not quite as effective at writing secure code.

## Considerations of AI In Cyber Security

> Now, I’m not entirely here to sing the praises of AI and say it’s the silver bullet to all your needs. If you’ve used AI before, you’ll know the pitfalls and frustrations one can face. And nowhere is that truer than in cyber security.
>
> While the usual considerations of using AI apply, such as not owning the output from AI, there are specific factors to consider before deploying it in cyber security.
> 
> One such consideration is the use of AI in activities like offensive penetration testing. While we have discussed some of AI's applications in these areas, caution remains essential. You do not want to explain to a client that their services and websites are down because an AI has caused a race condition or overwhelmed their systems.
>
> We must think carefully about the data AI learns from, how transparent and fair its decisions are, and how reliable it remains when the unexpected occurs. We cannot assume the output from AI is 100% correct. Efforts must be made to verify the information it provides. Additionally, managing challenges such as keeping data private, securing AI models, and informing users properly requires careful consideration.

## Parctical


> Phew! Ready for an exciting exercise? You will be interacting with Van SolveIT, who will guide you through three showcases of how AI can be used in cyber security:
> - **Red:** Generate and use an exploit script.
> - **Blue:** Analyse web logs of an attack that has occurred.
> - **Software:** Analyse source code for vulnerabilities.

> When you're ready, you can access Van SolveIT at `http://10.82.139.9`. Remember, you will need to do so either from the AttackBox or your own device connected to the VPN.

> When you're ready, you can access Van SolveIT at `http://10.82.139.9`. Remember, you will need to do so either from the AttackBox or your own device connected to the VPN.

## Usage Tips 

> - Chatbot responses may appear blank for a minute or two while it generates the reply. You will start to see Van SolveIT's responses in real time.
> - If the chatbot gets confused at any time, press the **Restart Chat** button at the top right of the page.
> - As you progress throughout the showcase, stages will unlock. You can go back to any stage that you have unlocked by clicking on the stage name on the top left.

## Questions 

### Execute the exploit provided by the red team agent against the vulnerable web application hosted at `10.82.139.9:5000`. What flag is provided in the script's output after it? Remember, you will need to update the IP address placeholder in the script with the IP of your vulnerable machine (`10.82.139.9:5000`)

#### Stage 1 : Introduction
The first stage is an introduction about what you are going to explore. It's pretty straightforward but it's a nice way to provide a practical example of AI applications. 
The AI is going to ask you a question, you just have to answer "yes" or "go ahead" to have the answer.

![](/2025/aoc_2025_day_4/AI-1.png)
#### Stage 2 : Red Team
![](/2025/aoc_2025_day_4/AI-2.png)

Ask the AI to generate a script and it will provide you with a script and how to use it. Just follow the instructions and don't forget to replace the victim's machine's IP. 

![](/2025/aoc_2025_day_4/AI-3.png)

![](/2025/aoc_2025_day_4/AI-4.png)

Once the Python script is executed, an HTML web page will be returned with the flag in it.

![](/2025/aoc_2025_day_4/AI-5.png)

#### Stage 3 : Blue Team


![](/2025/aoc_2025_day_4/AI-6.png)

Here we can see how AI turned a log into a more human-readable format with context and explanation of the attack.

#### Stage 4 : Software

![](/2025/aoc_2025_day_4/AI-7.png)

Same thing here, we got an explanation and actionable steps to take, as developers.

### Complete the AI showcase by progressing through all of the stages. What is the flag presented to you? 

Once everything is done, we got a flag and the room is completed:

![](/2025/aoc_2025_day_4/AI-8.png)

Take your time, and make sure to understand everything. You can also apply this IRL. Try to finish a room with the help of your favorite LLM.

Remember, never stop learning.

[h0neyp0t](https://www.instagram.com/h0neyp0t.sec/) 