<properties
   pageTitle="Het migreren en publiceren van een webtoepassing op een Azure Cloudservice vanuit Visual Studio | Microsoft Azure"
   description="Informatie over het migreren en publiceren van uw webtoepassing op een cloudservice Azure met behulp van Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Hoe u: migreren en publiceren van een webtoepassing op een Azure Cloudservice vanuit Visual Studio

Als u gebruik maken van de hostingservices en schaalbaarheid van Azure maakt, is het raadzaam migreren en publiceren van uw webtoepassing op een Azure cloudservice. U kunt een webtoepassing uitvoeren in Azure wordt aangegeven met minimale wijzigingen aan uw bestaande toepassing.

>[AZURE.NOTE] In dit onderwerp is over het implementeren van met cloudservices, niet op websites. Zie voor informatie over het implementeren naar websites, [Deploy een WebApp in Azure App-Service](./app-service-web/web-sites-deploy.md).

Zie de sectie **Projectsjablonen ondersteund** verderop in dit onderwerp voor een lijst met specifieke sjablonen die worden ondersteund voor zowel Visual C# en Visual Basic.

U moet eerst uw webtoepassing voor Azure van Visual Studio inschakelen. De volgende afbeelding ziet u de belangrijkste stappen voor het publiceren van uw bestaande webtoepassing door toe te voegen een Azure project moet worden gebruikt voor implementatie. Dit proces wordt een Azure project met de Webrol van de vereiste toegevoegd aan uw oplossing. Op basis van het type webproject dat u hebt, worden de Projecteigenschappen voor stroombaan ook bijgewerkt als het servicepakket vereist is voor extra stroombaan voor implementatie.

