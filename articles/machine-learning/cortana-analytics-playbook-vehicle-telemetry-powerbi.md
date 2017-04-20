<properties 
    pageTitle="Instellingsinstructies voertuig telemetrielogboek analytics oplossing sjabloon PowerBI Dashboard | Microsoft Azure" 
    description="De mogelijkheden van Cortana Intelligence gebruiken om te krijgen van realtime en bekijk inzichten in een voertuig gezondheid en sturende voorkeur werkwijze." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-template-powerbi-dashboard-setup-instructions"></a>Voertuig telemetrielogboek analytics oplossing sjabloon PowerBI Dashboard installatie-instructies

In dit **menu** koppelingen naar de hoofdstukken in deze playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]


De oplossing voertuig Telemetrielogboek Analytics gepresenteerd hoe auto dealerbedrijven, auto fabrikanten en ondernemingen van de mogelijkheden van Cortana Intelligence gebruikmaken kunnen krijgen realtime en bekijk inzichten over voertuig status en een voorkeur werkwijze naar station verbeteringen in het gebied van de klant nog steeds ondervindt, R & D en marketingcampagnes. In dit document bevat stapsgewijze instructies voor het hoe kunt u de PowerBI-rapporten en dashboards zodra de oplossing is geïmplementeerd in uw abonnement. 


## <a name="prerequisites"></a>Vereisten voor
1.  De voertuig Telemetrielogboek Analytics-oplossing implementeert naar [https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3](https://gallery.cortanaanalytics.com/SolutionTemplate/Vehicle-Telemetry-Analytics-3) navigeren  
2.  [Microsoft Power BI Desktop installeren](http://www.microsoft.com/download/details.aspx?id=45331)
3.  Een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/). Als u geen een Azure-abonnement hebt, aan de slag met Azure gratis abonnement
4.  Microsoft PowerBI-account
    

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence-Suite onderdelen
Als onderdeel van de sjabloon voertuig Telemetrielogboek Analytics-oplossing, zijn de volgende Cortana Intelligence-services geïmplementeerd in uw abonnement.

- **Gebeurtenis Hubs** voor ingesting miljoenen voertuig telemetrielogboek gebeurtenissen in Azure.
- **Stream analytische**s voor realtime inzichten over voertuig status en zich blijft voordoen die gegevens in de lange termijn opslag van uitgebreidere batch analysegegevens.
- **Machine Learning** voor detectie van de afwijking in realtime en batchbestand naar blog inzicht krijgen.
- **HDInsight** wordt gebruikt om gegevens bij het op schaal transformeren
- **Gegevens Factory** verwerkt situatie, planning, resourcebeheer en het monitoren van de pijplijn batchbestand.

**Power BI** krijgt deze oplossing een uitgebreide dashboard voor realtimegegevens en bekijk analyses visualisaties. 

De oplossing gebruik van twee verschillende gegevensbronnen: **gesimuleerd voertuig signalen en diagnostische gegevensset** en **voertuig catalogus**.

Een voertuig telematica simulator maakt deel uit van deze oplossing. Er genereert diagnostische informatie en signalen overeenkomt met de status van het voertuig en sturende patroon op een bepaald moment in tijd. 

De catalogus voertuig is een gegevensset verwijzing met Chassisnummer model toewijzen aan


## <a name="powerbi-dashboard-preparation"></a>Voorbereiding van PowerBI Dashboard

### <a name="deployment"></a>Implementatie

Wanneer de implementatie is voltooid, ziet u het volgende diagram met al deze onderdelen in het groen gemarkeerd. 

- Naar de bijbehorende services wilt controleren of u al deze hebt geïmplementeerd wilt gaan, klikt u op de pijl in de rechterbovenhoek van de groene knooppunten.
- Als u wilt de gegevens simulator-pakket downloaden, klikt u op de pijl in de rechterbovenhoek rechts op het knooppunt **Voertuig telematica Simulator** . Opslaan en pak de bestanden lokaal op uw computer. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/1-deployed-components.png)

