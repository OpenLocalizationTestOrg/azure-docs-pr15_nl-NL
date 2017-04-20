<properties
    pageTitle="Overzicht van Azure diagnostisch hulpprogramma | Microsoft Azure"
    description="Azure diagnostische hulpprogramma's voor foutopsporing, meten van de prestaties, bewaken, verkeer analyse in de cloudservices, virtuele machines en service stof gebruiken"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Wat is Microsoft Azure diagnostische gegevens


Azure diagnostische gegevens is de mogelijkheid binnen Azure waarmee het vastleggen van diagnostische gegevens op een gedistribueerde toepassing. U kunt de extensie diagnostische gegevens uit een aantal verschillende bronnen. Momenteel ondersteund zijn Azure Cloud Service Web en werknemer rollen, Azure virtuele Machines met Microsoft Windows en Service stof. Andere Azure services hebben hun eigen afzonderlijke diagnostische gegevens.

## <a name="data-you-can-collect"></a>Gegevens die u kunt verzamelen

Azure diagnostische hulpprogramma's kan de volgende soorten gegevens verzamelen:

Gegevensbron|Beschrijving
---|---
Prestatie-items | Het besturingssysteem en aangepaste prestatie-items
Toepassingslogboeken aan de     | Berichten geschreven door uw toepassing bijhouden
Gebeurtenislogboeken van Windows   | Gegevens die worden verzonden naar het Windows-systeem gebeurtenis logboekregistratie
Bron van gebeurtenis .NET    | Code gebeurtenissen met de klasse .NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) opslaan
IIS-logboeken             | Informatie over IIS-websites
Manifest op basis van ETW   | Gebeurtenis Aanwijzen voor Windows-gebeurtenissen gegenereerd door een proces
Crashdumps          | Informatie over de status van het proces in het geval een toepassing-storing
Aangepaste foutenlogboeken    | Logboeken gemaakt door uw toepassing of service
Infrastructuur van Azure diagnostische logboeken|Informatie over het hulpprogramma's voor diagnose zelf

De extensie Azure diagnostische gegevens kunt overbrengen van deze gegevens naar een Azure opslag-account of naar services zoals [Toepassing inzichten](./application-insights/app-insights-cloudservices.md)te verzenden. U kunt de gegevens voor foutopsporing en probleemoplossing, meten van de prestaties, bewaken Resourcegebruik, wordt verkeer analyse en capaciteit, planning en controle.


## <a name="versioning"></a>Versiebeheer
Zie [Geschiedenis van documentversies Azure diagnostische gegevens](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Volgende stappen
Kies welke service die u probeert voor het verzamelen van diagnostische gegevens op en het gebruik van de volgende artikelen aan de slag. Gebruik de koppelingen algemene Azure diagnostische hulpprogramma's voor verwijzing voor specifieke taken.

## <a name="web-apps"></a>WebApps
Houd er rekening mee dat WebApps diagnostisch hulpprogramma Azure niet gebruiken. Vergelijkbare informatie zoeken bij [Web Apps](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Diagnostisch hulpprogramma Azure met Cloudservices
- Als Visual Studio gebruikt, raadpleegt u [Gebruik Visual Studio om het traceren van een Cloud Services-toepassing](./vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag. Anders zien
- [Het controleren van cloudservices met Azure diagnostische gegevens](./cloud-services/cloud-services-how-to-monitor.md)
- [Azure diagnostische gegevens in een Cloud Services-toepassing instellen](./cloud-services/cloud-services-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen,

- [Gebruik van Azure diagnostische gegevens met toepassing inzichten voor Cloudservices](./application-insights/app-insights-cloudservices.md)
- [De stroom van een Cloud Services-toepassing met diagnostisch hulpprogramma Azure aanwijzen](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [PowerShell gebruiken voor het instellen van diagnostische gegevens in de Cloud Services](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuele Machines met Azure diagnostische gegevens
- Als Visual Studio gebruikt, raadpleegt u [Gebruik Visual Studio traceren Azure virtuele Machines](./vs-azure-tools-debug-cloud-services-virtual-machines.md) aan de slag. Anders zien
- [Azure diagnostische gegevens op een Azure virtuele Machine instellen](./virtual-machines-dotnet-diagnostics.md)

Zie voor meer geavanceerde onderwerpen,

- [PowerShell gebruiken voor het instellen van diagnostische gegevens op Azure virtuele Machines](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [Een Windows virtuele machine controle en diagnostische gegevens met behulp van Azure resourcemanager sjabloon maken](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Service stof met Azure diagnostische gegevens
Aan de slag op [een toepassing voor de Service stof Monitor](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Groot aantal artikelen van andere Service stof diagnostische gegevens zijn beschikbaar in de boomstructuur aan de linkerkant zodra u toegang hebt tot in dit artikel.

## <a name="general-azure-diagnostics-articles"></a>Algemeen Azure diagnostisch hulpprogramma artikelen
- [Configuratie van azure diagnostische Schema](https://msdn.microsoft.com/library/azure/mt634524.aspx) - informatie over het wijzigen van het schemabestand routeren naar diagnostische gegevens te verzamelen. Opmerking u kunt ook Visual Studio gebruiken om te wijzigen van het schemabestand.
- [Hoe Azure diagnostische gegevens worden opgeslagen in de opslagruimte van de Azure](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) - weten de namen van de tabellen en BLOB's waar de diagnostische gegevens is geschreven.
- Leer hoe [u items in Azure diagnostische hulpprogramma's](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Leer hoe u [Route Azure diagnostische informatie inzicht krijgen in toepassing](./azure-diagnostics-configure-applicationinsights.md)
- Als u problemen met diagnostische gegevens starten of uw gegevens zoeken in Azure Storage tabellen ondervindt, raadpleegt u [Azure diagnostisch hulpprogramma probleemoplossing](./azure-diagnostics-troubleshooting.md)
