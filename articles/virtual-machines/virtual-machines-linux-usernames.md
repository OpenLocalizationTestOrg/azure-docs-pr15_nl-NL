<properties 
    pageTitle="Gebruikersnamen voor Linux selecteren | Microsoft Azure" 
    description="Leer hoe u de gebruikersnamen voor een Linux virtuele machine te selecteren in Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



#<a name="selecting-user-names-for-linux-on-azure"></a>Gebruikersnamen voor Linux op Azure selecteren#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Wanneer u een Linux virtuele machine op Azure inrichten moet u de naam van een niet-hoofdsite-gebruiker die u later aan te melden bij de VM kunt gebruiken. U kunt de naam van de nieuwe gebruiker of als de inrichting van via de portal van Azure klassieke kunt u de standaard naam "azureuser" accepteren.

In de meeste gevallen deze gebruiker op de afbeelding won't aanwezig en wordt gemaakt tijdens het inrichten. Als de gebruiker zich op de afbeelding VM, klikt u vervolgens de Azure Linux-agent gewoon Hiermee configureert u het wachtwoord en/of SSH-sleutel voor die gebruiker op basis van de gegevens die u hebt opgegeven bij het maken van de VM.

Linux definieert **echter**een set gebruikersnamen die niet mogen worden gebruikt. Het inrichten wordt **mislukt** als u probeert een Linux VM met een bestaande systeemgebruiker, die is gedefinieerd als een gebruiker met UID 0-99 inrichten. Een typisch voorbeeld is de `root` gebruiker, waarvoor UID 0.

 - Zie ook: [Linux standaard Base - gebruikers-ID bereiken](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Hier volgt een lijst met algemene ingebouwde systeemgebruikers voor CentOS en Ubuntu dat u vermijden moet bij de inrichting van een Linux virtuele machine op Azure. Deze lijst ziet u alleen een voorbeeld, Zie de documentatie voor de verdeling om ervoor te zorgen dat de gebruikersnaam die u kiest u geen conflict met een bestaande gebruiker veroorzaakt.


## <a name="centos"></a>CentOS
- abrt
- adm
- audio
- Prullenbak
- cd-rom
- cgred
- daemon
- dbus
- bellen met
- DIP
- Schijfopruiming
- diskette
- FTP
- FTP
- spellen
- Gopher
- haldaemon
- stoppen
- kmem
- vergrendelen
- LP
- e-mail
- Man
- Mem
- nfsnobody
- niemand
- NTP
- operator
- oprofile
- postdrop
- postfix
- qpidd
- hoofdmap
- RPC
- rpcuser
- saslauth
- afsluiten
- slocate
- sshd
- stapdev
- stapusr
- synchroniseren
- sys
- tape
- testen
- tcpdump
- TTY (teksttelefoon)
- gebruikers
- utempter
- utmp
- uucp
- vcsa
- video
- wiel


## <a name="ubuntu"></a>Ubuntu
- adm
- beheerder
- audio
- back-up maken
- Prullenbak
- cd-rom
- crontab
- daemon
- bellen met
- DIP
- Schijfopruiming
- Faxvoorblad
- diskette
- integreert
- spellen
- gnats
- IRC
- kmem
- Liggend
- libuuid
- lijst
- LP
- e-mail
- Man
- MessageBus
- mlocate
- netdev
- nieuws
- niemand
- nogroup
- operator
- plugdev
- proxy
- hoofdmap
- SASL
- schaduw
- src
- SSH
- sshd
- personeel
- sudo
- synchroniseren
- sys
- Syslog
- tape
- TTY (teksttelefoon)
- gebruikers
- utmp
- uucp
- video
- voicemail
- whoopsie
- www-gegevens

 