<properties
   pageTitle="Logboeken verzamelen met behulp van Linux Azure diagnostisch hulpprogramma | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe voor het instellen van Azure diagnostische hulpprogramma's voor het verzamelen van Logboeken vanaf een Service stof Linux-cluster uitvoert in Azure wordt aangegeven."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Logboeken verzamelen met behulp van Azure diagnostische gegevens

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Wanneer u een cluster Azure-Service stof uitvoert, is het een goed idee om de logboeken verzamelen van alle knooppunten in een centrale locatie. Met de logboeken in een centrale locatie kunt u gemakkelijk analyseren en problemen oplossen, ongeacht of deze in uw services, uw toepassing of het cluster zelf zijn. Er is een manier om te uploaden en logboeken verzamelen gebruik van de extensie Azure diagnostische hulpprogramma's, waarin logboeken naar de opslag van Azure uploadt. U kunt de gebeurtenissen lezen uit de opslag en deze in een product zoals [Elastische zoeken](service-fabric-diagnostic-how-to-use-elasticsearch.md) of een andere log parseren oplossing te plaatsen.

## <a name="log-sources-that-you-might-want-to-collect"></a>Log bronnen die u wilt verzamelen
- **Logboeken aan de service stof**: geeft het platform via [LTTng](http://lttng.org) en geüpload naar uw account opslag. Logboeken zijn operationele gebeurtenissen of runtime-gebeurtenissen die Hiermee plaatst u het platform. Deze logboeken worden opgeslagen in de locatie waar het manifest cluster geeft. (Als u de details van de account opslag, zoekt u de tag **AzureTableWinFabETWQueryable** en zoek **StoreConnectionString**.)
- **Toepassingsgebeurtenissen**: geeft de code van uw service. U kunt elke oplossing logboekregistratie waarmee gegevens worden geschreven op tekst gebaseerde logboekbestanden--bijvoorbeeld LTTng gebruiken. Zie de documentatie LTTng op tracering van de toepassing voor meer informatie.  


## <a name="deploy-the-diagnostics-extension"></a>De extensie diagnostische gegevens implementeren
De eerste stap in de logboeken verzamelen is om te implementeren van de extensie diagnostische gegevens op elk van de VMs in het cluster Service stof. De extensie naar diagnostische logboeken verzamelt op elke VM en uploadt ze naar de opslag-account dat u opgeeft. De stappen verschillen afhankelijk van of u de Azure beheerportal of Azure Resource Manager gebruiken.

Als u wilt implementeren de extensie diagnostische gegevens op de VMs in het cluster als onderdeel van het maken van het cluster, stelt u **Diagnostische gegevens** op **op**. Nadat u het cluster hebt gemaakt, kunt u deze instelling niet wijzigen met behulp van de portal.

Configureer vervolgens Linux Azure diagnostische gegevens (LAD) om de bestanden verzamelen en deze in uw account opslag plaatsen. Dit proces wordt uitgelegd als scenario 3 ("uploaden uw eigen logboekbestanden") in het artikel [LAD gebruiken om te controleren en een diagnose stellen bij Linux VMs](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md). Dit proces volgen, krijgt u toegang tot de traces. U kunt de traces uploaden naar een visualizer van uw keuze.

U kunt ook de extensie diagnostische gegevens met behulp van Azure resourcemanager implementeren. Het proces is vergelijkbaar voor Windows en Linux en voor Windows-clusters in [Logboeken met Azure diagnostische gegevens verzamelen](service-fabric-diagnostics-how-to-setup-wad.md)wordt beschreven.

U kunt ook bewerkingen Management Suite, zoals wordt beschreven in [Bewerkingen Management Suite Log analyses met Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Nadat u deze configuratie, bewaakt de LAD-agent de logboekbestanden van de opgegeven. Wanneer een nieuwe regel wordt toegevoegd aan het bestand, maakt het een syslog-fragment dat wordt verzonden naar de opslag die u hebt opgegeven.


## <a name="next-steps"></a>Volgende stappen
Als u wilt weten over uitgebreider welke gebeurtenissen die u bekijken moet terwijl u het oplossen van problemen, raadpleegt u [LTTng documentatie](http://lttng.org/docs) en [LAD gebruiken](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
