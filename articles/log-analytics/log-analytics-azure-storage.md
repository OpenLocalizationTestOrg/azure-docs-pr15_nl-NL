<properties
    pageTitle="Verzamelen van gegevens van Azure opslagruimte in Log Analytics overzicht | Microsoft Azure"
    description="Azure resources kunnen schrijven logboeken en aan de doelstellingen aan een account Azure opslag, vaak met behulp van Azure diagnostische gegevens. Log Analytics kunnen deze gegevens indexeren en deze doorzoekbare maken."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Verzamelen van gegevens van Azure opslagruimte in Log Analytics-overzicht

Veel Azure resources kunnen logboeken en statistieken schrijven met een Azure opslag-account. Log analyses kunt gebruiken deze gegevens en het gemakkelijker maken om te controleren uw Azure bronnen.

Als u wilt schrijven met Azure storage een resource mogelijk Azure diagnostische hulpprogramma's gebruikt of een eigen manier om gegevens te schrijven. Deze gegevens kan worden geschreven in verschillende indelingen naar een van de volgende locaties:

+ Azure tabel
+ Azure blob
+ EventHub

Log Analytics ondersteunt Azure services die gegevens met behulp van [Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)schrijven. Log Analytics ook andere diensten die uitvoer logboeken en aan de doelstellingen in verschillende notaties en locaties.  

>[AZURE.NOTE] U normale Azure gegevens tarieven voor opslagruimte en transacties wanneer u diagnostische gegevens naar een opslag-account verzendt en voor wanneer de gegevens van uw account opslag Log Analytics leest in rekening worden gebracht.

![Azure opslag-diagram](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Ondersteunde Azure Resources

Log Analytics kunt verzamelen van gegevens voor de volgende Azure bronnen:

| Resourcetype | Logboeken (diagnostische categorieën) | Log Analytics-oplossing |
| --------------------------------------- | -------------------------------- | --------------- |
| Toepassing inzichten | Beschikbaarheid <br> Aangepaste gebeurtenissen <br> Uitzonderingen <br> Aanvragen <br> | Toepassing inzichten (Preview) |
| API-beheer | | *geen* (Preview) |
| Automatisering <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (Preview) |
| Belangrijke kluis <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (Preview) |
| Toepassingsgateway <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (Preview) |
| Beveiligingsgroep netwerk <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (Preview) |
| Service stof                          | ETWEvent <br> Operationele gebeurtenis <br> Betrouwbare acteur-gebeurtenis <br> Betrouwbare Service gebeurtenis| ServiceFabric (Preview) |
| Virtuele Machines | Linux Syslog <br> Windows-gebeurtenis <br> IIS-logboek <br> Windows ETWEvent | *geen* |
| Web rollen <br> Werknemer rollen | Linux Syslog <br> Windows-gebeurtenis <br> IIS-logboek <br> Windows ETWEvent | *geen* |

>[AZURE.NOTE] Voor het controleren van Azure virtuele machines (Linux en Windows), is het raadzaam te installeren de [Log Analytics VM extensie](log-analytics-azure-vm-extension.md). De agent vindt u meer inzicht in uw virtuele machines dan als u de diagnostische hulpprogramma's naar opslag geschreven.

U kunt ons prioriteit voorzien van extra logboeken voor OMS om te stemmen op onze [feedbackpagina](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy)analyseren.


- Zie [Azure analyseren diagnostische logboeken door middel van Log analyses](log-analytics-azure-storage-json.md) voor meer informatie over hoe Log Analytics de logboeken kunt lezen van Azure services die ondersteuning bieden voor [Azure diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md):
  - Azure belangrijke kluis
  - Azure automatisering
  - Toepassingsgateway
  - Beveiligingsgroepen netwerk
- Zie [Gebruik-blobopslag voor IIS- en -tabelopslag voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) voor meer informatie over hoe Log Analytics de logboeken lezen kunt voor voor Azure-services die schrijven diagnostisch hulpprogramma tabelopslag of IIS-logboeken geschreven naar blob-opslag, waaronder:
  - Service stof
  - Web rollen
  - Werknemer rollen
  - Virtuele Machines


Er is een toepassing inzichten in privé preview en deze gebruik continue exporteren naar blob storage. Neem contact op met uw team van Microsoft-Account als u wilt deelnemen aan de privé preview, of Raadpleeg de informatie over de [feedback-site](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Volgende stappen

- [De diagnostische logboeken Azure analyseren door middel van Log analyses](log-analytics-azure-storage-json.md) lezen van de logboekbestanden van Azure services die schrijven diagnostisch hulpprogramma met blob storage in JSON-indeling.
- [Gebruik-blobopslag voor IIS- en -tabelopslag voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) lezen van de logboekbestanden van Azure-services die schrijven diagnostisch hulpprogramma tabelopslag of IIS-logboeken naar blob storage geschreven.
- [Oplossingen inschakelen](log-analytics-add-solutions.md) voor bieden inzicht in de gegevens.
- [Gebruik zoekopdrachten gaat uitvoeren](log-analytics-log-searches.md) om de gegevens te analyseren.
