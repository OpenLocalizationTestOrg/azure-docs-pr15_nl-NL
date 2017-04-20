<properties 
    pageTitle="Gegevens Factory Use-Case - aanbevelingen" 
    description="Meer informatie over een use-case geïmplementeerd via Azure gegevens Factory samen met andere services." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Use-Case - aanbevelingen 

Azure gegevens Factory is een van de vele services gebruikt voor het implementeren van de Suite Cortana bedrijfsinformatie van oplossing accelerators.  Zie [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) pagina voor meer informatie over deze suite. Een algemene use-case die Azure gebruikers al hebt opgelost en geïmplementeerd met Azure gegevens fabriek en andere Cortana Intelligence-Onderdeelservices worden beschreven in dit document.

## <a name="scenario"></a>Scenario

Online winkels wilt meest lokken hun klanten producten te kopen door ze te presenteren met producten die ze zijn waarschijnlijk in geïnteresseerd bent en kunnen daarom waarschijnlijk kopen. Hiervoor moeten onlinewinkels hun gebruikerswachtwoord online-ervaring aanpassen met behulp van aangepaste aanbevelingen voor deze specifieke gebruiker. Deze persoonlijke aanbevelingen moeten worden gemaakt op basis van hun huidige en historische winkelen gedrag gegevens, productgegevens, nieuwe merken en segmentatiegegevens en de klantenservice.  Daarnaast kunnen ze de gebruiker aanbevelingen op basis van analyse van de algehele gebruik gedrag van alle hun gebruikers gecombineerd bieden.

Het doel van deze winkels is om te optimaliseren voor de gebruiker klikt u op naar verkoop conversies en hoger verkoopopbrengst verdienen.  Ze bereiken deze conversie door te geven van contextuele, gedrag gebaseerde aanbevelingen op basis van de klant interesses en acties. Voor deze zaak gebruiken we onlinewinkels als een voorbeeld van de bedrijven die optimaliseren voor hun klanten wilt gebruiken. Echter toepassen deze beginselen op een bedrijf dat wil deelnemen van zijn klanten rond de goederen en diensten en verbeter hun klanten kopen ervaring met gepersonaliseerde aanbevelingen.

## <a name="challenges"></a>Uitdagingen

Zijn er veel uitdagingen die nominale onlinewinkels wanneer u probeert te implementeren van dit type use-case. 

Gegevens van verschillende grootte en shapes moet eerst worden ingenomen uit meerdere gegevensbronnen, zowel on-premises implementatie en in de cloud. Deze gegevens bevatten productgegevens, gedrag van historische klantgegevens en gebruikersgegevens, terwijl de gebruiker de onlinedetailhandel-site gaat. 

Tweede, persoonlijke aanbevelingen moeten worden redelijk en nauwkeurig berekend en voorspeld. Naast het product, huisstijl en de gegevens van de browser en gedrag van de klant moeten onlinewinkels ook opnemen feedback van klanten van vroegere aankopen te houden bij het bepalen van de beste aanbevelingen voor de gebruiker. 

Derde, moeten de aanbevelingen onmiddellijk product aan de gebruiker een naadloze organisatienavigatie- en aanschaffen van ervaring, en bieden de meest recente en relevante aanbevelingen zijn. 

Tot slot moeten winkels meet de effectiviteit van hun benadering algehele Upsell kunt bijhouden en cross-verkopen verkoop uitkomsten van klik-naar-conversie en aanpassen aan zijn of haar toekomstige aanbevelingen.

## <a name="solution-overview"></a>Overzicht van oplossingen

