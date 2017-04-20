<properties
    pageTitle="Verbinding maken met lokale SQL Server via een WebApp in Azure App-Service van hybride-verbindingen"
    description="Een WebApp maken op Microsoft Azure en verbindt u deze met een lokale SQL Server-database"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="cephalin"/>

# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a>Verbinding maken met lokale SQL Server via een WebApp in Azure App-Service van hybride-verbindingen

Hybride verbindingen kunnen verbinden met [Azure-Service voor App](http://go.microsoft.com/fwlink/?LinkId=529714) -WebApps on-premises implementatie-bronnen die een statische TCP-poorten gebruiken. Ondersteunde resources omvatten Microsoft SQL Server, MySQL, HTTP-Web-API's, App Service en de meeste aangepaste Web-Services.

In deze zelfstudie leert u hoe u een App Service web-app in de [Portal van Azure](http://go.microsoft.com/fwlink/?LinkId=529715)maken, de webtoepassing verbinden met uw lokale lokale SQL Server-database met de nieuwe functie voor hybride verbinding, een eenvoudige ASP.NET-toepassing die u wilt gebruiken van de hybride verbinding en implementeren van de toepassing bij de App Service web-app maken. De voltooide web-app op Azure worden gebruikersreferenties opgeslagen in een database lidmaatschap on-premises. De zelfstudie wordt ervan uitgegaan geen ervaring met Azure of ASP.NET.

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.
>
>Het Web Apps-gedeelte van de functie hybride verbindingen is alleen beschikbaar in de [Portal van Azure](https://portal.azure.com). Een verbinding wilt maken in BizTalk-Services, raadpleegt u [Hybride verbindingen](http://go.microsoft.com/fwlink/p/?LinkID=397274).  

## <a name="prerequisites"></a>Vereisten voor ##

Als u wilt deze zelfstudie hebt voltooid, moet u de volgende producten. Alle zijn beschikbaar in gratis versies, zodat u kunt beginnen met het ontwikkelen van voor Azure volledig voor gratis.

- **Azure abonnement** - voor een gratis abonnement, raadpleegt u [De gratis proefversie Azure](/pricing/free-trial/).

- **Visual Studio-2013** - voor een gratis proefversie van Visual Studio 2013 downloaden, raadpleegt u [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs). Installeer deze voordat u verdergaat.

- **Microsoft .NET Framework 3.5 servicepack 1** - als het besturingssysteem Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7 of Windows Server 2008 R2 is, kunt u dit inschakelen in het Configuratiescherm > programma's en onderdelen > Windows-onderdelen in- of uitschakelen. Anders kunt u deze downloaden vanuit het [Microsoft Downloadcentrum](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).

- **SQL Server 2014 Express met hulpmiddelen** - Microsoft SQL Server Express gratis bekijken op de [pagina van de Microsoft-webdatabase Platform](http://www.microsoft.com/web/platform/database.aspx)downloaden. Kies de versie **Express** (niet LocalDB). De versie **uitdrukken met hulpmiddelen voor** bevat SQL Server Management Studio, die u in deze zelfstudie gebruiken wilt.

- **SQL Server Management Studio Express** - dat deel uitmaakt van de SQL Server 2014 Express met hulpmiddelen voor downloaden hierboven genoemde, maar als u deze afzonderlijk installeren wilt, kunt u downloaden en installeren vanaf de [downloadpagina van SQL Server Express](http://www.microsoft.com/web/platform/database.aspx).

De zelfstudie wordt ervan uitgegaan dat er een Azure-abonnement dat u Visual Studio 2013 hebt geïnstalleerd en u hebt geïnstalleerd of .NET Framework 3.5 ingeschakeld. De zelfstudie ziet u hoe u SQL Server 2014 Express installeren in een configuratie die ook met de functie Azure hybride verbindingen (een standaardexemplaar met een statische TCP-poorten werkt). Voordat u de zelfstudie start, download SQL Server 2014 Express met hulpprogramma's van de locatie die hierboven genoemde als u nog geen SQL Server is geïnstalleerd.

### <a name="notes"></a>Notities ###
Als u wilt gebruiken een lokale SQL Server- of SQL Server Express-database met de verbinding van een hybride, moet TCP/IP worden ingeschakeld voor een statische poort. Standaardexemplaren van SQL Server gebruiken statische poort 1433, terwijl benoemde exemplaren niet doen.

De computer waarop u de on-premises implementatie hybride Connection Manager-agent installeren:

- Moet uitgaande verbinding hebt met Azure via:

Poort|Waarom
---|---
80|**Vereist** voor HTTP-poort voor certificaatverificatie en eventueel ook voor gegevensconnectiviteit.
443|**Optioneel** voor gegevensconnectiviteit. Als uitgaande verbinding met de 443 niet beschikbaar is, wordt TCP-poort 80 gebruikt.
5671 en 9352|**Aanbevolen** maar optioneel voor gegevensconnectiviteit. Opmerking dat deze modus oplevert meestal sneller worden verwerkt. Als uitgaande verbinding met deze poorten niet beschikbaar is, wordt TCP-poort 443 gebruikt.
- Moeten kunnen bereiken van de *hostname*:*poortnummer* van uw on-premises implementatie resource.

De stappen in dit artikel wordt ervan uitgegaan dat u de browser van de computer waarop de on-premises implementatie hybride verbinding-agent worden gebruikt.

Als u al SQL Server in een configuratie en in een omgeving die voldoet aan de voorwaarden die hierboven hebt geïnstalleerd, kunt u overslaan en beginnen met [een SQL Server-database on-premises implementatie maken](#CreateSQLDB).

<a name="InstallSQL"></a>
## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a>A. Installatie van SQL Server Express, TCP/IP inschakelen en on-premises implementatie van een SQL Server-database maken ##

In dit gedeelte ziet u hoe u SQL Server Express installeren, TCP/IP inschakelen en een database maken zodat uw webtoepassing met de Portal Azure werkt.

### <a name="install-sql-server-express"></a>SQL Server Express installeren ###

1. Als wilt installeren SQL Server Express, voert u het bestand **SQLEXPRWT_x64_ENU.exe** of **SQLEXPR_x86_ENU.exe** die u hebt gedownload. De wizard SQL Server-installatie Center wordt weergegeven.

    ![Installatie van SQL Server][SQLServerInstall]

2. Kies **zelfstandige installatie van nieuwe SQL Server of functies toevoegen aan een bestaande installatie**. Volg de instructies, de instellingen, standaardwaarden en accepteren totdat u toegang hebt tot de pagina **Configuratie exemplaar** .

3. Kies op de pagina **Configuratie exemplaar** **standaard exemplaar**.

    ![Kies standaardexemplaar][ChooseDefaultInstance]

    De standaardexemplaar van SQL Server luistert standaard voor aanmeldingsaanvragen van SQL Server-clients op statische poort 1433, namelijk wat de functie hybride verbindingen is vereist. Benoemde exemplaren dynamische poorten en UDP, die niet worden ondersteund door hybride verbindingen gebruiken.

4. Accepteer de standaardinstellingen op de pagina **Configuratie van de Server** .

5. Op de pagina **Configuratie van Database-Engine** onder **Verificatiemodus**, kies **Gemengde modus (SQL Server-verificatie en Windows-verificatie)**en een wachtwoord.

    ![Gemengde modus kiezen][ChooseMixedMode]

    In deze zelfstudie gebruikt u SQL Server-verificatie. Zorg ervoor dat het wachtwoord dat u opgeeft, onthoudt omdat u deze later moet.

6. Stapsgewijs de rest van de wizard om de installatie te voltooien.

### <a name="enable-tcpip"></a>TCP/IP inschakelen ###
Als u wilt inschakelen TCP/IP, gebruikt u SQL Server Configuration Manager, die is geïnstalleerd tijdens de installatie van SQL Server Express. Volg de stappen in [TCP/IP netwerk-Protocol inschakelen voor SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) voordat u verdergaat.

<a name="CreateSQLDB"></a>
### <a name="create-a-sql-server-database-on-premises"></a>On-premises implementatie van een SQL Server-database maken ###

Een webtoepassing Visual Studio vereist een lidmaatschap-database die kan worden geopend door Azure. Hiervoor is vereist een SQL Server- of SQL Server Express-database (niet de LocalDB database waarin de sjabloon MVC al dan niet standaard), zodat u de database lidmaatschap naast wilt maken.

1. In SQL Server Management Studio verbinding met de SQL-Server die u zojuist hebt geïnstalleerd. (Als het **verbinding maken met de Server** dialoogvenster verschijnt niet automatisch, navigeer naar **Object Explorer** in het linkerdeelvenster, op **verbinding maken**en klik vervolgens op **Database-Engine**.)  ![Verbinding maken met de Server][SSMSConnectToServer]

    Kies de **Database-Engine**voor **servertype**. Voor **servernaam**, kunt u **localhost** of de naam van de computer die u gebruikt. Kies **SQL Server-verificatie**en meld u aan met de Software Assurance-gebruikersnaam en het wachtwoord dat u eerder hebt gemaakt.

2. Naar een nieuwe database maken met behulp van SQL Server Management Studio, met de rechtermuisknop op **Databases** in de Verkenner Object en klik vervolgens op **Nieuwe Database**.

    ![Nieuwe database maken][SSMScreateNewDB]

3. Klik in het dialoogvenster **Nieuwe Database** MembershipDB invoeren voor de naam van de database en klik vervolgens op **OK**.

    ![Databasenaam leveren][SSMSprovideDBname]

    Houd er rekening mee dat u niet geen wijzigingen in de database op dit punt aanbrengen volgt. De lidmaatschapsgegevens wordt automatisch later worden toegevoegd door uw webtoepassing wanneer u deze uitvoert.

4. In Object Explorer als u **Databases uitvouwen**, ziet u dat de database lidmaatschap is gemaakt.

    ![MembershipDB die zijn gemaakt][SSMSMembershipDBCreated]

<a name="CreateSite"></a>
## <a name="b-create-a-web-app-in-the-azure-portal"></a>B TE DRUKKEN. Een WebApp maken in de Portal van Azure ##

> [AZURE.NOTE] Als u al een web-app in de Portal van Azure die u wilt gebruiken voor deze zelfstudie hebt gemaakt, kunt u gaat u verder met het [maken van een hybride verbinding en een BizTalk-Service](#CreateHC) en doorgaan daarvandaan.

1. Klik in de [Portal van Azure](https://portal.azure.com)op **Nieuw** > **Web + Mobile** > **WebApp**.

    ![De knop Nieuw][New]

2. Uw web-app configureren en klik vervolgens op **maken**.

    ![De websitenaam van de][WebsiteCreationBlade]

3. Na een paar seconden de WebApp is gemaakt en de web-app blade wordt weergegeven. Het blad is een verticaal schuifbare dashboard waarmee u uw web-app beheren.

    ![Website uitvoeren][WebSiteRunningBlade]

    Om te controleren of dat de WebApp wordt uitgevoerd live, kunt u klikken op het pictogram **Bladeren** om de standaardpagina weer te geven.

U maakt vervolgens een hybride verbinding en een BizTalk-service voor het web-app.

<a name="CreateHC"></a>
## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a>C. De verbinding van een hybride en een Service BizTalk maken ##

1. Terug in de Portal, gaat u naar instellingen en klikt u op **toegang** > **de eindpunten van de verbinding hybride configureren**.

    ![Hybride verbindingen][CreateHCHCIcon]

2. Klik op het blad hybride verbindingen, op **toevoegen** > **nieuwe hybride verbinding**.

3. Klik op het blad **hybride-verbinding maken** :
    - Geef een naam voor de verbinding voor de **naam**.
    - Voer de naam van de computer van de SQL Server-host voor **Hostname**.
    - Voer voor **poort**1433 (de standaardpoort voor SQL Server).
    - Klik op **BizTalk-Service** > **Nieuwe BizTalk-Service** en voer een naam voor de BizTalk-service.

    ![Een hybride verbinding maken][TwinCreateHCBlades]

5. Klik tweemaal op **OK** .

    Wanneer het proces is voltooid, de **meldingen** gebied hebben een groene **SUCCESS** flash en de **verbinding van de hybride** blade hebben de nieuwe hybride verbinding met de status **niet**verbonden weergeven.

    ![Een hybride verbinding is gemaakt][CreateHCOneConnectionCreated]

U hebt nu een belangrijk onderdeel van de cloudinfrastructuur van de hybride-verbinding voltooid. U maakt vervolgens een overeenkomende deel van de on-premises implementatie.

<a name="InstallHCM"></a>
## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a>D TE DRUKKEN. Installeren van de on-premises implementatie hybride Connection Manager om de verbinding te voltooien ##

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

Nu dat de hybride verbinding-infrastructuur voltooid is, maakt u een webtoepassing die, wordt deze gebruikt.

<a name="CreateASPNET"></a>
## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a>E. Maak een eenvoudige ASP.NET webproject, de verbindingsreeks van de database en het project lokaal uitvoeren ##

### <a name="create-a-basic-aspnet-project"></a>Een eenvoudige ASP.NET-project maken ###
1. Visual Studio, in het menu **bestand** , een nieuw Project maken:

    ![Nieuwe Visual Studio-project][HCVSNewProject]

2. In de sectie **sjablonen** van het dialoogvenster **Nieuw Project** **Web** selecteren en kies **ASP.NET-webtoepassing**en klik vervolgens op **OK**.

    ![Kies ASP.NET-webtoepassing][HCVSChooseASPNET]

3. Klik in het dialoogvenster **Nieuw ASP.NET-Project** **MVC**kiezen en klik vervolgens op **OK**.

    ![Kies MVC][HCVSChooseMVC]

4. Als het project is gemaakt, wordt de pagina van het Leesmij-toepassing weergegeven. Voer het webproject geen nog.

    ![Leesmij voor pagina][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a>De verbindingsreeks van de database voor de toepassing bewerken ###

In deze stap geeft bewerken u de verbindingsreeks waarin staat dat uw toepassing waar vind ik de lokale database van SQL Server Express. De verbindingsreeks is in van de toepassing Web.config-bestand configuratiegegevens voor de toepassing bevat.

> [AZURE.NOTE] Om ervoor te zorgen dat uw toepassing gebruikmaakt van de database die u hebt gemaakt in SQL Server Express en niet de dia in Visual Studio standaard LocalDB, is het belangrijk dat u deze stap uitvoeren voordat u uw project.

1. Dubbelklik op het bestand Web.config in Solution Explorer.

    ![Web.config][HCVSChooseWebConfig]

2. Bewerk de sectie **connectionStrings** zodat deze verwijzen naar de SQL Server-database op uw lokale computer, na de syntaxis in het volgende voorbeeld:

    ![Verbindingsreeks][HCVSConnectionString]

    Houd tijdens het opstellen van de verbindingsreeks, rekening met het volgende:

    - Als u verbinding maakt met een benoemde in plaats van een standaardexemplaar (bijvoorbeeld YourServer\SQLEXPRESS), moet u uw SQL-Server voor het gebruik van statische poorten configureren. Zie voor informatie over het configureren van statische poorten, [het configureren van SQL Server op een specifieke poort luistert](http://support.microsoft.com/kb/823938). Standaard gebruik benoemde exemplaren UDP en dynamische poorten, die niet worden ondersteund door hybride verbindingen.

    - Het wordt aanbevolen dat u de poort opgeven (1433 al dan niet standaard, zoals wordt weergegeven in het voorbeeld) op de verbindingsreeks zodat u kunt er zeker van dat uw lokale SQL Server heeft TCP ingeschakeld en de juiste poort wordt gebruikt.

    - Onthoud dat met SQL Server-verificatie verbinding kunt maken, de gebruikers-ID en wachtwoord opgeven in de verbindingstekenreeks.

3. Klik op **Opslaan** in Visual Studio het Web.config-bestand wilt opslaan.

### <a name="run-the-project-locally-and-register-a-new-user"></a>Het project lokaal uitvoeren en een nieuwe gebruiker registreren ###

1. Nu uw nieuwe webproject lokaal uitvoeren door te klikken op de bladerknop onder foutopsporing. In dit voorbeeld wordt gebruikt dat Internet Explorer.

    ![Project uitvoeren][HCVSRunProject]

2. Kies in de rechterbovenhoek van de standaard-webpagina, **registreren** registreren van een nieuw account:

    ![Een nieuw account registreren][HCVSRegisterLocally]

3. Voer een gebruikersnaam en wachtwoord:

    ![Voer de gebruikersnaam en wachtwoord][HCVSCreateNewAccount]

    Hiermee wordt automatisch een database op uw lokale SQL Server waarin de lidmaatschapsgegevens voor uw toepassing gemaakt. Een van de tabellen (**dbo. AspNetUsers**) wachtruimten web app-gebruikersreferenties zoals de bouwstenen die u zojuist hebt ingevoerd. In deze tabel ziet u verderop in deze zelfstudie.

4. Sluit het browservenster van de standaard-webpagina. Hierdoor wordt de toepassing in Visual Studio voorkomen.

U bent nu klaar voor de volgende stap, dat wil zeggen de toepassing Azure publiceren en test deze.

<a name="PubNTest"></a>
## <a name="f-publish-the-web-application-to-azure-and-test-it"></a>F. De webtoepassing Azure publiceren en testen ##

Nu u bij uw App Service WebApp-uw toepassing publiceren en testen om te zien hoe de hybride verbinding die u eerder hebt geconfigureerd om verbinding maken met uw web-app in de database op uw lokale computer wordt gebruikt.

### <a name="publish-the-web-application"></a>De webtoepassing publiceren ###

1. U kunt uw publicatie-profiel voor de App Service-web-app in de Portal Azure downloaden. Klik op het blad voor de WebApp, klikt u op **Get profiel publiceren**en sla het bestand op uw computer.

    ![Download profiel publiceren][PortalDownloadPublishProfile]

    Vervolgens wordt u dit bestand importeren in uw Visual Studio-webtoepassing.

2. Visual Studio, met de rechtermuisknop op de naam van het project in Oplossingverkenner en selecteer **publiceren**.

    ![Selecteer publiceren][HCVSRightClickProjectSelectPublish]

3. Kies in het dialoogvenster **Web publiceren** op het tabblad **profiel** **importeren**.

    ![Importeren][HCVSPublishWebDialogImport]

4. Blader naar uw profiel voor gedownloade publiceren, selecteert u deze en klik vervolgens op **OK**.

    ![Blader naar het profiel][HCVSBrowseToImportPubProfile]

5. Uw publicatie gegevens zijn geïmporteerd en moet worden weergegeven op het tabblad **verbinding** van het dialoogvenster.

    ![Klik op publiceren][HCVSClickPublish]

    Klik op **publiceren**.

    Wanneer de publicatie is voltooid, wordt uw browser starten en weergeven van uw nu vertrouwde ASP.NET-toepassing--, behalve dat dit nu live in de cloud Azure is.

Vervolgens gebruikt u uw live webtoepassing om de verbinding hybride in actie weer te geven.

### <a name="test-the-completed-web-application-on-azure"></a>De voltooide webtoepassing op Azure testen ###

1. Klik op de bovenkant rechts van uw webpagina op Azure, kiest u op **aanmelden**.

    ![Test aanmelden][HCTestLogIn]

2. Uw App Service web-app is nu verbonden met uw webtoepassing lidmaatschap database op uw lokale computer. U kunt dit controleren, meld u aan met dezelfde referenties die u eerder in de lokale database hebt ingevoerd.

    ![Hallo begroeting][HCTestHelloContoso]

3. Verder uw nieuwe hybride om verbinding te testen, zich afmeldt bij uw Azure webtoepassing en registreren als een andere gebruiker. Een nieuwe gebruikersnaam en wachtwoord en klik vervolgens op **registreren**.

    ![Test registreren van een andere gebruiker][HCTestRegisterRelecloud]

4. Om te bevestigen dat de referenties van de nieuwe gebruiker zijn opgeslagen in uw lokale database via de mobiele verbinding voor hybride, opent u SQL Management Studio op uw lokale computer. Vouw de database **MembershipDB** in Object Explorer en vouwt u **tabellen**. Met de rechtermuisknop op de dbo **. AspNetUsers** lidmaatschap van een tabel en kies **rijen boven 1000 selecteert u** de resultaten wilt weergeven.

    ![De resultaten bekijken][HCTestSSMSTree]

5. Uw lidmaatschap van lokale-tabel ziet u nu beide accounts: de presentatie die u lokaal hebt gemaakt en die u hebt gemaakt in de cloud Azure. Die u hebt gemaakt in de cloud is opgeslagen op uw lokale database via Azure van hybride verbinding functie.

    ![Geregistreerde gebruikers in on-premises implementatie-database][HCTestShowMemberDb]

U hebt nu gemaakt en een ASP.NET-webtoepassing die gebruikmaakt van een hybride verbinding tussen een web-app in de cloud Azure en een lokale SQL Server-database geïmplementeerd. Gefeliciteerd!

## <a name="see-also"></a>Zie ook ##
[Overzicht van de hybride-gegevensverbindingen](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist maakt u kennis met hybride verbindingen (video kanaal 9)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[Overzicht van de hybride-gegevensverbindingen](/services/biztalk-services/)

[BizTalk Services: De tabbladen Dashboard, Monitor schaal, configureren en hybride verbinding](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[Samenstellen van een echte hybride Cloud met naadloze toepassing overdraagbaarheid (video kanaal 9)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[Toegang tot lokale bronnen hybride verbindingen met Azure App-Service](../app-service-web/web-sites-hybrid-connection-get-started.md)

[Overzicht van ASP.NET-identiteit](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
