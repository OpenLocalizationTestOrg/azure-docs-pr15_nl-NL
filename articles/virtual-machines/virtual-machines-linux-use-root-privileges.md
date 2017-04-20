<properties 
    pageTitle="Gebruik van de hoofdsite bevoegdheden op Linux virtuele machines | Microsoft Azure" 
    description="Informatie over het gebruik van de hoofdsite bevoegdheden op een Linux virtuele machine in Azure wordt aangegeven." 
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


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Gebruik van de hoofdsite bevoegdheden op Linux virtuele machines in Azure wordt aangegeven

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Standaard de `root` gebruiker is uitgeschakeld op Linux virtuele machines in Azure wordt aangegeven. Gebruikers kunnen opdrachten uitvoeren met verhoogde bevoegdheden met behulp van de `sudo` opdracht. De ervaring is echter wel afhankelijk van hoe het systeem is ingericht.

1. **SSH-sleutel en wachtwoord of wachtwoord alleen** - de virtuele machine is ingericht met een van beide een certificaat (`.CER` bestand) of SSH-toets, evenals een wachtwoord, of alleen een gebruikersnaam en wachtwoord. In dit geval `sudo` voordat het uitvoeren van de opdracht voor het wachtwoord van de gebruiker wordt gevraagd.

2. **Alleen SSH-sleutel** - de virtuele machine is ingericht met een certificaat (`.cer`, `.pem`, of `.pub` bestand) of SSH-toets, maar geen wachtwoord.  In dit geval `sudo` **niet** prompt om het wachtwoord van de gebruiker voordat de opdracht wordt uitgevoerd.


## <a name="ssh-key-and-password-or-password-only"></a>SSH-toets en het wachtwoord of wachtwoord alleen

Meld u aan bij de Linux virtuele machine met SSH sleutel of wachtwoord verificatie en voer vervolgens opdrachten met `sudo`, bijvoorbeeld:

    # sudo <command>
    [sudo] password for azureuser:

In dit geval wordt de gebruiker gevraagd om een wachtwoord. Na het invoeren van het wachtwoord `sudo` wordt de opdracht met uitgevoerd `root` bevoegdheden.

U kunt ook passwordless sudo inschakelen door het bewerken van de `/etc/sudoers.d/waagent` bestand, bijvoorbeeld:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Deze wijziging kan voor passwordless sudo door de gebruiker "azureuser".

## <a name="ssh-key-only"></a>SSH belangrijke alleen

Meld u aan bij de Linux virtuele machine met SSH belangrijke verificatie en voer vervolgens opdrachten met `sudo`, bijvoorbeeld:

    # sudo <command>

In dit geval de gebruiker wordt **niet** gevraagd om een wachtwoord. Na het drukken op `<enter>`, `sudo` wordt de opdracht met uitgevoerd `root` bevoegdheden.

 
