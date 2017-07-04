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
$ ./auto_vol -d memory.raw -f linux_test
Linux version 4.4.0-72-lowlatency (buildd@lcy01-17) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) #93-Ubuntu SMP PREEMPT Fri Mar 31 15:25:21 UTC 2017 (Ubuntu 4.4.0-72.93-lowlatency 4.4.49)
Make your Linux profile
```

Maintenant on sait qu'il faut faire une Ubuntu 16-04 avec un kernel 4.4.0-72-lowlatency.
https://github.com/volatilityfoundation/volatility/wiki/Linux



***Windows***

```bash
$ ./auto_vol -d memory.raw -f output
Analyze in progress....

Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6
Wrote output/screenshot/session_0.Service-0x0-3e5$.Default.png
Wrote output/screenshot/session_0.msswindowstation.mssrestricteddesk.png
Wrote output/screenshot/session_0.Service-0x0-3e7$.Default.png
Wrote output/screenshot/session_0.Service-0x0-3e4$.Default.png
Wrote output/screenshot/session_0.WinSta0.Default.png
WARNING : volatility.debug    : 0\WinSta0\Disconnect has no windows

Wrote output/screenshot/session_0.WinSta0.Winlogon.png
Wrote output/screenshot/session_1.WinSta0.Default.png
Wrote output/screenshot/session_1.WinSta0.Disconnect.png
Wrote output/screenshot/session_1.WinSta0.Winlogon.png
Volatility Foundation Volatility Framework 2.6
Processing: /home/billy/Documents/CTF/RootMe/cc/ch2.dmp
|******|
Volatility Foundation Volatility Framework 2.6
Volatility Foundation Volatility Framework 2.6

####################
## END ! Enj0y :) ##
####################
```

Cf. le dossier **output** pour voir la sortie. C'est tiré du dump C&C de Root-me.

## Détection d'OS

Le script est capable de détecter si le dump mémoire est un dump Windows ou Linux.
Si c'est un dump Windows, la batterie de commande s'execute normalement, sinon si c'est un dump Linux, il affiche le "Linux version" afin de pouvoir faire son profil.
À terme, les commandes pour Linux seront prises en compte.

## Commandes executées

Pour Windows :
1. Le nom du PC se fait via un "string, grep" (un printkey est prévu)
2. Les hash de la base SAM
3. Command
4. CMDScan
5. pstree
6. psxview (pour les process cachés)
7. Détection de bitlocker
8. Détection de Truecrypt + récupération des clés
9. Contenu du presse papier
10. Récupère les screenshots
11. Historique des navigateurs
12. Traffic réseau
13. Filescan

De plus, un **foremost** est fait sur le dump.

## TODO

1. Gérer les commandes Linux
2. Changer le strings, grep en printkey pour le nom du PC