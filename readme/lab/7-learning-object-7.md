---
icon: vial
---

# 7 - Learning Object 7️

## Tasks



1 - Identify a machine in the target domain where a Domain Admin session is available

2 - Compromise the machine and escalate privileges to Domain Admin by abusing reverse shell on dcorp-ci

3 - Escalate privilege to DA by abusing derivative local admin through dcorp-adminsrv. On dcorp-adminsrv, tackle application allowlisting using:

* Gaps in Applocker rules.
* Disable Applocker by modifying GPO applicable to dcorp-adminsrv.

Flag 10 \[dcorp-mgmt] - Process using svcadmin as service account 🚩

Flag 11 \[dcorp-mgmt] - NTLM hash of svcadmin account 🚩

Flag 12 \[dcorp-adminsrv] - We tried to extract clear-text credentials for scheduled tasks from? Flag value is like lsass, registry, credential vault etc 🚩

Flag 13 \[dcorp-adminsrv] - NTLM hash of srvadmin extracted from dcorp-adminsrv 🚩

Flag 14 \[dcorp-adminsrv] - NTLM hash of websvc extracted from dcorp-adminsrv 🚩

Flag 15 \[dcorp-adminsrv] - NTLM hash of appadmin extracted from dcorp-adminsrv 🚩



## Solutions

### 1 - Identify a machine in the target domain where a Domain Admin session is available.

Start InviShell and PowerView

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\Powerview.ps1
```













### 2 - Compromise the machine and escalate privileges to Domain Admin by abusing reverse shell on dcorp-ci





```powershell


```

```powershell
```

### 3 - Escalate privilege to DA by abusing derivative local admin through dcorp-adminsrv. On dcorp-adminsrv, tackle application allowlisting using:







3.1 - Gaps in Applocker rules.









3.2 - Disable Applocker by modifying GPO applicable to dcorp-adminsrv.





```powershell
```











### Flag 10 \[dcorp-mgmt] - Process using svcadmin as service account 🚩













<pre class="language-powershell"><code class="lang-powershell"><strong>
</strong></code></pre>



### Flag 11 \[dcorp-mgmt] - NTLM hash of svcadmin account 🚩













### Flag 12 \[dcorp-adminsrv] - We tried to extract clear-text credentials for scheduled tasks from? Flag value is like lsass, registry, credential vault etc 🚩











### Flag 13 \[dcorp-adminsrv] - NTLM hash of srvadmin extracted from dcorp-adminsrv 🚩











### Flag 14 \[dcorp-adminsrv] - NTLM hash of websvc extracted from dcorp-adminsrv 🚩











### Flag 15 \[dcorp-adminsrv] - NTLM hash of appadmin extracted from dcorp-adminsrv 🚩















```powershell
```
