# 3.3.1 - PowerView

## **PowerView** <a href="#powerview" id="powerview"></a>

**PowerView** is a versatile PowerShell tool specifically designed for Active Directory reconnaissance. Part of the PowerSploit framework, it allows penetration testers and red teamers to perform in-depth enumeration of AD environments. PowerView provides a comprehensive suite of cmdlets to gather information about users, groups, computers, permissions, trust relationships, and more.

### **PowerView Usage** <a href="#powerview-usage" id="powerview-usage"></a>

*   **Start InviShell** (using cmd)

    ```bash
    C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
    ```
*   **Start PowerView** (using powershell, if you've run InviShell powershell It's already running)

    <pre class="language-powershell"><code class="lang-powershell"><strong>. C:\AD\Tools\Powerview.ps1
    </strong></code></pre>
*   **Get Domain Information**

    ```powershell
    Get-NetDomain
    ```

    Retrieves information about the current domain.
*   **Enumerate Domain Controllers**

    ```powershell
    Get-NetDomainController
    ```

    Lists all Domain Controllers in the current domain.
*   **List Domain Users**

    ```powershell
    Get-NetUser
    ```

    Displays all users in the domain, along with detailed attributes.
*   **Find High-Value Targets**

    ```powershell
    Get-NetUser -AdminCount 1
    ```

    Lists all users flagged as administrators.
*   **Enumerate Domain Groups**

    ```powershell
    Get-NetGroup
    ```

    Retrieves all domain groups.

    ```powershell
    Get-NetGroupMember -GroupName "Domain Admins"
    ```

    Lists members of the "Domain Admins" group.
*   **Locate Domain Computers**

    ```powershell
    Get-NetComputer
    ```

    Lists all computers in the domain.
*   **Analyze Trust Relationships**

    ```powershell
    Get-NetDomainTrust
    ```

    Displays trust relationships between domains.
*   **Check ACLs on AD Objects**

    ```powershell
    Get-ObjectAcl -SamAccountName "Administrator" -ResolveGUIDs
    ```

    Shows ACLs for a specific user account, resolving GUIDs to human-readable names.
*   **Find Shares on Domain Computers**

    ```powershell
    Invoke-ShareFinder
    ```

    Locates shared folders across domain computers.
*   **Identify Delegation Configurations**

    ```powershell
    Get-NetUser -SPN
    ```

    Finds user accounts with Service Principal Names (SPNs), often used in Kerberos-based attacks.

***

## Labs

* [Domain Enumeration (Video Lab)](3.3.1.1-domain-enumeration-video-lab.md)
* [Post-Exploitation Basics THM Lab](https://dev-angelist.gitbook.io/writeups-and-walkthroughs/thm/post-exploitation-basics)