![Een webtoepassing naar Microsoft Azure publiceren](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] De **converteren**, opdracht **converteren naar Azure Cloud Service Project** wordt alleen weergegeven voor het webproject in uw oplossing. De opdracht is bijvoorbeeld niet beschikbaar voor een Silverlight-project in uw oplossing.
Wanneer u een servicepakket maken of de toepassing Azure publiceert, optreden waarschuwingen of fouten. Deze waarschuwingen en fouten kunt u problemen oplossen voordat u bij Azure implementeert. Bijvoorbeeld, verschijnt er een waarschuwing over een ontbrekende constructie. Zie voor meer informatie over hoe u eventuele waarschuwingen als fouten behandelen [configureren een Azure Cloud Service Project met Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Als u uw toepassing maken, uitvoeren lokaal met de emulator berekenen of uw project naar Azure publiceren, ziet u mogelijk het volgende foutbericht weergegeven in het lijstvenster van **Fout** : **het opgegeven pad, bestandsnaam, of beide te lang zijn**. Deze fout treedt op omdat de lengte van de volledig gekwalificeerde Azure projectnaam te lang is. De lengte van de naam van het project, inclusief het volledige pad, kan niet meer dan 146 tekens. Dit is bijvoorbeeld de naam van het volledige project inclusief bestandspad voor een Azure project dat wordt gemaakt voor een Silverlight-toepassing: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Mogelijk moet u uw oplossing verplaatsen naar een andere map met een korter pad verkleinen van de lengte van de volledig gekwalificeerde naam.

Voer de volgende stappen uit als u wilt migreren en publiceren van een webtoepassing Azure van Visual Studio.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Een webtoepassing inschakelen voor implementatie van Azure

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Een webtoepassing voor implementatie van Azure inschakelen

1. Als u wilt uw webtoepassing voor implementatie van Azure inschakelt, het snelmenu voor een webproject openen in uw oplossing en kies Azure implementatie-Project toevoegen.

    De volgende acties uitgevoerd:

    - Een Azure-project genoemd `<name of the web project>.Azure` wordt toegevoegd aan de oplossing voor uw toepassing.

    - Een Webrol voor het webproject wordt toegevoegd aan dit Azure project.

    - De **Lokale kopie** eigenschap is ingesteld op waar voor elke stroombaan die vereist voor MVC 2, 3, MVC, MVC zijn 4 en Silverlight Business-toepassingen. Hiermee voegt u deze samenstellen naar de servicepakket dat wordt gebruikt voor implementatie.

  >[AZURE.IMPORTANT] Als u andere stroombaan of de bestanden die vereist voor deze webtoepassing zijn hebt, moet u de eigenschappen voor deze bestanden handmatig instellen. Zie de sectie **Bestanden opnemen in de Service-pakket** verderop in dit artikel voor informatie over het instellen van deze eigenschappen.

  >[AZURE.NOTE] Als er al een Webrol voor een specifieke webproject in een Azure project in de oplossing, **converteren bestaat**, wordt **converteren naar Azure Cloud Service Project** niet weergegeven in het snelmenu voor dit webproject.

  Als u meerdere webprojecten in uw webtoepassing hebt en u wilt maken van web rollen voor elk webproject, moet u de stappen in deze procedure voor elk webproject uitvoeren. Hiermee maakt u afzonderlijke Azure projecten voor elke Webrol. Elk webproject kan afzonderlijk worden gepubliceerd. U kunt ook handmatig een andere Webrol toevoegen aan een bestaand Azure project in uw webtoepassing. Hiervoor opent u het snelmenu voor de map **rollen** in uw project Azure en kies **toevoegen**en klik vervolgens **Web rol Project in de oplossing**, kiest u het project toe te voegen als een Webrol en kies vervolgens de knop **OK** .

## <a name="use-an-azure-sql-database-for-your-application"></a>Een Azure SQL-Database gebruiken voor uw toepassing

Als u een verbindingsreeks voor uw webtoepassing die een SQL Server-database die zich op de lokale gebruikt hebt, moet u deze verbindingsreeks gebruik van een exemplaar van SQL-Database die Azure hosts in plaats daarvan.

>[AZURE.IMPORTANT] Uw abonnement, moet u het gebruik van de SQL-Database inschakelen. Als u toegang hebt tot uw abonnement van de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885), kunt u bepalen welke services vindt u uw abonnement. De volgende instructies gelden voor de uitgebrachte [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). Als u de [portal van Azure](http://portal.microsoft.com)gebruikt, gaat u verder met de volgende procedure.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Gebruik van een exemplaar van de SQL-Database in uw Webrol voor de verbindingsreeks

1. Als u wilt een exemplaar van SQL-Database in de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)maken, volgt u de stappen in het volgende artikel: [een SQL-databaseserver maken](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Wanneer u de firewallregels voor uw exemplaar van SQL-Database instellen, moet u het selectievakje **toestaan dat andere Azure services toegang tot deze server** in.

1. Als u een exemplaar van SQL-Database wilt gebruiken voor de verbindingsreeks hebt gemaakt, wilt u de stappen in de volgende sectie in het volgende artikel: [een SQL-Database maken](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Als u wilt kopiëren de ADO.NET-verbindingsreeks wilt gebruiken voor de verbindingsreeks, moet u de volgende stappen uitvoeren in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).  

  1. Kies de knop van de **Database** en open vervolgens het knooppunt voor het abonnement dat u gebruikt om te maken van uw exemplaar van SQL-Database.

  1. Als u wilt weergeven van de beschikbare exemplaren van SQL-Database, kiest u het knooppunt **SQL-Databases** .

  1. Als u wilt weergeven in de eigenschappen voor de database, kies de database. De **Eigenschappen** van weergave wordt weergegeven.

      >[AZURE.NOTE] Als u de **Eigenschappen** van weergave niet ziet, moet u mogelijk ook openen via de scheidingslijn.

  1. Als u wilt weergeven de verbindingstekenreeksen, kiest u de knop weglatingsteken (...) naast de weergave.

    Het dialoogvenster **Verbindingstekenreeksen** wordt weergegeven.

  1. Kopieer de verbindingsreeks ADO.NET, markeer de tekst en kiest u de toetsen Ctrl + C.

  1. Om het dialoogvenster te sluiten, klikt u op de knop **sluiten** .

1. Als u wilt vervangen de verbindingsreeks in het bestand web.config gebruik van dit exemplaar van SQL-Database, open het bestand web.config, markeer de bestaande verbinding tekenreeks invoer en kies vervolgens de toetsen Ctrl + V. De ADO.NET-verbindingsreeks voor het exemplaar van SQL-Database vervangt de bestaande verbindingsreeks.

1. U moet ook de parameter toevoegen `MultipleActiveResultSets=True` naar de verbindingsreeks. De verbindingsreeks moet de volgende indeling:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Optioneel) Een alternatief voor het wijzigen van de verbindingsreeks rechtstreeks in het bestand web.config is het toevoegen van een sectie in een van de bestanden web.config transformatie, afhankelijk van de build-configuratie die u kunt uw-pakket maken. Open het bestand Web.Debug.Config of het bestand Web.Release.Config. De volgende sectie in dit bestand toevoegen:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Sla het bestand dat u hebt gewijzigd en uw toepassing opnieuw publiceren.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Gebruik van een exemplaar van SQL-Database met behulp van de Azure klassieke portal

