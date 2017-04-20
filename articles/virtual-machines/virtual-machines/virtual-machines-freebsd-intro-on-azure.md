<properties
   pageTitle="Inleiding tot FreeBSD op Azure | Microsoft Azure"
   description="Meer informatie over het gebruik van FreeBSD virtuele machines op Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Inleiding tot FreeBSD op Azure
In dit onderwerp biedt een overzicht van de uitvoering van een FreeBSD virtuele machine in Azure wordt aangegeven.

## <a name="overview"></a>Overzicht
FreeBSD voor Microsoft Azure is een geavanceerde computer-besturingssysteem gebruikt om te power moderne servers, desktops, en ingesloten platforms. De afbeelding FreeBSD 10.3 wordt geleverd door Microsoft Corporation en is beschikbaar in Azure wordt aangegeven. Dit is gebaseerd op de FreeBSD 10.3 release en Azure VM Gast Agent [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) is geïnstalleerd. De-agent is verantwoordelijk voor de communicatie tussen de VM FreeBSD en de Azure stof voor bewerkingen, zoals de inrichting van de VM bij eerste gebruik (gebruikersnaam, wachtwoord, hostnaam, enzovoort) en functionaliteit voor selectieve VM extensies inschakelen.
Als voor toekomstige versies van FreeBSD is de strategie Blijf en de meest recente versies beschikbaar kort nadat ze zijn vrijgegeven door de FreeBSD release engineering team. De nieuwe versie is [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Een virtuele FreeBSD machine implementeren
Een virtuele FreeBSD machine implementeert, is een eenvoudig proces met een afbeelding van [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>VM extensies voor FreeBSD
Ondersteunde VM extensies in FreeBSD Hier volgen.

### <a name="vmaccess"></a>VMAccess

De extensie [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) kunt doen:

- Het wachtwoord van de oorspronkelijke sudo gebruiker opnieuw instellen.
- Maak een nieuwe sudo-gebruiker met het wachtwoord die zijn opgegeven.
- Stel de sleutel openbare host met de toets gegeven.
- Opnieuw instellen van de sleutel openbare host is opgegeven tijdens het VM inrichten als de sleutel host is opgegeven.
- Open de SSH poort (22) en de sshd_config terugzetten als reset_ssh is ingesteld op waar.
- Verwijder de bestaande gebruiker.
- Controleer schijven.
- Herstel een toegevoegde schijf.

### <a name="customscript"></a>CustomScript

De extensie [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) kunt doen:

- Als biedt, kunt u aangepaste scripts downloaden van Azure Storage of externe openbare opslag (bijvoorbeeld GitHub).
- De invoer punt script uitvoeren.
- Ondersteuning voor inline-opdrachten.
- Windows-stijl nieuwe regel in shell en Python scripts automatisch converteren.
- Stuklijst automatisch in shell en Python scripts verwijderen.
- Beveiligen van vertrouwelijke gegevens in CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Verificatie: gebruikersnamen en wachtwoorden, SSH toetsen
Wanneer u een FreeBSD virtuele machine maakt met behulp van de Azure-portal, moet u een gebruikersnaam, wachtwoord of SSH openbare sleutel opgeven.
Gebruikersnamen voor de implementatie van een FreeBSD virtuele machine op Azure mag niet overeenkomen met de namen van systeemaccounts (UID < 100) al aanwezig zijn in de virtuele machine (zoals "root").
Op dit moment wordt alleen de RSA SSH-sleutel ondersteund. Een met meerdere regels SSH-sleutel moet beginnen met '---BEGIN SSH2 openbare sleutel---' en eindigt op "---einde SSH2 openbare sleutel---".

## <a name="obtaining-superuser-privileges"></a>Het verkrijgen van bevoegdheden voor beheerder
Het gebruikersaccount waarmee wordt opgegeven tijdens de implementatie van virtuele machines exemplaar op Azure is een bevoegdheden-account. Het pakket van sudo is geïnstalleerd in de gepubliceerde FreeBSD afbeelding.
Nadat u bent aangemeld via dit gebruikersaccount, kunt u opdrachten als hoofdmap met behulp van de opdrachtsyntaxis van de uitvoeren.

    # sudo <COMMAND>

U kunt desgewenst een shell hoofdsite aanvragen met behulp van sudo -s.

## <a name="next-steps"></a>Volgende stappen
- Ga naar [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) om een VM FreeBSD maken.
- Als u uw eigen FreeBSD naar Azure wilt, raadpleegt u [maken en uploaden een VHD FreeBSD naar Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