Nu u klaar bent voor het configureren van het dashboard PowerBI met uitgebreide visualisaties krijgen realtime en bekijk inzichten in een voertuig gezondheid en sturende achterhalen. Het duurt ongeveer 45 minuten naar uren alle rapporten maken en configureren van het dashboard. 


### <a name="setup-power-bi-real-time-dashboard"></a>Power BI realtime Dashboard instellen

**Gesimuleerd gegevens genereren**

1. Op de lokale computer, gaat u naar de map waarin u het pakket voertuig telematica Simulator uitgepakt![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/2-vehicle-telematics-simulator-folder.png)
2.  De toepassing ***CarEventGenerator.exe***uitvoeren.
3.  Er genereert diagnostische informatie en signalen overeenkomt met de status van het voertuig en sturende patroon op een bepaald moment in tijd. Hiermee wordt gepubliceerd naar een exemplaar van Azure gebeurtenis Hub die is geconfigureerd als onderdeel van uw implementatie.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/3-vehicle-telematics-diagnostics.png)
     
**Start de dashboardtoepassing realtime**

De oplossing bevat een toepassing die een realtime dashboard in PowerBI genereert. Deze toepassing luistert naar een exemplaar van gebeurtenis-Hub, waaruit Stream Analytics gebeurtenissen continu publiceert. Voor elke gebeurtenis die deze toepassing ontvangt, worden de gegevens met een Machine Learning aanvraag en respons score eindpunt verwerkt. De resulterende gegevensset wordt gepubliceerd naar de push PowerBI API's voor visualisatie. 

De toepassing te downloaden:

1.  Klik op het knooppunt PowerBI in de diagramweergave te klikken en klik op de **Toepassing voor Real-time Dashboard downloaden**' koppeling in het deelvenster Eigenschappen.![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard-new1.png)
2.  Ophalen en de toepassing lokaal opslaan![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/4-real-time-dashboard-application.png)

3.  De toepassing **RealtimeDashboardApp.exe** uitvoeren
4.  Geldige Power BI-referenties opgeven, meld u aan en klik op **accepteren**
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
    
    ![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)


