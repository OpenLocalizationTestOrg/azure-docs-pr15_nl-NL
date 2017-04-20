<properties 
    pageTitle="Een app in Azure herstellen" 
    description="Informatie over het herstellen van uw app uit een back-up." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Een app in Azure herstellen

In dit artikel leest u hoe u herstelt een app in [Azure App Service](../app-service/app-service-value-prop-what-is.md) die u eerder reservekopie (Zie [Back-up van uw app in Azure wordt aangegeven](web-sites-backup.md)) hebt gemaakt. U kunt uw app met de gekoppelde databases (SQL-Database of MySQL) op aanvraag herstellen naar een vorige status of maak een nieuwe app op basis van een van de back-up van uw oorspronkelijke-app. Maken van een nieuwe app die wordt uitgevoerd naar de nieuwste versie parallel zijn handig voor A / B testen.

Terugzetten vanuit de back-ups is beschikbaar voor apps in **Standard** en **Premium** laag uitgevoerd. Voor informatie over de schaalbaarheid van uw app, raadpleegt u de [schaal van een app in Azure wordt aangegeven](web-sites-scale.md). **Premium** laag kunt een groter aantal dagelijkse back-ups moet worden uitgevoerd dan **standaard** laag.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Een app herstellen uit een bestaande back-up

1. Klik op het blad **Instellingen** van uw app in de Portal Azure, op **back-ups** om het **back-ups** blad weer te geven. Klik vervolgens op **Nu herstellen** in de opdrachtenbalk. 
    
    ![Kies nu herstellen][ChooseRestoreNow]

3. Klik in het blad **herstellen** , selecteert u eerst de back-bron. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    De optie **back-up App** ziet u alle de bestaande back-ups van de huidige app en u deze eenvoudig kunt selecteren. 
    De optie **opslag** kunt u een back-zipbestand selecteren in een bestaande opslag van Azure-account en container in uw abonnement. 
    Als u probeert een back-up van een andere app herstellen, gebruikt u de optie **opslag** .

4. Geef vervolgens de bestemming voor de app-terugzetten in de **bestemming herstellen**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Als u **overschrijven**kiest, worden alle bestaande gegevens in uw huidige app gewist. Controleer voordat u op **OK**hebt geklikt, of dat het precies is wat u wilt doen.
    
    U kunt **Bestaande App** om te zetten van de app back-up naar een andere app in dezelfde herverdelen groep selecteren. Voordat u deze optie gebruikt, moet hebt u al een andere app gemaakt in de resourcegroep met de configuratie van de database naar een gedefinieerd in de app back-up spiegelen. 
    
5. Klik op **OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Downloaden of een back-up van een account opslagruimte verwijderen
    
1. Vanuit het hoofdvenster **Blader** blad van de Portal Azure **opslag accounts**selecteren.
    
    Een lijst met uw bestaande opslag-accounts worden, weergegeven. 
    
2. Selecteer het account opslagruimte met de back-up die u wilt downloaden of verwijderen.
    
    Het blad voor de opslag-account worden, weergegeven.

3. Selecteer in het blad opslag accountn, de gewenste container
    
    ![Weergavecontainers][ViewContainers]

4. Selecteer back-bestand dat u wilt downloaden of verwijderen.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Klik op **downloaden** of **verwijderen** afhankelijk van wat u wilt doen.  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Monitor met een bewerking voor terugzetten
    
1. Ga naar het **Controlelogboek** blad in de portal van Azure voor details over het slagen of mislukken van het app-terugzetten. 
    
    Het blad **controlelogboeken bijhouden** worden al uw activiteiten, samen met niveau, status, resources en details van de tijd weergegeven.
    
2. Schuif omlaag naar het zoeken naar de gewenste bewerking voor terugzetten en klik op om deze te selecteren.

Het blad details weergegeven de beschikbare gegevens betrekking hebben op het terugzetten.
    
## <a name="next-steps"></a>Volgende stappen

U kunt ook back-up maken en terugzetten App Service-apps met REST API (Zie [Gebruik REST aan een back-up en herstellen van App Service apps](websites-csm-backup.md)).

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
