<properties
   pageTitle="Projecten upgraden naar de huidige versie van de Azure hulpmiddelen | Microsoft Azure"
   description="Meer informatie over het upgraden van een Azure project in Visual Studio naar de huidige versie van de Azure hulpmiddelen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projecten upgraden naar de huidige versie van de Azure-hulpmiddelen voor Visual Studio

## <a name="overview"></a>Overzicht

Nadat u de huidige versie van de hulpmiddelen Azure (of een eerdere versie die nieuwer is dan 1,6) hebt geïnstalleerd, projecten die zijn gemaakt met behulp van een extra Azure los voordat u 1,6 (November 2011) worden automatisch bijgewerkt zodra u deze opent. Als u projecten gemaakt met behulp van de 1,6 (November 2011)-versie van deze hulpprogramma's en er nog steeds deze versie is geïnstalleerd, kunt u deze projecten in de oudere versie openen en besluit of u ze upgrade uit te voeren.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Hoe uw project verandert wanneer u deze upgrade

Als een project automatisch bijgewerkt wordt of u opgeeft dat u wilt bijwerken, uw project is gewijzigd als u wilt werken met de huidige versies van bepaalde stroombaan en bepaalde eigenschappen eveneens gewijzigd terwijl deze sectie worden beschreven. Als uw project nodig andere wijzigingen die compatibel zijn met de nieuwere versie van de hulpmiddelen voor heeft, moet u deze wijzigingen handmatig aanbrengen.

- Het bestand web.config voor web rollen en het bestand app.config voor werknemer rollen worden bijgewerkt om te verwijzen naar de nieuwere versie van Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.

- De stroombaan Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll en Microsoft.WindowsAzure.ServiceRuntime.dll worden bijgewerkt naar de nieuwe versies.

- Publiceren profielen die in het projectbestand Azure (.ccproj opgeslagen waren) worden verplaatst naar een afzonderlijk bestand, met de extensie .azurePubXml, in de submap **publiceren** .

- Bepaalde eigenschappen in het profiel publiceren worden ter ondersteuning van nieuwe en gewijzigde functies bijgewerkt. **AllowUpgrade** wordt vervangen door **DeploymentReplacementMethod** omdat u kunt een uitgevouwen cloudservice tegelijk of stapsgewijs bijwerken.

- De eigenschap **UseIISExpressByDefault** is toegevoegd en ingesteld op ONWAAR, zodat de webserver die wordt gebruikt voor foutopsporing niet automatisch gewijzigd van Internet Information Services (IIS) IIS Express. IIS Express is de standaard-webserver voor projecten die zijn gemaakt met de nieuwere versies van de hulpmiddelen voor.

- Als Caching van Azure wordt gehost op een of meer van de rollen van uw project, worden bepaalde eigenschappen in de service te configureren (.cscfg-bestand) en de definitie van de service (.csdef-bestand) worden gewijzigd wanneer een project wordt bijgewerkt. Als u het project gebruikt het pakket Azure Caching NuGet, wordt het project wordt bijgewerkt naar de meest recente versie van het pakket. U moet open het bestand web.config en controleer of de clientconfiguratie slecht is onderhouden tijdens de upgrade. Als u de verwijzingen naar Caching van Azure-client stroombaan zonder het NuGet-pakket hebt toegevoegd, kan deze samenstellen niet worden bijgewerkt; u moet deze verwijzingen naar de nieuwe versies handmatig bijwerken.

>[AZURE.IMPORTANT] Voor F # projecten, moet u verwijzingen naar Azure stroombaan handmatig bijwerken zodat ze verwijzen naar de nieuwere versies van deze stroombaan.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Upgrade uitvoeren van een Azure project naar de huidige versie

1. Installeer de huidige versie van de Azure-hulpmiddelen in de installatie van Visual Studio die u wilt gebruiken voor het bijgewerkte project en open vervolgens het project dat u wilt bijwerken. Als het project is gemaakt met een extra Azure los voordat u 1,6 (November 2011), het project wordt automatisch bijgewerkt naar de huidige versie. Als het project is gemaakt met de November 2011 los en deze release nog steeds is geïnstalleerd, het project wordt geopend in deze release.

1. Open het snelmenu voor het projectknooppunt in Solution Explorer, kiest u **Eigenschappen**en kiest u op het tabblad van de **toepassing** van het dialoogvenster dat wordt weergegeven.

    Het tabblad **toepassing** ziet u de hulpmiddelen voor versie dat is gekoppeld aan het project. Als de huidige versie van Azure's wordt weergegeven, is al het project bijgewerkt. Als u een nieuwere versie van de hulpmiddelen voor dan wat het tabblad wordt hebt geïnstalleerd, verschijnt er een knop **bijwerken** .

1. Kies de knop **upgrade uitvoeren** voor het bijwerken van een project naar de huidige versie van de hulpmiddelen.

1. Maken van het project en klik vervolgens Verhelp eventuele fouten die uit API wijzigingen voortvloeien. Zie de documentatie voor de specifieke API voor informatie over het wijzigen van uw code voor de nieuwe versie.
