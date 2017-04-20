<properties
    pageTitle="Aan de slag met Stream Analytics: realtime fraude detectie | Microsoft Azure"
    description="Informatie over het maken van een oplossing van de detectie realtime fraude met Stream Analytics. Een gebeurtenis hub voor realtime gebeurtenis verwerking gebruiken."
    keywords="afwijking detectie, detectie van fraude, realtime afwijking detectie"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok" />



# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Aan de slag met Azure Stream Analytics: realtime fraude detectie

Informatie over het maken van een end-to-end-oplossing voor realtime fraude detectie met Azure Stream Analytics. Gebeurtenissen brengen naar een hub Azure gebeurtenis, schrijven Stream Analytics-query's voor aggregatie of waarschuwingen en de resultaten verzenden naar een sink uitvoer om inzicht te krijgen over gegevens met realtime verwerking. Realtime afwijking detectie voor telecommunicatie valt, maar de voorbeeld-techniek gelijkmatig is geschikt voor andere soorten fraude detectie zoals creditcard of identiteit diefstal-scenario's.

Stream Analytics is een volledig beheerde service lage latentie, ten zeerste beschikbaar, scalable complexe gebeurtenis verwerking leveren via streaming van gegevens in de cloud. Zie [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)voor meer informatie.


## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Scenario: Telecommunicatie en SIM detectie van fraude in realtime

Een bedrijf telecommunicatie heeft een grote hoeveelheid gegevens voor binnenkomende oproepen. Het bedrijf heeft de volgende handelingen uit de gegevens nodig:
* Deze gegevens naar beneden af op een beheerbare bedrag vergelijken en inzichten over het gebruik van de klant via tijd en geografische regio's te verkrijgen.
* U opsporen SIM fraude (meerdere oproepen die afkomstig zijn uit dezelfde identiteit rond tegelijk, maar in geografisch verschillende locaties) in realtime zodat ze eenvoudig reageren kunnen op de hoogte te stellen klanten of service afgesloten.

In canonieke Internet van dingen (IoT) scenario's er wordt een ton telemetrielogboek of de gegevens die worden gegenereerd – en klanten wilt aggregeren ze of Waarschuw via afwijkingen in realtime.

## <a name="prerequisites"></a>Vereisten voor

- [TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) downloaden van het Microsoft Download Center 
- Optioneel: Broncode van de gebeurtenis genereren van [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TelcoGenerator)

## <a name="create-an-azure-event-hubs-input-and-consumer-group"></a>De invoer van een Azure gebeurtenis Hubs en consumenten groep maken

De toepassing van de steekproef worden gebeurtenissen genereren en deze op een exemplaar van de gebeurtenis Hub voor realtime verwerking push. Service Bus gebeurtenis Hubs zijn de voorkeursmethode voor gebeurtenis opname van Stream analysegegevens en u kunt meer informatie over de gebeurtenis Hubs in [Azure Service Bus documentatie](/documentation/services/service-bus/).

De Hub van een gebeurtenis maken:

