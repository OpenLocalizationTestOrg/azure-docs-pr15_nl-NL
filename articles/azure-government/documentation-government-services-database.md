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
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Voor de overheid Azure-Databases

##  <a name="sql-database"></a>SQL-Database

Raadpleeg het<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft Beveiligingscentrum voor SQL Database-Engine</a> en [Azure SQL Database openbare documentatie](https://azure.microsoft.com/documentation/services/sql-database/) voor extra hulp bij het metagegevens zichtbaarheid configuratie en aanbevolen procedures voor beveiliging.

### <a name="variations"></a>Variaties

SQL-Database van V12 is in het algemeen beschikbaar zijn in Azure overheid.

Het adres van de SQL Azure-Servers in Azure overheid verschilt:

Servicetype|Azure openbare|Azure overheid
---|---|---
SQL-Database|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Overwegingen

De volgende informatie geeft u de grens Azure overheid voor SQL Azure:

| Geregeld/beheerde gegevens die zijn toegestaan | Geregeld/beheerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens worden opgeslagen en verwerkt in Microsoft Azure SQL bevatten Azure overheid geregeld gegevens. Hulpmiddelen voor databases moet u gebruiken voor de overdracht van de gegevens van Azure overheid geregeld gegevens. | Azure SQL-metagegevens is niet toegestaan om te exporteren beheerde gegevens bevatten. Deze metagegevens bevat alle configuratiegegevens ingevoerd bij het maken en onderhouden van uw opslagproduct.  Voer geen geregeld/beheerde gegevens in de volgende velden: Database naam, de naam van abonnement, resourcegroepen, servernaam, aanmelden bij Server-beheerder, namen van de implementatie, resourcenamen, Resource-labels

## <a name="azure-redis-cache"></a>Cache Azure bestand Vgx.

Zie voor meer informatie over deze service en hoe u dit gebruikt, [openbare documentatie Azure bestand Vgx. Cache](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Variaties

De URL's voor het openen en beheren van Azure bestand Vgx. Cache in Azure overheid verschillen:

Servicetype|Azure openbare|Azure overheid
---|---|---
Cache-eindpunt|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure-portal|https://Portal.Azure.com|https://Portal.Azure.us

>[AZURE.NOTE] Al uw scripts en code moet de juiste eindpunten en omgevingen. Zie [hoe u verbinding maakt met Azure overheid Cloud](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud)voor meer informatie.


### <a name="considerations"></a>Overwegingen

De volgende informatie geeft u de grens Azure overheid voor Azure bestand Vgx. Cache:

| Geregeld/beheerde gegevens die zijn toegestaan | Geregeld/beheerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens worden opgeslagen en verwerkt in Azure bestand Vgx. Cache bevatten Azure overheid geregeld gegevens. | Azure bestand Vgx. Cache metagegevens is niet toegestaan om te exporteren beheerde gegevens bevatten. Voer geen geregeld/beheerde gegevens in de volgende velden: naam, de naam van de VNET, de naam van abonnement, resourcegroepen, labels van de Resource, eigenschappen bestand Vgx. in Cache.  

##  <a name="next-steps"></a>Volgende stappen

Voor aanvullende informatie en updates abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
