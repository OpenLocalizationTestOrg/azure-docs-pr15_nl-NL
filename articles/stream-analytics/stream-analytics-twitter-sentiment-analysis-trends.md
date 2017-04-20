<properties
    pageTitle="Realtime Twitter sentiment-analyse met Stream Analytics | Microsoft Azure"
    description="Informatie over het gebruik van de Stream Analytics voor realtime Twitter sentiment analyse. Stapsgewijze instructies uit gebeurtenis generatie naar gegevens op een live dashboard."
    keywords="realtime twitter trendanalyse, sentiment analyse, sociale media analyse, trend analyse voorbeeld"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Sociale media analyse: Twitter-Real-time sentiment analyse in Azure Stream Analytics

Informatie over het maken van een oplossing sentiment analyse van sociale media analysegegevens doordat realtime Twitter-gebeurtenissen in Azure gebeurtenis Hubs. U kunt een Azure Stream Analytics-query om de gegevens te analyseren. U wordt vervolgens de resultaten voor later perusal opslaan of een dashboard en de [Power BI](https://powerbi.com/) gebruiken om aan te bieden inzichten in realtime.

Sociale media analysehulpmiddelen helpen organisaties trending onderwerpen, dat wil zeggen, onderwerpen en attitudes die een grote hoeveelheid berichten in sociale media hebben begrijpen. Sentiment analyse, ook wel *advies mining*genoemd, gebruikt sociale media analysehulpmiddelen om te bepalen attitudes richting van een product, idee, enzovoort. Realtime Twitter trendanalyse is een uitstekende voorbeeld omdat het model hashtag-abonnement kunt u specifieke trefwoorden beluisteren en ontwikkelen van sentiment analyse van de feed.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Scenario: Sentiment analyse in realtime

Een bedrijf dat een nieuws media-website heeft is een voordeel ten opzichte van de concurrenten door site-inhoud die direct relevant is voor de lezers geïnteresseerd. Het bedrijf gebruikt sociale media analyse over onderwerpen die relevant voor lezers zijn door te doen realtime sentiment analyses van Twitter-gegevens. Specifieke taken om aan te duiden trending onderwerpen in realtime op Twitter, moet het bedrijf realtime analytische gegevens over de tweet volume en sentiment voor belangrijke onderwerpen. Zo is, in wezen is het nodig een sentiment analyse analyse-engine die gebaseerd op deze sociale media-feed.

## <a name="prerequisites"></a>Vereisten voor
-   Twitter-account en [OAuth-toegangstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) van het Microsoft Download Center
-   Optioneel: Broncode voor twitter-client vanaf [GitHub](https://aka.ms/azure-stream-analytics-twitterclient)

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Een gebeurtenis hub invoer en een consumenten-groep maken

De toepassing van de steekproef wordt gebeurtenissen genereren en deze op een gebeurtenis Hubs-exemplaar (voor korte de hub van een gebeurtenis) push. Service Bus gebeurtenis hubs zijn de voorkeursmethode voor gebeurtenis opname van Stream analysegegevens. Zie de documentatie gebeurtenis Hubs in [Service Bus documentatie](/documentation/services/service-bus/).

Gebruik de volgende stappen uit om te maken van een gebeurtenis-hub.

1.  Klik op **Nieuw**in de portal Azure > **APP SERVICES** > **SERVICE BUS** > **GEBEURTENIS HUB** > **Snelle maken**, en geef een naam, regio en nieuwe of bestaande naamruimte.  
2.  Als een goede gewoonte, moet elke taak Stream Analytics lezen uit een eenmalige gebeurtenis Hubs consumenten-groep. We begeleiden u bij het maken van een groep consumenten later. U kunt meer informatie over groepen consumenten bij [Azure gebeurtenis Hubs overzicht](https://msdn.microsoft.com/library/azure/dn836025.aspx). Als u wilt een groep consumenten hebt gemaakt, gaat u de zojuist gemaakte gebeurtenis-hub, klik op het tabblad **Consumenten groepen** , klikt u op **maken** vanaf de onderkant van de pagina en geef een naam voor uw groep consumenten.
3.  Als u wilt verlenen van toegang tot de gebeurtenis-hub, moeten we een gedeelde-beleid maken. Klik op het tabblad **configureren** van uw hub-gebeurtenis.
4.  Klik onder **Gedeeld-beleid**maken van nieuw beleid met machtigingen **beheren** .

    ![Gedeeld Clienttoegangsbeleid waar u een beleid met machtigingen beheren maken kunt.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Klik op **Opslaan** onder aan de pagina.
6.  Ga naar het **DASHBOARD**op onder aan de pagina **VERBINDINGSGEGEVENS** en vervolgens kopiëren en opslaan van de verbindingsgegevens. (Gebruik een pictogram voor het exemplaar dat wordt weergegeven onder het zoekpictogram.)

## <a name="configure-and-start-the-twitter-client-application"></a>Configureren en te starten de Twitter-clienttoepassing

We hebben een clienttoepassing die verbinding met Twitter-gegevens via [De Twitter Streaming API's maken](https://dev.twitter.com/streaming/overview) voor het verzamelen van Tweet gebeurtenissen over een met parameters set onderwerpen opgegeven. Het hulpprogramma van de bron openen [Sentiment140](http://help.sentiment140.com/) wordt gebruikt om een waarde sentiment toewijzen aan elke tweet.

- 0 = negatief
- 2 = neutraal
- 4 = positief

Vervolgens worden Tweet gebeurtenissen verplaatst naar de hub gebeurtenis.  

Volg deze stappen voor het instellen van de toepassing:

1.  [De oplossing TwitterClient downloaden](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Open TwitterClient.exe.config en oauth_consumer_key, oauth_consumer_secret oauth_token en oauth_token_secret met Twitter-tokens die u uw waarden hebt vervangen.  

    [Stappen voor het genereren van een OAuth-toegangstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Houd er rekening mee dat u moet een lege toepassing om een token te genereren.  
3.  Vervang de waarden EventHubConnectionString en EventHubName in TwitterClient.exe.config door de verbindingsreeks en de naam van de hub van de gebeurtenis. De verbindingsreeks die u eerder hebt gekopieerd, hebt u zowel de verbindingsreeks en de naam van de gebeurtenis-hub, dus moet u scheiden en elk label in het juiste veld wilt plaatsen. Houd rekening met de volgende verbindingsreeks bijvoorbeeld:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    Het bestand TwitterClient.exe.config bevatten uw instellingen zoals in het volgende voorbeeld:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    Het is belangrijk te weten dat de tekst ' EntityPath = "wordt __niet__ weergegeven in de waarde EventHubName.

4.  *Optioneel:* Pas de trefwoorden te zoeken.  Standaard deze toepassing Hiermee wordt gezocht naar "Azure, Skype, XBox, Microsoft, Seattle".  Desgewenst kunt u de waarden voor **twitter_keywords** in TwitterClient.exe.config, aanpassen.
5.  Voer TwitterClient.exe om uw toepassing te starten. U ziet Tweet gebeurtenissen met de **CreatedAt**, **onderwerp**en **SentimentScore** waarden naar uw hub gebeurtenis is verzonden.

    ![Sentiment analyse: SentimentScore-waarden die zijn verzonden naar een hub voor de gebeurtenis.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Een taak Stream analyses maken

Nu dat Tweet gebeurtenissen streaming realtime uit Twitter, kunnen we een taak Stream Analytics instellen zodat u kunt deze gebeurtenissen in realtime analyseren.

### <a name="provision-a-stream-analytics-job"></a>Een taak Stream Analytics inrichten

1.  Klik in de [portal van Azure](https://manage.windowsazure.com/)op **Nieuw** > **GEGEVENSSERVICES** > **STREAM ANALYTICS** > **Snelle maken**.
2.  Opgeven van de volgende waarden en klik vervolgens op **STREAM ANALYTICS-taak maken**:

    * **TAAKNAAM**: Voer een taaknaam.
    * **Regio**: Selecteer het gebied waar u wilt uitvoeren van de taak. Houd rekening met de taak en de hub gebeurtenis plaatsen in dezelfde regio om ervoor te zorgen betere prestaties en om ervoor te zorgen dat u niet betaalt om gegevens te brengen tussen regio's.
    * **Opslag-ACCOUNT**: Kies het Azure opslag-account dat u gebruiken wilt voor de opslag van controlegegevens voor alle Stream Analytics-taken die in dit gebied worden uitgevoerd. Hebt u de optie om een bestaande opslag-account te kiezen of een nieuwe opmerking te maken.

3.  Klik op **STREAM ANALYTICS** in het linkerdeelvenster voor een overzicht van de Stream Analytics-taken.  
    ![Pictogram voor stream Analytics-service](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    De nieuwe taak wordt weergegeven met de status van **gemaakt**. Zoals u ziet dat de knop **START** vanaf de onderkant van de pagina is uitgeschakeld. Moet u de taak invoer, uitvoer en query voordat u kunt de taak.


### <a name="specify-job-input"></a>Taak invoer opgeven
1.  In uw project Stream Analytics **INVOERITEMS** vanaf de bovenkant van de pagina op en klik vervolgens op **Invoer toevoegen**. Het dialoogvenster dat wordt geopend begeleidt u door een aantal van de stappen voor het instellen van uw invoer.
2.  **GEGEVENSSTROOM**op en klik vervolgens op de juiste knop.
3.  **HUB voor de GEBEURTENIS**op en klik vervolgens op de juiste knop.
4.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **Invoer ALIAS**: Voer een beschrijvende naam voor deze taak invoert, zoals *TwitterStream*. Houd er rekening mee dat u deze naam in de query later gebruikt.
    **GEBEURTENIS-HUB**: als de gebeurtenis hub die u hebt gemaakt, hetzelfde als de taak Stream Analytics-abonnement is, selecteert u de naamruimte waarop de gebeurtenis hub zich bevindt.

        Als uw hub gebeurtenis zich in een ander abonnement, klikt u op **Gebruik gebeurtenis Hub uit een ander abonnement**en voert u de gegevens voor **SERVICE BUS NAAMRUIMTE**, **De naam van de GEBEURTENIS-HUB**, **GEBEURTENIS HUB BELEIDSNAAM**, **GEBEURTENIS HUB BELEIDSSLEUTEL**en **Aantal van GEBEURTENIS HUB PARTITIES**handmatig.

    * **De naam van de HUB GEBEURTENIS**: Selecteer de naam van de hub gebeurtenis.

    * **De naam van een GEBEURTENIS HUB-beleid**: Selecteer de gebeurtenis hub-beleid dat u eerder in deze zelfstudie hebt gemaakt.

    * **GEBEURTENIS HUB consumenten groep**: Typ de naam van de groep voor consumenten die u eerder in deze zelfstudie hebt gemaakt.
5.  Klik op de juiste knop.
6.  Geef de volgende waarden:

    * **GEBEURTENIS SERIALISATIEFUNCTIE OPMAKEN**: JSON
    * **Codering**: UTF8

7.  Klik op de knop **controleren** en deze bron en om te bevestigen dat Stream Analytics verbinding met de hub gebeurtenis maken kunt.

### <a name="specify-job-query"></a>Taak-query opgeven

Stream Analytics ondersteunt een eenvoudige, declaratieve query-model dat transformaties wordt beschreven. Meer informatie over de taal, raadpleegt u [Query Naslaggids voor Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Deze zelfstudie kunt u de auteur en testen van meerdere query's via Twitter-gegevens.

#### <a name="sample-data-input"></a>Voorbeeld gegevensinvoer

Valideren van uw query op gegevens van de werkelijke hoeveelheid werk, kunt u de functie **VOORBEELDGEGEVENS** gebeurtenissen extraheren van de stream en een .json-bestand van de gebeurtenissen voor het testen van maken.

1.  Selecteer de gebeurtenis hub invoer en klik op **VOORBEELDGEGEVENS** onder aan de pagina.
2.  In het dialoogvenster dat wordt geopend, Geef een **BEGINTIJD** om te beginnen met het verzamelen van gegevens en een **duur** van de hoeveelheid extra gegevens om te gebruiken.
3.  Klik op de knop **DETAILS** en klik vervolgens op de koppeling **Klik hier** om te downloaden en sla het bestand wordt gegenereerd .json.

#### <a name="pass-through-query"></a>Pass Through-query
Als u wilt starten, wordt doen we een eenvoudige Pass Through-query die projecten alle velden in een gebeurtenis.

1.  Klik op **QUERY** boven aan de pagina van de taak Stream Analytics.
2.  Klik in de editor code vervangen door de oorspronkelijke querysjabloon het volgende:

        SELECT * FROM TwitterStream

    Zorg ervoor dat de naam van de invoerbron overeenkomt met de naam van de invoer die u eerder hebt opgegeven.

3.  Klik op **testen** onder de query-editor.
4.  Ga naar uw .json voorbeeldbestand.
5.  Klik op de knop **controleren** en de resultaten onder de definitie van de query weer te geven.

    ![Resultaten weergegeven onder de definitie van query](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Aantal tweets op onderwerp: gewor-venster met aggregatie

Als u wilt vergelijken het aantal vermeldingen tussen onderwerpen, gebruiken we een [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) om het aantal vermeldingen op onderwerp elke vijf seconden.

1.  De query in de editor om te wijzigen:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    Het trefwoord **Tijdstempel door** deze query gebruikt om op te geven van een tijdstempelveld in de nettolading moet worden gebruikt in de tijdelijke berekening. Als u dit veld is niet opgeeft, de bewerking windowing uitgevoerd met behulp van de tijd die elke gebeurtenis in de hub gebeurtenis ontvangen.  Meer informatie in de sectie "Ontvangst van tijd tegenover toepassing Time" van [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Deze query ook toegang heeft tot een tijdstempel op voor het einde van elke venster met behulp van de eigenschap **System.Timestamp** .

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

#### <a name="identify-trending-topics-sliding-window"></a>Trending onderwerpen identificeren: schuifregelaar venster

Als u wilt identificeren trending onderwerpen, kijken we naar onderwerpen die langs de drempelwaarde voor vermeldingen in een bepaalde hoeveelheid tijd. Voor de toepassing van deze zelfstudie hebt we controleren op onderwerpen die meer dan 20 keer worden vermeld in de laatste vijf seconden met behulp van een [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx).

1.  De query in de editor om te wijzigen: Selecteer System.Timestamp als tijd, onderwerp, tellen (*) als vermeldingen van TwitterStream tijdstempel door CreatedAt groep door SLIDINGWINDOW(s, 5), onderwerp ONDERVINDT tellen (*) > 20

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

    ![Schuifregelaar venster queryuitvoer](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Aantal vermeldingen en sentiment: gewor-venster met aggregatie
De laatste query die we test wordt gebruikt **TumblingWindow** om het aantal vermeldingen, gemiddelde, minimum, maximum en de standaarddeviatie van de sentiment score voor elk onderwerp elke vijf seconden.

1.  De query in de editor om te wijzigen:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.
3.  Dit is de query die we voor onze dashboard gebruiken.  Klik op **Opslaan** onder aan de pagina.


## <a name="create-output-sink"></a>Uitvoer sink maken

Nu dat we de stroom van een gebeurtenis, een gebeurtenis hub invoer voor gebeurtenissen en van een query wilt uitvoeren van een transformatie via de stream, nemen hebt gedefinieerd is de laatste stap het definiëren van een sink uitvoer voor de taak.  We wordt de gebeurtenissen geaggregeerde tweet schrijven van onze query taak met Azure-blobopslag.  U kunt ook uw resultaten push met Azure SQL-Database, Azure-tabelopslag, of gebeurtenis Hubs, afhankelijk van uw specifieke toepassing nodig heeft.

Gebruik de volgende stappen uit om te maken van een container voor blobopslag, als u er nog geen hebt:

1.  Gebruik van een bestaand opslag-account of maak een nieuw account voor de opslag door te klikken op **Nieuw** > **GEGEVENSSERVICES** > **opslag** > **Snelle maken**, en volg vervolgens de instructies op het scherm.
2.  Selecteer het account opslagruimte op **CONTAINERS** boven aan de pagina en klik vervolgens op **toevoegen**.
3.  Geef een **naam** voor de container en de **toegang** ingesteld op **Openbare Blob**.

## <a name="specify-job-output"></a>Taak-uitvoer opgeven

1.  In uw project Stream Analytics **uitvoer** boven aan de pagina op en klik vervolgens op **Uitvoer toevoegen**. Het dialoogvenster dat wordt geopend begeleidt u bij de verschillende stappen voor het instellen van uw uitvoer.
2.  **BLOB STORAGE**op en klik vervolgens op de juiste knop.
3.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **UITVOERALIAS**: Voer een beschrijvende naam voor de uitvoer van deze taak.

    * **Abonnement**: als de blobopslag die u hebt gemaakt in hetzelfde als de taak Stream Analytics-abonnement is, klikt u op **Gebruik opslag-Account vanaf huidige abonnement**. Als uw opslag in een ander abonnement, klikt u op **Gebruik opslag Account uit een ander abonnement**en gegevens voor **Opslag-ACCOUNT**, **Opslag ACCOUNTSLEUTEL**en **CONTAINER**handmatig invoeren.

    * **Opslag-ACCOUNT**: Selecteer de naam van het account opslag.

    * **CONTAINER**: Selecteer de naam van de container.

    * **Bestandsnaam VOORVOEGSEL**: Typ een Bestandsvoorvoegsel gebruiken bij het schrijven van blob uitvoer.

4.  Klik op de juiste knop.
5.  Geef de volgende waarden:
    * **GEBEURTENIS SERIALISATIEFUNCTIE OPMAKEN**: JSON
    * **Codering**: UTF8
6.  Klik op de knop **controleren** en deze bron en om te bevestigen dat Stream Analytics succes verbinding met het account opslag maken kunt.

## <a name="start-job"></a>Taak starten

Aangezien een taak invoer, query, en uitvoer alle zijn opgegeven, zijn we gaan de Stream Analytics-taak.

1.  Klik op de taak **DASHBOARD**, op **starten** onder aan de pagina.
2.  In het dialoogvenster dat wordt geopend, klikt u op **taak BEGINTIJD**en klik vervolgens op de knop **controleren** op de onderkant van het dialoogvenster. De taakstatus wordt gewijzigd in **starten** en kort **uitgevoerd**wordt gewijzigd.


## <a name="view-output-for-sentiment-analysis"></a>Weergave-uitvoer voor sentiment analyse

Nadat uw taak is uitgevoerd en de realtime Twitter-stream processing, kies u hoe u wilt weergeven van de uitvoer voor sentiment analyse. Gebruik een hulpmiddel zoals [Azure opslag Explorer](https://azurestorageexplorer.codeplex.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) de uitvoer van uw project weergeven in realtime. Van hieruit kunt u [Power BI](https://powerbi.com/) voor het uitbreiden van uw toepassing om op te nemen van een aangepaste dashboard zoals dat in de volgende schermafbeelding.

![Sociale media analyse: Stream Analytics sentiment analyse (advies mining) uitvoer in een Power BI-dashboard.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Ondersteuning
Probeer onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor meer hulp.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