1.  Klik in de [portal van Azure](https://manage.windowsazure.com/) op **Nieuw** > **App Services** > **Service Bus** > **Gebeurtenis Hub** > **Snelle maken**. Geef een naam, regio en nieuwe of bestaande naamruimte maken van een nieuwe gebeurtenis Hub.  
2.  Als een goede gewoonte, moet elke taak Stream Analytics lezen uit een eenmalige gebeurtenis Hub consumenten groep. We helpt u bij het proces van het maken van een groep consumenten onderstaande en kunt u [meer informatie over groepen consumenten](https://msdn.microsoft.com/library/azure/dn836025.aspx). Een consumenten als groep wilt maken, navigeer naar de zojuist gemaakte Hub voor de gebeurtenis en klikt u op het tabblad **Groepen consumenten** en vervolgens klikt u op **maken** vanaf de onderkant van de pagina en geef een naam voor uw groep consumenten.
3.  Toegang verlenen tot de Hub gebeurtenis moeten we een gedeelde-beleid maken.  Klik op het tabblad **configureren** van uw Hub-gebeurtenis.
4.  Klik onder **Gedeeld-beleid**maken van nieuw beleid met machtigingen **beheren** .

    ![Gedeeld Clienttoegangsbeleid waar u een beleid met machtigingen beheren maken kunt.](./media/stream-analytics-get-started/stream-ananlytics-shared-access-policies.png)

5.  Klik op **Opslaan** onder aan de pagina.
6.  Ga naar het **Dashboard** en **Verbindingsgegevens** aan de onderkant van de pagina, klikt u op en vervolgens kopiëren en de verbindingsgegevens opslaan.

## <a name="configure-and-start-event-generator-application"></a>Configureren en gebeurtenis genereren-toepassing start

We hebben een clienttoepassing die worden gegenereerd steekproef binnenkomende oproep metagegevens en druk op gebeurtenis Hub opgegeven. Volg de onderstaande stappen voor het instellen van deze toepassing.  

1.  Download het [bestand TelcoGenerator.zip](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip). Pak vervolgens deze naar een map.

    **Opmerking**: Windows mogelijk blokkeren het gedownloade zip-bestand. Klik met de rechtermuisknop op het bestand en selecteer Eigenschappen. Als het bericht ' dit bestand is afkomstig van een andere computer en ter bescherming van deze computer mogelijk geblokkeerd." vervolgens het vak 'Deblokkeren' maatstreepjes en klik op toepassen op het zip-bestand.

2.  Vervang de waarden Microsoft.ServiceBus.ConnectionString en EventHubName in **telcodatagen.exe.config** door uw gebeurtenis Hub-verbindingsreeks en de naam.

    **Opmerking**: de verbindingsreeks gekopieerd van de Azure portal plaatsen de naam van de verbinding aan het einde. Verwijder de "; EntityPath =<value>"= veld uit de sleutel toevoegen.

3.  Start de toepassing. Het gebruik is als volgt:

   telcodatagen.exe [#NumCDRsPerHour] [SIM kaart fraude kans] [#DurationHours]

Het volgende voorbeeld wordt 1000 gebeurtenissen met een kans 20 procent van fraude genereren in de loop van 2 uur.

    telcodatagen.exe 1000 .2 2

Hier ziet u records naar de Hub gebeurtenis is verzonden. Sommige sleutelvelden die we in deze toepassing van de detectie realtime fraude gebruiken worden hier gedefinieerd:

| Record | Definitie |
| ------------- | ------------- |
| CallrecTime | Begintijd van tijdstempel voor het gesprek. |
| SwitchNum | Telefoon schakeloptie gebruikt om verbinding maken met het gesprek. |
| CallingNum | Het telefoonnummer van de beller. |
| CallingIMSI | Internationale mobiele abonnee identiteit (IMSI).  Unieke id van de beller. |
| CalledNum | Het telefoonnummer van de ontvanger. |
| CalledIMSI | Internationale mobiele abonnee identiteit (IMSI).  Unieke id van de ontvanger. |


## <a name="create-stream-analytics-job"></a>Stream Analytics-taak maken
Nu dat we een reeks telecommunicatie gebeurtenissen hebben, kunnen we een taak Stream Analytics instellen zodat u kunt deze gebeurtenissen in realtime analyseren.

### <a name="provision-a-stream-analytics-job"></a>Een taak Stream Analytics inrichten

1.  Klik in de Azure-portal op **Nieuw > gegevensservices > Stream Analytics > snelle maken**.
2.  Opgeven van de volgende waarden en klik vervolgens op **Stream Analytics-taak maken**:

    * **Taaknaam**: Voer een taaknaam.

    * **Regio**: Selecteer het gebied waar u wilt uitvoeren van de taak. Houd rekening met de taak en de hub gebeurtenis plaatsen in dezelfde regio om ervoor te zorgen betere prestaties en om ervoor te zorgen dat u niet betaalt om gegevens te brengen tussen regio's.

    * **Opslag-Account**: Kies het Azure opslag-account dat u gebruiken wilt voor de opslag van controlegegevens voor alle Stream Analytics-taken in dit gebied wordt uitgevoerd. Hebt u de optie om een bestaande opslag-account te kiezen of een nieuwe opmerking te maken.

3.  Klik op **Stream Analytics** in het linkerdeelvenster voor een overzicht van de Stream Analytics-taken.

    ![Pictogram voor stream Analytics-service](./media/stream-analytics-get-started/stream-analytics-service-icon.png)

4.  De nieuwe taak wordt weergegeven met de status van **gemaakt**. Zoals u ziet dat de knop **Start** vanaf de onderkant van de pagina is uitgeschakeld. Moet u de taak invoer, uitvoer en query voordat u kunt de taak.

### <a name="specify-job-input"></a>Taak invoer opgeven
1.  In uw taak Stream Analytics **invoeritems** vanaf de bovenkant van de pagina op en klik vervolgens op **Invoer toevoegen**. Het dialoogvenster dat wordt geopend begeleidt u door een aantal van de stappen voor het instellen van uw invoer.
2.  **Gegevensstroom**selecteren en klik vervolgens op de juiste knop.
3.  Selecteer de **Gebeurtenis Hub**en klik vervolgens op de juiste knop.
4.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **Invoer Alias**: Voer een beschrijvende naam voor deze taak input zoals *CallStream*. Houd er rekening mee dat u deze naam in de query later gebruikt.
    * **Gebeurtenis-Hub**: als de gebeurtenis Hub die u hebt gemaakt, hetzelfde als de taak Stream Analytics-abonnement is, selecteert u de naamruimte waarop de gebeurtenis hub zich bevindt.

    Als uw hub gebeurtenis zich in een ander abonnement, selecteert u **Gebruik gebeurtenis Hub uit een ander abonnement** en gegevens voor de **Service Bus Namespace**, **De naam van de gebeurtenis-Hub**, **Gebeurtenis Hub beleidsnaam**, **Gebeurtenis Hub beleidssleutel**en **Aantal van gebeurtenis Hub partities**handmatig invoeren.

    * **De naam van de Hub gebeurtenis**: Selecteer de naam van de Hub gebeurtenis.

    * **De naam van een gebeurtenis Hub-beleid**: Selecteer de gebeurtenis-hub-beleid eerder in deze zelfstudie hebt gemaakt.

    * **Gebeurtenis Hub consumenten groep**: Typ de consumenten groep eerder in deze zelfstudie hebt gemaakt.
5.  Klik op de juiste knop.
6.  Geef de volgende waarden:

    * **Gebeurtenis serialisatiefunctie opmaken**: JSON
    * **Codering**: UTF8
7.  Klik op de knop controleren en deze bron en om te bevestigen dat Stream Analytics verbinding met de hub gebeurtenis maken kunt.

### <a name="specify-job-query"></a>Taak-query opgeven

Een eenvoudige, declaratieve query model ondersteunt stream Analytics voor de beschrijving van transformaties voor realtime verwerking. Meer informatie over de taal, raadpleegt u [Query Naslaggids voor Azure Stream Analytics](https://msdn.microsoft.com/library/dn834998.aspx). Deze zelfstudie kunt u de auteur en testen van meerdere query's via uw realtime gegevensstroom gesprek.

#### <a name="optional-sample-input-data"></a>Optioneel: Invoer voorbeeldgegevens
Valideren van uw query op gegevens van de werkelijke hoeveelheid werk, kunt u de functie **Voorbeeldgegevens** gebeurtenissen extraheren van de stream en maak een. JSON-bestand van de gebeurtenissen voor het testen.  De volgende stappen uitgelegd hoe u dit wilt doen en we een voorbeeldbestand [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json) voor testdoeleinden ook hebt opgegeven.

1.  De invoer van de gebeurtenis Hub en klik op **Voorbeeldgegevens** onder aan de pagina.
2.  In het dialoogvenster dat wordt weergegeven, Geef een **Begintijd** om te beginnen met het verzamelen van gegevens uit en een **duur** van de hoeveelheid extra gegevens om te gebruiken.
3.  Klik op de knop controleren om te beginnen steekproeven gegevens van de invoer.  Dit kan een paar minuten duren voor het gegevensbestand moet worden geproduceerd.  Wanneer het proces is voltooid, klikt u op **Details** en download en sla de. JSON-bestand dat wordt gegenereerd.

    ![Download en sla verwerkte gegevens in een JSON-bestand](./media/stream-analytics-get-started/stream-analytics-download-save-json-file.png)

#### <a name="passthrough-query"></a>Indirecte query

Als u archiveren van elke gebeurtenis wilt, kunt u een indirecte-query gebruiken om te lezen van alle velden in de nettolading van de gebeurtenis of het bericht. Beginnen met, kan een eenvoudige indirecte-query die projecten alle velden in een gebeurtenis.

1.  Klik op **Query** vanaf de bovenkant van de pagina van de taak Stream Analytics.
2.  Voeg de volgende naar de code-editor:

        SELECT * FROM CallStream

    > Zorg ervoor dat de naam van de invoerbron overeenkomt met de naam van de invoer die u eerder hebt opgegeven.

3.  Klik op **testen** onder de query-editor.
4.  Geef een testbestand, een die u hebt gemaakt met de vorige stappen of [telco.json](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json)gebruiken.
5.  Klik op de knop controleren en de resultaten weergegeven onder de definitie van de query weer te geven.

    ![Definitie van queryresultaten](./media/stream-analytics-get-started/stream-analytics-sim-fraud-output.png)


### <a name="column-projection"></a>Kolom raming

We kunt nu de geretourneerde velden naar een kleinere set verminderen.

1.  De query in de editor om te wijzigen:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum
        FROM CallStream

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

    ![Uitvoer in query-editor.](./media/stream-analytics-get-started/stream-analytics-query-editor-output.png)

### <a name="count-of-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Aantal inkomende oproepen per regio: gewor-venster met aggregatie

Als u wilt vergelijken van het bedrag dat binnenkomende oproepen per regio we wordt gebruikmaken van een [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) om het aantal binnenkomende gesprekken die zijn gegroepeerd op SwitchNum elke vijf seconden.

1.  De query in de editor om te wijzigen:

        SELECT System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount
        FROM CallStream TIMESTAMP BY CallRecTime
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Het trefwoord **Tijdstempel door** deze query gebruikt om op te geven van een tijdstempelveld in de nettolading moet worden gebruikt in de tijdelijke berekening. Als dit veld niet is opgegeven, zou de bewerking windowing worden uitgevoerd met de tijd die elke gebeurtenis op gebeurtenis Hub aangekomen. Zie ["Ontvangst van tijd tegenover toepassing Time" in de Stream Analytics Query Naslaggids](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Houd er rekening mee dat u toegang hebben tot een tijdstempel op voor het einde van elke venster met behulp van de eigenschap **System.Timestamp** .

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

    ![Queryresultaten voor Timestand door](./media/stream-analytics-get-started/stream-ananlytics-query-editor-rerun.png)

### <a name="sim-fraud-detection-with-a-self-join"></a>SIM fraude detectie met Self-joins

Als u wilt identificeren frauduleuze gebruik bespreken we voor oproepen die afkomstig zijn van dezelfde gebruiker maar op verschillende locaties in minder dan 5 seconden.  We [deelnemen aan](https://msdn.microsoft.com/library/azure/dn835026.aspx) de stroom van gesprek gebeurtenissen met zichzelf om deze zaken te controleren.

1.  De query in de editor om te wijzigen:

        SELECT System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1,
        CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
        ON CS1.CallingIMSI = CS2.CallingIMSI
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum

2.  Klik op **opnieuw uitvoeren** onder de query-editor om de resultaten van de query te bekijken.

    ![Queryresultaten van een join](./media/stream-analytics-get-started/stream-ananlytics-query-editor-join.png)

### <a name="create-output-sink"></a>Uitvoer sink maken

Nu dat we hebt gedefinieerd de stroom van een gebeurtenis, een gebeurtenis Hub invoer voor het nemen van gebeurtenissen en een query naar het uitvoeren van een transformatie via de stream, is de laatste stap het definiëren van een sink uitvoer voor de taak.  We wordt gebeurtenissen voor frauduleuze gedrag schrijven met Blob storage.

Volg de onderstaande stappen voor het maken van een container voor Blob storage als u er nog geen hebt.

1.  Gebruik van een bestaand opslag-account of maak een nieuw account voor de opslag door te klikken op **Nieuw > GEGEVENSSERVICES > opslag > snelle maken** en de instructies te volgen.
2.  Selecteer het account opslagruimte op **CONTAINERS** boven aan de pagina en klik vervolgens op **toevoegen**.
3.  Geef een **naam** voor de container en stel de **ACCESS** op openbare Blob.

## <a name="specify-job-output"></a>Taak-uitvoer opgeven

1.  In uw taak Stream Analytics **uitvoer** vanaf de bovenkant van de pagina op en klik vervolgens op **Uitvoer toevoegen**. Het dialoogvenster dat wordt geopend begeleidt u door een aantal van de stappen voor het instellen van uw uitvoer.
2.  Selecteer **BLOB STORAGE**en klik vervolgens op de juiste knop.
3.  Typ of Selecteer de volgende waarden op de derde pagina:

    * **UITVOERALIAS**: Voer een beschrijvende naam voor de uitvoer van deze taak.
    * **Abonnement**: als de blobopslag die u hebt gemaakt, hetzelfde als de taak Stream Analytics-abonnement is, selecteert u **Gebruik opslag-Account vanaf huidige abonnement**. Als uw opslag in een ander abonnement, selecteer **Gebruik opslag Account uit een ander abonnement** en gegevens voor **Opslag-ACCOUNT**, **ACCOUNTSLEUTEL opslag**, **CONTAINER**handmatig invoeren.
    * **Opslag-ACCOUNT**: Selecteer de naam van het account opslag.
    * **CONTAINER**: Selecteer de naam van de container.
    * **Bestandsnaam VOORVOEGSEL**: typen in een Bestandsvoorvoegsel gebruiken bij het schrijven van blob uitvoer.

4.  Klik op de juiste knop.
5.  Geef de volgende waarden:

    * **GEBEURTENIS SERIALISATIEFUNCTIE OPMAKEN**: JSON
    * **Codering**: UTF8

6.  Klik op de knop controleren en deze bron en om te bevestigen dat Stream Analytics succes verbinding met het account opslag maken kunt.

## <a name="start-job-for-real-time-processing"></a>Taak voor realtime verwerking starten

Aangezien een taak invoer, query, en uitvoer alle zijn opgegeven, zijn we de Stream Analytics-taak voor realtime fraude detectie gaan.

1.  Klik op de taak **DASHBOARD**, op **starten** onder aan de pagina.
2.  In het dialoogvenster dat wordt weergegeven, selecteert u **taak BEGINTIJD** en klik vervolgens op de knop controleren vanaf de onderkant van het dialoogvenster. De taakstatus wordt gewijzigd in **starten** en wordt kort verplaatsen naar **uitgevoerd**.

## <a name="view-fraud-detection-output"></a>Weergave fraude detectie uitvoer

Gebruik een hulpmiddel zoals [Azure opslag Explorer](https://azurestorageexplorer.codeplex.com/) of [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) frauduleuze gebeurtenissen weergeven terwijl ze naar uw uitvoer in realtime worden geschreven.  

![Detectie van fraude: frauduleuze gebeurtenissen bekeken in realtime](./media/stream-analytics-get-started/stream-ananlytics-view-real-time-fraudent-events.png)

## <a name="get-support"></a>Ondersteuning
Probeer onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)voor meer hulp.


## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
