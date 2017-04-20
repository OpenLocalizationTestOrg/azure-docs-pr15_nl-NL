<properties
        pageTitle="Opnieuw Linux VM wachtwoord en SSH-sleutel uit de CLI | Microsoft Azure"
        description="Het gebruik van de extensie VMAccess van de Azure-Interface met opdrachtregel (CLI) opnieuw instellen van een Linux VM wachtwoord of SSH-toets, de SSH-configuratie oplossen en consistentie van de schijf controleren"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Hoe u opnieuw instellen van een Linux VM wachtwoord of SSH-toets, de SSH-configuratie oplossen en schijf consistentie met de extensie VMAccess controleren


Als u geen verbinding met een Linux virtuele machine op Azure vanwege een vergeten wachtwoord, een onjuiste Secure Shell (SSH)-sleutel maken, of een probleem met de configuratie SSH, met de extensie VMAccessForLinux met de CLI Azure opnieuw het wachtwoord of SSH-toets in te stellen, de SSH-configuratie oplossen en consistentie van de schijf controleren. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Met de CLI Azure kunt u de opdracht **azure vm extensie set** uit een interface voor de opdrachtregel (Terminal, opdrachtprompt we vaker doen,) access-opdrachten. **Azure help vm extensie instellen** voor gebruik van de extensie voor gedetailleerde uitvoeren.

Met de CLI Azure, kunt u de volgende taken uitvoeren:

