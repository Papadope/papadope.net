---
author:
  name: "papadope"
date: 2020-06-23
linktitle: Web Fundamentals
type:
- post
- posts
title: Web Fundamentals
weight: 10
series:
- Tryhackme
---

![Room Image](/webfundamentals/roomphoto.png)

For your own information this is a room for subscribers. So it’s time to be one 🚀

In this tutorial I will not develop all the tasks. As I will not quote relevant material, as the material provided by NinjaJc01 is remarkable and enough.

So let’s get past all the previous tasks and head straight to the last one.

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

### Task 5 — Mini CTF

As will be known, there are many solutions to a problem.
But I am a person who always chooses the most “difficult” path. That’s why I chose to complete this room only with the use of the curl command.

For the sake of exploration, I will not give the answers! But I will show you the way to find them.

#### 1 — What's the GET flag?

Τhe first flag is the easiest. You may already be familiar with the curl command. All you need to do is execute the command using the specific url provided by the room.

You need to run this command :

``curl {machine_ip}:8081/ctf/get``

![getflag](/webfundamentals/getflag.jpeg)

#### 2 — What’s the POST flag?

The second flag though need some research first. Some of you may already know about it but i didn’t know how to use curl command like this! Αll you needed to do was ask for it politely!

You need to run this command :

``curl {machine_ip}:8081/ctf/post -X POST — data “flag_please”``

![postflag](/webfundamentals/postflag.jpeg)

#### 3 — What’s the “Get a cookie” flag?

The third flag was about to get the cookie inside the terminal. I know how to do it on a browser but as i said before i chose the terminal way.

To be honest, it was a little tricky to find the right command here!

![getcookie](/webfundamentals/terminalcookie.jpeg)

#### 4 — What’s the “Set a cookie” flag?

Jesus Christ that flag!! I don’t know why but i was trying for at least half an hour until i get it right! I did it from the browser in seconds but it was to hard to find the right command to do it from inside the terminal!

![setcookie](/webfundamentals/setcookie.jpeg)

Thank you very much for reading.