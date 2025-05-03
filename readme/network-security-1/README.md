# 3️⃣ 3 - AD Enumeration

### Topics

> 1. [Host & User Identification](2.1.md)
> 2. [Common Services Enumeration](3.2-common-services-enum/)
> 3. [Domain Enumeration](3.3-domain-enumeration/)

[AD Enumeration Lab](https://dev-angelist.gitbook.io/writeups-and-walkthroughs/homemade-labs/active-directory/ad-enumeration)

{% hint style="danger" %}
#### ❗ Disclaimer&#x20;

**Never use tools and techniques on real IP addresses, hosts or networks without proper     authorization!**❗
{% endhint %}

### AD **Enumeration** <a href="#a-d-enumeration" id="a-d-enumeration"></a>

Active Directory (AD) is the backbone of many enterprise IT infrastructures, managing user authentication, authorization, and resource access. During penetration testing or red team engagements, **enumerating Active Directory** is a critical step for gathering intelligence about the environment. This process involves systematically identifying valuable information that can be used to map out the network, discover potential attack paths, and exploit misconfigurations or vulnerabilities.

**Why Enumerate Active Directory?** Active Directory is complex and interconnected, making it a prime target for attackers. Enumeration helps uncover:

* Domain structure and trust relationships.
* User accounts, groups, and their permissions.
* Domain Controllers (DCs) and critical services like DNS, LDAP, SMB, and Kerberos.
* Misconfigurations, such as weak passwords, open shares, and insecure policies.

**Key Enumeration Goals:**

1. **Map the Environment:** Identify key assets, including Domain Controllers and critical servers.
2. **Identify Users:** Discover domain accounts and their roles.
3. **Assess Permissions:** Look for overprivileged users, groups, or objects.
4. **Locate Weaknesses:** Misconfigurations, legacy systems, or unpatched vulnerabilities.
5. **Set the Stage for Attacks:** Gather the information needed for credential attacks, privilege escalation, or lateral movement.

**Common Enumeration Tools and Techniques:** Enumeration can be performed using a variety of tools and techniques, including:

* **Nmap** for network scanning and service discovery.
* **SMB and LDAP enumeration** tools to query shared resources and directory structures.
* **BloodHound** for mapping AD relationships and privilege escalation paths.
* **Kerberos-based tools** like Kerbrute to discover valid accounts through pre-authentication failures.
* **PowerShell scripts** for gathering system and domain information.

**Reconnaissance Without Credentials:** Even without valid domain credentials, attackers can leverage null sessions, misconfigured services, and network discovery tools to gain valuable information. These findings often serve as a foothold to further access.

## Labs

Refers to [Learning Object 1](../lab/1-learning-object-1.md) and [Learning Object 3](../lab/3-learning-object-3.md) labs
