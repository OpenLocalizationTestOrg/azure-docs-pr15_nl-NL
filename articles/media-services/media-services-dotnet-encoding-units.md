<properties 
    pageTitle="Het toevoegen van codering eenheden" 
    description="Leer hoe u het toevoegen van codering eenheden met .NET"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Hoe u de schaal coderen met .NET SDK

> [AZURE.SELECTOR]
- [Portal](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Overzicht

>[AZURE.IMPORTANT] Zorg ervoor dat Bekijk het [Overzicht](media-services-scale-media-processing-overview.md) onderwerp voor meer informatie over de schaalbaarheid van media processing onderwerp.
 
Ga als volgt te werk als u wilt wijzigen van het eenheidstype gereserveerde per en het aantal gereserveerde eenheden met .NET SDK codering:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Een Ondersteuningsticket openen

Standaard kunt elke Media Services-account tot maximaal 25 versleutelen en schalen 5 op aanvraag Streaming gereserveerde eenheden. U kunt een hogere limiet aanvragen via een ondersteuningsticket.

###<a name="open-a-support-ticket"></a>Open een ondersteuningsticket

Als u wilt openen van een ondersteuning tickets als volgt:

1. Klik op [ondersteuning](https://manage.windowsazure.com/?getsupport=true). Als u niet bent aangemeld, wordt u gevraagd uw referenties invoeren.

1. Selecteer uw abonnement.

1. Selecteer onder type ondersteuning "Technische".

1. Klik op 'Tickets maken'.

1. Selecteer "Azure Media Services' in de lijst met producten op de volgende pagina voorgesteld.

1. Selecteer een probleemtype' "die geschikt is voor uw probleem.

1. Klik op Continue.

1. Volg de instructies op volgende pagina en voer de gegevens over uw probleem.

1. Klik op verzenden om de tickets openen.



##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
