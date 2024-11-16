---
description: AS - Certified Red Team Professional (CRTP) - Notes
---

# 📝 Certified Red Team Professional (CRTP) - Notes

<div align="left">

<figure><img src=".gitbook/assets/image.png" alt="" width="207"><figcaption><p><a href="https://www.alteredsecurity.com/post/certified-red-team-professional-crtp">https://www.alteredsecurity.com/post/certified-red-team-professional-crtp</a></p></figcaption></figure>

</div>

{% embed url="https://www.alteredsecurity.com/post/certified-red-team-professional-crtp" %}

{% hint style="info" %}
All contributions to the project to improve it, add new contributions, correct it and update it are welcome.
{% endhint %}

## What is Certified Red Team Professional? <a href="#viewer-d2lek" id="viewer-d2lek"></a>

Altered Security's Certified Red Team Professional (**CRTP**) is a beginner friendly hands-on **red team** certification. It is one of the most popular beginner Red Team certification.

A certification holder has the skills to understand and assess security of an E**nterprise Active Directory** environment.

### What are the requirements for CRTP certification? <a href="#viewer-esim4" id="viewer-esim4"></a>

To get certified, a student must solve a 24 hours hands-on exam in a fully patched Enterprise Active Directory environment containing multiple domains and forests. Like the course, the certification challenges a student to compromise the exam environment using feature abuse and functionalities.

### What is the goal of the CRTP exam? <a href="#viewer-lgmk" id="viewer-lgmk"></a>

The 24 hour hands-on exam consists of 5 target servers in addition to a foothold student machine. The goal is to OS level command execution on all 5 targets.

### Who owns the Certified Red Team Professional (CRTP)? <a href="#viewer-2he9v" id="viewer-2he9v"></a>

Altered Security owns the courses, labs and certification name. We used to offer this in partnership with Pentester Academy. However, the reseller contract ended on 15th January 2023.

### Does the Certified Red Team Professional (CRTP) certificate expire? <a href="#viewer-f56kk" id="viewer-f56kk"></a>

Yes, the CRTP certificate has a validity of 3 years to keep up with changing technologies and skill requirements. You can renew the certificate without any additional costs. You can find the renewal process here - https://www.alteredsecurity.com/post/renewal-process-for-altered-security-certifications

### Does Attacking and Defending Active Directory or CRTP labs use updated Windows version? <a href="#viewer-9cjc7" id="viewer-9cjc7"></a>

Yes! The CRTP labs are updated to Server 2022. The lab mimics a real world enterprise environment and the users need to rely on misconfigurations and feature abuse to challenge the lab.

<figure><img src="https://static.wixstatic.com/media/628794_3744024c76874b21808fcc3765e6f663~mv2.png/v1/fill/w_740,h_329,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/628794_3744024c76874b21808fcc3765e6f663~mv2.png" alt="CRTP Lab"><figcaption><p>CRTP Lab</p></figcaption></figure>

## What will you Learn?

The Attacking and Defending Active Directory Lab enables you to:

* Practice various attacks in a fully patched realistic Windows environment with Server 2022 and SQL Server 2017 machine.
* Multiple domains and forests to understand and practice cross trust attacks.
* Learn and understand concepts of well-known Windows and Active Directory attacks.
* Learn to use Windows as an attack platform and using trusted features of the OS like .NET, PowerShell and others for attacks.
* Bypassing defenses like Windows Defender, Microsoft Defender for Endpoint (MDE) and Microsoft Defender for Identity (MDI).
*

## Course duration & Topics ⏳📚 <a href="#course-duration-and-topics" id="course-duration-and-topics"></a>

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

🛣️ [**RoadMap / Exam Preparation**](roadmap-exam-preparation.md) 🧑🏻‍🏫

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

## Training and Labs

The best way to take good practice is using this lab (1st lab required more resources than 2nd)

{% embed url="https://github.com/Orange-Cyberdefense/GOAD/tree/main" %}

### Altered Security Resources <a href="#ejpt-exam" id="ejpt-exam"></a>

{% embed url="https://www.alteredsecurity.com/adlab" %}

{% embed url="https://www.alteredsecurity.com/trainings" %}

## Resources 📑📘

### 👉[ RoadMap / Exam Preparation for CRTP](roadmap-exam-preparation.md) 🛣️

### 👉[ CRTP Cheat Sheet ](crtp-cheat-sheet.md)📔
