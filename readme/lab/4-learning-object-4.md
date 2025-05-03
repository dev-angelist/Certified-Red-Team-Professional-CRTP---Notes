---
icon: vial
---

# 4 - Learning Object 4️

## Tasks



1 - Enumerate all domains in the moneycorp.local forest

2 - Map the trusts of the dollarcorp.moneycorp.local domain

3 - Map External trusts in moneycorp.local forest

4 - Identify external trusts of dollarcorp domain. Can you enumerate trusts for a trusting forest?

Flag 4 \[Student VM] - Trust Direction for the trust between dollarcorp.moneycorp.local and eurocorp.local  🚩



## Solutions

### 1 - Enumerate all domains in the moneycorp.local forest

Start InviShell and PowerView

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\Powerview.ps1
```

Using Get-ForestDomain we obtain all domains relative to moneycorp.local forest

```powershell
Get-DomainTrust -Domain dollarcorp.moneycorp.local | select TargetName,TrustAttributes,TrustDirection
```

```powershell
TargetName                    TrustAttributes TrustDirection
----------                    --------------- --------------
moneycorp.local               WITHIN_FOREST   Bidirectional
us.dollarcorp.moneycorp.local WITHIN_FOREST   Bidirectional
eurocorp.local                FILTER_SIDS     Bidirectional
```

### 2 - Map the trusts of the dollarcorp.moneycorp.local domain

Using Get-DomainTrust command we're able to retrieve Trusts and relative direction for dollarcorp.moneycorp.local domain

```powershell
Get-DomainTrust -Domain dollarcorp.moneycorp.local
```

```powershell
SourceName      : dollarcorp.moneycorp.local
TargetName      : moneycorp.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : WITHIN_FOREST
TrustDirection  : Bidirectional
WhenCreated     : 11/12/2022 5:59:01 AM
WhenChanged     : 4/19/2025 4:04:35 AM

SourceName      : dollarcorp.moneycorp.local
TargetName      : us.dollarcorp.moneycorp.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : WITHIN_FOREST
TrustDirection  : Bidirectional
WhenCreated     : 11/12/2022 6:22:51 AM
WhenChanged     : 5/1/2025 5:09:26 AM

SourceName      : dollarcorp.moneycorp.local
TargetName      : eurocorp.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : FILTER_SIDS
TrustDirection  : Bidirectional
WhenCreated     : 11/12/2022 8:15:23 AM
WhenChanged     : 5/1/2025 5:09:25 AM
```

### 3 - Map External trusts in moneycorp.local forest

Using Get-DomainTrust and the trust attribute "FILTER\_SIDS" we can display all external trust relationship. Enabling FILTER\_SIDS ensures that only the primary SID is considered during authorization, ignoring any SIDHistory.

As saw in the last tasks, the current forest is moneycorp.local, so it's not necessary to specify it.

```powershell
Get-DomainTrust | ?{$_.TrustAttributes -eq "FILTER_SIDS"}
```

```powershell
SourceName      : dollarcorp.moneycorp.local
TargetName      : eurocorp.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : FILTER_SIDS
TrustDirection  : Bidirectional
WhenCreated     : 11/12/2022 8:15:23 AM
WhenChanged     : 5/1/2025 5:09:25 AM
```

### 4 - Identify external trusts of dollarcorp domain. Can you enumerate trusts for a trusting forest?

We just know these information regarding dollarcorp domain

```powershell
Get-DomainTrust -Domain dollarcorp.moneycorp.local | select TargetName,TrustAttributes,TrustDirection
```

```powershell
TargetName                    TrustAttributes TrustDirection
----------                    --------------- --------------
moneycorp.local               WITHIN_FOREST   Bidirectional
us.dollarcorp.moneycorp.local WITHIN_FOREST   Bidirectional
eurocorp.local                FILTER_SIDS     Bidirectional
```

and the relative external trust:

```powershell
Get-DomainTrust -Domain dollarcorp.moneycorp.local | ? { $_.TrustAttributes -match "FILTER_SIDS" }
```

```powershell
SourceName      : dollarcorp.moneycorp.local
TargetName      : eurocorp.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : FILTER_SIDS
TrustDirection  : Bidirectional
WhenCreated     : 11/12/2022 8:15:23 AM
WhenChanged     : 5/1/2025 5:09:25 AM
```

To answer at the question: "Can you enumerate trusts for a trusting forest?" Remembering that the external forest is: eurocorp.local, we can enurate all domains of the forest checking the domain trust:

```powershell
Get-ForestDomain -Forest eurocorp.local | %{Get-DomainTrust -Domain $_.Name}
```

```powershell
SourceName      : eurocorp.local
TargetName      : eu.eurocorp.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : WITHIN_FOREST
TrustDirection  : Bidirectional
WhenCreated     : 11/12/2022 5:49:08 AM
WhenChanged     : 5/1/2025 5:03:47 AM

SourceName      : eurocorp.local
TargetName      : dollarcorp.moneycorp.local
TrustType       : WINDOWS_ACTIVE_DIRECTORY
TrustAttributes : FILTER_SIDS
TrustDirection  : Bidirectional
WhenCreated     : 11/12/2022 8:15:23 AM
WhenChanged     : 5/1/2025 5:09:25 AM

Exception calling "FindAll" with "0" argument(s): "A referral was returned from the
server.
"
At C:\AD\Tools\Powerview.ps1:23860 char:20
+             else { $Results = $Searcher.FindAll() }
+                    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : DirectoryServicesCOMException
```

There's an error message because we can't enumerate trusts of a domain for which we haven't visibility into.

In addition, if we try to check forest domain for eurocorp.local, we can't extract full informations

```powershell
Get-ForestDomain -Forest eurocorp.local
```

```powershell
Forest                  : eurocorp.local
DomainControllers       : {eurocorp-dc.eurocorp.local}
Children                : {eu.eurocorp.local}
DomainMode              : Unknown
DomainModeLevel         : 7
Parent                  :
PdcRoleOwner            : eurocorp-dc.eurocorp.local
RidRoleOwner            : eurocorp-dc.eurocorp.local
InfrastructureRoleOwner : eurocorp-dc.eurocorp.local
Name                    : eurocorp.local

Forest                  :
DomainControllers       :
Children                :
DomainMode              :
DomainModeLevel         :
Parent                  :
PdcRoleOwner            :
RidRoleOwner            :
InfrastructureRoleOwner :
Name                    : eu.eurocorp.local
```

### Flag 4 \[Student VM] - Trust Direction for the trust between dollarcorp.moneycorp.local and eurocorp.local  🚩

The trust direction between dollarcorp.moneycorp.local and eurocorp.local can be determined by inspecting the TrustDirection field using:

```powershell
Get-DomainTrust -Domain dollarcorp.moneycorp.local | ? { $_.TargetName -eq "eurocorp.local" } | select TargetName,TrustAttributes,TrustDirection
```

```powershell
TargetName     TrustAttributes TrustDirection
----------     --------------- --------------
eurocorp.local FILTER_SIDS     Bidirectional
```
