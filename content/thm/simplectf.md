---
author:
  name: "papadope"
date: 2020-06-24
linktitle: Simple CTF
type:
- post
- posts
title: Simple CTF
weight: 10
series:
- Tryhackme
---

![Room Image](/simplectf/simplectf.png)

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

This is a small room to be honest. It’s one of the smallest room that i have ever done inside THM, but that doesn’t mean that it’s not fun to play with it!

#### 1 — How many services are running under port 1000?

To answer that question you need to start a scan with our beautiful tool called “nmap”. Usually i save the output of the nmap scan inside a file that’s why my command have the “-oN {file_name}” at the end.

```

nmap -sV -sS -T4 -p- -vv {machine_ip} -oN portscan.txt

```

Answer : ``2``

#### 2 — What is running on the higher port?

For this question, i did a little research on the machine because i couldn’t find anything with nmap tag “ — script”. So i started a gobuster search to find available url paths. I choose to save the gobuster’s output that’s why i have the “-o {file_name}” at the end of the command.

```gobuster dir -w /usr/share/dirb/wordlists/common/txt -u http://{machine_ip} -t 20 -o gobusterscan.txt```

Go Buster result :

```

/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/robots.txt (Status: 200)
/server-status (Status: 403)
/simple (Status: 301)

```

After visiting almost every single one of them, i went to the “/simple” one and i found a CMS Made Simple Application inside!
That’s interesting.

I looked for other interesting things also and i found that a version.

![Version Image](/simplectf/version.png)

And now our beloved assistant Google. I simply googled about “cms made simple exploit” and i found that there are plenty of them.
Also, i searched with “searchsploit” because..why not?

![Searchsploit Image](/simplectf/searchsploit.png)

I saw that there is one about sql injection!
I googled the specific Injections that i have found and i stuck with this one.

[CMS Made Simple](https://www.exploit-db.com/exploits/46635)

Answer : ```CVE-2019–9053```

#### 4 — To what kind of vulnerability is the application vulnerable?

After the hole research we have done up to this point we know that we are going to make SQL Injection.

Answer : ```sqli```

#### 5 — What’s the password?

That was also fun to complete!!

First i read the hole documentation about the SQL Injection i can complete. When i scrolled down, while i was looking for a way to exploit the vulnerability, i saw a python script. I copied the script inside a file and named it exploit.py. But when i tried to run it i saw that some libraries where missing from my python so i had to install them myself.

You can check that guide to help you understand how “pip” works inside python and how to use it.

[How to Install pip](https://www.poftut.com/how-to-install-pip-in-debian-ubuntu-kali/)

```

#1- Be sure that you have pip installed.
#2- After that be sure that you have installed the libraries that python script needs to run.
#3- I didn’t had two of them. So i download these two :
- pip install requests
- pip install termcolor

```

When you are done, you can start the script!

Answer : ```secret```

#### 6 — Where can you login with the details obtained?

As we know about which ports are open, in the previous scan i made with nmap, i also scanned for services so i know that at 2222 port we have an ssh. So with these kind of information we can SSH the machine :

```

ssh {username_script_found}@{machine_ip} -p 2222

```

Answer : ```ssh```

#### 7 — What’s the user flag?

When you access the shell it’s time to see what is inside.

```

ls -la

```

You are going to notice a user.txt file.

```

cat user.txt

```

![user.txt](/simplectf/usertxt.png)

Answer : ```G00d j0b, keep up!```

#### 8 — Is there any other user in the home directory? What’s its name?

Just go to a previous directory!

```

cd ..

```

Just do ls inside the directory you are inside now :

![otheruser](/simplectf/searchingroot.png)

Done!

Answer : ```sunbath```

#### 9 — What can you leverage to spawn a privileged shell?

Well, now let’s see what can we do with “sudo” command.

![sudo](/simplectf/sudo.png)

We see now that we can use vim directory to gain root access!

```

sudo vim -c “:!/bin/sh”

```

![root](/simplectf/root.png)

Aaand did it! 😃

#### 10 — What’s the root flag?

That was a little bit easier than i though to be honest.

```

cd /root/

```

or you can simply:

```

cat /root/root.txt

```

Answer : ```W3ll d0n3. You made it!```

Thank you very much for reading.


