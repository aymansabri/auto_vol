# Auto volatility

Ce script bash execute les commandes de bases de Volatility, il a pour but de gagner du temps lors d'une investigation.

## Usage

```bash
$ auto_vol -h
auto_vol [-h] [-d <dump_name>] [-f <folder_name>] -- Script that performs basic volatility command and stores them into a directory

where:
	-h	Show this help
	-d 	Name of the memory dump to analyze
	-f	Name of the folder
```

#### Exemples
***Linux***

```bash
▶ ./auto_vol2 -d ~/Documents/CTF/RootMe/findmeagain/backup/memory.raw -f linux

	                          ,;::,      
                                  ,;;;'''      
                                  ,;;;'';      
                                  ,;;;'''     
                                  ,.''''    
                                  ,   
                                  ,   
                         ,;:::;;;;.,,,,,,,,,  
                       ,;;;:;:;;;;.,,,,,,,,,,  
                     ,::;;    ;;;;.,,,    ,,,,,  
                    ,:::;;    ;;;;.,,,    ,,,,,,  
                    ,:::;;    ;;;;.,,,    ,,,,,,  
                    ,:::;;    ;;;;.,,,    ,,...,  
                    ,:::;;    ;;;;.,,,    ,,....  
                    ,;::;;    ;;;;.,,,    ,,.... 
                    ,;;;;;;;;;;;;;.,,,,...,,.... 
                    ,;;;;;;;;;;;;;.,,,,...,,.... 
                     ,;;;;;;;;;;;;.,,,,...,,... 
                              ;;;;.,,, 
                          ;;;;;;;;.,,,,..., 
                           ,,...,,........ 
                             ,;;;;.,,,,
                               :;;.,,
                                 :.

Linux version 4.4.0-72-lowlatency (buildd@lcy01-17) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) #93-Ubuntu SMP PREEMPT Fri Mar 31 15:25:21 UTC 2017 (Ubuntu 4.4.0-72.93-lowlatency 4.4.49) o The intent is to make the tool independent of Linux version dependencies,

BOOT_IMAGE=/boot/vmlinuz-4.4.0-72-lowlatency

When the Ubuntu VM is up :
sudo apt-get install linux-image-4.4.0-72-lowlatency linux-headers-4.4.0-72-lowlatency volatility-tools
Restart it, press SHIFT and boot on the new Kernel.

Please enter the name of the new profile : 
SuperProfile
cd /usr/src/volatility-tools/linux && make && zip SuperProfile.zip /usr/src/volatility-tools/linux/module.dwarf /boot/System.map-4.4.0-72-lowlatency-generic
Then place the zip file in : /usr/lib/python2.7/site-packages/volatility/plugins/overlays/linux

Actual profiles installed : 
LinuxFindMe2x64                   - A Profile for Linux FindMe2 x64
LinuxUbuntu16041x64               - A Profile for Linux Ubuntu16041 x64
LinuxUbuntu16044_440-72-LowLatx64 - A Profile for Linux Ubuntu16044_440-72-LowLat x64
LinuxUbuntu16044_440-72x64        - A Profile for Linux Ubuntu16044_440-72 x64
LinuxUbuntu1604x64                - A Profile for Linux Ubuntu1604 x64

Which profiles are you wanting to use ?
LinuxUbuntu16044_440-72-LowLatx64
[+] Good profile, crypto_LUKS find ! Trying to get the Master Key...
Keyfind progress: 100%
[+] Master Key stored in key.bin !

Foremost in progress..Processing: /home/maki/Documents/CTF/RootMe/findmeagain/backup/memory.raw
|***|
```

Il faut entrer le nom du futur profile et le script sort toutes les commandes à mettre pour pouvoir générer le profile.
Ce script permet de voir si une clé LUKS est récupérable, pour ça j'utilise un script trouvé sur un autre github : aeskeyfind.c, la version de ce dépot est juste la version compilée.
https://github.com/volatilityfoundation/volatility/wiki/Linux



***Windows***

```bash
▶ ./auto_vol -d ~/Documents/CTF/RootMe/cc/ch2.dmp -f test

                            ,;::,      
                                  ,;;;'''      
                                  ,;;;'';      
                                  ,;;;'''     
                                  ,.''''    
                                  ,   
                                  ,   
                         ,;:::;;;;.,,,,,,,,,  
                       ,;;;:;:;;;;.,,,,,,,,,,  
                     ,::;;    ;;;;.,,,    ,,,,,  
                    ,:::;;    ;;;;.,,,    ,,,,,,  
                    ,:::;;    ;;;;.,,,    ,,,,,,  
                    ,:::;;    ;;;;.,,,    ,,...,  
                    ,:::;;    ;;;;.,,,    ,,....  
                    ,;::;;    ;;;;.,,,    ,,.... 
                    ,;;;;;;;;;;;;;.,,,,...,,.... 
                    ,;;;;;;;;;;;;;.,,,,...,,.... 
                     ,;;;;;;;;;;;;.,,,,...,,... 
                              ;;;;.,,, 
                          ;;;;;;;;.,,,,..., 
                           ,,...,,........ 
                             ,;;;;.,,,,
                               :;;.,,
                                 :.

Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Wrote test/screenshot/session_0.Service-0x0-3e5$.Default.png
Wrote test/screenshot/session_0.msswindowstation.mssrestricteddesk.png
Wrote test/screenshot/session_0.Service-0x0-3e7$.Default.png
Wrote test/screenshot/session_0.Service-0x0-3e4$.Default.png
Wrote test/screenshot/session_0.WinSta0.Default.png
WARNING : volatility.debug    : 0\WinSta0\Disconnect has no windows

Wrote test/screenshot/session_0.WinSta0.Winlogon.png
Wrote test/screenshot/session_1.WinSta0.Default.png
Wrote test/screenshot/session_1.WinSta0.Disconnect.png
Wrote test/screenshot/session_1.WinSta0.Winlogon.png
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6


Foremost in progress..
Processing: /home/billy/Documents/CTF/RootMe/cc/ch2.dmp
|******|
```

Cf. le dossier **output** pour voir la sortie. C'est tiré du dump C&C de Root-me.

## Détection d'OS

Le script est capable de détecter si le dump mémoire est un dump Windows ou Linux.
Si c'est un dump Windows, la batterie de commande s'execute normalement, sinon si c'est un dump Linux, il affiche le "Linux version" afin de pouvoir faire son profil.
À terme, les commandes pour Linux seront prises en compte.

## Commandes executées

Pour Windows :
1. Le nom du PC se fait via un "string, grep" (un printkey est prévu)
2. Les hash de la base SAM (hashdump)
3. Consoles (consoles)
4. CMDScan (cmdscan)
5. pstree (pstree)
6. psxview (pour les process cachés) (psxview)
7. Détection de bitlocker 
8. Détection de Truecrypt + récupération des clés
9. Contenu du presse papier (clipboard)
10. Récupère les screenshots (screenshots)
11. Historique des navigateurs (iehistory)
12. Traffic réseau (netscan)
13. Filescan (filescan)

Pour Linux : 
1. Détection de profile (Testé sur du Ubuntu 16.04 pour le moment)
2. Détection de clé Master Key LUKS + extraction
3. Linux_banner

De plus, un **foremost** est fait sur le dump.

## TODO

1. Gérer les commandes Linux -> In progress...
1.1. Auto mount des dump de disque (chiffré LUKS ou non)
2. Changer le strings, grep en printkey pour le nom du PC
3. Améliorer l'output