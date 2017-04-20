<properties
   pageTitle="Toegang krijgen tot het VM-ID"
   description="Openen en gebruiken van Azure VM unieke ID wordt beschreven"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Openen en gebruiken van Azure VM unieke ID

Azure VM unieke ID is een 128-id-codering en opgeslagen in alle Azure IaaS VM van SMBIOS en kan momenteel worden gelezen met platform BIOS opdrachten.

Azure VM unieke ID is een alleen-lezen-eigenschap. Azure unieke VM-ID wordt niet gewijzigd na het opnieuw opstarten afsluiten (hetzij die is gepland voor niet-geplande), starten en stoppen opgeheven toewijzen, service-retoucheren of herstellen op hun plaats staan. Nieuwe Azure VM-ID is echter geconfigureerd als de VM een momentopname is en gekopieerd als u wilt maken van een nieuw exemplaar.

> [AZURE.NOTE] Als u oudere VMs gemaakt en uw VM om automatisch een Azure unieke id uitgevoerd omdat deze nieuwe functie hebt geïmplementeerd (18 September 2014), neem opnieuw starten


Toegang krijgen tot Azure unieke VM ID vanuit de VM:


## <a name="create-a-vm"></a>Een VM maken
 

Zie [maken een virtuele Machine](virtual-machines-linux-creation-choices.md) voor meer informatie


## <a name="connect-to-the-vm"></a>Verbinding maken met de VM
 

Zie voor meer informatie, [SSH van Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Unieke ID van query VM

Opdracht (wordt **Ubuntu**):

    sudo dmidecode | grep UUID
    
Voorbeeld van de verwachte resultaten:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Vanwege Big Endian bit ordening, is de werkelijke unieke VM-ID in dit geval:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Unieke ID van Azure VM kan worden gebruikt in verschillende scenario´s of de VM wordt uitgevoerd op Azure of on-premises implementatie en kan helpen uw licentievoorwaarden, rapportage of algemene bijhouden vereisten voor die u mogelijk op van uw Azure IaaS-implementaties. Veel onafhankelijke softwareleveranciers bouwen van toepassingen en te certificeren ze op Azure mogelijk nodig om aan te geven van een VM Azure overal in de levenscyclus van de en te geven als de VM wordt uitgevoerd op Azure, on-Premises of op andere providers cloud. Deze id platform kunt bijvoorbeeld als de software goed is licentie wordt gedetecteerd of helpen om te relateren VM gegevens naar de bron zoals om u te helpen bij het instellen van de juiste statistieken voor het juiste platform en voor het bijhouden en relateren van deze gegevens onder andere toepassingen.
