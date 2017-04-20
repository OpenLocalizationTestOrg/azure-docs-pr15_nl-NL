
<properties
    pageTitle="Problemen met RemoteApp cloud verzamelingen - maken | Microsoft Azure"
    description="Meer informatie over het oplossen van fouten bij RemoteApp cloud siteverzameling maken"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Problemen met maken RemoteApp cloud-verzamelingen

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Als u hebt met het maken van een siteverzameling cloud problemen, de volgende informatie raadplegen.

## <a name="your-image-is-invalid"></a>Uw afbeelding is ongeldig ##
Als u een bericht zoals 'GoldImageInvalid' ziet wanneer u Azure wacht voor het inrichten van uw siteverzameling, betekent dit dat de afbeelding van uw sjabloon niet voldoet aan de [vereisten van de afbeelding gedefinieerd](remoteapp-imagereqs.md). Ja, Ga lezen die [vereisten](remoteapp-imagereqs.md), uw afbeelding oplossen en probeert te maken van uw siteverzameling opnieuw.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Veelvoorkomende fouten zichtbaar in de beheerportal van Azure

    DNS server could not be reached
    ProvisioningTimeout

Cloud verzamelingen vaak mislukt tijdens het maken van vanwege u aangepaste afbeeldingen gebruikt.  Als u een van de bovenstaande fouten ziet en u een aangepaste afbeelding gebruikt om te maken van de siteverzameling, controleert u de volgende onderdelen:

- Zorg ervoor dat de aangepaste afbeelding die u hebt geüpload aan de afbeelding voldoet.
- Het voorkomende probleem is meestal dat de afbeelding niet correct syspreped is.  
- Controleer of de afbeelding kunt starten vanuit Hyper-V of maakt u een IAAS VM rechtstreeks in uw Azure-abonnement met de afbeelding. Als de VM mislukt opstarten en niet wilt starten, klikt u vervolgens dit meestal wordt aangegeven dat de aangepaste afbeelding niet correct is voorbereid.  Controleer of de aangepaste afbeelding is gebouwd hoe volgen voor het maken van de afbeelding van een aangepaste sjabloon voor RemoteApp

Als u een van de Microsoft-afbeeldingen die wordt geleverd bij uw abonnement gebruikt, kunt u de verzameling opnieuw maken. Als het probleem zich blijft voordoen vervolgens Neem contact op met Microsoft ondersteuning.

    PlatformImageTrialModeOnly

Als u deze fout ziet dit betekent meestal dat u een upgrade is uitgevoerd met een betaalde account, maar u probeert te gebruiken van de afbeelding van een Microsoft verstrekt dat geldt alleen tijdens de proefabonnement modus van de service. In dit geval probeert te maken van de cloud-verzameling opnieuw, maar moet u de juiste afbeelding opgeven.
