<properties
    pageTitle="Power BI dashboard op Stream Analytics | Microsoft Azure"
    description="Gebruik een realtime streaming Power BI-dashboard verzamelen bedrijfsinformatie en analyseren van grote hoeveelheden gegevens uit een taak Stream Analytics."
    keywords="dashboard voor gebruiksanalyses, realtime dashboard"
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
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Streamen Analytics en Power BI: een dashboard realtime analytics voor gegevensstromen

Azure Stream Analytics kunt u profiteren van een van de toonaangevende bedrijfsinformatiehulpprogramma's, Microsoft Power BI. Informatie over het gebruik van Azure Stream Analytics om grote hoeveelheden, streaming gegevens en krijgen de inzicht in een dashboard voor gebruiksanalyses van realtime Power BI te analyseren.

[Microsoft Power BI](https://powerbi.com/) gebruiken voor het maken van een live dashboard snel. [Bekijk een video het scenario illustreren](https://www.youtube.com/watch?v=SGUpT-a99MA).

In dit artikel leert u hoe uw eigen aangepaste bedrijfsinformatiehulpprogramma's met behulp van Power BI als een uitvoer voor uw taken Azure Stream analyses maken en gebruiken van een realtime dashboard.

## <a name="prerequisites"></a>Vereisten voor

* Microsoft Azure-Account
* Een invoer voor de taak Stream Analytics streaming gegevens uit in beslag neemt. Stream Analytics accepteert invoer van Azure gebeurtenis Hubs of Azure Blob storage.  
* Werk- of schoolaccount account voor Power BI

## <a name="create-azure-stream-analytics-job"></a>Azure Stream Analytics-taak maken

Klik op **Nieuw, Data Services, Stream analyses, snelle maken**in [Klassieke Azure-Portal](https://manage.windowsazure.com).

Geef de volgende waarden, klik vervolgens op **taak van de Stream analyses maken**:

* **De naam van de taak** - Geef de taaknaam van een. Bijvoorbeeld: **DeviceTemperatures**.
* **Regio** - Selecteer de regio waar u de taak die zich bevindt. Houd rekening met de taak en de hub gebeurtenis plaatsen in het gebied dezelfde voor optimale prestaties en te voorkomen dat gegevens doorverbinden kosten tussen regio's.
* **Opslag Account** - Kies het opslag-Account dat u gebruiken wilt voor de opslag van controlegegevens voor alle Stream Analytics-taken in dit gebied wordt uitgevoerd. U hebt de optie voor het kiezen van een bestaand opslag-Account of een nieuw account te maken.

Klik op **Stream Analytics** in het linkerdeelvenster voor een overzicht van de Stream Analytics-taken.

![graphic1][graphic1]

> [AZURE.TIP] De nieuwe taak wordt weergegeven met de status **Niet gestart**. Zoals u ziet dat de knop **Start** vanaf de onderkant van de pagina is uitgeschakeld. Dit is normaal zoals moet u de taak invoer, uitvoer, query, enzovoort voordat u kunt de taak.

## <a name="specify-job-input"></a>Taak invoer opgeven

Voor deze zelfstudie, wordt uitgegaan van een dat u gebeurtenis Hub gebruikt als invoerwaarde met JSON serialisatie en UTF-8-codering.

* Klik op de taaknaam.
* **Ingangen** vanaf de bovenkant van de pagina op en klik vervolgens op **Invoer toevoegen**. Het dialoogvenster dat wordt geopend begeleidt u door een aantal van de stappen voor het instellen van uw invoer.
*   **Gegevensstroom**selecteren en klik vervolgens op de juiste knop.
*   Selecteer de **Gebeurtenis Hub**en klik vervolgens op de juiste knop.
*   Typ of Selecteer de volgende waarden op de derde pagina:
  * **Invoer Alias** - Voer een beschrijvende naam voor deze taak invoer. Houd er rekening mee dat u deze naam in de query later gebruikt.
  * Er is een **Gebeurtenis Hub** - als de gebeurtenis Hub u hebt gemaakt in hetzelfde abonnement als de taak Stream analyses, selecteer de naamruimte waarop de gebeurtenis hub zich bevindt.
*   Als uw hub gebeurtenis zich in een ander abonnement, selecteert u **Gebruik gebeurtenis Hub uit een ander abonnement** en gegevens voor de **Service Bus Namespace**, **De naam van de gebeurtenis-Hub**, **Gebeurtenis Hub beleidsnaam**, **Gebeurtenis Hub beleidssleutel**en **Aantal van gebeurtenis Hub partities**handmatig invoeren.

> [AZURE.NOTE]  In dit voorbeeld wordt het standaardaantal partities, welke 16 is gebruikt.

* **De naam van de gebeurtenis-Hub** - Selecteer de naam van de Azure gebeurtenis Hub die u hebt.
* **De naam van het beleid voor gebeurtenis-Hub** - Selecteer de gebeurtenis Hub-beleid voor de gebeurtenis Hub die u gebruikt. Zorg ervoor dat dit beleid machtigingen heeft beheren.
*   **Gebeurtenis Hub consumenten groep** : U kunt leeg laat, of een consumenten groep er op uw Hub gebeurtenis. Houd er rekening mee dat elke groep consumenten van een gebeurtenis Hub slechts 5 lezers tegelijk kunt laten. Zo is, de groep rechts consumenten voor uw taak hiervan bepalen. Als u het veld leeg laat, wordt de standaardgroep voor consumenten gebruikt.

*   Klik op de juiste knop.
*   Geef de volgende waarden:
  * **Gebeurtenis serialisatiefunctie opmaken** - JSON
  * **Codering** - UTF8
*   Klik op de knop controleren en deze bron en om te bevestigen dat Stream Analytics verbinding met de Hub gebeurtenis maken kunt.

## <a name="add-power-bi-output"></a>Power BI-uitvoer toevoegen

1.  **Uitvoer** vanaf de bovenkant van de pagina op en klik vervolgens op **Uitvoer toevoegen**. Ziet u dat Power BI vermeld als een uitvoeroptie.

    ![graphic2][graphic2]  

2.  Selecteer **Power BI** en klik op de juiste knop.
3.  Hier ziet u een scherm als volgt uit:

    ![graphic3][graphic3]  

4.  In deze stap leveren een werk- of schoolaccount-account voor de uitvoer van de taak Stream Analytics. Als u al Power BI-account hebt, selecteert u **Nu Autoriseer**. Als dat niet zo is, kiest u **Nu registreren**. [Hier ziet u een goede blog doorlopen details van Power BI registreren](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Volgende ziet u een scherm als volgt uit:

    ![graphic4][graphic4]  

Geef waarden zoals hieronder:

* **Uitvoeralias** : U kunt elke uitvoeralias die u gemakkelijk om te verwijzen naar plaatsen. Deze uitvoeralias is met name handig als u besluit om meerdere uitvoer voor uw taak. U moet in dat geval verwijzen naar deze uitvoer in uw query. Laten we eens gebruik bijvoorbeeld de waarde van de alias uitvoer = "OutPbi".
* **De naam van de gegevensset** - Geef de gegevenssetnaam van een die u wilt dat uw Power BI uitvoeren om te laten. We gaan gebruiken, bijvoorbeeld "pbidemo".
*   **Tabelnaam** - Geef de naam van een tabel onder de gegevensset van uw Power BI-uitvoer. Stel dat we Roep deze "pbidemo". Power BI-uitvoer van Stream Analytics taken mogelijk op dit moment alleen één tabel hebt in een gegevensset.
*   **Werkruimte** – Selecteer een werkruimte in uw Power BI-tenant waaronder de gegevensset wordt gemaakt.

>   [AZURE.NOTE] U moet niet expliciet maken deze gegevensset en de tabel in uw Power BI-account. Deze wordt automatisch gemaakt wanneer u uw taak Stream Analytics starten en de taak reageert uitvoer in Power BI begint. Als uw query taak geen resultaten oplevert, wordt de gegevensset en de tabel niet gemaakt.

*   Klik op **OK**, **Verbinding testen** en nu u uitvoer configuratie is voltooid.

>   [AZURE.WARNING] Bedenk ook dat als Power BI al een gegevensset en een tabel met dezelfde naam als die u hebt opgegeven in deze taak Stream analyses, de bestaande gegevens overschreven.


## <a name="write-query"></a>Query schrijven

Ga naar het tabblad **Query** op van uw taak. Schrijf uw query, die de uitvoer van de plaats waar u wilt dat in uw Power BI. Bijvoorbeeld, is het mogelijk iets zoals de volgende SQL-query:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Uw taak starten. Controleer of uw hub gebeurtenis gebeurtenissen ontvangt en genereert de verwachte resultaten van de query. Als uw query uitvoer 0 rijen, worden Power BI gegevensset en tabellen niet automatisch gemaakt.

## <a name="create-the-dashboard-in-power-bi"></a>Het dashboard maken in Power BI

Ga naar de [Powerbi.com](https://powerbi.com) en meld u aan met uw account voor werk- of schoolaccount. Als de Stream Analytics taak query uitvoer resultaten, ziet u dat uw gegevensset al is gemaakt:

![graphic5][graphic5]

Voor het maken van het dashboard, gaat u naar de optie Dashboards en maak een nieuw Dashboard.

![graphic6][graphic6]

In dit voorbeeld we label wordt deze "Demo Dashboard".

Klik nu op de gegevensset gemaakt door uw taak Stream Analytics (pbidemo in ons voorbeeld huidige). U wordt worden gehouden aan een pagina met het maken van een grafiek boven aan deze dataset. Hieronder ziet maar een voorbeeld van de rapporten die u kunt maken.

Selecteer Σ temp en afstemmen van velden. Ze gaat automatisch naar waarde en as voor de grafiek:

![graphic7][graphic7]

Hiermee krijgt automatisch u een grafiek zoals hieronder:

![graphic8][graphic8]

Klik in de sectie waarde Klik op de vervolgkeuzelijst omlaag voor temp en kies **gemiddelde** voor de temperatuur en klik op de grafiek, klikt u op de **visualisatie** en kies **lijndiagram**:

![graphic9][graphic9]

Nu krijgt u een lijndiagram van gemiddelde na verloop van tijd.  Met de optie pincode zoals hieronder, maar u kunt vastmaken dit aan uw dashboard die u eerder hebt gemaakt:

![graphic10][graphic10]

Nu wanneer u het dashboard met dit vastgemaakte rapport bekijkt, ziet u rapport bij te werken in realtime. Probeer wilt dat de gegevens in uw gebeurtenissen – de Prikker temp of een ander nummer wijzigen en u ziet het realtime-effect dat doorgevoerd in uw dashboard.

Opmerking dat deze zelfstudie gedemonstreerd hoe maken maar één type grafiek voor een gegevensset. Power BI kunt u andere klant bedrijfsinformatiehulpprogramma's voor uw organisatie maken. Bekijk de video [Aan de slag met Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) voor een ander voorbeeld van een Power BI-dashboard.

Raadpleeg de [Power BI-sectie](stream-analytics-define-outputs.md#power-bi) van het [dat lidmaatschap Stream Analytics uitvoer](stream-analytics-define-outputs.md "Hiermee kunt u inzicht Stream Analytics")voor meer informatie over het configureren van een Power BI-uitvoer en gebruikmaken van de Power BI-groepen. Een andere handige resource voor meer informatie over het maken van Dashboards met Power BI is [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Beperkingen en aanbevolen procedures

Power BI dienst gelijktijdigheid en de doorvoer beperkingen, zoals hier wordt beschreven: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Prijzen van Power BI")

Vanwege de terechtkomt Power BI zelf meest natuurlijk naar gevallen waarin Azure Stream Analytics een belangrijke gegevens laden wilt verkleinen heeft.
We raden u aan TumblingWindow of HoppingWindow gebruiken om ervoor te zorgen dat gegevens push maximaal 1 push/tweede zijn zou en dat uw query binnen de vereisten doorvoer terechtkomt – kunt u de volgende vergelijking om de waarde zodat uw venster in seconden te berekenen:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Als voorbeeld – als er 1000 apparaten verzenden van gegevens elke tweede, u bent in de Power BI Pro SKU die ondersteuning biedt voor 1.000.000 rijen/uur en u gegevens wilt ophalen gemiddelde per apparaat op Power BI kunt u doen maximaal een push elke 4 seconden per apparaat (zoals hieronder weergegeven):  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Wat betekent dat de oorspronkelijke query om te worden gewijzigd:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>PowerBI weergave vernieuwen

Een vraag is "Waarom niet het dashboard automatisch bijwerken in PowerBI?".

Daartoe in PowerBI gebruikmaken van Q & A en een vraag zoals "Maximumwaarde door temp waar tijdstempel vandaag is" en die tegel aan het dashboard vastmaken.

### <a name="renew-authorization"></a>Autorisatie verlengen

U moet uw Power BI-account opnieuw te verifiëren als het wachtwoord heeft gewijzigd sinds de taak is gemaakt of laatst geverifieerd. U moet dus ook Power BI autorisatie voor elke 2 weken verlengen als meervoudige verificatie MFA () is geconfigureerd op uw tenant Azure Active Directory (AAD). Een symptoom van dit probleem is geen uitvoer taak en een 'verifiëren gebruikersfout' in de logboeken aan de bewerking:

![graphic12][graphic12]

Als een taak probeert te starten terwijl het token is verlopen, wordt een foutbericht weergegeven en het begin van de taak, mislukt. De fout ziet er nu ongeveer zoals hieronder:

![Fout in de PowerBI](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

U lost dit probleem op door uw actieve taak stoppen en Ga naar uw Power BI-uitvoer.  Klik op de koppeling "Verlengen autorisatie" en uw taak van de laatste keer dat kan worden gestopt om te voorkomen van gegevensverlies opnieuw starten.

![PowerBI gegevensvalidatie verlenging](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Nadat de autorisatie wordt vernieuwd met Power BI ziet u een melding voor een groene in het gebied autorisatie:

![PowerBI gegevensvalidatie verlenging](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
