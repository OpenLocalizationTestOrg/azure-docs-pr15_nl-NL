<properties 
    pageTitle="Bewaken en Azure gegevens Factory pijpleidingen beheren" 
    description="Leer hoe u via controle en beheer App bewaken en Azure gegevens factory's en pijpleidingen beheren." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Bewaken en Azure gegevens Factory pijpleidingen met nieuwe controle en beheer App beheren
> [AZURE.SELECTOR]
- [Via Azure Portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
- [Gebruik van de cmdlets voor controle en beheer-App](data-factory-monitor-manage-app.md)

In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in uw pijpleidingen en waarschuwingen maken voor een melding ontvangen op fouten bij het gebruik van de **controle- en de App Management**. U kunt ook de volgende video voor meer informatie over het gebruik van de controle en beheer App bekijken.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Starten van de cmdlets voor controle en beheer App een
Klik op **controle & beheren** tegel op het blad **Gegevens FACTORY** voor uw gegevens factory als u wilt starten de Monitor en de App Management.

![Cmdlets voor controle tegel op Data Factory-startpagina](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Ziet u dat de controle- en de App Management gestart in een afzonderlijk tabbladvenster.  

![Cmdlets voor controle en beheer-App](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Als u ziet dat de webbrowser blijft hangen bij "Machtigen...", **cookies van derden blokkeren en sitegegevens** instelling uitschakelen/Schakel (of) behouden ingeschakeld en een uitzondering voor **login.microsoftonline.com** maken en probeer het opnieuw starten van de app.


Als u windows van de activiteit in de lijst onder niet ziet, klikt u op de knop **vernieuwen** op de werkbalk om de lijst te vernieuwen. Bovendien de juiste waarden voor de **Begintijd** en **eindtijd** filters instellen.  


## <a name="understanding-the-monitoring-and-management-app"></a>Informatie over de cmdlets voor controle en beheer-App
Er zijn drie tabbladen (**Resource Explorer**, **Monitoring weergaven**en **waarschuwingen**) aan de linkerkant en het eerste tabblad (Resource Explorer) is standaard ingeschakeld. 

### <a name="resource-explorer"></a>Resource Explorer
U ziet het volgende: 

- Resource Explorer **structuurweergave** in het linkerdeelvenster.
- **Diagramweergave** aan de bovenkant.
- De lijst van de **Activiteit Windows** onderaan in het middelste deelvenster.
- **Eigenschappen**/**Activiteit venster Explorer** tabbladen in het rechterdeelvenster. 

In Resource Explorer ziet u alle resources (pijpleidingen, gegevenssets, gekoppelde services) in de fabriek gegevens in een structuurweergave. Wanneer u een object in Resource Explorer selecteert, ziet u het volgende: 

- gekoppelde gegevens Factory entiteit is in de diagramweergave gemarkeerd.
- dat is gekoppeld activiteit windows (Klik op [hier](data-factory-scheduling-and-execution.md) voor meer informatie over windows activiteit) zijn gemarkeerd in de lijst activiteit Windows onderaan.  
- Eigenschappen van het geselecteerde object in het venster Eigenschappen in het rechterdeelvenster. 
- De definitie van de JSON van het geselecteerde object indien van toepassing. Bijvoorbeeld: een gekoppelde service of een gegevensset of een pijplijn. 

![Resource Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Zie [plannen en uitvoeren van](data-factory-scheduling-and-execution.md) een artikel voor gedetailleerde informatie over activiteitvenster. 

### <a name="diagram-view"></a>Diagramweergave
De diagramweergave van een fabriek gegevens biedt een door één venster bewaken en de gegevens fabriek en de activa beheren. Wanneer u een Data Factory-entiteit (gegevensset/verkooppijplijn) in de diagramweergave selecteert, ziet u het volgende:
 
- de factory-entiteit van gegevens is geselecteerd in de boomstructuur
- bijbehorende activiteit windows zijn gemarkeerd in de lijst activiteit Windows.
- Eigenschappen van het geselecteerde object in het venster Eigenschappen

Wanneer de pijplijn is ingeschakeld (niet in een onderbroken), wordt aangegeven met een groene lijn. 

![Verkooppijplijn uitgevoerd](./media/data-factory-monitor-manage-app/PipelineRunning.png)

U ziet dat er drie opdrachtknoppen voor de pijplijn in de diagramweergave. U kunt de tweede knop onderbreken van de pijplijn. Onderbreking niet de actieve activiteiten beëindigen en doorgaan tot voltooiing laten. Derde knop onderbreekt u de verkooppijplijn en beëindigd de bestaande activiteiten uitvoeren. Eerste knop hervat de pijplijn. Wanneer uw verkooppijplijn is onderbroken, ziet u de kleur wijzigen voor de pijplijn tegel als volgt.

![Een pauze invoegen/cv op de tegel](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

U kunt meerdere items selecteren twee of meer pijpleidingen (via CTRL) en gebruik de opdracht balk knoppen te onderbreken/hervatten meerdere pijpleidingen tegelijk.

![Een pauze invoegen/cv op opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

U kunt alle activiteiten in de pijplijn, weergeven door met de rechtermuisknop op de tegel verzend-of ontvangsttraject en te klikken op **openen verkooppijplijn**.

![Menu openen verkooppijplijn](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

In de weergave van geopende verkooppijplijn ziet u alle activiteiten in de pijplijn. In dit voorbeeld is er slechts één activiteit: kopie activiteit. Als u terug naar de vorige weergave, klikt u op gegevens factory-naam in het menu breadcrumb aan de bovenkant.

![Geopende verkooppijplijn](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

In de weergave verkooppijplijn wanneer u klikt op een gegevensset uitvoer of wanneer u de muisaanwijzer over de gegevensset uitvoer, ziet u de activiteit Windows pop-upvenster voor deze gegevensset.

![Pop-upvenster activiteit](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

U kunt klikken op een activiteitvenster om details te bekijken voor deze in **het eigenschappenvenster in het rechterdeelvenster** . 

![Eigenschappen van de activiteit-venster](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Overschakelen naar het tabblad van de **Activiteit venster Explorer** meer details kunnen zien in het rechterdeelvenster.

![Activiteit venster Verkenner](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

U kunt ook zien **opgelost variabelen** voor elke activiteit uitvoeren in de sectie **pogingen** proberen. 

![Opgelost variabelen](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Ga naar het tabblad **Script** om de JSON-script-definitie voor het geselecteerde object weer te geven.   

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

U kunt de activiteit windows op drie plaatsen zien:

- Activiteit Windows pop-upvenster in de diagramweergave (middelste deelvenster).
- Activiteit venster Verkenner in het rechterdeelvenster.
- Activiteit Windows lijst in het onderste deelvenster.

In de activiteit Windows pop- en activiteit venster Explorer, kunt u naar de vorige week en volgende week, met pijlen naar rechts en schuift.

![Activiteit venster Explorer links/rechts pijlen](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Onderaan in de diagramweergave te klikken ziet u de knoppen om te zoomen inzoomen, uitzoomen, zodat deze past, 100% in-en uitzoomen, indeling vergrendelen. De knop van de indeling vergrendelen Hiermee voorkomt u dat u per ongeluk tabellen en pijpleidingen in de diagramweergave en is standaard ingeschakeld. U kunt deze optie uitschakelt en entiteiten navigeren in het diagram. Wanneer u deze uitschakelen, kunt u de laatste knop automatisch geplaatst tabellen en pijpleidingen. U kunt ook inzoomen op / uitzoomen muiswieltje gebruiken.

![Opdrachten voor diagram weergave-en uitzoomen](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Lijst met Windows
De activiteit in windows in de linkerbenedenhoek van het middelste deelvenster weergegeven alle vensters van de activiteit voor de gegevensset die u hebt geselecteerd in de resource explorer of diagramweergave. Standaard is de lijst in de aflopende volgorde, wat betekent dat er het meest recente activiteitvenster aan de bovenkant. 

![Lijst met Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Deze lijst niet automatisch vernieuwen, dus de knop Vernieuwen op de werkbalk te handmatig vernieuwen gebruiken.  


De vensters activiteit zijn in een van de volgende statussen:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Beschrijving</th>
</tr>
<tr>
    <td rowspan="8">In afwachting van</td><td>ScheduleTime</td><td>De tijd is niet komen voor het activiteitvenster om uit te voeren.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>De boven afhankelijkheden bent niet klaar.</td>
</tr>
<tr>
<td>ComputeResources</td><td>De bronnen berekeningscluster zijn niet beschikbaar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle exemplaren van de activiteit zijn bezet waarop andere activiteit windows wordt uitgevoerd.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Activiteit is onderbroken en de vensters activiteit kan niet worden uitgevoerd totdat deze wordt hervat.</td>
</tr>
<tr>
<td>Probeer het opnieuw</td><td>Uitvoering van de activiteit opnieuw wordt gestart.</td>
</tr>
<tr>
<td>Gegevensvalidatie</td><td>Gegevensvalidatie nog niet is gestart.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Wachten op de validatie naar een nieuwe poging gedaan.</td>
</tr>
<tr>
<tr
<td rowspan="2">InProgress</td><td>Valideren</td><td>Validatie in voortgang.</td>
</tr>
<td></td>
<td>Het activiteitvenster wordt verwerkt.</td>
</tr>
<tr>
<td rowspan="4">Is mislukt</td><td>Time-out</td><td>Uitvoering duurt langer dan die is toegestaan door de activiteit.</td>
</tr>
<tr>
<td>Geannuleerd</td><td>Geannuleerd door een gebruiker.</td>
</tr>
<tr>
<td>Gegevensvalidatie</td><td>Validatie is mislukt.</td>
</tr>
<tr>
<td></td><td>Genereren en/of het activiteitvenster valideren is mislukt.</td>
</tr>
<td>Gereed</td><td></td><td>Het activiteitvenster is gereed voor gebruik.</td>
</tr>
<tr>
<td>Overgeslagen</td><td></td><td>Het activiteitvenster wordt niet verwerkt.</td>
</tr>
<tr>
<td>Geen</td><td></td><td>Het venster van een activiteit die gebruikt bestaan met een andere status, maar is opnieuw ingesteld.</td>
</tr>
</table>


Wanneer u een venster van de activiteit in de lijst klikt, ziet u informatie erover in **Eigenschappen** of **Activiteit Windows Verkenner** -venster aan de rechterkant.

![Activiteit venster Verkenner](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Activiteit windows vernieuwen  
De gegevens worden niet automatisch vernieuwd, zodat u de **vernieuwen gebruiken** (de tweede knop) naar de knop op de opdrachtbalk handmatig vernieuwen van de activiteit in windows-lijst.  
 

### <a name="properties-window"></a>Eigenschappenvenster
Het venster Eigenschappen is in het deelvenster uiterst rechts van de app controle en beheer. 

![Eigenschappenvenster](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Eigenschappen van het item dat u hebt geselecteerd in de resource explorer (structuurweergave) (of) diagram weergave (of) activiteit windows lijst wordt weergegeven. 

### <a name="activity-window-explorer"></a>Activiteit venster Verkenner

De **Activiteit venster Verkenner** -venster is in het deelvenster uiterst rechts van de controle- en de App Management. Meer informatie over het activiteitvenster die u hebt geselecteerd in de activiteit Windows pop- of activiteit Windows-lijst wordt weergegeven. 

![Activiteit venster Verkenner](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

U kunt overstappen naar een ander activiteitvenster door erop te klikken in de agendaweergave aan de bovenkant. U kunt ook de **pijl-links**/**pijl-rechts** knoppen boven om activiteit windows van de vorige/volgende week weer te geven.

U kunt de knoppen op de werkbalk in het onderste deelvenster **opnieuw** het activiteitvenster of **vernieuwen** gebruik van de details in het deelvenster. 

### <a name="script"></a>Script 
U kunt het tabblad **Script** om weer te geven van de JSON-definitie van de geselecteerde gegevens Factory-entiteit (gekoppelde service, gegevensset en verkooppijplijn). 

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Systeemweergaven gebruiken
De controle- en de App Management bevat vooraf gedefinieerde systeemweergaven (**recente activiteit windows**, **mislukt activiteit windows**, **Windows van de activiteit In uitvoering**) waarmee u recente/mislukt/in uitvoering activiteit vensters voor uw gegevens factory weergeven. 

Overschakelen naar het tabblad **Controle weergaven** aan de linkerkant door erop te klikken. 

![Tabblad weergaven bewaken](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Er zijn drie systeemweergaven worden ondersteund. Selecteer een optie om te zien recente activiteit windows (of) mislukte activiteit windows (of) in uitvoering activiteit windows in de lijst activiteit Windows (onderaan in het middelste deelvenster). 

Wanneer u de optie **recente activiteit windows** selecteert, ziet u alle vensters van recente activiteit in de aflopende volgorde van de **laatste poging tijd**. 

U kunt de weergave **mislukt activiteit windows** gebruiken om alle vensters van mislukte activiteit in de lijst weer te geven. Selecteer een mislukte activiteitvenster in de lijst met informatie over deze in de **Eigenschappen van** venster (of) **Activiteit venster Explorer**wilt weergeven. U kunt ook alle logboeken voor een mislukte activiteitvenster downloaden. 


## <a name="sorting-and-filtering-activity-windows"></a>Sorteren en filteren van de activiteit in windows
De **Begintijd** en **eindtijd** instellingen in de opdrachtenbalk filter activiteit windows wijzigen. Nadat u wijzigt de begintijd en eindtijd, klikt u op de knop naast eindtijd de activiteit in Windows-lijst te vernieuwen.

![Begin- en eindtijden](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Allen tijde zijn momenteel in UTC-indeling in de controle en beheer-App. 

Klik op de naam van een kolom in de **lijst met activiteit Windows**(bijvoorbeeld: Status). 

![Snelmenu van de kolom activiteit Windows-lijst](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

U kunt het volgende doen:

- Sorteren in oplopende volgorde.
- Sorteren in aflopende volgorde.
- Filteren op een of meer waarden (klaar, wachten, enz.)

Wanneer u een filter op een kolom opgeeft, ziet u de filterknop is ingeschakeld voor die kolom om aan te geven dat de waarden in de kolom gefilterde waarden zijn. 

![Filteren in de kolom van activiteit Windows-lijst](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

U kunt het dezelfde pop-upvenster filters wissen. Schakel alle filters voor de activiteit in windows-lijst, klikt u op de knop filter wissen op de balk met opdrachten. 

![Alle filters in de lijst activiteit Windows wissen](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Batch acties uitvoeren

### <a name="rerun-selected-activity-windows"></a>Geselecteerde activiteit windows opnieuw uit te voeren
Selecteer een activiteitvenster, klikt u op de pijl-omlaag voor de eerste knop en selecteer **opnieuw uitvoeren** / **opnieuw uit te voeren met stroom in verkooppijplijn**. Als u **opnieuw uitvoeren met stroom in verkooppijplijn** optie selecteert, wordt deze opnieuw uitgevoerd alle vensters van boven activiteit ook. 
    ![Een activiteitvenster opnieuw uit te voeren](./media/data-factory-monitor-manage-app/ReRunSlice.png)

U kunt ook meerdere vensters van de activiteit in de lijst selecteren en deze opnieuw uit te voeren op hetzelfde moment. U kunt filteren activiteit windows op basis van de status (bijvoorbeeld: **mislukt**) en voer de vensters mislukte activiteit los het probleem waardoor de vensters activiteit mislukt. Zie de volgende sectie voor meer informatie over het filteren van de activiteit in windows in de lijst.  

### <a name="pauseresume-multiple-pipelines"></a>Meerdere pijpleidingen onderbreken/hervatten
U kunt meerdere items selecteren twee of meer pijpleidingen (via CTRL) en gebruik de opdracht balk knoppen (gemarkeerd rode rechthoek in de volgende afbeelding) te onderbreken/hervatten ze tegelijk.

![Schorten/cv op opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Waarschuwingen maken 
De pagina waarschuwingen kunt u een melding maakt, bestaande waarschuwingen weergeven/bewerken/verwijderen. U kunt ook uitschakelen/inschakelen een melding. Als u wilt zien van de pagina waarschuwingen, klikt u op het tabblad waarschuwingen.

![Tabblad waarschuwingen](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Een waarschuwing maken

1. Klik op **Melding toevoegen** als u wilt toevoegen van een melding. U ziet de pagina Details. 

    ![Waarschuwingen - detailpagina maken](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Geef de **naam** en **Beschrijving** voor de waarschuwing en klik op **volgende**. Hier ziet u de pagina **Filters** .

    ![Waarschuwingen - pagina Filters maken](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Selecteer de **gebeurtenis**, **status**en **substatus** (optioneel) waarop u wilt dat de gegevens Factory-service om u te waarschuwen en klik op **volgende**. Hier ziet u de pagina van de **geadresseerden** .

    ![Waarschuwingen - geadresseerden pagina maken](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. Selecteer de optie **e-abonnement beheerders** en/of **de beheerder van de extra e-mail**, en klik op **Voltooien**. Hier ziet u de melding in de lijst. 
    
    ![Lijst met meldingen](./media/data-factory-monitor-manage-app/AlertsList.png)

Gebruik de knoppen die is gekoppeld aan de melding naar bewerken/verwijderen/uitschakelen/inschakelen een melding in de lijst waarschuwingen. 

### <a name="eventstatussubstatus"></a>Status-gebeurtenis/substatus
De volgende tabel vindt de lijst met beschikbare gebeurtenissen en statussen (en substatus).

De gebeurtenisnaam van de | Status | Substatus
-------------- | ------ | ----------
Activiteiten uitvoeren gestart | De slag | Starten
Activiteit klaar uitvoeren | Is geslaagd | Is geslaagd 
Activiteit klaar uitvoeren | Is mislukt| Mislukte resourcetoewijzing<br/><br/>Mislukte worden uitgevoerd<br/><br/>Time-out<br/><br/>De validatie is mislukt<br/><br/>Afgebroken
Op aanvraag HDI Cluster gestart maken | De slag | &nbsp; |
Op aanvraag HDI Cluster gemaakt | Is geslaagd | &nbsp; |
Op aanvraag HDI Cluster verwijderd | Is geslaagd | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Om te bewerken of verwijderen/uitschakelen een waarschuwing


![Knoppen voor meldingen](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


