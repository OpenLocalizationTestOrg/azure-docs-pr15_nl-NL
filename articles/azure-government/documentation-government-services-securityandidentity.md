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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure overheid beveiliging en -identiteit

##  <a name="key-vault"></a>Belangrijke kluis
Zie voor meer informatie over deze service en hoe u dit gebruikt, de <a href="https://azure.microsoft.com/documentation/services/key-vault">openbare documentatie Azure-toets kluis.</a>

### <a name="data-considerations"></a>Aandachtspunten voor de gegevens
De volgende informatie geeft u de grens Azure overheid voor Azure-toets kluis:

| Geregeld/beheerde gegevens die zijn toegestaan | Geregeld/beheerde gegevens niet toegestaan |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle gegevens worden versleuteld met een sleutel Azure-toets kluis kan Regulated/beheerde gegevens bevatten. | Azure-toets kluis metagegevens is niet toegestaan om te exporteren beheerde gegevens bevatten. Deze metagegevens bevat alle configuratiegegevens ingevoerd bij het maken en onderhouden van uw sleutel kluis.  Voer geen Regulated/gebruikersgegevens in de volgende velden: groep resourcenamen, sleutel kluis namen, de naam van abonnement |

Toets kluis is in het algemeen beschikbaar zijn in Azure overheid. Zoals in openbare is het geen extensie, dus toets kluis alleen beschikbaar via PowerShell en CLI is.

## <a name="next-steps"></a>Volgende stappen

Aanvullende informatie en updates, abonneren op de <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure overheid Blog.</a>
