<properties
    pageTitle="RBAC: Ingebouwde rollen | Microsoft Azure"
    description="In dit onderwerp worden de ingebouwde rollen voor Rolgebaseerd toegangsbeheer (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Ingebouwde rollen

Azure Rolgebaseerd Access besturingselement RBAC () wordt geleverd met de volgende ingebouwde functies die kunnen worden toegewezen aan gebruikers, groepen en -services. U kunt de definities van ingebouwde rollen niet wijzigen. U kunt echter [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) aanpassen aan de specifieke behoeften van uw organisatie maken.

## <a name="roles-in-azure"></a>Rollen in Azure wordt aangegeven

De volgende tabel bevat een korte beschrijving van de ingebouwde functies. Klik op de naam van de functie voor een gedetailleerd overzicht van **Acties** en **notactions** voor de rol. De eigenschap **Acties** bevat de toegestane acties op Azure resources. Tekenreeksen met de actie kunnen jokertekens gebruiken. De eigenschap **notactions** geeft de bewerkingen die zijn uitgesloten van de toegestane acties.

>[AZURE.NOTE] De roldefinities van Azure zijn voortdurend ontwikkeling. In dit artikel is up-to-date zo mogelijk, maar u kunt altijd de meest recente rollen definities vinden in Azure PowerShell. Gebruik de cmdlets `(get-azurermroledefinition "<role name>").actions` of `(get-azurermroledefinition "<role name>").notactions` zoals van toepassing.

