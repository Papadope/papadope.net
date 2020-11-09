---
author:
  name: "papadope"
date: 2020-07-03
linktitle: ToolsRus
type:
- post
title: ToolsRus
weight: 10
series:
- Tryhackme
---

![Room Image](/toolsrus/roomphoto.png)

For your own information this is a room for subscribers. So it’s time to be one.

Your challenge is to use the tools listed below to enumerate a server, gathering information along the way that will eventually lead to you taking over the machine.

This task requires you to use the following tools:

* Dirbuster
* Hydra
* Nmap
* Nikto
* Metasploit

---

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

---

For the sake of exploration, I will not give the answers! But I will show you the way to find them.

#### 1 —What directory can you find, that begins with a g?

For this question we are going to use the first tool in our list : dirbuster.
You need to write this command :

``gobuster dir -w {your_wordlist} -u http://{target_ip}/ -x php,txt,html -t 40 -o ~/TryHackMe/ToysRus/gobusterscan.txt``

* I used this wordlist /usr/share/dirb/wordlists/common.txt.
* I used the -x flag to scan for files too and the -o flag to save the output inside a directory.

![dirbuster](/toolsrus/finddir.jpeg)

#### 2 — Whose name can you find from this directory?

To find the answer here, simply go to the directory we just found and read the bold name that appears.
You need to enter this path to your browser :

``http://{target_ip}/{directory_found}``

![browser](/toolsrus/findname.jpeg)

#### 3 — What directory has basic authentication?

For this question you need to read the results from dirbuster again and think what would be the right answer here or just visit every single url.

![basicauth](/toolsrus/basicauth.jpeg)

---

![protecteddir](/toolsrus/protectedir.jpeg)

#### 4 — What is bob’s password to the protected part of the website?

If you found the previous answer, you can continue here. If not try until you find the right directory because we are going to use this knowledge for the next tool we are going to use.

To find the answer to this question we need hydra.
You need to run this command :

``hydra -l {the_username_we_found} -P {your_wordlist} -f {target_ip} http-get /{the_protected_url} -t 4 -V``

* I used this wordlist : /usr/share/wordlists/rockyou-short.txt.

![hydra](/toolsrus/hydra.jpeg)

#### 5 — What other port that serves a webs service is open on the machine?

If you haven’t done a port scan with nmap yet, you need one now to answer the question.
You need to run this command :

``nmap -Pn -T4 {target_ip} -oN ~/TryHackMe/ToysRus/nmapscan.txt``

* I used the -oN flag to save the output of the nmap scan.

![nmap](/toolsrus/nmap.jpeg)

#### 6 — Going to the service running on that port, what is the name and version of the software?

``Answer format: Full_name_of_service/Version``

From the scan above, if you just visit the port, you can find the answer to this question.
Visit the url :

``http://{target_ip}:{port_you_found}``

![portfound](/toolsrus/portfound.jpeg)

#### 7 —Use Nikto with the credentials you have found and scan the /manager/html directory on the port found above.

Τhis took me a long time until nikto find something interesting.
You need to run this command :

``nikto -h http://{target_ip}:{port_you_found}/manager/html -id {username_found}:{password_found} -o ~/TryHackMe/ToysRus/niktoscan.txt``

* I used the -o flag to save the output of nikto scan.
* You should look for tomcat documentation files.

#### 8 — What is the server version (run the scan against port 80)?

We could answer this question and the question #5 with one nmap command, but i wanted to do it in steps. So for this question we are going to use nmap again.
You need to run this command :

``nmap -sV -A -T4 -vv {target_ip} -oN ~/TryHackMe/ToysRus/nmap2scan.txt``

* I use -vv flag to increase the verbosity level.
* I use -oN flag as i said before to save the output of the nmap results.

![serververs](/toolsrus/serververs.jpeg)

#### 9 —What version of Apache-Coyote is this service using?

Just read carefully the results of the same nmap scan we did in question #8 and you will find the answer waiting for you.

#### 10 —Use Metasploit to exploit the service and get a shell on the system.
What user did you get a shell as?

First of all, we need to run metasploit. Run the command : ``msfconsole``

Then we need to find what exploit we are going to use for here, so we are going to search for it.
Run the command :

``search tomcat``

![searchexp](/toolsrus/searchexploit.png)

I looked only for excellent rank and only for manager exploits. That left me with 16 & 17 options here, so i tested them.

After i found the right one, i used the command show options to find what data do i need to change.

![showoptions](/toolsrus/showoptions.jpeg)

In here the only options i need to change is :

* httppassword
* httpusername
* rhosts
* rport
* lhosts - To your THM Vpn address.

When you set the options right, you simply run the exploit and get a shell from the meterpreter session with the command shell. To find the answer to the question you need to find the user you are currently using within the shell so you need to run the command whoami and the shell will answer.

![finduser](/toolsrus/finduser.jpeg)

#### 11 —What text is in the file /root/flag.txt

Now that we are a superuser we can enter inside /root directory and simply use the command cat to read the flag.txt file.

![rootflag](/toolsrus/flagtxt.jpeg)

Thank you very much for reading.


