---
author:
  name: "papadope"
date: 2021-06-05
linktitle: CatPictures
type:
- post
title: CatPictures
weight: 10
series:
- Tryhackme
---

![Room Image](/catpictures/roompicture.jpeg)

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

This is a small room to be honest. It’s one of the smallest room that i have ever done inside THM, but that doesn’t mean that it’s not fun to play with it!

---

## Enumeration

![nmap](/catpictures/nmap.png)

These are the open ports. But, as you can see, if you have already done a *nmap* scan, that you are missing a port. I am going to explain everything, don't worry.

### Port 8080

Getting the website as it seems running in phpBB. Try registering in the form you get some errors about MX records while you are trying to register with a valid or not email. So I left it like it is and kept going. After *clicking* and *searching* almost everything manually, I found that :

![phpBB](/catpictures/enum_find.png)

Please notice that if you are going to fuzz the box, do it carefully. Until I realize that I am the one that kills the box I lost almost 40 minutes trying to find out why my box dies.

---

### Port Knocking

The process of **PortKnocking** amuzes me almost every time. Seems like a magic trick to me.
I thought almost instantly that these numbers are useful for finding the password of **4420 port**.

I read articles about *magic numbers* and how can I use them. I start messing with them a lot before thinking simply knock the ports.

I used **knockd**. I personally find it really easy to use.

```

knock <MACHINE_IP> 1111 2222 3333 4444

```

### FTP Explanation

After knocking these ports, you can see that if you start scanning the machine again, a magic port will appear. Connect and see

![ftp](/catpictures/ftp.png)

* If you read the **note.txt** you have it!

## Reverse Shell

1. Start your listener.
2. Execute a command to get a response.
3. Try to elevate to a tty.

Then enumerate more.

---

## Next step

Search and search. After a while I found an interesting file. After I used *cat* on it I got :

![file](/catpictures/hmm.png)

Run the file, use the password and check the directory. An **id_rsa** has been created. Copy it and gain access.

First flag found!

## Enumerate again

![weird](/catpictures/weird.png)

When I first saw this, I said *Hmm ok, nothing weird here, I need to search more*. But after a while I saw that **.bash_history** is not linked anywhere (ex. **/dev/null**) so I opened the file to read it. 

* Check carefully for any **paths** that you might need to investigate.

---

## Becoming the King

1. Create a file locally with a revshell inside.

![rv](/catpictures/rev_python.png)

2. Start a python server.

3. Change the script ( **clean.sh** ) & curl it through.

![curlitis](/catpictures/curlitis.png)

4. Start your listener, become root while wait.

Root is yours.
Well done.

Thank you very much for reading.