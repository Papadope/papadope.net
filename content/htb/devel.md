---
author:
  name: "papadope"
date: 2021-02-23
linktitle: Devel
type:
- post
title: Devel
weight: 10
series:
- HackTheBox
---

![Room Image](/devel/room.png)

## Enumeration

From a really quick nmap we can realize that there are 2 open ports on the machine.

![nmap](/devel/nmap.png)

---

## FTP Anonymous Login

Getting that FTP really quick we can see that there are some interesting files and a super useful folder inside (I have already uploaded some files while I was trying to get a RV) :

![ftp](/devel/ftp.png)

---

After a really simple Google Search I found that I can create a payload with **msfvenom**

![msfvenom](/devel/creatingpayload.png)

Set up the same payload in exploit/multi/handler and you shell will pop up!

![RV](/devel/meterpreter.png)

---

## Stop Looking For Flags

I am in penetration testing almost 8 months now and one of the **MANY** things that I've learned is to stop looking for flags immediatelly. Get to know your **target**. The best you know the easiest will become to get the flags or to find a vulnerability/exploit that will help you to **privesc** and get NT Authority fast & easy.

## MSF Exploit Suggester

![exploitsuggester](/devel/exploitsuggester.png)

Running an exploit suggester through the meterpreter session reveals some useful information about the target. I searched almost every single one of them and that's the reason I could find more than one ways to get **NT Authority/System** on that box.

---

## MS_10_015 Privesc

That was easy. Set your active session on the payload:

```

windows/local/ms10_015_kitrap0d

```

Set the lhost,lport & exploit.

![privesc](/devel/privesc.png)

Job done. Flags are pretty obvious and you are going to find them both really quick.

## Bonus Session

I was thinking about OSCP (as always) and I wanted to do it without msf. So I searched for something more "manually" and I found another way to privesc by uploading an .exe file that was already compiled. I took it from a github repo. You can check this repo [here](https://github.com/SecWiki/windows-kernel-exploits/tree/master/MS10-059).

Thank you very much for reading.