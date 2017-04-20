<properties
   pageTitle="Overzicht van Azure Lake gegevensopslag | Microsoft Azure"
   description="Begrijpen wat is de gegevensopslag Lake Azure en de waarde van die deze via andere opgeslagen gegevens biedt"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Overzicht van Azure Lake gegevensopslag

Azure Lake gegevensopslag is een hele bedrijf hyper-schaal opslagplaats voor grote gegevens analytische werkbelasting. Azure gegevens Lake kunt u gegevens van elke grootte, type en opname snelheid op één enkele locatie voor operationele en experimentele analytics vastleggen.

> [AZURE.TIP] Gebruik de [Lake gegevensopslag leerpad](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) om te beginnen met het verkennen van de Azure gegevens Lake Store-service.

Azure Lake gegevensopslag zijn toegankelijk vanaf Hadoop (beschikbaar met HDInsight cluster) met de WebHDFS-compatibele REST API's. Er is speciaal ontworpen om in te schakelen analytics op gegevens in de cache en is afgestemd voor gegevens analytics-scenario's. Afmelden bij het vak gezocht alle bedrijfstoepassingen mogelijkheden, beveiliging, beheerbaarheid, schaalbaarheid, betrouwbaarheid en beschikbaarheid, essentiële voor echte enterprise gebruik gevallen.


