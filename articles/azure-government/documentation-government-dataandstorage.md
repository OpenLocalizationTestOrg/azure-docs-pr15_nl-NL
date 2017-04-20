<properties
    pageTitle="Azure overheid documentatie | Microsoft Azure"
    description="Dit vindt u een vergelijking van functies en informatie over het ontwikkelen van toepassingen voor de overheid van Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/30/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-data-and-storage"></a>Gegevens van Azure overheid en opslag

##  <a name="azure-storage"></a>Azure-opslag

Zie voor meer informatie over deze service en hoe u dit gebruikt, [Azure Storage openbare documentatie](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Variaties

De URL's voor opslag-accounts in Azure overheid verschillen:

Servicetype|Azure openbare|Azure overheid
---|---|---
-Blobopslag|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
Opslag in wachtrij|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Table Storage|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Al uw scripts en code moet account voor de eindpunten.  Zie [Azure opslag verbindingstekenreeksen configureren](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

Zie voor meer informatie over API's de <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Cloud opslag Account Constructor</a>.

Het achtervoegsel eindpunt voor gebruik in deze overbelastingen is core.usgovcloudapi.net 

### <a name="considerations"></a>Overwegingen

De volgende informatie geeft u de grens Azure overheid voor Azure Storage:

| Geregeld/beheerde gegevens die zijn toegestaan | Geregeld/beheerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gegevens hebt ingevoerd, opgeslagen en verwerkt binnen een Azure-opslag product exporteren beheerde gegevens bevatten. Statische verificators, zoals wachtwoorden en smartcard pincodes voor toegang tot Azure platformonderdelen. Persoonlijke sleutels van certificaten die worden gebruikt om Azure platformonderdelen te beheren. Andere beveiliging informatie/geheimen, zoals certificaten, versleuteling toetsen outmodel toetsen en opslag toetsen die zijn opgeslagen in Azure services. | Azure opslag metagegevens is niet toegestaan om te exporteren beheerde gegevens bevatten. Deze metagegevens bevat alle configuratiegegevens ingevoerd bij het maken en onderhouden van uw opslagproduct.  Voer geen Regulated/gebruikersgegevens in de volgende velden: resourcegroepen, namen van de implementatie, resourcenamen, Resource-labels  

##  <a name="premium-storage"></a>Premium-opslag

Zie voor meer informatie over deze service en hoe u dit gebruikt, [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Variaties

Premium opslag is beschikbaar in de USGov Virginia. Dit geldt ook voor DS-reeks virtuele Machines. 

### <a name="considerations"></a>Overwegingen

Dezelfde opslag gegevens overwegingen bovenstaande zijn van toepassing op premium opslag accounts. 

##  <a name="sql-database"></a>SQL-Database

Raadpleeg het<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft Beveiligingscentrum voor SQL Database-Engine</a> en [Azure SQL Database openbare documentatie](https://azure.microsoft.com/documentation/services/sql-database/) voor extra hulp bij het metagegevens zichtbaarheid configuratie en aanbevolen procedures voor beveiliging.

### <a name="variations"></a>Variaties

SQL-Database van V12 is in het algemeen beschikbaar zijn in Azure overheid.

Het adres van de SQL Azure-Servers in Azure overheid verschilt:

Servicetype|Azure openbare|Azure overheid
---|---|---
SQL-Database|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Overwegingen

De volgende informatie geeft u de grens Azure overheid voor Azure Storage:

| Geregeld/beheerde gegevens die zijn toegestaan | Geregeld/beheerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens worden opgeslagen en verwerkt in Microsoft Azure SQL bevatten Azure overheid geregeld gegevens. Hulpmiddelen voor databases moet u gebruiken voor de overdracht van de gegevens van Azure overheid geregeld gegevens. | Azure SQL-metagegevens is niet toegestaan om te exporteren beheerde gegevens bevatten. Deze metagegevens bevat alle configuratiegegevens ingevoerd bij het maken en onderhouden van uw opslagproduct.  Voer geen geregeld/beheerde gegevens in de volgende velden: Database naam, de naam van abonnement, resourcegroepen, servernaam, aanmelden bij Server-beheerder, namen van de implementatie, resourcenamen, Resource-labels

##  <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
