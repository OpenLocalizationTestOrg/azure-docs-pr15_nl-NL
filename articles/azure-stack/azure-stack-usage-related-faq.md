<properties
    pageTitle="Veelgestelde vragen betrekking gebruik | Microsoft Azure"
    description="Lijst van Azure stapel meters vergelijking met Azure gebruik API, gebruikstijd en de tijd gerapporteerd foutcodes."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="azure-stack-usage-api-faqs"></a>Azure stapel gebruik API-Veelgestelde vragen
In dit artikel vindt u antwoorden op enkele veelgestelde vragen over de API Azure stapel gebruik.

## <a name="what-meter-ids-can-i-see"></a>Welke meter id's kan ik zien?

Op dit moment wordt gebruik voor het netwerk, opslag- en berekeningscluster resource providers gerapporteerd.

| **Resource-provider** | **Meter-ID** |**De naam van de meter** | **Eenheid** | **Aanvullende informatie** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Netwerk** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Gebruik van openbare IP-adres | IP-adres |                    
| **Opslag**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*uur | Totale capaciteit dat door tabellen |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*uur | Totale capaciteit dat door BLOB van pagina 's |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*uur  | Totale capaciteit verbruikt door wachtrij |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*uur  | Totale capaciteit dat door blok BLOB 's |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Tellen in 10,000s aanvragen   | Tabel serviceaanvragen (in 10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Ingress gegevens in GB | Tabel service gegevens ingress in GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress in GB | Tabel service gegevens egress in GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Aanvragen tellen in 10,000s | Serviceaanvragen BLOB (in 10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Ingress gegevens in GB          | BLOB service gegevens ingress in GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress in GB              | BLOB service gegevens egress in GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Aanvragen tellen in 10,000s   | Wachtrij serviceaanvragen (in 10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Ingress gegevens in GB         | Wachtrij service gegevens ingress in GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress in GB  | Wachtrij service gegevens egress in GB 
| **Berekenen** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | VM grootte uur | VM grootte |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Hoe vergelijk de Azure stapel gebruik API's tot de [API voor gebruik van Azure](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (momenteel in openbare preview)?

-   De Tenant gebruik API komt overeen met de API Azure, klikt u met één uitzondering: de vlag *showDetails* momenteel niet ondersteund in Azure stapel.

-   De Provider gebruik API geldt alleen voor Azure stapel.

-   De [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) die beschikbaar is in Azure is momenteel niet beschikbaar in Azure stapel.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Wat is het verschil tussen gebruikstijd en Time gerapporteerd?

Gebruiksrapporten gegevens bestaan uit twee belangrijkste tijdwaarden:

-   **Tijd gerapporteerd**. De tijd waarop de gebeurtenis gebruik het systeem gebruik ingevoerd

-   **Gebruikstijd**. De tijd waarop de resource Azure stapel verbruikte

Ziet u mogelijk een verschil in waarden voor gebruikstijd en gerapporteerde tijd voor een specifieke gebruik gebeurtenis. De vertraging mogelijk zo lang als meerdere uren in een omgeving.

Op dit moment kunt u zoeken *alleen op basis van tijd gerapporteerd*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Wat betekenen deze foutcodes gebruik API?

| **HTTP-statuscode** | **Foutcode** | **Beschrijving** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400/Ongeldige aanvraag        | *NoApiVersion*     | De *api-versie* queryparameter ontbreekt.
| 400/Ongeldige aanvraag        | *InvalidProperty*  | Een eigenschap ontbreekt of een ongeldige waarde heeft. De ontbrekende eigenschap wordt aangeduid met het bericht in de foutcode in de hoofdtekst van het antwoord.
| 400/Ongeldige aanvraag        | *RequestEndTimeIsInFuture*  | De waarde voor *ReportedEndTime* is in de toekomst. Waarden zijn in de toekomst niet toegestaan voor dit argument.
| 400/Ongeldige aanvraag        | *SubscriberIdIsNotDirectTenant*    | Een oproep provider API gebruikt een abonnements-ID die is niet een geldige tenant van de beller.
| 400/Ongeldige aanvraag        | *SubscriptionIdMissingInRequest*   | De abonnements-ID van de beller ontbreekt.
| 400/Ongeldige aanvraag        | *InvalidAggregationGranularity*   | Een ongeldige aggregatie granulatie is aangevraagd. Geldige waarden zijn dagelijkse en per uur.
| 503                    | *ServiceUnavailable*   | Er is een herstelbare fout opgetreden omdat de service bezet is of het gesprek is wordt vertraagd. |

## <a name="next-steps"></a>Volgende stappen
[Klant facturering en financiële Azure gestapelde](azure-stack-billing-and-chargeback.md)

[Resourcegebruik provider API](azure-stack-provider-resource-api.md)

[Resourcegebruik API tenant](azure-stack-tenant-resource-usage-api.md)
