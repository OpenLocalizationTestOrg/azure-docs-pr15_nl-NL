<properties
    pageTitle="Een App op VM schaal Sets implementeren | Microsoft Azure"
    description="Een app op VM schaal Sets implementeren"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Een App op VM schaal Sets implementeren

Een toepassing wordt uitgevoerd op een VM schaal instellen wordt meestal geïmplementeerd op drie manieren:

- Het installeren van nieuwe software op de afbeelding van een platform bij de implementatie. Een afbeelding platform in deze context is een afbeelding van het besturingssysteem van Azure Marketplace, zoals Ubuntu 16.04, Windows Server 2012 R2, enzovoort.

U kunt nieuwe software installeren op de afbeelding van een platform [VM extensie](../virtual-machines/virtual-machines-windows-extensions-features.md). Extensie VM is software die wordt uitgevoerd wanneer een VM wordt geïmplementeerd. U kunt een code, die u tevreden bent bij de implementatie met de extensie van een aangepast script uitvoeren. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) volgt een voorbeeld van Azure resourcemanager sjabloon met twee VM bestandsnaamextensies: een Linux aangepast Script-extensie voor de installatie Apache en PHP, en een diagnostische extensie prestatiegegevens die worden gebruikt door Azure automatisch schalen uitzenden.

Een voordeel van deze methode is u hebt een niveau van de scheiding tussen uw toepassingscode en het besturingssysteem, en uw toepassing afzonderlijk kunt bijhouden. Natuurlijk kunnen betekent dat er zijn ook meer bewegende onderdelen en VM implementatietijd niet langer als er veel voor het script downloaden en te configureren.

**Als u vertrouwelijke informatie in uw opdracht aangepast Script Extension (zoals een wachtwoord) doorgeeft, zorg ervoor dat u om op te geven de `commandToExecute` in de `protectedSettings` kenmerk van de extensie aangepast Script in plaats van de `settings` kenmerk.**

- Een aangepaste VM-afbeelding met zowel het besturingssysteem en de toepassing in een enkel VHD maken. Hier wordt de set schaal bestaat uit een set van VMs hebt gekopieerd uit een afbeelding die is gemaakt door u, die u wilt bijhouden. Hiervoor is geen extra configuratie VM implementatie tegelijk. Echter in de `2016-03-30` versie van VM schaal Sets (en eerdere versies), de schijven OS voor de VMs in de set schaal zijn beperkt tot een enkel opslag-account. U kunt dus maximaal 40 VMs hebben in een set schaal, in tegenstelling tot de 100 VM per schaal limiet met platform afbeeldingen instellen. Zie [Overzicht van schaal ontwerp instellen](./virtual-machine-scale-sets-design-overview.md) voor meer informatie.

- Een platform of een aangepaste afbeelding die in feite een container host distribueren en uw toepassing installeren als een of meer containers die u met een hulpmiddel voor het beheer orchestrator of configuratie beheren. Het leuke van deze methode is dat u hebt uw cloudinfrastructuur van de toepassingslaag geabstraheerd en deze afzonderlijk kunt bijhouden.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Wat gebeurt er tijdens een VM schaal instellen schalen af?

Wanneer u een of meer VMs aan een schaal instellen toevoegt door het vergroten van de capaciteit – of handmatig of via automatisch schalen – de toepassing automatisch geïnstalleerd. Voor voorbeeld als u de schaal instellen extensies die zijn gedefinieerd heeft, ze worden uitgevoerd op een nieuwe VM telkens wanneer die deze is gemaakt. Als de set schaal is gebaseerd op een aangepaste afbeelding, is een nieuwe VM een kopie van de aangepaste bronafbeelding. Als de schaal instellen VMs zijn container hosts, en vervolgens hebt opstartcode de containers in een aangepast Script extensie laden of extensie mogelijk een agent die bij een orchestrator cluster (zoals Azure Container Service registreert) installeren.

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Hoe beheer u toepassingsupdates in VM schaal Sets?

Voor de toepassingsupdates in VM schaal Sets volgt belangrijkste kan op drie manieren van de drie voorgaande toepassing implementatiemethoden:

* Bijwerken met VM uitbreidingen. Een VM-extensies die zijn gedefinieerd voor een VM schaal instellen worden uitgevoerd telkens wanneer een nieuwe VM wordt geïmplementeerd, een bestaande VM is reimaged of extensie VM wordt bijgewerkt. Als u bijwerken van uw toepassing wilt, rechtstreeks bijwerken van een toepassing tot en met extensies is een goede manier, maar u de definitie van de extensie hoeft bij te werken. Een eenvoudige manier om te doen, is door te wijzigen van de fileUris zodat deze verwijzen naar de nieuwe software.

* De methode onveranderlijke aangepaste afbeelding. Wanneer u de toepassing (of app-onderdelen) in een afbeelding VM Fancy kunt u zich richten op het bouwen van een betrouwbare pijplijn opbouwen, testen en implementatie van de afbeeldingen te automatiseren. U kunt uw architectuur te vergemakkelijken snelle wisselen van een set gefaseerde schaal in bedrijf ontwerpen. Een goed voorbeeld van deze methode is het [stuurprogramma van Azure Spinnaker werken](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Azure resourcemanager ondersteunen verpakker en Terraform, zodat u kunt ook uw afbeeldingen 'als code"definiëren en deze in Azure maakt, gebruikt u de VHD in een set schaal. Doen zou dus worden echter problematisch voor afbeeldingen op Marketplace, waar extensies/aangepaste scripts belangrijker omdat u niet rechtstreeks de bits van Marketplace bewerken.

* Containers bijwerken. Abstracte de levenscyclus van Toepassingsbeheer naar een niveau boven de cloudinfrastructuur, bijvoorbeeld door de toepassingen die en app-onderdelen naar containers en deze containers tot en met de container orchestrators en configuratie managers zoals Chef/Puppet beheren.

De schaal ingesteld VMs een stabiele substraat voor de containers en moet worden alleen incidentele beveiligings- en OS-gerelateerde updates. Zoals vermeld, is de Service van de Container Azure een goed voorbeeld van deze aanpak en het bouwen van een service eromheen.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Hoe u een update OS Implementeer tussen update domeinen?

Stel dat u wilt bijwerken van uw afbeelding OS terwijl de actieve VM schaal instellen. Een manier om te doen is afbeeldingen bij de VM één VM tegelijk. U kunt dit doen met PowerShell of Azure CLI. Zijn er afzonderlijke opdrachten voor het bijwerken van het model VM schaal instellen (hoe de configuratie wordt gedefinieerd) en "handmatige upgrade" aanroepen op afzonderlijke VMs.

[Hier](https://github.com/gbowerman/vmsstools) volgt een voorbeeld Python script waarmee het bijwerken van een domein één update VM schaal instellen op een moment automatisch. (Voorbehoud: deze meer van een aankoopbewijs concept dan een harde productie gereed-oplossing is, maar u mogelijk wilt toevoegen van enkele fout foutcontrole enz.).