![Azure gegevens Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Enkele van de belangrijkste functies van het meer Azure-gegevens onder andere de volgende.

### <a name="built-for-hadoop"></a>Die is ingebouwd voor Hadoop

De store Lake van Azure-gegevens is een Apache Hadoop-bestandssysteem die compatibel is met een Hadoop Distributed bestand System (HDFS) en het werkt met het Hadoop-systeem.  Uw bestaande HDInsight-toepassingen en services die gebruikmaken van de API WebHDFS kunnen eenvoudig integreren met Lake gegevensopslag. Gegevensopslag Lake stelt tevens een WebHDFS-compatibele REST interface voor toepassingen

Gegevens die zijn opgeslagen in Lake gegevensopslag kunnen eenvoudig worden geanalyseerd met Hadoop analytische kaders zoals MapReduce of component. Microsoft Azure HDInsight clusters kunnen worden deze is ingericht en geconfigureerd voor het rechtstreeks toegang tot gegevens die zijn opgeslagen in Lake gegevensopslag.

### <a name="unlimited-storage-petabyte-files"></a>Onbeperkte opslag, petabyte bestanden

Azure Lake gegevensopslag onbeperkte opslagruimte biedt en is geschikt voor het opslaan van tal van gegevens voor analytics. Deze bevat geen opleggen eventuele beperkingen ten aanzien van het formaat van de account, grootte of de hoeveelheid gegevens die kunnen worden opgeslagen in een meer gegevens. Afzonderlijke bestanden kunnen variëren van kB tot petabytes grootte zodat u een uitstekende keus voor de opslag van elk type gegevens. Gegevens worden blijvend opgeslagen door meerdere kopieën te maken en er is geen limiet voor de tijdsduur waarvoor de gegevens in het gegevens meer kan worden opgeslagen.

### <a name="performance-tuned-for-big-data-analytics"></a>Prestaties van grote gegevens analysegegevens afgestemd

Azure Lake gegevensopslag is geoptimaliseerd voor het uitvoeren van grote schaal analytische systemen die nodig hebben enorme doorvoersnelheid query en grote hoeveelheden gegevens analyseren. Delen van een bestand als een het meer gegevens dubbele over een aantal afzonderlijke opslagservers. Hiermee wordt de gelezen doorvoer verbeterd tijdens het lezen van het bestand parallel voor de uitvoering van analyses van gegevens.


### <a name="enterprise-ready-highly-available-and-secure"></a>Gereed voor Enterprise: Grote beschikbaarheid en beveiligen

Azure Lake gegevensopslag biedt de beschikbaarheid van de standaard- en betrouwbaarheid. De activa van uw gegevens worden blijvend opgeslagen door overbodige kopieën beschermen tegen onverwachte fouten te maken. Ondernemingen kunnen Lake van Azure-gegevens in de bijbehorende oplossingen gebruiken als een belangrijk onderdeel van hun bestaande gegevens-platform.

Gegevensopslag Lake biedt beveiliging ook enterprise-grade voor gegevens in de cache. Zie [de gegevens beveiligen in Azure Lake gegevensopslag](#DataLakeStoreSecurity)voor meer informatie.


### <a name="all-data"></a>Alle gegevens

Azure Lake gegevensopslag gegevens kunt opslaan in de oorspronkelijke indeling, in de huidige staat zonder de voorgaande transformaties. Gegevensopslag Lake hoeft niet een schema worden gedefinieerd voordat de gegevens worden geladen, waardoor het snel aan de afzonderlijke analytische framework interpreteren van de gegevens en het definiëren van een schema op het moment van de analyse. Kunt opslaan van bestanden van willekeurige grootte en indelingen, maakt het mogelijk Lake gegevensopslag om gestructureerd, gedeeltelijk gestructureerd en ongestructureerd gegevens te verwerken.

Azure Lake gegevensopslag containers voor gegevens zijn in principe mappen en bestanden. U werken met gegevens in de cache met SDK's, Azure-Portal en Azure Powershell. Zo lang maken als u uw gegevens in het archief met deze interfaces en gebruiken van de juiste containers plaatst, kunt u allerlei soorten gegevens kunt opslaan. Gegevensopslag Lake voert geen een speciaal op basis van het type gegevens dat is opgeslagen.


## <a name="DataLakeStoreSecurity"></a>Beveiligingsgegevens in Azure Lake gegevensopslag

Azure Lake gegevensopslag Azure Active Directory gebruikt voor verificatie en toegangsbeheerlijsten (ACL's) om toegang tot uw gegevens beheren.

| Functie                                 | Beschrijving                              |
|-----------------------------------------|------------------------------------------|
| Verificatie | Azure Lake gegevensopslag geïntegreerd met Azure Active Directory (AAD) voor identiteit en toegangsbeheer voor alle gegevens die zijn opgeslagen in de gegevensopslag Lake Azure. Als gevolg van de integratie, Azure gegevens meer voordelen van alle AAD-functies, zoals meervoudige verificatie, voorwaardelijke toegang, Rolgebaseerd toegangsbeheer, controle op het gebruik van toepassingen, beveiliging en meldingen, enzovoort. Azure Lake gegevensopslag ondersteunt het OAuth 2.0-protocol voor verificatie met in de REST-interface. |
| Toegangsbeheer                          | Azure Lake gegevensopslag biedt toegangsbeheer door de ondersteuning van POSIX-achtige machtigingen die worden aangeboden door het WebHDFS-protocol. In de huidige versie, kunnen ACL's op de hoofdmap, submappen, evenals afzonderlijke bestanden worden ingeschakeld. De ACL's die u op de hoofdmap toepast wordt ook van toepassing op alle onderliggende mappen/bestanden ook.|

Wilt u meer informatie over het beveiligen van gegevens in Lake gegevensopslag. Volg de onderstaande koppelingen.

* Zie voor instructies over het beveiligde gegevens in Lake gegevensopslag [beveiligen gegevens in de gegevensopslag Lake Azure](data-lake-store-secure-data.md).
* Video's het beste? [Bekijk deze video](https://mix.office.com/watch/1q2mgzh9nn5lx) voor het beveiligen van gegevens die zijn opgeslagen in Lake gegevensopslag.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Toepassingen die compatibel is met Azure Lake gegevensopslag

Azure Lake gegevensopslag is compatibel met meest openen bron onderdelen in het Hadoop-systeem. Dit ook werkt naadloos samen netjes met andere Azure-services. Hierdoor Lake gegevensopslag een perfecte optie voor de behoeften van uw gegevens opslag. Volg de onderstaande koppelingen voor meer informatie over hoe Lake gegevensopslag kan worden gebruikt met bron openen onderdelen, evenals andere Azure-services.

* Zie [toepassingen en services die compatibel is met Azure Lake gegevensopslag](data-lake-store-compatible-oss-other-applications.md) voor een overzicht van bron openen toepassingen met Lake gegevensopslag.
* Zie [integratie met andere Azure services](data-lake-store-integrate-with-other-services.md) kunnen begrijpen hoe Lake gegevensopslag kan worden gebruikt met andere services Azure om in te schakelen van een grotere groep scenario's beschreven.
* Zie [scenario's voor het gebruik van gegevens Lake Store](data-lake-store-data-scenarios.md) voor meer informatie over het gebruik van Lake gegevensopslag in scenario's zoals ingesting gegevens, gegevensverwerking downloaden van gegevens en gegevens visualiseren.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Wat is het bestandssysteem Azure Lake gegevensopslag (adl: / /)?

Gegevensopslag Lake kunnen worden geraadpleegd via het nieuwe bestandssysteem, de AzureDataLakeFilesystem (adl: / /), in Hadoop-omgeving (beschikbaar met HDInsight cluster). Toepassingen en services met adl: / / profiteren van verder optimalisatie van prestaties die niet zijn momenteel beschikbaar in WebHDFS zijn. Hierdoor Lake gegevensopslag hebt u de flexibiliteit of gebruik de beste prestaties met de aanbevolen optie van het gebruik van adl: / / of blijven de API WebHDFS rechtstreeks gebruiken voor het behoud van bestaande code. Azure HDInsight volledig maakt gebruik van de AzureDataLakeFilesystem voor de beste prestaties op Lake gegevensopslag.

U kunt toegang tot uw gegevens in het Lake gegevensopslag met `adl://<data_lake_store_name>.azuredatalakestore.net`. Voor meer informatie over hoe u toegang tot de gegevens in de gegevens Lake Store, raadpleegt u [weergave-eigenschappen van de opgeslagen gegevens](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Hoe start ik Azure Lake gegevensopslag gebruiken?

Zie [aan de slag met Lake gegevensopslag via de Portal Azure](data-lake-store-get-started-portal.md), over voor het inrichten van een gegevensopslag Lake met behulp van de Azure-Portal. Zodra u hebt deze is ingericht Azure gegevens Lake, leert u hoe u grote gegevens aanbiedingen zoals Azure gegevens Lake Analytics of Azure HDInsight met Lake gegevensopslag. U kunt ook een .NET-toepassing maken van een account Azure Lake gegevensopslag en bewerkingen zoals gegevens uploaden, downloaden van gegevens, enzovoort maken.

- [Aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Aan de slag met Azure Lake gegevensopslag met .NET SDK](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Gegevensopslag Lake video 's

Als u liever de video's voor meer informatie over bekijken, biedt Lake gegevensopslag video's op een bereik van functies.

* [Maak een Account Azure Lake gegevensopslag](https://mix.office.com/watch/1k1cycy4l4gen)
* [De Data Explorer gebruiken voor het beheren van gegevens in Azure Lake gegevensopslag](https://mix.office.com/watch/icletrxrh6pc)
* [Azure gegevens Lake Analytics verbinden met Azure Lake gegevensopslag](https://mix.office.com/watch/qwji0dc9rx9k)
* [Access Azure Lake gegevensopslag via gegevens Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)
* [Azure HDInsight verbinden met Azure Lake gegevensopslag](https://mix.office.com/watch/l93xri2yhtp2)
* [Access Azure Lake gegevensopslag via component en varken](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [DistCp (Hadoop Distributed kopie) gebruiken om gegevens te kopiëren naar en vanuit Azure Lake gegevensopslag](https://mix.office.com/watch/1liuojvdx6sie)
* [Apache Sqoop gebruiken om gegevens te verplaatsen tussen relationele gegevensbronnen en Azure Lake gegevensopslag](https://mix.office.com/watch/1butcdjxmu114)
* [Integratie van gegevens met behulp van Azure gegevens Factory voor Azure Lake gegevensopslag](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Gegevens in de winkel Azure gegevens Lake beveiligen](https://mix.office.com/watch/1q2mgzh9nn5lx)