### <a name="configure-powerbi-reports"></a>Rapporten over het PowerBI configureren
De realtime rapporten en het dashboard moet u over 30-45 minuten duren om te voltooien. Blader naar [http://powerbi.com](http://powerbi.com) en u aanmelden.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Een nieuwe gegevensset wordt gegenereerd in Power BI. Klik op de gegevensset **ConnectedCarsRealtime** .

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Sla het leeg rapport met **Ctrl + s**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Geef rapportnaam *Real-time voor voertuig Telemetrielogboek Analytics - rapporten*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Realtime-rapporten
Er zijn drie realtime rapporten in deze oplossing:

1.  Voertuigen betrekking heeft
2.  Voertuigen onderhoud vereisen
3.  Voertuigen systeemstatus statistieken

U kunt alle drie realtime rapporten configureren of stoppen na een fase en gaat u verder met het volgende gedeelte van het configureren van de batch-rapporten. We raden u aan het maken van alle drie rapporten kunt visualiseren van de volledige inzichten van het realtime pad van de oplossing.  

### <a name="1-vehicles-in-operation"></a>1. voertuigen betrekking heeft
  
Dubbelklik op **pagina 1** en wijzig de naam "Voertuigen betrekking heeft"  
    ![Auto's - voertuigen betrekking heeft verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Selecteer **chassisnummer** veld in **velden** en kies visualisatietype als **"Kaart"**.  

Kaartvisualisaties wordt gemaakt, zoals wordt weergegeven in de afbeelding.  
    ![Verbonden auto's - Select chassisnummer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.  

**Plaats** en **chassisnummer** uit velden selecteert. Visualisatie wijzigen aan **'Map'**. Sleep **chassisnummer** in het waardengebied. Sleep **stad** uit velden naar **legenda** .   
    ![Auto's - Kaartvisualisaties verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)
  
Selecteer sectie **format** in **Visualisaties**, klikt u op **titel** en de **tekst** wijzigen in **"Voertuigen betrekking heeft op stad"**.  
    ![Auto's - voertuigen betrekking heeft op stad verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Laatste visualisatie ziet er als wordt weergegeven in afbeelding.    
    ![Verbonden auto's - definitief visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.  

Selecteer **plaats** en **chassisnummer**, visualisatie wijzigen in een **Gegroepeerd kolomdiagram**. Controleer of het veld **plaats** in het **gebied as** en **chassisnummer** in het **gebied van de waarde**  

Grafiek door **"Aantal vin"** sorteren  
    ![Verbonden auto's - aantal chassisnummer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

**Titel** van een grafiek wijzigen in **"Voertuigen betrekking heeft op stad"**  

Klik op de sectie **indeling** en selecteer vervolgens **Gegevens kleuren**, klikt u op de **'In'** Alles **Weergeven**  
    ![Verbonden auto's - weergeven alle gegevens kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

De kleur van afzonderlijke plaats wijzigen door te klikken op het Kleurenpictogram.  
    ![Verbonden auto's - kleuren wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.  

Selecteer **Gegroepeerd kolomdiagram** visualisatie van visualisaties, sleept u het veld **plaats** in **as** gebied **Model** in het gebied van de **legenda** en **chassisnummer** in het gebied van de **waarde** .  
    ![Verbonden auto's - gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Verbonden auto's - weergeven](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)
  
De volgorde van alle visualisatie op deze pagina zoals wordt weergegeven in de afbeelding.  
    ![Verbonden auto's - visualisaties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Het "Voertuigen betrekking heeft" realtime-rapport is geconfigureerd. U kunt doorgaan met het volgende realtime-rapport maken of hier stoppen en configureren van het dashboard. 

### <a name="2-vehicles-requiring-maintenance"></a>2. voertuigen onderhoud vereisen
  
Klik op ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om toe te voegen een nieuw rapport, wijzig de naam **"voertuigen vereisen onderhoud"**

![Verbonden auto's - voertuigen onderhoud vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Selecteer **chassisnummer** veld en wijzig visualisatietype in **kaart**.  
    ![Verbonden auto's - Vin Kaartvisualisaties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

We hebben een veld met de naam 'MaintenanceLabel' In de gegevensset. In dit veld kan een waarde van '0' of '1' hebben." Dit is ingesteld door het model Azure Machine Learning deze is ingericht als onderdeel van de oplossing en geïntegreerd met het realtime pad. De waarde '1' geeft dat een voertuig is onderhoud nodig. 

Een filter **Niveau van de pagina** voor de weergave van gegevens voor voertuigen, waarin onderhoud vereisen toevoegen: 

1. Sleep het veld **'MaintenanceLabel'** naar **Pagina niveau Filters**.  
![Verbonden auto's - pagina niveau Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  

2. Klik op menu **Basisfilteropties** presenteren onderin MaintenanceLabel pagina niveau Filter.  
![Auto's - verbonden Basisfilteropties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  

3.  De filterwaarde ingesteld op **'1'**    
![Verbonden auto's - filterwaarde](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  


Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.  

Selecteer **Gegroepeerd kolomdiagram** in visualisaties  
![Verbonden auto's - Vind Kaartvisualisaties](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Verbonden auto's - gegroepeerd kolomdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Sleep veld **Model** in **as** gebied **chassisnummer** naar **waarde** gebied. Sorteer visualisatie door **het aantal chassisnummer**.  **Titel** van een grafiek wijzigen in **"Voertuigen vereisen onderhoud door model"**  

**Chassisnummer** velden slepen naar **Kleurverzadiging** bijwonen **velden** ![velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) sectie **visualisatie** tabblad  
![Verbonden auto's - kleurverzadiging](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

**Gegevens kleuren** wijzigen in visualisaties van sectie **-indeling**  
Minimum-kleur te wijzigen: **F2C812**  
Maximum kleur om te wijzigen: **FF6300**  
![Auto's - kleurwijzigingen verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Auto's - verbonden nieuwe visualisatie kleuren](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.  

Selecteer **gegroepeerde kolomdiagram** in visualisaties, Sleep **chassisnummer** veld in **waarde** gebied, sleep veld **plaats** in het gebied voor de **as** . Grafiek door **"Aantal vin"**sorteren. **Titel** van een grafiek wijzigen in **"Voertuigen vereisen onderhoud per stad"**   
![Verbonden auto's - voertuigen onderhoud per stad vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.  

Selecteer kaartvisualisatie **Met meerdere rijen** uit visualisaties, **Model** en **chassisnummer** naar het gedeelte **velden** slepen.  
![Verbonden auto's - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Volgorde van alle van de visualisatie, het uiteindelijke rapport ziet er als volgt:  
![Verbonden auto's - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Het "Voertuigen vereisen onderhoud" realtime-rapport is geconfigureerd. U kunt doorgaan met het volgende realtime-rapport maken of hier stoppen en configureren van het dashboard. 

### <a name="3-vehicles-health-statistics"></a>3. voertuigen systeemstatus statistieken
  
Klik op ![toevoegen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) om toe te voegen nieuw rapport, wijzig de naam **"voertuigen systeemstatus statistieken"**  

Selecteer **toelichtingen** visualisatie in visualisaties en sleep het veld **snelheid** in **waarde, waarde bij Minimum, maximumwaarde** gebieden.  
![Verbonden auto's - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Wijzig de standaardaggregatie van **snelheid** in het **gebied van de waarde** in **gemiddelde** 

De standaardaggregatie van **snelheid** in **Minimum gebied** tot een **Minimum** moeten wijzigen

Wijzig de standaardaggregatie van **snelheid** in **Maximum gebied** in **Maximum**

![Verbonden auto's - kaart met meerdere rijen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Wijzig de naam van de **Toelichtingen titel** **"Gemiddelde snelheid"** 
 
![Verbonden auto's - toelichtingen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.  

Op dezelfde manier een **toelichtingen** voor **gemiddelde engine olive oil**, **gemiddelde brandstof**en **gemiddelde engine gematigde klimaatzone**toevoegen.  

Wijzigen de standaardaggregatie van velden in elke toelichtingen als per bovenstaande stappen voor **"Gemiddelde snelheid"** meter.

![Verbonden auto's - meters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.

Selecteer **regel- en gegroepeerd kolomdiagram** in visualisaties, en sleep het veld **plaats** naar **As gedeeld**, Sleep **snelheid**, **tirepressure en engineoil velden** in het waardengebied van **Kolom** , hun type aggregatie wijzigen naar **gemiddelde**. 

Sleep het veld **engineTemperature** naar **Lijn** waardengebied, wijzigt u het aggregatietype met **gemiddelde**. 

![Verbonden auto's - visualisaties velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Wijzig de **titel** van de grafiek in **"gemiddelde snelheid, band druk, engine olive oil en engine temperatuur"**.  

![Verbonden auto's - visualisaties velden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.

Selecteer **Treemap** visualisatie in visualisaties, sleept u het veld **Model** naar het gebied van de **groep** en sleep het veld **MaintenanceProbability** naar het gebied **waarden** .

Wijzig de **titel** van de grafiek in **"Voertuig modellen vereisen onderhoud"**.

![Verbonden auto's - titel van de grafiek wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.

Selecteer **100% gestapeld staafdiagram** in visualisatie, sleept u het veld **city** naar het gebied **as** en sleep de **MaintenanceProbability**, **RecallProbability** velden naar het gebied **waarde** .

![Verbonden auto's - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klik op **Opmaak**, selecteer **Gegevens kleuren**en stelt u de kleur van de **MaintenanceProbability** op de waarde **"F2C80F"**.

Wijzig de **titel** van de grafiek in **' kans van een voertuig onderhoud & intrekken per plaats '**.

![Verbonden auto's - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie.

Selecteer **Vlakdiagram** in visualisatie van visualisaties, sleept u het veld **Model** naar het gebied **as** en sleept u de velden **engineOil, tirepressure, snelheid en MaintenanceProbability** in het gebied **waarden** . De aggregatie-type wijzigen naar **"-animatie-effecten**. 

![Auto's - aggregatie wijzigingstype verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Wijzig de titel van de grafiek in **"Engine olive oil gemiddelde, druk, snelheid en onderhoud kans door model genoeg"**.

![Verbonden auto's - titel van de grafiek wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klik op het lege gebied als u wilt toevoegen van nieuwe visualisatie:

1. Selecteer **Spreidingsdiagram** visualisatie van visualisaties.
2. Sleep het veld **Model** in het gebied **Details** en **legenda** .
3. Sleep het veld **brandstof** naar het gebied van de **x-as** , de aggregatie wijzigen in **gemiddelde**.
4. Sleep vanuit **engineTemparature** in **gebied van de y-as**, wijzigt u de aggregatie met **gemiddelde**
5. Sleep het veld **chassisnummer** naar het gebied **grootte** .


![Verbonden auto's - toevoegen nieuwe visualisatie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Een **titel** voor de grafiek wijzigen in **"Gemiddelden van brandstof, Engine temperatuur door Model"**.

![Verbonden auto's - titel van de grafiek wijzigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Het uiteindelijke rapport eruitziet zoals hieronder wordt weergegeven.

![Auto's definitief rapport verbonden](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Visualisaties van de pincode van de rapporten aan het realtime dashboard
  
Een lege dashboard maken door te klikken op het pluspictogram naast Dashboards. U kunt noem deze "Voertuig Telemetriedashboard-analyses"

![Verbonden auto's-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Pincode de visualisatie van de bovenstaande rapporten aan het dashboard. 
 
![Verbonden auto's-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Het dashboard ziet er als volgt wanneer alle drie lijsten worden gemaakt en de bijbehorende visualisaties zijn vastgemaakt aan het dashboard. Als u geen alle rapporten hebt gemaakt, uw dashboard kan er anders uitzien. 

![Verbonden auto's-Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gefeliciteerd! U hebt het realtime dashboard gemaakt. Terwijl u eraan CarEventGenerator.exe en RealtimeDashboardApp.exe uitvoeren, ziet u updates voor live op het dashboard. Het moet ongeveer 10 tot 15 minuten duren om de volgende stappen te voltooien.

 
##  <a name="setup-power-bi-batch-processing-dashboard"></a>Power BI batch verwerking dashboard instellen

>[AZURE.NOTE] Het duurt ongeveer twee uur (vanuit het afronden van de implementatie) voor de pijplijn complete batchbestand execution heeft voltooid en het verwerken van een jaar zegt gegenereerde gegevens. Wacht dus voor de verwerking om te voltooien voordat u verder gaat met de volgende stappen. 

**De ontwerpfunctie PowerBI-bestand downloaden**

-   Een vooraf geconfigureerde ontwerpfunctie PowerBI-bestand maakt deel uit van de implementatie
-   Klik op het knooppunt PowerBI in de diagramweergave te klikken en klikt u op koppeling in het deelvenster Eigenschappen **het ontwerpfunctie PowerBI-bestand downloaden**![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9.5-download-powerbi-designer.png)

-   Lokaal opslaan

**Rapporten over het PowerBI configureren**

-   Open de ontwerpfunctie 'VehicleTelemetryAnalytics - bureaublad-Report.pbix'-bestand met het PowerBI-bureaubladprogramma. Als u nog, installeert u het bureaublad PowerBI uit [PowerBI bureaublad installeren](http://www.microsoft.com/download/details.aspx?id=45331). 

-   Klik op het **bewerken van query's**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

- Dubbelklik op de **bron**.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

- Server-verbindingsreeks bijwerken met de SQL Azure-server waarop hebt u deze is ingericht als onderdeel van de implementatie. Klik op het knooppunt Azure SQL in het diagram en de naam van de server van het deelvenster Eigenschappen weergeven.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11.5-view-server-name.png)

- Laat **Database** als *connectedcar*.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

- Klik op **OK**.
- Hier ziet u **Windows referentie** tabblad geselecteerd door standaard, wijzigt u deze naar de **Database referenties** door te klikken op het tabblad van de **Database** op rechts.
- Geef de **gebruikersnaam** en **wachtwoord** van uw Azure SQL-Database die is opgegeven tijdens de installatie van de implementatie.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

- Klik op **verbinding maken**
- Herhaal de bovenstaande stappen voor elk van de drie resterende query's bijwonen rechterdeelvenster en klikt u vervolgens de details van gegevensbron verbinding bijwerken.
- Klik op **sluiten en laden**. Power BI Desktop bestand gegevenssets zijn verbonden met de SQL Azure-databasetabellen.
- **Sluiten** Power BI Desktop-bestand.

![](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

- Klik op de knop **Opslaan** als de wijzigingen wilt opslaan. 
 
U hebt nu alle rapporten overeenkomt met het pad van de batch verwerking in de oplossing geconfigureerd. 


## <a name="upload-to-powerbicom"></a>Uploaden naar de *powerbi.com*
 
1.  Navigeer naar de PowerBI-web-portal op http://powerbi.com en u aanmelden.
2.  Klik op **gegevens ophalen**  
3.  De Power BI Desktop-bestand uploaden.  
4.  Als u wilt uploaden, klikt u op **gegevens ophalen -> bestanden ophalen lokaal bestand ->**  
5.  Navigeer naar de **"VehicleTelemetryAnalytics – bureaublad Report.pbix"**  
6.  Nadat het bestand wordt geüpload, wordt terug naar uw Power BI-werkruimte.  

Een gegevensset, rapport en een lege dashboard wordt gemaakt voor u.  
 

Vastzetten grafieken naar bestaande dashboard **Voertuig Analytics Telemetriedashboard** in **Power BI**. Klik op het lege dashboard hiervoor hebt gemaakt en ga vervolgens naar de sectie **rapporten** klikt u op het zojuist geüploade rapport.  

![Voertuig Telemetrielogboek PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 


**Houd rekening met dat het rapport bevat zes pagina's:**  
Pagina 1: Een voertuig dichtheid  
Pagina 2: Realtime voertuig systeemstatus  
Pagina 3: Voertuigen serieus basis van hoeveelheid werk   
Pagina 4: Ingetrokken voertuigen  
Pagina 5: Brandstof efficiënt basis van hoeveelheid werk voertuigen  
Pagina 6: Contoso-Logo  

![Verbonden auto's PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)
 

**Vanaf de pagina-3**, pincode voor de volgende handelingen uit:  

1.  Aantal Chassisnummer  
    ![Verbonden auto's PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 

2.  Serieus voertuigen aangestuurd door model – watervalgrafiek  
    ![Voertuig Telemetrielogboek - pincode grafieken 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Vanaf de pagina-5**, pincode voor de volgende handelingen uit: 
 
1.  Aantal chassisnummer    
    ![Voertuig Telemetrielogboek - pincode grafieken 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2.  Efficiënt voertuigen brandstof door model: gegroepeerd kolomdiagram  
    ![Voertuig Telemetrielogboek - pincode grafieken 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Vanaf de pagina-4**, pincode voor de volgende handelingen uit:  

1.  Aantal chassisnummer  
    ![Voertuig Telemetrielogboek - pincode grafieken 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 

2.  Ingetrokken voertuigen per plaats, model: Treemap  
    ![Voertuig Telemetrielogboek - pincode grafieken 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Vanaf de pagina-6**, pincode voor de volgende handelingen uit:  

1.  Logo van Contoso Motors  
    ![Voertuig Telemetrielogboek - pincode grafieken 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Organiseren van het dashboard**  

1.  Ga naar het dashboard
2.  Plaats de muisaanwijzer op elke grafiek en de naam wijzigen op basis van de naamgeving die beschikbaar zijn in de onderstaande voltooid dashboard-afbeelding. Ook navigeren de grafieken moeten de onderstaande dashboard lijken.  
    ![Voertuig Telemetrielogboek - Dashboard 2 ordenen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
    ![Voertuig Telemetrielogboek PowerBI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3.  Als u kunt alle rapporten hebt gemaakt, zoals in dit document is vermeld, wordt het definitief voltooid dashboard er als de volgende afbeelding. 

![Voertuig Telemetrielogboek - Dashboard 2 ordenen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Gefeliciteerd! U de rapporten hebt gemaakt en het dashboard krijgen realtime, bekijk en inzichten op voertuig gezondheid en sturende batch achterhalen.  
