---
author:
  name: "papadope"
date: 2020-06-29
linktitle: Nmap
type:
- post
- posts
title: Nmap
weight: 10
series:
- Tryhackme
---

![Room Image](/nmap/nmap.png)

## What is Nmap?

Nmap, short for Network Mapper, is a free, open-source tool for vulnerability scanning and network discovery. Network administrators use Nmap to identify what devices are running on their systems, discovering hosts that are available and the services they offer, finding open ports and detecting security risks.

Nmap can be used to monitor single hosts as well as vast networks that encompass hundreds of thousands of devices and multitudes of subnets.

Though Nmap has evolved over the years and is extremely flexible, at heart it’s a port-scan tool, gathering information by sending raw packets to system ports. It listens for responses and determines whether ports are open, closed or filtered in some way by, for example, a firewall. Other terms used for port scanning include port discovery or enumeration.

You can read more inside the official site of nmap.org :

[Nmap Official](https://nmap.org/)

---

## Deploy

First of all, let’s deploy our machine. So click on the green deploy button if you haven’t done it already. Afterwards, to access the machine, you need to be inside TryHackMe network. So, get connected to THM VPN. We are not going to talk here about how can you connect to the VPN but there are awesome guides out there that can help you manage the frustration.

---

#### 1 — First, how do you access the help menu?

Answer : ``-h``

For the sake of exploration, I will not give the rest of the answers! But I will show you the way to find them :)

There is a must have cheat sheet you need to read before you get started with Nmap :

[Cheat Sheet](https://www.stationx.net/nmap-cheat-sheet/)

They also provide it in pdf, so you dont need to be online all the time to read the Nmap Cheat Sheet.

Also, as i already provided the first answer, you can read most of the answers of Task 2 inside nmap help page with the command nmap -h.

Or you can read the hole Nmap Reference Guide you can find inside your machine with the command man nmap.

---

#### 1 — Let’s go ahead and start with the basics and perform a syn scan on the box provided. What will this command be without the host IP address?

Answer : ``nmap -sS``

#### 2 — After scanning this, how many ports do we find open under 1000?

Answer : ``Look inside the photo``

![openports](/nmap/openports.png)

#### 3 — What communication protocol is given for these ports following the port number?

Answer : ``You can easily find the answer from the photo``

#### 4 — Perform a service version detection scan, what is the version of the software running on port 22?

#### 5 — Perform an aggressive scan, what flag isn’t set under the results for port 80?

#### 6 — Perform a script scan of vulnerabilities associated with this box, what denial of service (DOS) attack is this box susceptible to? Answer with the name for the vulnerability that is given as the section title in the scan output. A vuln scan can take a while to complete. In case you get stuck, the answer for this question has been provided in the hint, however, it’s good to still run this scan and get used to using it as it can be invaluable.

We are going to find these 3 questions together with a single nmap scan!

```

nmap -sC -sV -T4 -A -vv {machine_ip}

```

![nmapscan](/nmap/nmapscan.png)

---

![nmapscan](/nmap/nmapscan1.png)

Answer : ``You can find the answer from doing the same scan or just reading these photos``

Thank you very much for reading.