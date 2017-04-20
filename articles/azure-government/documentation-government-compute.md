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
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Azure overheid berekenen

##  <a name="virtual-machines"></a>Virtuele Machines

Zie voor meer informatie over deze service en hoe u dit gebruikt, [Azure virtuele Machines grootte](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Variaties

De volgende VM SKU's zijn algemeen beschikbaar (GA) in voor de overheid Azure:

VM SKU|Amerikaans beurs VA|Amerikaans beurs IA|Notities
---|---|---|---
A|GA|GA|Geen
Dv1|GA|-|Geen
DSv1|GA|-|Geen
Dv2|GA|GA|15 is binnenkort beschikbaar
F|GA|GA|Geen
G|Geplande|-|Geen

###  <a name="data-considerations"></a>Aandachtspunten voor de gegevens

De volgende informatie geeft aan wat de grens Azure overheid voor Azure virtuele Machines:

| Geregeld/beheerde gegevens die zijn toegestaan | Geregeld/beheerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gegevens hebt ingevoerd, opgeslagen en verwerkt binnen een VM exporteren beheerde gegevens bevatten. Binaire bestanden binnen Azure virtuele Machines uitgevoerd. Statische verificators, zoals wachtwoorden en smartcard pincodes voor toegang tot Azure platformonderdelen. Persoonlijke sleutels van certificaten die worden gebruikt om Azure platformonderdelen te beheren. SQL-verbindingstekenreeksen.  Andere beveiliging informatie/geheimen, zoals certificaten, versleuteling toetsen outmodel toetsen en opslag toetsen die zijn opgeslagen in Azure services.  | Metagegevens is niet toegestaan om te exporteren beheerde gegevens bevatten. Deze metagegevens bevat alle configuratiegegevens ingevoerd bij het maken en onderhouden van uw Azure virtuele machines.  Voer geen Regulated/gebruikersgegevens in de volgende velden: Tenant rolnamen, resourcegroepen, namen van de implementatie, resourcenamen, Resource-labels  

## <a name="next-steps"></a>Volgende stappen

Aanvullende informatie en updates, abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
