---
description: ToBeUpdated
---

# 🛣️ RoadMap / Exam Preparation

[![](https://github.com/sidneysimas/eCPPTv2-PTP-Notes/raw/main/.gitbook/assets/image%20\(1\).png)](https://github.com/sidneysimas/eCPPTv2-PTP-Notes/blob/main/.gitbook/assets/image%20\(1\).png)

Here below the **path** I used and which I would recommend to reach a level necessary to pass the exam. 👇

### Background Information

* Windows Fundamentals Module 🏠 [THM Room](https://tryhackme.com/module/windows-fundamentals)
* Intro To Offensive Security 🏠 [THM Room](https://tryhackme.com/room/introtooffensivesecurity)
* Pentesting Fundamentals 🏠 [THM Room](https://tryhackme.com/room/pentestingfundamentals)
* eJPTv2 Ine Full Course 🗒️ [eJPTv2 Notes](https://app.gitbook.com/o/s2H3MdEB0Qp2IbE58Gxw/s/PNcjhcAuvH4mlZKYrNu3/)

### Concepts and Pratice

* Post Exploitation Basics 🏠 [THM Room](https://tryhackme.com/room/postexploit)
* Sudo Security Bypass 🏠 [THM Room](https://tryhackme.com/room/sudovulnsbypass)
* Windows Privilege Escalation 🗒️ [Hackersploit Article](https://hackersploit.org/windows-privilege-escalation-fundamentals/)
* Windows Privesc Arena 🏠 [THM Room](https://tryhackme.com/room/windowsprivescarena)
* Windows Privesc 🏠 [THM Room](https://tryhackme.com/room/windows10privesc)
* Bypass UAC 🏠 [THM Room](https://tryhackme.com/room/bypassinguac)
* Post-Exploitation Basics 🏠 [THM Room](https://tryhackme.com/r/room/postexploit)
* Active Directory Basics 🏠 [THM Room](https://tryhackme.com/r/room/winadbasics)
* Enumerating Active Directory 🏠 [THM Room](https://tryhackme.com/r/room/adenumeration)
* VulnNet: Active 🏠 [THM Room](https://tryhackme.com/r/room/vulnnetactive)
* Active Directory Hardening 🏠 [THM Room](https://tryhackme.com/r/room/activedirectoryhardening)
* Compromising Active Directory🏠 [THM Room](https://tryhackme.com/module/hacking-active-directory)
* Blue 🚩 [THM CTF](https://tryhackme.com/room/blue) 🟢 - [My Writeup](https://app.gitbook.com/s/rRWtuMw6xkkeDjZfkcWC/thm/eternal-blue)
* Active 🚩 HTB CTF 🟢 - My Writeup
* Return 🚩 HTB CTF 🟢 - My Writeup
* Sauna 🚩 HTB CTF 🟢 - My Writeup
* Forest 🚩 HTB CTF 🟢 - My Writeup
* Cascade 🚩 HTB CTF 🟠 - My Writeup
* Intelligence 🚩 HTB CTF 🟠 - My Writeup
* Monteverde 🚩 HTB CTF 🟠 - My Writeup
* Resolute 🚩 HTB CTF 🟠 - My Writeup
* Blackfield 🚩 HTB CTF 🔴 - My Writeup
* Mantis 🚩 HTB CTF 🔴 - My Writeup
* Search 🚩 HTB CTF 🔴 - My Writeup

## Learning Course Topics

23 Learning Objectives, 59 Tasks, > _120 Hours of Torture_

**1 - Active Directory Enumeration**

* Use scripts, built-in tools and Active Directory module to enumerate the target domain.
* Understand and practice how useful information like users, groups, group memberships, computers, user properties etc. from the domain controller is available to even a normal user.
* Understand and enumerate intra-forest and inter-forest trusts. Practice how to extract information from the trusts.
* Enumerate Group policies.
* Enumerate ACLs and learn to find out interesting rights on ACLs in the target domain to carry out attacks.
* Learn to use BloodHound and understand its applications in a red team operation.

**2 - Offensive PowerShell Tradecraft**

* Learn how PowerShell tools can still be used for enumeration.
* Learn to modify existing tools to bypass Windows Defender.
* Bypass PowerShell security controls and enhanced logging like System Wide Transcription, Anti Malware Scan Interface (AMSI), Script Blok Logging and Constrained Language Mode (CLM)

**3 - Offensive .NET Tradecraft**

* Learn how to modify and use .NET tools to bypass Windows Defender and Microsoft Defender for Endpoint (MDE).
* Learn to use .NET Loaders that can run assemblies in-memory.

**4 - Local Privilege Escalation**

* Learn and practice different local privilege escalation techniques on a Windows machine.
* Hunt for local admin privileges on machines in the target domain using multiple methods.
* Abuse enterprise applications to execute complex attack paths that involve bypassing antivirus and pivoting to different machines.

**5 - Domain Privilege Escalation**

* Learn to find credentials and sessions of high privileges domain accounts like Domain Administrators, extracting their credentials and then using credential replay attacks to escalate privileges, all of this with just using built-in protocols for pivoting.
* Learn to extract credentials from a restricted environment where application whitelisting is enforced. Abuse derivative local admin privileges and pivot to other machines to escalate privileges to domain level.
* Understand the classic Kerberoast and its variants to escalate privileges.
* Enumerate the domain for objects with unconstrained delegation and abuse it to escalate privileges.
* Find domain objects with constrained delegation enabled. Understand and execute the attacks against such objects to escalate privileges to a single service on a machine and to the domain administrator using alternate tickets.
* Learn how to abuse privileges of Protected Groups to escalate privileges

**6 - Domain Persistence and Dominance**

* Abuse Kerberos functionality to persist with DA privileges. Forge tickets to execute attacks like Golden ticket, Silver ticket and Diamond ticket to persist.
* Subvert the authentication on the domain level with Skeleton key and custom SSP.
* Abuse the DC safe mode Administrator for persistence.
* Abuse the protection mechanism like AdminSDHolder for persistence.
* Abuse minimal rights required for attacks like DCSync by modifying ACLs of domain objects.
* Learn to modify the host security descriptors of the domain controller to persist and execute commands without needing DA privileges.

**7 - Cross Trust Attacks**

* Learn to elevate privileges from Domain Admin of a child domain to Enterprise Admin on the forest root by abusing Trust keys and krbtgt account.
* Execute intra-forest trust attacks to access resources across forest.
* Abuse SQL Server database links to achieve code execution across forest by just using the databases.

**8 - Abusing AD CS**&#x20;

* Learn about Active Directory Certificate Services and execute some of the most popular attacks.
* Execute attacks across Domain trusts to escalate privileges to Enterprise Admins.

**9 - Defenses and bypass – MDE EDR**

* Learn about Microsoft’s EDR – Microsoft Defender for Endpoint.
* Understand the telemetry and components used by MDE for detection.
* Execute an entire chain of attacks across forest trust without triggering any alert by MDE.
* Use Security 365 dashboard to verify MDE bypass.

**10 - Defenses and bypass – MDI**

* Learn about Microsoft Identity Protection (MDI).
* Understand how MDI relies on anomaly to spot an attack.
* Bypass various MDI detections throughout the course.

**11 - Defenses and bypass – Architecture and Work Culture Changes**

* Learn briefly about architecture and work culture changes required in an organization to avoid the discussed attacks. We discuss Temporal group membership, ACL Auditing, LAPS, SID Filtering, Selective Authentication, credential guard, device guard, Protected Users Group, PAW, Tiered Administration and ESAE or Red Forest

**12 - Defenses – Monitoring**

* Learn about useful events logged when the discussed attacks are executed.

**13 - Defenses and Bypass – Deception**

* Understand how Deception can be effective deployed as a defense mechanism in AD.
* Deploy decoy user objects, which have interesting properties set, which have ACL rights over other users and have high privilege access in the domain along with available protections.
* Deploy computer objects and Group objects to deceive an adversary.
* Learn how adversaries can identify decoy objects and how defenders can avoid the detection.

### Other Resources

* [Windows Privilege Escalation - Video EN](https://www.youtube.com/watch?v=n382EGuJP8Y\&list=PLJnLaWkc9xRh8hmNFWyzWMFgAHo8Lgr93)
* [OSCP Guide 10/12 – Active Directory - Video EN](https://www.youtube.com/watch?v=26M3POQ\_51A\&list=PLJnLaWkc9xRgOyupMhNiVFfgvxseWDH5x\&index=10)
* [The Cyber Mentor (TCM) - Hacking Active Directory for Beginners - Video EN](https://www.youtube.com/watch?v=VXxH4n684HE)
* [The Cyber Mentor (TCM) - Windows Privilege Escalation for Beginners - Video EN](https://www.youtube.com/watch?v=uTcrbNBcoxQ)
* [Cisco and Pentester Academy Attacking Active Directory Class with Nikhil Mittal - Video EN](https://www.youtube.com/watch?v=1fiZbYhEkYA)
* [Active Directory - John Hammond Series - Video EN](https://www.youtube.com/watch?v=pKtDQtsubio\&list=PL1H1sBF1VAKVoU6Q2u7BBGPsnkn-rajlp)

{% embed url="https://viperone.gitbook.io/pentest-everything/everything/everything-active-directory" %}

## Training and Labs

The best way to take good practice is using this lab (1st lab required more resources than 2nd)

{% embed url="https://github.com/Orange-Cyberdefense/GOAD/tree/main" %}

### Altered Security Resources <a href="#ejpt-exam" id="ejpt-exam"></a>

{% embed url="https://www.alteredsecurity.com/adlab" %}

{% embed url="https://www.alteredsecurity.com/trainings" %}

### [**Reporting**](https://github.com/sidneysimas/eCPPTv2-PTP-Notes/blob/main/readme/metasploit-and-ruby-1)

* 🗒️[How to write a PT Report — My Notes](https://dev-angelist.gitbook.io/eccptv2-ptp-notes/readme/metasploit-and-ruby-1/7.1)
* ⏩ [Writing a PT Report — TCM](https://www.youtube.com/watch?v=EOoBAq6z4Zk\&t=102s)
* ⏩ [ITProTV Report](https://www.youtube.com/watch?v=NEz4SfjjwvU\&list=WL\&index=11)
* ⏩ [OSCP — How to Take Effective Notes](https://www.youtube.com/watch?v=yYmDQY1zKKE)
* ⏩ [OSCP — How to Write a Report](https://www.youtube.com/watch?v=Ohm0LhFFwVA)

### CheatSheet <a href="#user-content-e940" id="user-content-e940"></a>

* 🗒️[ CRTP - CheatSheet](crtp-cheat-sheet.md)
