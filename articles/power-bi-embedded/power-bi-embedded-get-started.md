<properties
   pageTitle="Aan de slag met Microsoft Power BI ingesloten"
   description="Power BI ingesloten, voegt u interactieve Power BI-rapporten in uw business intelligence-toepassing"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Aan de slag met Microsoft Power BI ingesloten

**Power BI ingesloten** is een Azure-service waarmee softwareontwikkelaars om toe te voegen interactieve Power BI-rapporten in hun eigen toepassingen. **Power BI ingesloten** werkt met bestaande toepassingen zonder nieuw ontwerp of wijzigen van de manier waarop gebruikers aanmelden.

Resources voor **Microsoft Power BI ingesloten** is tot en met de [Azure ARM-API's](https://msdn.microsoft.com/library/mt712306.aspx)ingericht. In dit geval is de resource die u inrichten een **Verzameling van Power BI-werkruimte**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Een werkruimte-verzameling maken
Een **Siteverzameling van de werkruimte** is de op het hoogste niveau Azure resource en een container voor de inhoud die worden ingesloten in uw toepassing. Een **Werkruimte siteverzameling** kan worden gemaakt op twee manieren::

   -    Handmatig met behulp van de Azure-Portal
   -    Via een programma met Azure Resource Manager(ARM) API 's

Laten we Doorloop de stappen voor het maken van een **Siteverzameling van de werkruimte** met behulp van de Azure-Portal.

   1.   Open en meld u aan bij de **Portal van Azure**: [http://portal.azure.com](http://portal.azure.com).

   2.   Klik op **+ Nieuw** boven in het venster.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   Klik op **Power BI ingesloten**onder **gegevens + Analytics** .
   4.   Klik op de **Aanmaakdatum Blade**, voer de vereiste informatie. Zie [Power BI ingesloten prijzen](http://go.microsoft.com/fwlink/?LinkID=760527)voor **prijzen**.

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Klik op **maken**.

De **Werkruimte siteverzameling** duurt even duren. Als voltooid, gaat u naar de **Werkruimte siteverzameling Blade**geleid.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

De **Aanmaakdatum Blade** bevat de informatie die u moet de API's die werkruimten inhoud maken en implementeren om ze te bellen.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Weergave toegangstoetsen Power BI-API

Een van de belangrijkste onderdelen van de gegevens die nodig zijn om te bellen van de Power BI REST API's zijn de **Toegangstoetsen**. Deze worden gebruikt om te genereren van de **app tokens** die worden gebruikt voor verificatie van uw API-aanvragen. Als u wilt uw **Toegangstoetsen**weergeven, klikt u op **Toegangstoetsen** op de **Instellingen Blade**. Zie voor meer informatie over **app tokens**, [Authenticating en machtiging met Power BI ingesloten](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Gaat u ' melding dat er twee toetsen.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Kopieer deze toetsen en veilig opslaan in uw toepassing. Is het belangrijk dat u deze toetsen behandelen zoals u zou doen met een wachtwoord, omdat ze toegang tot alle inhoud in uw **Siteverzameling van de werkruimte**wordt bieden.

Terwijl de twee sleutels worden weergegeven, is slechts één toets op een bepaald moment nodig. De tweede sleutel wordt geboden zodat u de toetsen regelmatig opnieuw genereren kunt zonder te onderbreken toegang tot de service.

Nu dat u een exemplaar van Power BI voor uw toepassing, en de **Toegangstoetsen**hebt, kunt u een rapport importeren in uw eigen app. Voordat u informatie over het importeren van een rapport, worden de volgende sectie maken Power BI-gegevenssets en rapporten wilt insluiten in een app.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Power BI-gegevenssets en rapporten wilt insluiten in een app maken

Nu dat u een exemplaar van Power BI voor uw toepassing hebt gemaakt, en **Toegangstoetsen**hebt, moet u de Power BI-gegevenssets en rapporten die u wilt insluiten. Gegevenssets en rapporten kunnen worden gemaakt met behulp van **Power BI Desktop**. U kunt downloaden [Power BI Desktop voor vrij te geven](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Of, als u wilt snel aan de slag, kunt u de [detailhandel analyse steekproef PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547) downloaden. Zie meer informatie over het gebruik van **Power BI Desktop**, [Aan de slag met Power BI Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Met **Power BI Desktop**, verbinding met de gegevensbron door een kopie van de gegevens importeren in **Power BI Desktop** of rechtstreeks verbinden met de gegevensbron met behulp van **DirectQuery**.

Dit zijn de verschillen tussen het gebruik van **importeren** en **DirectQuery**.

|Importeren | DirectQuery
|---|---
|Tabellen, kolommen, *en gegevens* zijn geïmporteerd of gekopieerd naar **Power BI Desktop**. Terwijl u met visualisaties werkt, een **Power BI Desktop** query een kopie van de gegevens. Als u wilt zien welke wijzigingen die zijn aangebracht in de onderliggende gegevens, moet u vernieuwen of gegevensset op te halen, een volledige, huidige opnieuw.|Alleen de *tabellen en kolommen* zijn geïmporteerd of gekopieerd naar **Power BI Desktop**. Terwijl u met visualisaties werkt, een **Power BI Desktop** query de onderliggende gegevensbron, wat betekent dat u altijd actuele gegevens weergeeft.

Zie [verbinding maken met een gegevensbron](power-bi-embedded-connect-datasource.md)voor meer informatie over het verbinding maken met een gegevensbron.

Nadat u uw werk in **Power BI Desktop opslaat**, wordt een PBIX-bestand gemaakt. Dit bestand bevat het rapport. Bovendien als u gegevens importeert de PBIX de volledige gegevensset bevat of als u **DirectQuery**gebruikt, de PBIX alleen een gegevensset schema bevat. U kunt via een programma de PBIX implementeren in uw werkruimte met de [Power BI importeren API](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI ingesloten** heeft extra API's om te wijzigen van de server en database waarnaar uw gegevensset wijst en de referenties van een service-account waarmee de gegevensset kunt verbinding maken met uw database instellen. Zie [bericht SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) en [Patch Gateway-gegevensbron](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Volgende stappen
U gemaakt in de voorgaande stappen, een verzameling werkruimte en uw eerste rapport en gegevensset. Nu is het tijd wilt weten hoe u de programmacode schrijven voor **Power BI ingesloten**. Als u aan de slag, die we hebben gemaakt een steekproef web-app: [aan de slag met het voorbeeld](power-bi-embedded-get-started-sample.md). In het voorbeeld ziet u hoe naar:

  - Inhoud van de voorziening
      - Een werkruimte maken
      - Een PBIX-bestand importeren
      - Verbindingstekenreeksen met de bijwerken en referenties voor uw gegevenssets.

  - Een rapport veilig insluiten

## <a name="see-also"></a>Zie ook
- [Aan de slag met steekproef](power-bi-embedded-get-started-sample.md)
- [Verificatie en machtiging met Power BI ingesloten](power-bi-embedded-app-token-flow.md)
- [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