1. Kies in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), het knooppunt SQL-Databases.

  - Als het exemplaar van SQL-Database die u wilt gebruiken wordt weergegeven, kiest u om dit te openen.

  - Als u alle exemplaren dat nog niet hebt gemaakt, kies de gewenste koppeling en maakt u een exemplaar.

1. Wanneer u open of maak een exemplaar van de database, kiest u de koppeling **Tekenreeksen met de verbinding** .

1. Onder aan de pagina en kies de koppeling naar firewallinstellingen, configureren en accepteer de standaardwaarden of configureren van de waarden die u nodig hebt.

1. Kopieer de verbindingsreeks ADO.NET, plak deze in uw bestand web.config via de oude verbindingsreeks voor de database on-premises implementatie en zorg ervoor dat u om toe te voegen `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Een webtoepassing Azure publiceren

### <a name="to-publish-a-web-application-to-azure"></a>Een webtoepassing Azure publiceren

1. Test de toepassing in de lokale ontwikkeling omgeving met de Azure emulator berekenen, open het snelmenu voor het Azure project voor het Webrol en kies **als opstartproject instellen**. Kies **Foutopsporing**, **Starten foutopsporing** (toetsenbord: **F5**).

    Het dialoogvenster voor het **starten van de omgeving Azure foutopsporing** wordt geopend en de toepassing wel wordt gestart in de browser. Zie de tabel in deze sectie voor meer informatie over het starten van elk type webtoepassing in de emulator berekeningscluster.

1. Als u de services voor uw toepassing publiceren naar Azure instellen, moet u een Microsoft-account en een Azure-abonnement hebben. Voer de stappen in het volgende onderwerp voor het instellen van uw services: [voorbereiden om te publiceren of een Azure-toepassing van Visual Studio implementeren](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Als u wilt publiceren de webtoepassing Azure, opent u het snelmenu voor het webproject en kies **publiceren naar Azure**.

    Het dialoogvenster **Publiceren Azure-toepassing** opent en Visual Studio start het implementatieproces. Zie voor meer informatie over hoe u de toepassing publiceert, het gedeelte van het **publiceren van een toepassing Azure Visual Studio** in [een Cloudservice met de hulpmiddelen Azure publiceren](vs-azure-tools-publishing-a-cloud-service.md).

    >[AZURE.NOTE] U kunt ook de webtoepassing uit het Azure project publiceren. Open het snelmenu voor het project Azure hiervoor en kies **publiceren**.

1. Als u wilt zien van de voortgang van de implementatie, kunt u het venster **Azure activiteitenlogboek** weergeven. Dit logboek wordt automatisch weergegeven wanneer de implementatieproces wordt gestart. U kunt het artikel in het gebeurtenissenlogboek gedetailleerde gegevens uitvouwen zoals wordt weergegeven in de volgende afbeelding:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Optioneel) De implementatieproces wilt annuleren, opent u het snelmenu voor het artikel in het gebeurtenissenlogboek en kies **Annuleren en verwijderen**. Hiermee stopt implementatie en Hiermee verwijdert u de implementatieomgeving van Azure.

    >[AZURE.NOTE] Als u wilt deze implementatieomgeving verwijderen nadat deze is geïmplementeerd, moet u de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Optioneel) Nadat uw rol-sessies hebt gestart, weergegeven Visual Studio de implementatieomgeving automatisch in het knooppunt **Azure berekenen** in de **Cloud Explorer** of **Server Explorer**. Hier kunt u de status van de afzonderlijke rol exemplaren weergeven.

    De volgende afbeelding ziet de rol exemplaren in **Server Explorer** terwijl ze nog steeds in de stand Bezig met initialiseren:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Voor toegang tot uw toepassing na implementatie, klik op de pijl naast uw implementatie wanneer de status **voltooid** wordt weergegeven in het **logboek met Azure activiteit**. Hiermee worden de URL voor uw webtoepassing weergegeven in Azure wordt aangegeven. Zie de volgende tabel voor de informatie over het starten van een bepaalde soort webtoepassing van Azure.

    De volgende tabel bevat de details over het specifieke webtoepassingen starten vanuit Azure of uitvoeren of fouten opsporen in een webtoepassing lokaal met de Emulator Azure berekenen:

  	|Web toepassingstype|Uitvoeren/foutopsporing lokaal met de Emulator berekenen|Uitgevoerd in Azure wordt aangegeven|
  	|---|---|---|
  	|ASP.NET-webtoepassing|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **implementatie** voor het **gebeurtenissenlogboek van Azure** laden van de startpagina in de browser.|
  	|ASP.NET-MVC 2-webtoepassing|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **implementatie** voor het **gebeurtenissenlogboek van Azure** laden van de startpagina in de browser.|
  	|ASP.NET-MVC 3-webtoepassing|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **implementatie** voor het **gebeurtenissenlogboek van Azure** laden van de startpagina in de browser.|
  	|ASP.NET-MVC 4-webtoepassing|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|Kies de URL-hyperlink weergegeven op het tabblad **implementatie** voor het **gebeurtenissenlogboek van Azure** laden van de startpagina in de browser.|
  	|De webtoepassing ASP.NET leegmaken|U moet een ASPX-pagina toevoegen in uw toepassing die u als startpagina voor uw webproject hebt ingesteld. Kies vervolgens **Foutopsporing**, **Starten foutopsporing** op de menubalk (toetsenbord: Kies de toets **F5** .).|Als u een standaard ASPX-pagina in uw toepassing hebt, kiest u de URL-hyperlink weergegeven op het tabblad **implementatie** voor het **logboek met Azure activiteit** en deze pagina wordt geladen in de browser. Als u een ander ASPX-pagina hebt, moet u Ga naar deze specifieke pagina's met behulp van de volgende indeling voor uw url:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-toepassing|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|U nodig hebt om te navigeren naar de specifieke pagina voor uw toepassing met behulp van de volgende indeling voor de url:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-toepassing voor bedrijven|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|U nodig hebt om te navigeren naar de specifieke pagina voor uw toepassing met behulp van de volgende indeling voor de url:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight-toepassing voor navigatie|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|U nodig hebt om te navigeren naar de specifieke pagina voor uw toepassing met behulp van de volgende indeling voor de url:`<url for deployment>/<name of page>.aspx`|
  	|WCF-servicetoepassing|U moet het .svc-bestand als startpagina instellen voor uw project WCF-Service. Kies vervolgens **Foutopsporing**, **Starten foutopsporing** op de menubalk (toetsenbord: Kies de toets **F5** .).|U nodig hebt om te navigeren naar het bestand svc voor uw toepassing met behulp van de volgende indeling voor de url:`<url for deployment>/<name of service file>.svc`|
  	|WCF-werkstroom-servicetoepassing|U moet het .svc-bestand als startpagina instellen voor uw project WCF-Service. Kies vervolgens **Foutopsporing**, **Starten foutopsporing** op de menubalk (toetsenbord: Kies de toets **F5** .).|U nodig hebt om te navigeren naar het bestand svc voor uw toepassing met behulp van de volgende indeling voor de url:`<url for deployment>/<name of service file>.svc`|
  	|ASP.NET-dll entiteiten|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|Moet u de verbindingsreeks bijwerken (Zie de volgende sectie). U moet ook Navigeer naar de specifieke pagina voor uw toepassing met behulp van de volgende indeling voor de url:`<url for deployment>/<name of page>.aspx`|
  	|ASP.NET-dynamische gegevens Linq naar SQL|Kies op de menubalk, **Foutopsporing**, **Starten foutopsporing** (toetsenbord: Kies de toets **F5** .).|U moet de stappen in deze procedure: een SQL Azure-database gebruiken voor uw toepassing (Zie eerdere sectie in dit onderwerp). U moet ook Navigeer naar de specifieke pagina voor uw toepassing met behulp van de volgende indeling voor de url:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Een verbindingsreeks voor ASP.NET dynamisch entiteiten bijwerken

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Een verbindingsreeks voor ASP.NET dynamisch entiteiten bijwerken

1. Als u wilt maken van een SQL Azure-database die kan worden gebruikt voor een webtoepassing ASP.NET dynamische entiteiten, volg de stappen in de procedure **een SQL Azure-database voor uw toepassing gebruiken** eerder in dit onderwerp.

1. Voeg de tabellen en velden die u nodig voor deze database vanaf de [portal van Azure klassieke hebt](http://go.microsoft.com/fwlink/?LinkID=213885).

1. De verbindingsreeks voor dit type toepassing heeft de volgende indeling in het bestand web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    De waarde *connectionString* als volgt met de verbindingsreeks ADO.NET voor de SQL Azure-database bijwerken:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Sla het bestand web.config met de wijzigingen die u hebt aangebracht in de verbindingsreeks in het menu Kies **bestand**, **web.config opslaan**.

## <a name="supported-project-templates"></a>Ondersteunde Project-sjablonen

Als u wilt publiceren een webtoepassing Azure, moet de toepassing een van de projectsjablonen gebruiken voor C# of Visual Basic dat wordt vermeld in de onderstaande tabel.

|Groep van Project-sjabloon|Project-sjabloon|
|---|---|
|Web|ASP.NET-webtoepassing|
|Web|ASP.NET-MVC 2-webtoepassing|
|Web|ASP.NET-MVC 3-webtoepassing|
|Web|ASP.NET-MVC4 webtoepassing.|
|Web|De webtoepassing ASP.NET leegmaken|
|Web|ASP.NET-MVC 2 lege webtoepassing.|
|Web|ASP.NET-dynamische gegevensentiteiten webtoepassing|
|Web|ASP.NET-dynamische gegevens Linq aan SQL webtoepassing.|
|Silverlight|Silverlight-toepassing|
|Silverlight|Silverlight-toepassing voor bedrijven|
|Silverlight|Silverlight-toepassing voor navigatie|
|WCF|WCF-servicetoepassing|
|WCF|WCF-werkstroom-servicetoepassing|
|Werkstroom|WCF-werkstroom-servicetoepassing|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over publiceren, [voorbereiden om te publiceren of distribueren van de toepassing Azure in Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Ook [Instelling omhoog met de naam verificatiereferenties](vs-azure-tools-setting-up-named-authentication-credentials.md)raadplegen.