+ [Het wachtwoord opnieuw instellen](#pwresetcli)
+ [Beginwaarden van de SSH-toets](#sshkeyresetcli)
+ [Opnieuw het wachtwoord en de toets SSH](#resetbothcli)
+ [Een nieuwe sudo-gebruikersaccount maken](#createnewsudocli)
+ [De configuratie SSH opnieuw](#sshconfigresetcli)
+ [Een gebruiker verwijderen](#deletecli)
+ [De status van de extensie VMAccess weergeven](#statuscli)
+ [Consistentie van toegevoegde schijven controleren](#checkdisk)
+ [Toegevoegde schijven op uw VM Linux herstellen](#repairdisk)


## <a name="prerequisites"></a>Vereisten voor

U moet als volgt te werk:

- Moet u voor het [installeren van de Azure CLI](../xplat-cli-install.md) en [verbinding maken met uw abonnement](../xplat-cli-connect.md) Azure resources die zijn gekoppeld aan uw account te gebruiken.
- De juiste modus voor het implementatiemodel klassieke instellen door te typen van de volgende handelingen uit bij de opdrachtprompt:
        
        azure config mode asm
        
- Een nieuw wachtwoord of set SSH toetsaanslagen hebben als u wilt een opnieuw. U kunt deze niet nodig als u wilt dat de configuratie SSH opnieuw in te stellen.


## <a name="pwresetcli"></a>Het wachtwoord opnieuw instellen

1. Een bestand met de naam PrivateConf.json met deze regels maken. Vervang de haakjes en de & #60; tijdelijke aanduiding & #62; waarden door uw eigen gegevens.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Deze opdracht, wordt vervangen door de naam van uw VM voor & #60; vm-naam & #62; uitvoeren.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Beginwaarden van de SSH-toets

1. Een bestand met de naam PrivateConf.json met deze inhoud maken. Vervang de haakjes en de & #60; tijdelijke aanduiding & #62; waarden door uw eigen gegevens.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Deze opdracht, wordt vervangen door de naam van uw VM voor & #60; vm-naam & #62; uitvoeren.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Zowel het wachtwoord en de toets SSH opnieuw instellen

1. Een bestand met de naam PrivateConf.json met deze inhoud maken. Vervang de haakjes en de & #60; tijdelijke aanduiding & #62; waarden door uw eigen gegevens.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Deze opdracht, wordt vervangen door de naam van uw VM voor & #60; vm-naam & #62; uitvoeren.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Een nieuwe sudo-gebruikersaccount maken

Als u uw gebruikersnaam in te voeren vergeet, kunt u VMAccess naar een nieuwe record met de instantie sudo maken. In dit geval wordt de bestaande gebruikersnaam en wachtwoord niet gewijzigd.

U maakt een nieuwe sudo-gebruiker met wachtwoord toegang, gebruik het script in [het wachtwoord opnieuw instellen](#pwresetcli) en geef de naam van de nieuwe gebruiker.

U maakt een nieuwe sudo-gebruiker met belangrijke toegang SSH, het script in [de sleutel SSH opnieuw](#sshkeyresetcli) gebruiken en geef de naam van de nieuwe gebruiker.

U kunt ook [opnieuw het wachtwoord en de SSH-toets](#resetbothcli) gebruiken om te maken van een nieuwe gebruiker met zowel wachtwoord als SSH belangrijke access.

## <a name="sshconfigresetcli"></a>De configuratie SSH opnieuw

Als de configuratie SSH zich in een ongewenste staat, kan u ook toegang tot de VM verliezen. U kunt de extensie VMAccess opnieuw instellen van de configuratie de standaardstatus. Hiervoor moet u alleen de sleutel 'reset_ssh' in 'Waar' instellen. De extensie wordt de server SSH opnieuw opstarten en opent u de SSH-poort van uw VM opnieuw instellen van de configuratie SSH in standaardwaarden. Het gebruikersaccount (naam, wachtwoord of SSH toetsen) wordt niet gewijzigd.

> [AZURE.NOTE] Het configuratiebestand SSH die wordt opnieuw bevindt zich op /etc/ssh/sshd_config.

1. Een bestand met de naam PrivateConf.json met dit inhoudstype maken.

        {
        "reset_ssh":"True"
        }

2. Deze opdracht, wordt vervangen door de naam van uw VM voor & #60; vm-naam & #62; uitvoeren. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Een gebruiker verwijderen

Als u een gebruikersaccount zonder logboekregistratie in voor de VM rechtstreeks verwijderen wilt, kunt u dit script.

1. Een bestand met de naam PrivateConf.json met dit inhoudstype, wordt vervangen door de naam van de gebruiker wilt verwijderen voor & #60; usernametoremove & #62; maken. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Deze opdracht, wordt vervangen door de naam van uw VM voor & #60; vm-naam & #62; uitvoeren. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>De status van de extensie VMAccess weergeven

Als u wilt weergeven in de status van de extensie VMAccess, moet u deze opdracht uitvoeren.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< a = naam 'checkdisk' <</a>consistentie van toegevoegde schijven controleren

Als u wilt uitvoeren fsck op alle schijven in uw VM Linux, moet u als volgt te werk:

1. Een bestand met de naam PublicConf.json met dit inhoudstype maken. Controleer de dat schijf nodig heeft een Booleaanse waarde of om schijven die zijn gekoppeld aan uw VM al dan niet te controleren. 

        {   
        "check_disk": "true"
        }

2. Voer deze opdracht moet worden uitgevoerd, wordt vervangen door de naam van uw VM voor & #60; vm-naam & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Schijven herstellen 

Om te herstellen schijven die niet wilt koppelen of configuratiefouten mountains hebt, gebruikt u de extensie VMAccess de configuratie van de koppeling op de computer van de virtuele Linux opnieuw in te stellen. Wordt vervangen door de naam van de schijf voor & #60; yourdisk & #62;.

1. Een bestand met de naam PublicConf.json met dit inhoudstype maken. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Voer deze opdracht moet worden uitgevoerd, wordt vervangen door de naam van uw VM voor & #60; vm-naam & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Volgende stappen

* Als u gebruiken van Azure PowerShell-cmdlets of Azure resourcemanager sjablonen opnieuw het wachtwoord of SSH-toets in te stellen wilt, de configuratie SSH oplossen en consistentie van de schijf controleren, raadpleegt u de [VMAccess extensie documentatie op GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* U kunt ook de [Azure-portal](https://portal.azure.com) gebruiken het wachtwoord opnieuw in te stellen of SSH-sleutel van een Linux VM geïmplementeerd in het implementatiemodel klassieke. U kunt de portal Doe momenteel niet gebruiken als volgt voor een VM Linux geïmplementeerd in het implementatiemodel resourcemanager-.

* Zie [VM extensions en functies](virtual-machines-linux-extensions-features.md) voor meer informatie over het gebruik van VM uitbreidingen voor Azure virtuele machines.
