<properties
    pageTitle="Azure-toets kluis oplossing in Log Analytics | Microsoft Azure"
    description="U kunt de oplossing Azure-toets kluis in Log Analytics moet worden gereviseerd Azure-toets kluis Logboeken."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure-toets kluis (Preview)-oplossing in Log Analytics

>[AZURE.NOTE] Dit is een [voorbeeld-oplossing](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

U kunt de oplossing Azure-toets kluis in Log Analytics moet worden gereviseerd Azure-toets kluis AuditEvent Logboeken.

U kunt logboekregistratie van gebeurtenissen controleren voor Azure-toets kluis inschakelen. Deze logboeken worden met Azure-blobopslag waar ze kunnen vervolgens worden geïndexeerd door Log Analytics om te zoeken en analyse geschreven.

## <a name="install-and-configure-the-solution"></a>Installeren en configureren van de oplossing

Gebruik de volgende instructies voor het installeren en configureren van de oplossing Azure-toets kluis:

1.  [Diagnostische gegevens vastleggen voor sleutel kluis](../key-vault/key-vault-logging.md) bronnen die u wilt controleren inschakelen
2.  Log Analytics als u wilt de logboekbestanden van blobopslag lezen met behulp van het proces wordt beschreven in de [JSON-bestanden in-blobopslag](../log-analytics/log-analytics-azure-storage-json.md)configureren.
3.  De oplossing Azure-toets kluis inschakelen met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  

## <a name="review-azure-key-vault-data-collection-details"></a>Controleer Azure-toets kluis gegevens verzamelen details

Azure-toets kluis oplossing verzamelt diagnostische logboeken van Azure-blobopslag voor Azure-toets kluis.
Geen-agent is vereist voor het verzamelen van gegevens.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor Azure-toets kluis.

| Platform | Directe agent | Agent van systemen Center Operations Manager (SCOM) | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | Frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Azure|![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Ja](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Nee](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 minuten|

## <a name="use-azure-key-vault"></a>Azure belangrijke kluis gebruiken

Nadat u de oplossing hebt geïnstalleerd, kunt u het overzicht van aanvraag kunt invullen statussen na verloop van tijd voor uw gecontroleerde toets kluizen met behulp van de **Azure toets kluis** tegel weergeven op de pagina **Overzicht** van Log Analytics.

![afbeelding van Azure-toets kluis-tegel](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Nadat u op de tegel **Overzicht** , kunt u samenvattingen van uw logboeken weergeven en vervolgens analyseren voor de volgende categorieën:

- Het volume van alle belangrijke kluis bewerkingen na verloop van tijd
- Is mislukt bewerking volumes na verloop van tijd
- Latentie van gemiddeld operationele door bewerking
- Kwaliteit van de service voor bewerkingen met het aantal bewerkingen die meer dan 1000 ms duren en een lijst met bewerkingen die meer dan 1000 ms uitvoeren

![afbeelding van Azure-toets kluis dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![afbeelding van Azure-toets kluis dashboard](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Om voor elke willekeurige bewerking te bekijken

1. Klik op de tegel **Azure toets kluis** op de pagina **Overzicht** .
2. Op het dashboard **Azure toets kluis** de samenvatting van de gegevens in een van de bladen controleren en klik op een om gedetailleerde informatie over deze op de pagina log weer te geven.

    Klik op alle pagina's log zoeken, kunt u de resultaten bekijken door de tijd, gedetailleerde resultaten en de geschiedenis van log zoeken. U kunt ook filteren op aspecten en de resultaten te beperken.

## <a name="log-analytics-records"></a>Logboekrecords Analytics

De oplossing Azure-toets kluis analyseert records die een soort **KeyVaults** die zijn verzameld uit [AuditEvent logboeken](../key-vault/key-vault-logging.md) in Azure diagnostische hulpprogramma's.  Eigenschappen voor deze records zijn in de volgende tabel.  

| Eigenschap | Beschrijving |
|:--|:--|
| Type | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | IP-adres van de client die de aanvraag hebben gedaan |
| Categorie      | Toets kluis logboekbestanden is AuditEvent de waarde enkel, beschikbaar.|
| CorrelationId | Een optioneel GUID die de client doorgeven kunt als u wilt relateren aan de clientzijde logboeken waaraan de logboekbestanden service aan de clientzijde (toets kluis). |
| DurationMs | Benodigde tijd om de aanvraag REST API (in milliseconden). Dit netwerklatentie, zodat het moment dat u aan de clientzijde meten mogelijk niet overeenkomen met ditmaal niet opgenomen. |
| HttpStatusCode_d | HTTP-statuscode die het resultaat van de aanvraag |
| Id_s       | Unieke ID van de aanvraag kunt invullen |
| Identity_o | De identiteit van de token die bij het maken van de REST API-aanvraag is ingediend. Dit is meestal een "gebruiker", 'service principal' of een combinatie "gebruiker + toepassings-id" zoals in het geval van een aanvraag die resulteert uit een Azure PowerShell-cmdlet. |
| OperationName      | Naam van de bewerking, zoals beschreven in [Azure toets kluis vastleggen](../key-vault/key-vault-logging.md)|
| OperationVersion      | REST API versie aangevraagd door de client|
| RemoteIPLatitude | Breedtegraad van de client die de aanvraag hebben gedaan |
| RemoteIPLongitude | Lengte van de client die de aanvraag hebben gedaan |
| RemoteIPCountry | Land van de client die de aanvraag hebben gedaan  |
| RequestUri_s | URI van de aanvraag kunt invullen |
| Resource   | Naam van de belangrijkste kluis |
| ResourceGroup | Resourcegroep van de belangrijkste kluis |
| ResourceId | Azure resourcemanager Resource-ID. Toets kluis logboekbestanden is dit altijd de sleutel kluis resource-ID. |
| ResourceProvider | *MICROSOFT. KEYVAULT* |
| ResultSignature  | HTTP-status|
| ResultType      | Resultaat van de aanvraag REST API|
| SubscriptionId | Azure abonnement-ID van het abonnement met de toets kluis |


## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoekopdrachten in het logboek in Log Analytics](log-analytics-log-searches.md) gedetailleerde Azure-toets kluis gegevens moeten worden weergegeven.
