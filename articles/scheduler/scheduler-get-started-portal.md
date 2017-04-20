<properties
 pageTitle="Aan de slag met Azure Scheduler in Azure-portal | Microsoft Azure"
 description="Aan de slag met Azure Scheduler in Azure-portal"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Aan de slag met Azure Scheduler in Azure-portal

Het is eenvoudig geplande taken maken in Azure Scheduler. In deze zelfstudie leert u hoe een taak wilt maken. Ook leert u de mogelijkheden voor controle en beheer van Scheduler.

## <a name="create-a-job"></a>Een taak maken

1.  Meld u aan bij [Azure-portal](https://portal.azure.com/).  

2.  Klik op **+ Nieuw** > Typ _Taakplanner_ in het zoekvak > **Scheduler** selecteert in zoekresultaten > Klik op **maken**.

     ![][marketplace-create]

3.  Een taak die gewoon http://www.microsoft.com/ met een GET-aanvraag raakt maken. Voer in het scherm **Geplande taak** in de volgende informatie:

    1.  **Naam:**`getmicrosoft`  

    2.  **Abonnement:** Uw Azure-abonnement   

    3.  **Vervullen van de siteverzameling:** Selecteer een bestaande taak verzameling of klik op **Nieuw** > Voer een naam.

4.  Definieer vervolgens de volgende waarden in de **Actie-instellingen**:

    1.  **Actietype:**` HTTP`  

    2.  **Methode:**`GET`  

    3.  **URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Tot slot een schema laten we definiëren. De taak kan worden gedefinieerd als een eenmalige taak, maar we kiest u een schema terugkeerpatroon:

    1. **Terugkeerpatroon**:`Recurring`

    2. **Starten**: de datum van vandaag

    3. **Terugkerende elke**:`12 Hours`

    4. **Eindigt op**: twee dagen na de huidige systeemdatum  

      ![][recurrence-schedule]

6.  Klik op **maken**

## <a name="manage-and-monitor-jobs"></a>Beheren en controleren van taken

Nadat een taak is gemaakt, wordt deze weergegeven in het hoofdvenster Azure dashboard. Klik op de taak en een nieuw venster wordt geopend met de volgende tabbladen:

1.  Eigenschappen  

2.  Actie-instellingen  

3.  Planning  

4.  Geschiedenis

5.  Gebruikers

    ![][job-overview]

### <a name="properties"></a>Eigenschappen

Deze eigenschappen alleen-lezen beschrijven de metagegevens management voor de taak Scheduler.

   ![][job-properties]


### <a name="action-settings"></a>Actie-instellingen

Klikken op een taak in het scherm **taken** , kunt u voor het configureren van die taak. Hiermee kunt u configureren van geavanceerde instellingen, als u niet hebt geconfigureerd dat ze in de wizard snel aan te maken.

Voor alle actietypen, kunt u het beleid opnieuw en de fout actie wijzigen.

Voor de taak actie-typen, HTTP en HTTPS, kunt u de methode wijzigen naar eventuele toegestane HTTP-woord. U kunt ook toevoegen, verwijderen of wijzigen van de kop- en informatie van basisverificatie.

Voor de opslag wachtrij actietypen, kunt u de opslag-account, de wachtrijnaam van de, SA's token en hoofdtekst kan wijzigen.

Voor actie-bustypen met service kan u de naamruimte, onderwerp/wachtrij pad verificatie-instellingen, transporttype, de berichteigenschappen van het en berichttekst wijzigen.

   ![][job-action-settings]

### <a name="schedule"></a>Planning

Hiermee kunt u het schema, configureren als u wijzigen van de planning die u hebt gemaakt wilt in de wizard snel aan te maken.

Dit is een verkoopkans om [complexe planningen en geavanceerde terugkeerpatroon in uw project](scheduler-advanced-complexity.md) te maken

U kunt de begindatum worden gepland wijzigen en tijd, terugkeerpatroon planning en het einde datum en tijd (als de taak terugkerend.)

   ![][job-schedule]


### <a name="history"></a>Geschiedenis

Geselecteerde aan de doelstellingen voor elke taak kan worden uitgevoerd op het tabblad **Geschiedenis** weergegeven in het systeem voor de geselecteerde taak. Deze gegevens bieden realtime waarden voor de status van uw Scheduler:

1.  Status  

2.  Meer informatie  

3.  Herhalingspogingen

4.  Exemplaar: 1e, 2e, 3e, enzovoort.

5.  Begintijd van worden uitgevoerd  

6.  Eindtijd van de uitvoering

   ![][job-history]

U kunt klikken op een uitvoeren voor **Geschiedenis Details**, inclusief het hele antwoord voor elke worden uitgevoerd. Dit dialoogvenster kunt u het antwoord naar het Klembord kopiëren.

   ![][job-history-details]

### <a name="users"></a>Gebruikers

Azure Rolgebaseerd Access besturingselement RBAC () kunt fijnmazige toegangsbeheer voor Azure Scheduler. Informatie over het gebruik van het tabblad gebruikers, raadpleegt u [Azure Role-Based toegangsbeheer](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Zie ook

 [Wat is Scheduler?](scheduler-intro.md)

 [Scheduler concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)

 [Abonnementen en facturering in Azure Scheduler](scheduler-plans-billing.md)

 [Het maken van complexe planningen en geavanceerde terugkeerpatroon met Azure Scheduler](scheduler-advanced-complexity.md)

 [Scheduler REST API verwijzing](https://msdn.microsoft.com/library/mt629143)

 [Overzicht van scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Scheduler hoge beschikbaarheid en betrouwbaarheid](scheduler-high-availability-reliability.md)

 [Limieten voor Scheduler, standaardwaarden en foutcodes](scheduler-limits-defaults-errors.md)

 [Scheduler uitgaande verificatie](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
