---
author:
  name: "papadope"
date: 2020-7-07
linktitle: Agent Sudo
type:
- post
title: Agent Sudo
weight: 10
series:
- Tryhackme
---

![Room Image](/agentsudo/roomimage.png)

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

This is a small room to be honest. It’s one of the smallest room that i have ever done inside THM, but that doesn’t mean that it’s not fun to play with it!

---

## Enumeration

### Task 1 - How many open ports?

For this question I used nmap.

```

nmap -Pn -A -T4 -vv {target_ip}

```

![Nmap Image](/agentsudo/nmap.jpeg)

### Task 2 - How you redirect yourself to a secret page?

For this part I used BurpSuite.

You need to visit the target IP first and just read the message that Agent R left there for the users of this machine!

![Message Image](/agentsudo/message.jpeg)

I looked at source code, didn’t find anything. I also did a go buster scan to search for other directories. Nothing.

So i opened Burp Suite and intercept the request. If you look closely every Agent’s name is a single letter so we are going to use that to find the secret directory. So i set the intercept ON on Burp Suite and sent the request to Intruder. Changed the User-Agent section to a single letter, as the letter we get from the Agent R.

![Burp1 Image](/agentsudo/burp1.png)

After that i made a simple list of payloads with single letters just to see if i can find anything weird.

![Burp2 Image](/agentsudo/burp2.png)

Click Add and enter the same letters as i did. When you are ready click on Start Attack.

![Burp3 Image](/agentsudo/burp3.png)

As you can observe too, the only letter we get something weird about is the letter C. So i went to the Repeater inside Burp Suite and provided to the User-Agent section with the letter C.

![Burp4 Image](/agentsudo/burp4.png)

And we have the answer we where looking for.

###  What is the agent name?

If you set the User-Agent section to just C you get a response about a .php file. So i used that too to find the agent name by changing the path inside the request and just resend the request.

![Burp5 Image](/agentsudo/burp5.png)

Now we found the name too!

---

## Task 3 

Done enumerate the machine? Time to brute your way out.

### FTP password

For that we are going to use Hydra.

You need to run the command : 

```

hydra -l {username_you_found} -P /usr/share/wordlists/rockyou-short.txt ftp://{target_ip} -t 4 -vV

```

* I prefer first to use the short edition of the rockyou.txt file.

![Hydra Image](/agentsudo/hydra.jpeg)

### Zip File Password

After we get into the ftp with the credentials we found above we must look for what’s inside the ftp client.

![FTP Image](/agentsudo/ftp.png)

Hmm, that’s interesting. We found 3 files. I’m going to get them all to my local machine with the get command.

![Get Image](/agentsudo/getftp.png)

* You can download all the files from the FTP using the command :

```

mget *

```

After i got the files, i tried Exiftool, StegHide but nothing came up. 

I googled about other tools that i could use to get information from .jpg, .png files and i found about binwalk. So i tried with binwalk and that’s what we got!

![binwalk](/agentsudo/binwalk.png)

We see inside the new folder we got these files.

We use zip2john 8702.zip > output.txt to make the zip file a readable file for john and we go for the hash with john.

* zip2john 8702.zip > output.txt
* john output.txt

And you crack it!

### Steg Password

For this question you need to extract the file we found above first.

You need the command : 

```

7z -e 8702.zip

```

* I tried with unzip but it didn't worked.

After you enter the password you are going to see a file like this :

![7z](/agentsudo/7z.jpeg)

This is an encoded message. I decoded it with a tool called Decodify

![decodify](/agentsudo/decodify.jpeg)

Get the tool by clicking the [Decodify](https://github.com/s0md3v/Decodify) here.

And we found the answer to question 3 :)

### Who is the other agent (full name)

For this question i used StegHide. From our downloaded files we got a cute-alien.jpg, but is asking for a passphrase. 

I used the encoded message we decoded and i got a message.txt.

So from the message.txt we got both answer 4 and answer 5 :)

![steghide](/agentsudo/stehide.jpeg)

---

## What is the user flag?

Now that we got our answers and complete the task 3 we can use them to ssh to the server and find the user flag.

![userflag](/agentsudo/usertxt.jpeg)

### What is the incident of the photo called?

We see inside /home/{username_we_found} directory that there is another file “Alien_autospy.jpg”

I used the command scp to get the file locally.

```

scp <user@machineip>:Alien_autospy.jpg /localdir/

```

After opening the file and with some google search I found the answer.

## Privilege Escalation

Enough with the extraordinary stuff? Time to get real.

### CVE number for the privesc.

I did a sudo -l command here and a sudo -V in order to find something useful to exploit for privesc.

![sudo](/agentsudo/sudo.png)

I googled the version and i found a very easy one exploit we can use.

### What is the root flag? & Who is Agent R?

After you become root you can get the answer for both of these questions together.

![root](/agentsudo/root&agent.jpeg)

Thank you very much for reading.