| De rolnaam van de | Beschrijving |
| --------- | ----------- |
| [API Management Service Inzender](#api-management-service-contributor) | API Management-services kunt beheren |
| [Toepassing inzichten onderdeel Inzender](#application-insights-component-contributor) | Onderdelen van de toepassing inzichten kunt beheren |
| [Automatisering Operator](#automation-operator) | Kunnen starten, stoppen, onderbreken en hervatten taken |
| [BizTalk Inzender](#biztalk-contributor) | BizTalk-services kunt beheren |
| [ClearDB MySQL DB Inzender](#cleardb-mysql-db-contributor) | ClearDB MySQL-databases kunt beheren |
| [Inzender](#contributor) | Alles behalve access kunt beheren. |
| [Gegevens Factory Inzender](#data-factory-contributor) | Kunt maken en beheren van gegevens factory's en onderliggende resources daarin. |
| [DevTest Labs gebruiker](#devtest-labs-user) | Kunnen alles weergeven en verbinding kunt maken, start opnieuw starten en afsluiten virtuele machines |
| [DNS-Zone Inzender](#dns-zone-contributor) | DNS-zones en records kunnen beheren |
| [DocumentDB Account Inzender](#documentdb-account-contributor) | DocumentDB accounts kunt beheren |
| [Intelligente systemen Account Inzender](#intelligent-systems-account-contributor) | Intelligente systemen accounts kunt beheren |
| [Netwerk Inzender](#network-contributor) | Alle resources van het netwerk kunt beheren |
| [Nieuwe Relic APM Account Inzender](#new-relic-apm-account-contributor) | Nieuwe Relic prestaties Toepassingsbeheer accounts en toepassingen kunt beheren |
| [Eigenaar](#owner) | Alles, inclusief toegang kunt beheren |
| [Lezer](#reader) | Alles kunt bekijken, maar geen wijzigingen aanbrengen |
| [Bestand Cache Inzender Vgx.](#redis-cache-contributor) | Bestand Vgx. cache kunt beheren |
| [Scheduler taak verzamelingen Inzender](#scheduler-job-collections-contributor) | Scheduler taak verzamelingen kunt beheren |
| [Zoeken Service Inzender](#search-service-contributor) | Zoekservices kunt beheren |
| [Beveiliging Manager](#security-manager) | Beveiligingsonderdelen, beveiligingsbeleid voor apparaten en virtuele machines kunt beheren |
| [SQL DB Inzender](#sql-db-contributor) | SQL-databases, maar niet hun-beveiliging beleid kunt beheren |
| [Manager van SQL-beveiliging](#sql-security-manager) | Het beleid-beveiliging van de SQL-servers en databases kunt beheren |
| [SQL Server Inzender](#sql-server-contributor) | SQL-servers en databases, maar niet hun-beveiliging beleid kunt beheren |
| [Klassieke opslag Account Inzender](#classic-storage-account-contributor) | Klassieke opslag accounts kunt beheren |
| [Opslag Account Inzender](#storage-account-contributor) | Opslag accounts kunt beheren |
| [Beheerder van de gebruiker toegang](#user-access-administrator) | De toegang van gebruikers tot Azure bronnen kunt beheren |
| [Klassieke VM Inzender](#classic-virtual-machine-contributor) | Klassieke virtuele machines, maar niet het virtuele netwerk of opslag-account waarmee ze zijn verbonden kunt beheren |
| [VM Inzender](#virtual-machine-contributor) | Virtuele machines, maar niet het virtuele netwerk of opslag-account waarmee ze zijn verbonden kunt beheren |
| [Klassieke netwerk Inzender](#classic-network-contributor) | Klassieke virtuele netwerken en gereserveerde IP-adressen kunt beheren |
| [Web abonnement Inzender](#web-plan-contributor) | Web-abonnementen kunt beheren |
| [Website Inzender](#website-contributor) | Websites, maar niet de web-abonnementen waarop deze zijn aangesloten kunt beheren |

## <a name="role-permissions"></a>Rolmachtigingen
De volgende tabellen worden de specifieke machtigingen die u aan elke rol beschreven. Deze kunt **Acties**, waardoor u machtigingen, en **NotActions**, die ze beperken opnemen.

### <a name="api-management-service-contributor"></a>API Management Service Inzender
API Management-services kunt beheren

| **Acties** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Maken en beheren van API Management-Services |
| Microsoft.Authorization/*/read | Autorisatie lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lees rollen en roltoewijzingen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="application-insights-component-contributor"></a>Toepassing inzichten onderdeel Inzender
Onderdelen van de toepassing inzichten kunt beheren

| **Acties** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.Insights/components/* | Maken en beheren van inzichten onderdelen |
| Microsoft.Insights/webtests/* | Maken en beheren van web tests |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="automation-operator"></a>Automatisering Operator
Kunnen starten, stoppen, onderbreken en hervatten taken

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en roltoewijzingen |
| Microsoft.Automation/automationAccounts/jobs/read | Lees automatisering account taken |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Een taak van de account automatisering hervatten |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Een taak van de account automatisering stoppen |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Automatisering account taak streams lezen |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Een taak van de account automatisering uitstellen |
| Microsoft.Automation/automationAccounts/jobs/write | Schrijven automatisering account taken |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Een taak van automatisering rapportageschema lezen |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Een taak van automatisering rapportageschema lezen |
| Microsoft.Automation/automationAccounts/read | Automatisering accounts lezen |
| Microsoft.Automation/automationAccounts/runbooks/read | Automatisering runbooks lezen |
| Microsoft.Automation/automationAccounts/schedules/read | Automatisering rapportageschema lezen |
| Microsoft.Automation/automationAccounts/schedules/write | Automatisering rapportageschema schrijven |
| Microsoft.Insights/components/* | Maken en beheren van inzichten onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="biztalk-contributor"></a>BizTalk Inzender
BizTalk-services kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en roltoewijzingen |
| Microsoft.BizTalkServices/BizTalk/* | Maken en beheren van BizTalk-services |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Inzender
ClearDB MySQL-databases kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| successbricks.cleardb/databases/* | Maken en beheren van ClearDB MySQL-databases |

### <a name="contributor"></a>Inzender
Alles behalve access kunt beheren

| **Acties** ||
| ------- | ------ |
| * | Maken en beheren van bronnen van alle typen |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Rollen en roltoewijzingen verwijderen niet |  
| Microsoft.Authorization/*/Write | Rollen en roltoewijzingen maken geen |

### <a name="data-factory-contributor"></a>Gegevens Factory Inzender
Maken en beheren van gegevens factory's en onderliggende resources daarin.

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.DataFactory/dataFactories/* | Maken en beheren van gegevens factory's en onderliggende resources daarin. |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="devtest-labs-user"></a>DevTest Labs gebruiker
Kunnen alles weergeven en verbinding kunt maken, start opnieuw starten en afsluiten virtuele machines

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Compute/availabilitySets/read | De eigenschappen van beschikbaarheid sets gelezen |
| Microsoft.Compute/virtualMachines/*/read | Lees de eigenschappen van een virtuele machine (VM formaten, runtime status, VM extensies, enz.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Toewijzing van virtuele machines |
| Microsoft.Compute/virtualMachines/read | De eigenschappen van een virtuele machine gelezen |
| Microsoft.Compute/virtualMachines/restart/action | Start opnieuw virtuele machines |
| Microsoft.Compute/virtualMachines/start/action | Virtuele machines starten |
| Microsoft.DevTestLab/*/read | De eigenschappen van een laboratorium gelezen |
| Microsoft.DevTestLab/labs/createEnvironment/action | Een testomgeving |
| Microsoft.DevTestLab/labs/formulas/delete | Formules verwijderen |
| Microsoft.DevTestLab/labs/formulas/read | Alleen formules |
| Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen van formules |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Beleidsregels voor testomgeving evalueren |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Deelnemen aan een groep met laden verdeling backend-adressen |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Deelnemen aan een taakverdeling binnenkomende NAT-regel |
| Microsoft.Network/networkInterfaces/*/read | De eigenschappen van een netwerkinterface (bijvoorbeeld alle de netwerktaakverdelers die de netwerkinterface een deel van is) lezen |
| Microsoft.Network/networkInterfaces/join/action | Deelnemen aan een virtuele Machine naar een netwerkinterface |
| Microsoft.Network/networkInterfaces/read | Netwerkinterfaces lezen |
| Microsoft.Network/networkInterfaces/write | Netwerkinterfaces schrijven |
| Microsoft.Network/publicIPAddresses/*/read | De eigenschappen van een openbare IP-adres gelezen |
| Microsoft.Network/publicIPAddresses/join/action | Deelnemen aan een openbare IP-adres |
| Microsoft.Network/publicIPAddresses/read | Lees het openbare IP-netwerkadressen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Deelnemen aan een virtueel netwerk |
| Microsoft.Resources/deployments/operations/read | Implementatie bewerkingen lezen |
| Microsoft.Resources/deployments/read | Alleen-implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Storage/storageAccounts/listKeys/action | Lijst opslag account toetsen |

### <a name="dns-zone-contributor"></a>DNS-Zone Inzender
DNS-zones en records kunnen worden beheren.

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/\*/lezen | Lees rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/\* | Maken en beheren van waarschuwingsregels |
| Microsoft.Network/dnsZones/\* | DNS-zones en records maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | De status van de bronnen lezen |
| Microsoft.Resources/deployments/\* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/\* | Maken en ondersteuningstickets beheren |


### <a name="documentdb-account-contributor"></a>DocumentDB Account Inzender
DocumentDB accounts kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.DocumentDb/databaseAccounts/* | DocumentDB rekeningen maken en beheren |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="intelligent-systems-account-contributor"></a>Intelligente systemen Account Inzender
Intelligente systemen accounts kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.IntelligentSystems/accounts/* | Intelligente systemen rekeningen maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="network-contributor"></a>Netwerk Inzender
Alle resources van het netwerk kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.Network/* | Maken en beheren van netwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="new-relic-apm-account-contributor"></a>Nieuwe Relic APM Account Inzender
Nieuwe Relic prestaties Toepassingsbeheer accounts en toepassingen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| NewRelic.APM/accounts/* | Nieuwe Relic toepassing prestaties management rekeningen maken en beheren |

### <a name="owner"></a>Eigenaar
Alles, inclusief toegang kunt beheren

| **Acties** ||
| ------- | ------ |
| * | Maken en beheren van bronnen van alle typen |

### <a name="reader"></a>Lezer
Alles kunt bekijken, maar geen wijzigingen aanbrengen

| **Acties** ||
| ------- | ------ |
| * / lezen | Lees resources met alle typen, behalve geheimen. |

### <a name="redis-cache-contributor"></a>Bestand Cache Inzender Vgx.
Bestand Vgx. cache kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Cache/redis/* | Maken en beheren van cache bestand Vgx. |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="scheduler-job-collections-contributor"></a>Scheduler taak verzamelingen Inzender
Scheduler taak verzamelingen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Scheduler/jobcollections/* | Maken en beheren van verzamelingen van taak |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren  |

### <a name="search-service-contributor"></a>Zoeken Service Inzender
Zoeken-services kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Search/searchServices/* | Maken en beheren van zoekservices |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren  |

### <a name="security-manager"></a>Beveiliging Manager
Beveiligingsonderdelen, beveiligingsbeleid voor apparaten en virtuele machines kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.ClassicCompute/*/read | Configuratiegegevens lezen klassieke berekenen virtuele machines |
| Microsoft.ClassicCompute/virtualMachines/*/write | Configuratie voor virtuele machines schrijven |
| Microsoft.ClassicNetwork/*/read | Configuratie van informatie over klassieke netwerk lezen  |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Security/* | Beveiligingsonderdelen en beleid voor het maken en beheren |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren  |

### <a name="sql-db-contributor"></a>SQL DB Inzender
SQL-databases, maar niet hun-beveiliging beleid kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Lees rollen en rol toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Sql/servers/databases/* | Maken en beheren van de SQL-databases |
| Microsoft.Sql/servers/read | SQL-Servers lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Beleid voor het controlelogboek bewerken niet |
| Microsoft.Sql/servers/databases/auditingSettings/* | Controle-instellingen bewerken niet |
| Microsoft.Sql/servers/databases/auditRecords/read  | Controlerecords lezen niet |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Beleid voor de verbinding bewerken niet |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Gegevens masking beleidsregels bewerken niet |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid voor apparaten bewerken niet |
| Microsoft.Sql/servers/databases/securityMetrics/* | Beveiliging aan de doelstellingen bewerken niet |

### <a name="sql-security-manager"></a>Manager van SQL-beveiliging
Het beleid-beveiliging van de SQL-servers en databases kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Alleen Microsoft autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Sql/servers/auditingPolicies/* | Maken en beheren van SQL server-controle beleid |
| Microsoft.Sql/servers/auditingSettings/* | Maken en beheren van SQL server controle-instelling |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Maken en beheren van controle beleidsregels voor SQL server-database |
| Microsoft.Sql/servers/databases/auditingSettings/* | Maken en beheren van SQL server-database controle-instellingen |
| Microsoft.Sql/servers/databases/auditRecords/read | Alleen controlerecords |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Maken en beheren van beleidsregels voor SQL server-database |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Maken en beheren van SQL server-databasegegevens masking beleidsregels |
| Microsoft.Sql/servers/databases/read | Alleen SQL-databases |
| Microsoft.Sql/servers/databases/schemas/read | Schema's lezen SQL server-database |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Tabelkolommen voor alleen SQL server-database |
| Microsoft.Sql/servers/databases/schemas/tables/read | Alleen SQL server-databasetabellen |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Maken en beheren van meldingen beveiligingsbeleid van SQL server-database |
| Microsoft.Sql/servers/databases/securityMetrics/* | Maken en beheren van de doelstellingen van SQL server-database beveiliging |
| Microsoft.Sql/servers/read | SQL-Servers lezen |
| Microsoft.Sql/servers/securityAlertPolicies/* | Maken en beheren van SQL server waarschuwing beveiligingsbeleid |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="sql-server-contributor"></a>SQL Server Inzender
SQL-servers en databases, maar niet hun-beveiliging beleid kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisatie lezen|
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Sql/servers/* | Maken en beheren van de SQL-servers |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | SQL server-controle beleid bewerken niet |
| Microsoft.Sql/servers/auditingSettings/* | SQL server controle-instellingen bewerken niet |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Controle beleidsregels voor SQL server-database bewerken niet |
| Microsoft.Sql/servers/databases/auditingSettings/* | SQL server-database controle-instellingen bewerken niet |
| Microsoft.Sql/servers/databases/auditRecords/read  | Controlerecords lezen niet |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Beleidsregels voor SQL server-database bewerken niet |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server-databasegegevens masking beleidsregels bewerken niet |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid van SQL server-database bewerken niet |
| Microsoft.Sql/servers/databases/securityMetrics/* | SQL server-database beveiliging doelstellingen bewerken niet |
| Microsoft.Sql/servers/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid SQL server bewerken niet |

### <a name="classic-storage-account-contributor"></a>Klassieke opslag Account Inzender
Klassieke opslag accounts kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisatie lezen |
| Microsoft.ClassicStorage/storageAccounts/* | Opslag rekeningen maken en beheren |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="storage-account-contributor"></a>Opslag Account Inzender
Kunt opslagruimte accounts beheren, maar geen toegang tot deze.

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Alle autorisatie lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.Insights/diagnosticSettings/* | Diagnostische instellingen beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Storage/storageAccounts/* | Opslag rekeningen maken en beheren |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="user-access-administrator"></a>Beheerder van de gebruiker toegang
De toegang van gebruikers tot Azure bronnen kunt beheren

| **Acties** ||
| ------- | ------ |
| * / lezen | Lees resources met alle typen, behalve geheimen. |
| Microsoft.Authorization/* | Machtigingen beheren |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="classic-virtual-machine-contributor"></a>Klassieke VM Inzender
Klassieke virtuele machines, maar niet het virtuele netwerk of opslag-account waarmee ze zijn verbonden kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisatie lezen |
| Microsoft.ClassicCompute/domainNames/* | Maken en beheren van klassieke berekeningscluster domeinnamen |
| Microsoft.ClassicCompute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Deelnemen aan netwerk-beveiligingsgroepen |
| Microsoft.ClassicNetwork/reservedIps/link/action | Gereserveerde IP-adressen koppelen |
| Microsoft.ClassicNetwork/reservedIps/read | Alleen gereserveerd IP-adressen |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Deelnemen aan virtuele netwerken |
| Microsoft.ClassicNetwork/virtualNetworks/read | Virtuele netwerken lezen |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Account schijven lezen |
| Microsoft.ClassicStorage/storageAccounts/images/read | Opslag account afbeeldingen lezen |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lijst opslag account toetsen |
| Microsoft.ClassicStorage/storageAccounts/read | Klassieke opslag accounts lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="virtual-machine-contributor"></a>VM Inzender
Virtuele machines, maar niet het virtuele netwerk of opslag-account waarmee ze zijn verbonden kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisatie lezen |
| Microsoft.Compute/availabilitySets/* | Maken en beheren van berekeningscluster beschikbaarheid sets |
| Microsoft.Compute/locations/* | Maken en beheren van berekeningscluster locaties |
| Microsoft.Compute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.Compute/virtualMachineScaleSets/* | Maken en beheren van VM schaal sets |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Deelnemen aan netwerk gateway backend-mailadres van toepassingen |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Deelnemen aan laden verdeling backend-mailadres van toepassingen |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Deelnemen aan taakverdeling inkomende NAT-toepassingen |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Deelnemen aan taakverdeling binnenkomende NAT regels |
| Microsoft.Network/loadBalancers/read | Netwerktaakverdelers lezen |
| Microsoft.Network/locations/* | Maken en beheren van netwerklocaties |
| Microsoft.Network/networkInterfaces/* | Maken en beheren van netwerkinterfaces |
| Microsoft.Network/networkSecurityGroups/join/action | Deelnemen aan netwerk beveiligingsgroepen |
| Microsoft.Network/networkSecurityGroups/read | Beveiligingsgroepen netwerk lezen |
| Microsoft.Network/publicIPAddresses/join/action | Deelnemen aan het openbare IP-netwerkadressen |
| Microsoft.Network/publicIPAddresses/read | Lees het openbare IP-netwerkadressen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken lezen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Deelnemen aan virtueel netwerksubnetten |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Storage/storageAccounts/listKeys/action | Lijst opslag account toetsen |
| Microsoft.Storage/storageAccounts/read | Opslag accounts lezen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="classic-network-contributor"></a>Klassieke netwerk Inzender
Klassieke virtuele netwerken en gereserveerde IP-adressen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisatie lezen |
| Microsoft.ClassicNetwork/* | Maken en beheren van klassieke-netwerken te gebruiken |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="web-plan-contributor"></a>Web abonnement Inzender
Web-abonnementen kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisatie lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/serverFarms/* | Maken en beheren van server-farms |

### <a name="website-contributor"></a>Website Inzender
Websites, maar niet de web-abonnementen waarop deze zijn aangesloten kunt beheren

| **Acties** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autorisatie lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van inzichten waarschuwingsregels |
| Microsoft.Insights/components/* | Maken en beheren van inzichten onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resource groep implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Resourcegroepen lezen |  
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/certificates/* | Maken en beheren van de van Websitecertificaten |
| Microsoft.Web/listSitesAssignedToHostName/read | Sites die zijn toegewezen aan een hostnaam lezen |
| Microsoft.Web/serverFarms/join/action | Deelnemen aan server-farms |
| Microsoft.Web/serverFarms/read | Server-farms lezen |
| Microsoft.Web/sites/* | Maken en beheren van websites |

## <a name="see-also"></a>Zie ook
- [Toegangsbeheer op basis van rollen](role-based-access-control-configure.md): aan de slag met RBAC in de portal van Azure.
- [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aan uw behoeften voldoet van access.
- [Maken een access-geschiedenisrapport wijzigen](role-based-access-control-access-change-history-report.md): veranderende roltoewijzingen in RBAC bijhouden van.
- [Toegangsbeheer op basis van rollen probleemoplossing](role-based-access-control-troubleshooting.md): suggesties voor het oplossen van veelvoorkomende problemen ophalen.
