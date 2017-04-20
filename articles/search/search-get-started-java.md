<properties
    pageTitle="Aan de slag met Azure zoeken in Java | Microsoft Azure | De zoekservice gehoste cloud"
    description="Het maken van een gehoste cloud-zoektoepassing op Azure Java als uw programmeertaal gebruiken."
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

# <a name="get-started-with-azure-search-in-java"></a>Aan de slag met Azure zoeken in Java
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Informatie over het maken van een aangepaste Java-zoektoepassing waarin Azure zoeken naar de zoekfunctie. Deze zelfstudie gebruikt de [Azure zoeken Service REST API](https://msdn.microsoft.com/library/dn798935.aspx) om de objecten en de bewerkingen die worden gebruikt in deze oefening te maken.

Als u wilt uitvoeren in dit voorbeeld, moet u een Azure Search-service, die u bij de [Portal van Azure aanmelden kunt](https://portal.azure.com)hebben. Zie [maken een Azure Search-service in de portal](search-create-service-portal.md) voor stapsgewijze instructies.

We de volgende software te testen in dit voorbeeld gebruikt:

- [Eclips IDE voor Java EE ontwikkelaars](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Zorg ervoor dat de EE-versie te downloaden. Een van de stappen verificatie vereist een functie die alleen in deze editie kunt vinden.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Over de gegevens

In dit voorbeeld-toepassing gebruikt de gegevens uit de [Verenigde Staten geologische Services (USG)](http://geonames.usgs.gov/domestic/download_data.htm), gefilterd op de status van Rhode eiland om de grootte van de gegevensset te reduceren. We gebruiken deze gegevens maken van een search-servicetoepassing die oriëntatiepunten gebouwen zoals ziekenhuizen en scholen, evenals geologische functies zoals streams, meren en topontmoetingen retourneert.

In deze toepassing, wordt het programma **SearchServlet.java** genereert en laadt de index met een [indexering](https://msdn.microsoft.com/library/azure/dn798918.aspx) constructie voor het ophalen van de gefilterde USG gegevensset van een openbare Azure SQL-Database. Vooraf gedefinieerde referenties en verbindingsinformatie naar de gegevensbron online beschikbaar in de programmacode. Met de toegang tot de gegevens is geen verdere configuratie nodig.

> [AZURE.NOTE] We een filter toegepast op deze dataset om te blijven onder de Documentlimiet 10.000 voor de gratis prijzen laag. Deze limiet is niet van toepassing als u de standaard laag gebruikt, en kunt u deze code als u wilt gebruiken, een gegevensset groter te maken. Zie [beperkingen en limieten](search-limits-quotas-capacity.md)voor meer informatie over de capaciteit voor elke laag prijzen.

## <a name="about-the-program-files"></a>Over de programmabestanden

De volgende lijst worden de bestanden die relevant voor dit voorbeeld zijn.

- Search.JSP: Biedt de gebruikersinterface
- SearchServlet.java: Biedt methoden (vergelijkbaar met een controller in MVC)
- SearchServiceClient.java: Aanvragen van de verwerkt HTTP
- SearchServiceHelper.java: Een helperklasse die statische methoden biedt
- Document.Java: Biedt het gegevensmodel
- Config.Properties: Hiermee stelt u de URL van de service zoekopdracht en api-sleutel
- Pom.XML: Een Maven afhankelijkheid

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Zoek de servicenaam en api-sleutel van de zoekservice van Azure

Alle REST API voor gesprekken in Azure zoeken naar is vereist dat u de URL van de service en een api-sleutel opgeven. 

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com).
2. Klik in de balk sprong op **Search-service** te geven van de Azure zoeken services deze is ingericht voor uw abonnement.
3. Selecteer de service die u wilt gebruiken.
4. Klik op het servicedashboard ziet u tegels voor alle noodzakelijke informatie en het pictogram van de sleutel voor toegang tot de beheerder toetsen.

    ![][3]

5. Kopieer de URL van de service en de sleutel van een beheerder. Moet u deze later, wanneer u deze aan het bestand **config.properties toevoegt** .

## <a name="download-the-sample-files"></a>Voorbeeldbestanden downloaden

1. Ga naar [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) op Github.

2. Klik op **Downloaden ZIP**het ZIP-bestand op schijf opslaan en vervolgens extraheren alle bestanden die deze bevat. Houd rekening met het ophalen van de bestanden naar uw Java-werkruimte zodat u later gemakkelijk te vinden het project.

3. De voorbeeldbestanden zijn alleen-lezen. Met de rechtermuisknop op mapeigenschappen en schakelt u het kenmerk alleen-lezen.

Alle wijzigingen van de volgende bestand en uitvoeren-instructies worden tegen bestanden in deze map worden ingesteld.  

## <a name="import-project"></a>Project importeren

1. In Eclips, kiest u **bestand** > **importeren** > **algemene** > **Bestaande projecten in de werkruimte**.

    ![][4]

2. In de **hoofdmap selecteert**, blader naar de map met voorbeeldbestanden. Selecteer de map met de map .project. Het project moet worden weergegeven in de lijst met **projecten** als een geselecteerd item.

    ![][12]

3. Klik op **Voltooien**.

4. Gebruik **Projectverkenner** kunt weergeven en bewerken van de bestanden. Als dit nog niet is geopend, klikt u op **venster** > **Weergave tonen** > **Projectverkenner** of gebruik de snelkoppeling om dit te openen.

## <a name="configure-the-service-url-and-api-key"></a>De URL van service en api-sleutel configureren

1. Dubbelklik in **Projectverkenner**op **config.properties** als de configuratieinstellingen met de servernaam en api-sleutel wilt bewerken.

2. Raadpleeg de stappen eerder in dit artikel, waar u de URL van de service en api-sleutel in de [Portal van Azure gevonden](https://portal.azure.com)om de waarden die u nu **config.properties**wordt invoeren.

3. In **config.properties**, kunt u 'Api-sleutel' vervangen door de api-sleutel voor uw service. Vervolgens servicenaam (het eerste onderdeel van de URL-http://servicename.search.windows.net) Hiermee vervangt u "servicenaam" in hetzelfde bestand.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Het project, opbouwen en runtime omgevingen configureren

1. Eclips in Projectverkenner met de rechtermuisknop op het project > **Eigenschappen** > **Aspecten van Project**.

2. Selecteer **dynamische webmodule**, **Java**en **JavaScript**.

    ![][6]

3. Klik op **toepassen**.

4. Selecteer **venster** > **Voorkeuren** > **Server** > **runtimeomgevingen** > **toevoegen.**.

5. Vouw Apache en selecteer de versie van de Apache Tomcat-server die u eerder hebt geïnstalleerd. In ons system geïnstalleerde we versie 8.

    ![][7]

6. Geef de installatiemap Tomcat op de volgende pagina. Klik op een Windows-computer, wordt dit waarschijnlijk C:\Program Files\Apache Software Foundation\Tomcat *versie*zijn.

6. Klik op **Voltooien**.

7. Selecteer **venster** > **Voorkeuren** > **Java** > **geïnstalleerd JREs** > **toevoegen**.

8. Selecteer in **JRE toevoegen**, **Standaard VM**.

10. Klik op **volgende**.

11. In JRE definitie in JRE thuis, klikt u op **map**.

12. Ga naar **Program Files** > **Java** en selecteer de JDK u eerder hebt geïnstalleerd. Het is belangrijk om te selecteren van de JDK als de JRE.

13. Kies de **JDK**in JREs is geïnstalleerd. Uw instellingen moeten er ongeveer de volgende schermafbeelding uit.

    ![][9]

14. (Optioneel) Selecteer **venster** > **Webbrowser** > **Internet Explorer** naar de toepassing openen in een externe browservenster. Met een externe-browser, kunt u een betere ervaring van de Web-toepassing.

    ![][8]

U hebt nu de configuratietaken uit te voeren voltooid. Vervolgens gaat u maken en uitvoeren van het project.

## <a name="build-the-project"></a>Het project maken

1. In Projectverkenner met de rechtermuisknop op de naam van het project en kies **Uitvoeren als** > **Maven... maken** voor het configureren van het project.

    ![][10]

8. Typ 'schone installatie' in de configuratie van bewerken, in doelstellingen, en klik vervolgens op **uitvoeren**.

Statusberichten zijn uitvoer naar het consolevenster. Hier ziet u bouwen SUCCESS waarin wordt aangegeven dat het project ingebouwd zonder fouten.

## <a name="run-the-app"></a>De app uitvoeren

In deze laatste stap, kunt u de toepassing wordt uitgevoerd in een lokale server runtime-omgeving.

Als u dit nog niet hebt nog een server runtime-omgeving in Eclips opgegeven, moet u dat eerst te doen.

1. Vouw in Projectverkenner **webinhoud**.

5. Met de rechtermuisknop op **Search.jsp** > **uitvoeren als** > **worden uitgevoerd op de Server**. Selecteer de Apache Tomcat-server en klik vervolgens op **uitvoeren**.

> [AZURE.TIP] Als u een niet-standaard-werkruimte gebruikt voor de opslag van uw project, moet u **Configuratie uitvoeren** zodat deze verwijzen naar de projectlocatie om te voorkomen dat een serverfout opstart wijzigen. Projectverkenner met de rechtermuisknop op **Search.jsp** > **Uitvoeren als** > **Configuraties uitvoeren**. Selecteer de Apache Tomcat-server. Klik op **argumenten**. Klik op **Workspace** of **Bestandssysteem** om in te stellen van de map met het project.

Wanneer u de toepassing uitvoert, moet u een browservenster, Zie een zoekvak voor het invoeren van termen leveren aan te geven.

Wacht ongeveer een minuut voordat u op **Zoeken** om aan te geven van de servicetijd kunt maken en de index laden. Als u een HTTP 404-foutbericht krijgt, moet u alleen iets langer wachten voordat u probeert het opnieuw.

## <a name="search-on-usgs-data"></a>Zoeken op USG gegevens

De gegevensset USG records die relevant naar de status van Rhode eiland zijn opgenomen. Als u op **Zoeken** op een lege zoekvak, krijgt u de bovenste 50 posten, de standaardinstelling.

Invoeren van een zoekterm krijgt met het zoekprogramma iets om in te gaan. Geef een regionale naam. "Roger Williams" is de eerste gouverneur van Rhode eiland. Veel parken, gebouwen en scholen zijn benoemde na hem.

![][11]

U kunt ook proberen een van deze voorwaarden:

- Pawtucket
- Pembroke
- goose + cape

## <a name="next-steps"></a>Volgende stappen

Dit is de eerste Azure zoeken zelfstudie op basis van Java en de gegevensset USG. Over een periode, breiden we uit deze zelfstudie om u te illustreren extra zoekfuncties die u mogelijk wilt gebruiken in uw aangepaste oplossingen.

Als u al achtergrondinformatie in Azure zoeken, kunt u dit voorbeeld als een springboard voor verdere experimenten, misschien de [zoekpagina](search-pagination-page-layout.md)eindeffecten of [beperkte navigatie](search-faceted-navigation.md)implementeren. U kunt ook op de pagina met zoekresultaten verbeteren door te voegen telt en batchen van documenten, zodat gebruikers door de resultaten bladeren kunnen.

Ervaring met Azure zoeken? Het is raadzaam probeert andere zelfstudies voor het ontwikkelen van begrijpen wat u kunt maken. Ga naar onze [documentatiepagina](https://azure.microsoft.com/documentation/services/search/) meer informatie. U kunt ook de koppelingen weergeven in onze [Video en Zelfstudievideo lijst](search-video-demo-tutorial-list.md) voor toegang tot meer informatie.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
