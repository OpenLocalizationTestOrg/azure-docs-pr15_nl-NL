<properties
   pageTitle="Technische minimumvereisten voor het maken van een gegevensservice Marketplace | Microsoft Azure"
   description="Meer informatie over de vereisten voor het maken van een gegevensservice om te implementeren en te verkopen op de Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Technische minimumvereisten voor het maken van een gegevensservice bieden Azure Marketplace

>[AZURE.IMPORTANT] **Op dit moment we zijn niet langer onboarding eventuele nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet ophalen goedgekeurd voor de aanbieding.** Als u een SaaS bedrijfstoepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u een IaaS-toepassingen hebt of ontwikkelaars-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Het proces zorgvuldig voordat u begint lezen en begrijpen waar en waarom elke stap wordt uitgevoerd. Zoveel mogelijk, u moet uw bedrijfsgegevens en andere gegevens voorbereiden, benodigde hulpprogramma's downloaden en/of technische onderdelen maken voordat u begint met het maakproces aanbieding.

U kunt de volgende items klaar zijn voordat u begint met het proces nodig hebt:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Bepalen op welke technologie wordt gebruikt voor het publiceren van uw voorstel gegevensservice

Een uitgever kunt beslissen tussen meerdere technologieën bij het publiceren van gegevensservice in Azure Marketplace. De belangrijkste technologieën die worden ondersteund hieronder beschreven. Ongeacht welke technologie wordt gebruikt voor het publiceren van de Service van gegevens, de gegevens via de **OData-feed** die worden aangeboden door Azure Marketplace-Service door de eindgebruiker worden gebruikt. Volledige informatie over de OData-service die kunt u vinden op [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure-Database

Gegevensset klaar in SQL Azure wordt aangegeven dat het is van Publisher verantwoordelijkheid. Moet u zich hebt geabonneerd Azure, inrichten van de juiste grootte van de Database en uw gegevens uploaden naar SQL Azure DB. Publisher is ook verantwoordelijk zijn of haar gegevens altijd up-to-date kunt houden. Meer informatie over het abonneren op Azure Services die u kunt vinden in [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Wanneer u verplaatst u de gegevens in SQL Azure wordt aangegeven, kan het Azure Marketplace blootgesteld tabellen en weergaven. De uitgever kunt opgeven welke tabellen/weergaven en kolommen worden blootgesteld aan de eindgebruiker. Verder kunt provider van de inhoud ook opgeven welke kolommen kunnen worden opgevraagd door de eindgebruiker en de functies die alleen in de nettolading worden geretourneerd. Het resultaat is een hoog niveau van flexibiliteit over welke gegevens in de database moet worden weergegeven. Kolommen die kunnen worden doorzocht moeten worden ondersteund door een of meer database indexen.

## <a name="rest-based-web-service"></a>Op basis van REST-webservice

Protocol ondersteund: **HTTPS alleen**

Bestaande REST gebaseerd-services kunnen toegankelijk zijn via de Azure Marketplace. Omdat de gegevensset is altijd beschikbaar aan de eindgebruiker als een OData-feed, gebaseerd de behoeften van Azure Marketplace-service kan toewijzen van de service met een OData-service. Als u wilt doen, zodat de REST op basis van eindpunten moeten laten zien van alle parameters als HTTP parameters.

De nettolading moet in een formulier dat kan worden toegewezen aan een ATOM-antwoord wordt verzonden. Daarom bevatten de reactie van de services moet uit XML-indeling en kan alleen een herhalend element dat de waarden nettolading (zoals Recordset) bevat. De Azure Marketplace-service wordt het herhalende knooppunt toewijzen aan het knooppunt vermelding in ATOM en de nettolading-waarden in de eigenschap knooppunten binnen het knooppunt invoer.

Verificatie-informatie (zoals API key, verificatie toegangstoken, enz.) moet worden opgegeven als een parameter HTTP of in de koptekst HTTP (sleutelwaarde paar) – basisverificatie wordt ook ondersteund. Een geldige sleutel moet worden opgegeven en alle aanvragen tot en met Azure Marketplace worden gesteld door die toets. Er gebeurt gebruiker cmdlets voor controle en facturering op de laag Azure Marketplace.

Fouten geretourneerd door de service moeten worden toegewezen aan HTTP-statuscodes. Als de service retourneert een XML die de fout bevat gaan deze door de service Azure Marketplace naar HTTP-statuscodes worden toegewezen.

## <a name="soap-based-web-services"></a>Op basis van SOAP-webservices

Protocol: **HTTPS alleen**

De vereisten zijn dat hetzelfde als in de REST op basis van de sectie service. De enige verschil is dat kunnen ook in de hoofdtekst van een XML-die naar een van de Publisher-service wordt geboekt met elk verzoek tot en met Azure Marketplace parameters worden geleverd. Dit betekent dat HTTP parameters vindt u de gebruiker aan de front worden vertaald in XML-elementen van een XML-document dat aan het verzoek om de webservice van de provider van de inhoud wordt geplaatst.

## <a name="odata-based-web-services"></a>Op basis van OData-webservices

Protocol: **HTTPS alleen**

Gegevens kunnen worden weergegeven als een OData-service met Azure Marketplace. Het systeem dient te geven van de service via en Ga naar de hoofdsite van de service vervangen door de service-hoofdsite van Azure Marketplace – om ervoor te zorgen dat alle mislukken volgende oproepen gaat doorlopen Azure Marketplace.

OData-services hoeft niet te gaan ten opzichte van een database in de backend. OData ondersteuning biedt voor elk type opslag of de bedrijfslogica om de service te sturen.


## <a name="next-steps"></a>Volgende stappen
Nu dat u de minimumvereisten beoordeeld en de benodigde taken uitgevoerd, kunt u verdergaan met het maken van uw voorstel gegevensservice zoals wordt beschreven in de [Data Service publicerende Guide](marketplace-publishing-data-service-creation.md).

Of, als u bekijken van de algehele proces en de desbetreffende artikelen voor elk van de publicerende fasen wilt, Ga naar het artikel [aan de slag: een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
