<properties
    pageTitle="Aan de slag met Azure zoeken in NodeJS | Microsoft Azure | De zoekservice gehoste cloud"
    description="Doorloop bouwen van een zoektoepassing op een gehoste cloudservice zoeken op Azure NodeJS als uw programmeertaal gebruiken."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Aan de slag met Azure zoeken in NodeJS
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Informatie over het maken van een aangepaste NodeJS-zoektoepassing waarin Azure zoeken naar de zoekfunctie. Deze zelfstudie gebruikt de [Azure zoeken Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) om de objecten en de bewerkingen die worden gebruikt in deze oefening te maken.

We [NodeJS](https://nodejs.org) en NPM, [Sublime tekst 3](http://www.sublimetext.com/3)en Windows PowerShell in Windows 8.1 ontwikkelen en test u deze code gebruikt.

Als u wilt uitvoeren in dit voorbeeld, moet u een Azure Search-service, die u bij de [Portal van Azure aanmelden kunt](https://portal.azure.com)hebben. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor stapsgewijze instructies.

## <a name="about-the-data"></a>Over de gegevens

In dit voorbeeld-toepassing gebruikt de gegevens uit de [Verenigde Staten geologische Services (USG)](http://geonames.usgs.gov/domestic/download_data.htm), gefilterd op de status van Rhode eiland om de grootte van de gegevensset te reduceren. We gebruiken deze gegevens maken van een search-servicetoepassing die oriëntatiepunten gebouwen zoals ziekenhuizen en scholen, evenals geologische functies zoals streams, meren en topontmoetingen retourneert.

In deze toepassing, wordt het programma **DataIndexer** genereert en laadt de index met een [indexering](https://msdn.microsoft.com/library/azure/dn798918.aspx) constructie voor het ophalen van de gefilterde USG gegevensset van een openbare Azure SQL-Database. Referenties en verbinding gegevens met de online gegevensbron zijn beschikbaar in de programmacode. Er is geen verdere configuratie nodig.

> [AZURE.NOTE] We een filter toegepast op deze dataset om te blijven onder de Documentlimiet 10.000 voor de gratis prijzen laag. Als u de standaard laag gebruikt, is deze limiet niet van toepassing. Zie voor meer informatie over de capaciteit voor elke laag prijzen [zoeklimieten voor de service](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Zoek de servicenaam en api-sleutel van de zoekservice van Azure

Nadat u de service hebt gemaakt, Ga terug naar de portal om de URL of `api-key`. Verbindingen met de zoekservice vereisen dat u alleen de URL hebt en een `api-key` om te verifiëren van het gesprek.

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com).
2. Klik in de balk sprong op **Search-service** te geven van de Azure zoeken services deze is ingericht voor uw abonnement.
3. Selecteer de service die u wilt gebruiken.
4. Klik op het servicedashboard ziet u tegels voor alle noodzakelijke informatie, alsmede het pictogram van de sleutel voor toegang tot de toetsen beheerder.

    ![][3]

5. Kopieer de URL van de service, een beheerder-sleutel en de sleutel van een query. Moet u alle drie later, wanneer u deze aan het bestand config.js toevoegt.

## <a name="download-the-sample-files"></a>Voorbeeldbestanden downloaden

Ga op een van de volgende methoden om te downloaden van de steekproef.

1. Ga naar [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Klik op **Downloaden ZIP**, het ZIP-bestand opslaan en vervolgens extraheren alle bestanden die deze bevat.

Alle wijzigingen van de volgende bestand en uitvoeren-instructies worden tegen bestanden in deze map worden ingesteld.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Werk de config.js. met uw zoekopdracht service-URL en api-sleutel

De URL, de admin-toets en query-toets in configuratiebestand met de URL en api-sleutel die u eerder hebt gekopieerd opgeven.

Beheerder toetsen verlenen volledige controle over de service-bewerkingen uitvoeren, inclusief het maken of verwijderen van een index en het laden van documenten. Query toetsen zijn geschikt daarentegen voor alleen-lezen-bewerkingen, meestal gebruikt door clienttoepassingen die verbinding met Azure zoeken maken.

In dit voorbeeld wordt de query-sleutel zodat het wordt aanbevolen van het gebruik van de query-toets in clienttoepassingen versterken opnemen.

De volgende schermafbeelding ziet u **config.js** openen in een tekst-editor met de desbetreffende posten, zodat u kunt zien waar het bestand wilt bijwerken met de waarden die geldig voor de zoekservice zijn gescheiden.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Host een runtimeomgeving voor de steekproef

Het voorbeeld werkt alleen een HTTP-server, die u kunt installeren globaal met npm.

Een PowerShell-upvenster voor de volgende opdrachten gebruiken.

1. Navigeer naar de map waarin het bestand **package.json** .
2. Type `npm install`.
2. Type `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>De index maken en uitvoeren van de toepassing

1. Type `npm run indexDocuments`.
2. Type `npm run build`.
3. Type `npm run start_server`.
4. Uw browser bij rechtstreekse`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Zoeken op USG gegevens

De gegevensset USG records die relevant naar de status van Rhode eiland zijn opgenomen. Als u op **Zoeken** op een lege zoekvak, krijgt u de bovenste 50 posten, de standaardinstelling.

Invoeren van een zoekterm krijgt met het zoekprogramma iets om in te gaan. Geef een regionale naam. "Roger Williams" is de eerste gouverneur van Rhode eiland. Veel parken, gebouwen en scholen zijn benoemde na hem.

![][9]

U kunt ook proberen een van deze voorwaarden:

- Pawtucket
- Pembroke
- goose + cape


## <a name="next-steps"></a>Volgende stappen

Dit is de eerste Azure zoeken zelfstudie op basis van NodeJS en de gegevensset USG. Over een periode, breiden we uit deze zelfstudie om u te illustreren extra zoekfuncties die u mogelijk wilt gebruiken in uw aangepaste oplossingen.

Als u al achtergrondinformatie in Azure zoeken, kunt u dit voorbeeld gebruiken als een springboard voor probeert suggesters (voor tekst aanvullen of AutoAanvullen query's), filters en beperkte navigatie. U kunt ook op de pagina met zoekresultaten verbeteren door te voegen telt en batchen van documenten, zodat gebruikers door de resultaten bladeren kunnen.

Ervaring met Azure zoeken? Het is raadzaam probeert andere zelfstudies voor het ontwikkelen van begrijpen wat u kunt maken. Ga naar onze [documentatiepagina](https://azure.microsoft.com/documentation/services/search/) meer informatie. U kunt ook de koppelingen weergeven in onze [Video en Zelfstudievideo lijst](search-video-demo-tutorial-list.md) voor toegang tot meer informatie.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