In dit voorbeeld use-case is kan worden opgelost en geïmplementeerd door reële Azure gebruikers met behulp van Azure gegevens Factory en andere Cortana Intelligence componentservices, zoals [HDInsight](https://azure.microsoft.com/services/hdinsight/) en [Power BI](https://powerbi.microsoft.com/).

De online winkel gebruikt een Azure Blob store, een lokale SQL server Azure SQL-DB en een relationele datamart als hun opties voor het opslaan van gegevens in de hele werkstroom.  Het archief blob bevat klantgegevens, gedrag klantgegevens en informatie productgegevens. De informatie Productgegevens huisstijl productinformatie bevat en een product catalogus opgeslagen on-premises implementatie in een SQL-data-warehouse. 

Alle gegevens is gecombineerd en een product aanbeveling systeem voor het leveren van persoonlijke aanbevelingen op basis van de klant interesses en acties, terwijl de gebruiker producten in de catalogus op de website bladert geladen. Ook bekijken met de klanten producten die zijn gerelateerd aan het product dat ze bekijkt op basis van de website van de algehele gebruikspatronen die niet zijn gerelateerd aan een één gebruiker.

![use-case-diagram](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabytes onbewerkte-web-logboekbestanden worden dagelijks uit van de fabrikant van de online-website gegenereerd als gedeeltelijk gestructureerd-bestanden. De logboekbestanden onbewerkte web en gegevens over de klant en product catalogus is regelmatig ingenomen in een Azure-blobopslag verplaatsing van de gegevens Factory globaal gedistribueerde gegevens als een service gebruiken. De onbewerkte logboekbestanden voor de dag zijn partitioneren (per jaar en maand) in-blobopslag langdurige opslag.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) wordt gebruikt om de onbewerkte logboekbestanden in de winkel blob partitioneren en verwerken van de aangezogen Logboeken bij het op schaal zowel component en varken-scripts gebruiken. De gepartitioneerde web Logboeken gegevens wordt vervolgens verwerkt als u wilt extraheren van de benodigde ingangen voor een machine learning-aanbeveling systeem om te genereren van de persoonlijke aanbevelingen.

De aanbeveling systeem dat wordt gebruikt voor de machine learning-in dit voorbeeld is een bron openen-machine learning-aanbeveling platform van [Apache Mahout](http://mahout.apache.org/).  Een [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) of aangepaste model kan worden toegepast op het scenario.  Het model Mahout wordt gebruikt om te voorspellen de overeenkomsten tussen items op de website op basis van de algehele gebruikspatronen en genereert u het persoonlijke aanbevelingen op basis van de individuele gebruiker.

Ten slotte wordt de resultatenset van persoonlijke aanbevelingen verplaatst naar een relationele datamart voor verbruik door de winkel-website.  De resultatenset kan ook worden geraadpleegd rechtstreeks vanuit blobopslag door een andere toepassing of verplaatst naar extra winkels voor andere consumenten en gebruik gevallen.

## <a name="benefits"></a>Voordelen

Door hun product aanbeveling strategie optimaliseren en deze aan de bedrijfsdoelstellingen uitlijnen, voldaan de oplossing van de fabrikant van de online-aanbevelingen en marketing doelstellingen. Daarnaast kunnen konden ze mogelijk maken en de werkstroom van de aanbeveling product op een efficiënte, betrouwbare en efficiënte manier beheren. De methode gemaakt dat ze hun model bijwerken en de doeltreffendheid op basis van de maateenheden van verkoop klik-naar-conversie uitkomsten verfijnen. Met behulp van Azure gegevens Factory, zijn ze kunnen hun resourcebeheer tijd in beslag nemen en duur handmatige cloud verlaten en naar de cloud op aanvraag resourcebeheer verplaatsen. Deze zijn er daarom kunnen Bespaar tijd, geld, en hun verkorten aan de implementatie van de oplossing. Gegevens over de afkomst weergaven en operationele servicestatus kwam eenvoudig te visualiseren en oplossen met de intuïtieve Data Factory voor controle en beheer UI verkrijgbaar via de portal van Azure. De oplossing kan nu worden weergegeven die is gepland en beheerd zodat klaar gegevens betrouwbaar is geproduceerd en is bezorgd aan gebruikers en gegevens en verwerking afhankelijkheden automatisch zonder menselijke tussenkomst worden beheerd.

Door deze persoonlijke winkelwagen gebruikservaring, de online winkel gemaakt van een klant meer Concurrentieanalyse, aantrekkelijker ervaring en daarom vergroot de klanttevredenheid van de verkoop- en algehele.



  