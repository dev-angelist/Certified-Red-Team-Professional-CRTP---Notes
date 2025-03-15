# 5.1 - Privilege Escalation

## Privilege Escalation

Privilege Escalation is usually the third step (after Reconnaissance and Domain Enumeration) regarding attack methodology.

In an AD environment we can perform privilege escalation for this scope:

* Hunting for Local Admin access on other machines
* Hunting for high privilege domain account (like as DOmain Administrator).

There're various ways to escalate privileges on Windows Box:

* Missing patches
* Feature Abuse
* Automated deployment and AutoLogon psw in cleartext
* AlwaysInstallElevated (Any user can run MSI as SYSTEM)
* Misconfigured Services
* DLL Hijacking and more
* Unquoted Service Path
* Scheduled Task
* Kerberos and NTLM Relaying

More details (not related to AD) are explained here: [Windows Privilege Escalation](https://dev-angelist.gitbook.io/windows-privilege-escalation)

### Tools

While, more common tools to help us into process are:

* [**PowerUp**](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1): [https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1)
* [**Privesc**](https://github.com/itm4n/PrivescCheck): [https://github.com/itm4n/PrivescCheck](https://github.com/itm4n/PrivescCheck)
* [**WinPeas**](https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS): [https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS](https://github.com/peass-ng/PEASS-ng/tree/master/winPEAS)

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
