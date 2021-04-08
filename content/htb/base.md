---
author:
  name: "papadope"
date: 2021-04-08
linktitle: Base
type:
- post
title: Base
weight: 10
series:
- Tryhackme
---

![Room Image](/base/room.png)

## Enumeration

![nmap](/base/nmap.png)

Nmap scan.

---

![ferox](/base/ferox.png)

---

### Jump to reality

I thought brute-forcing the login page almost instantly, but brute-force is always the last resort.

I went to the **_uploaded** directory and saw that there are some uploaded files there, so we got our `uploads` directory.

Simply going to the `upload.php` page gives back nothing so I need to login somehow. I guessed that if this is a very easy box there should only be one username and that's the `admin` one.

After trying to enter as `admin` with the password `admin` I got an error that username & password are incorrect so I kept going.

### Interesting files

![login](/base/login.png)

The interesting file here is `login.php.swp`. With a bit of research after trying to get the data from the *swap* file I finally did it with **strings**.

Things I tried:

* vim -r login.php.swp
* vim login.php.swp & then `:recovery`

---

After getting the text inside the file just read it carefully.

![swp](/base/swp.png)

---

## Am I the only on who hates www-data?

Some manual searching and basic stuff done here. Versions, permissions etc.

Linpeas gave me an interesting file, that I should find it alone but I didn't thought about it cause I thought that I had already read the file.

![interesting](/base/interesting.png)

Search through the file or simply let linpeas do it's job.

## John to Root

You su as **john** and you can instantly go for **sudo -l**. The link you need is [here](https://gtfobins.github.io/gtfobins/find/#find). 


```

sudo -u root /usr/bin/find . -exec /bin/sh \; -quit

```

Thank you very much for reading.