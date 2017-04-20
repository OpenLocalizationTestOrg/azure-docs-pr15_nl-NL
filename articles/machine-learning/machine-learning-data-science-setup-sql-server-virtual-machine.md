<properties
    pageTitle="Een SQL Server-VM als een server IPython notitieblok instellen | Microsoft Azure"
    description="Instellen van een virtuele Machine met SQL Server en IPython van gegevens wetenschappelijk."
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
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Een SQL-Server Azure virtuele machine instellen als een server IPython notitieblok voor geavanceerde analyses

In dit onderwerp ziet hoe u inrichten en configureren van een SQL Server-VM moet worden gebruikt als onderdeel van een cloudgebaseerde gegevens wetenschappelijke-omgeving. Het virtuele Windows-computer is geconfigureerd met ondersteunende hulpprogramma's zoals IPython notitieblok, Azure opslag Explorer en AzCopy, evenals andere hulpprogramma's die handig om gegevens wetenschappelijke projecten zijn. Azure opslag Explorer en AzCopy, bijvoorbeeld handige manieren om gegevens te uploaden naar Azure-blobopslag van uw lokale computer of om deze te downloaden naar uw lokale computer uit blobopslag te bieden.

De galerie met Azure virtuele machines bevat een aantal afbeeldingen met Microsoft SQL Server. Selecteer de afbeelding van een SQL Server VM die geschikt is voor de gegevensbehoeften van uw. Aanbevolen afbeeldingen zijn:

- SQL Server 2012 SP2 Enterprise voor kleine en middelgrote gegevens grootte
- SQL Server 2012 SP2 Enterprise geoptimaliseerd voor DataWarehousing werkbelasting voor groot is om te zeer grote gegevens grootte

 > [AZURE.NOTE] SQL Server 2012 SP2 Enterprise afbeelding **is niet inbegrepen bij een gegevensschijf**. U moet toevoegen en/of een of meer virtuele vaste schijven als uw gegevens wilt opslaan als bijlage toevoegen. Wanneer u een Azure virtuele machines maakt, heeft een schijf voor het besturingssysteem dat is toegewezen aan het station C en een tijdelijke schijf toegewezen aan het station D. Gebruik niet het station D voor de opslag van gegevens. Als de naam aangeeft, biedt deze alleen tijdelijke opslag. Deze optie biedt geen redundantie of back-up omdat deze zich niet in Azure opslag.


##<a name="Provision"></a>Verbinding maken met de klassieke Azure-Portal en inrichten van een SQL Server-VM

1.  Meld u aan bij de [Klassieke Azure-Portal](http://manage.windowsazure.com/) met uw account.
    Als u niet een Azure-account hebt, bezoekt u de [gratis proefabonnement van Azure](https://azure.microsoft.com/pricing/free-trial/).

2.  In de klassieke Portal van Azure, in de linkerbenedenhoek van de webpagina, klikt u op **+ Nieuw**op **berekenen**, klikt u op **VM**, en klik vervolgens op **FROM GALERIE**.

3.  Klik op de pagina **maken een virtuele Machine** selecteert u de afbeelding van een virtuele machine met SQL Server op basis van de gegevensbehoeften van uw en klik vervolgens op de pijl-rechts in de rechterbenedenhoek van de pagina. Zie [Aan de slag met SQL Server in Azure virtuele Machines](http://go.microsoft.com/fwlink/p/?LinkId=294720) onderwerp in de documentatie van [SQL Server in Azure virtuele Machines](http://go.microsoft.com/fwlink/p/?LinkId=294719) instellen voor de meest recente informatie over de ondersteunde SQL Server-afbeeldingen op Azure.

    ![Selecteer SQL Server VM][1]

4.  Op de eerste pagina van de **VM configuratie** , kunt u de onderstaande informatie opgeven:

    -   Geef een **naam VM**.
    -   Typ in het vak **Nieuwe gebruikersnaam in te voeren** unieke gebruikersnaam voor de VM lokale beheerdersaccount.
    -   Typ een sterk wachtwoord in het vak **Nieuw wachtwoord** . Zie [Sterke wachtwoorden](http://msdn.microsoft.com/library/ms161962.aspx)voor meer informatie.
    -   Typ het wachtwoord opnieuw in het vak **Wachtwoord bevestigen** .
    -   Selecteer de gewenste **grootte** in de vervolgkeuzelijst.

     > [AZURE.NOTE] De grootte van de virtuele machine is opgegeven tijdens het inrichten: A2 de grootte van de kleinste die aanbevolen voor productie werkbelasting is. De aanbevolen minimumgrootte voor een virtuele machine is A3 wanneer u SQL Server Enterprise Edition gebruikt. Selecteer A3 of hoger wanneer u SQL Server Enterprise Edition gebruikt. Selecteer A4 wanneer u SQL Server 2012 of 2014 Enterprise geoptimaliseerd voor transacties werkbelasting afbeeldingen gebruikt.
Selecteer A7 wanneer u SQL Server 2012 of 2014 Enterprise geoptimaliseerd voor gegevens opslag werkbelasting afbeeldingen gebruikt. De grootte geselecteerd beperkt het aantal gegevensschijven die u kunt configureren. Zie [VM grootten voor Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx)voor de meest recente informatie over de beschikbare VM grootte en het aantal gegevensschijven die u aan een virtuele machine koppelen kunt. Zie [Virtuele Macines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/)voor prijsinformatie.

    Klik op de pijl-rechts aan de rechterkant onder om door te gaan.

    ![VM configuratie][2]

5.  Op de tweede pagina van de **VM configuratie** , bronnen voor netwerken, opslag en beschikbaarheid te configureren:

    -   Kies in het vak **Cloudservice** **maken een nieuwe cloudservice**.
    -   Geef het eerste deel van een DNS-naam van uw keuze wordt in het vak **De naam van de DNS-Service Cloud** zodat deze een naam in de indeling **TESTNAME.cloudapp.net** is voltooid
    -   Selecteer een gebied waar u deze virtuele afbeelding wordt gehost in het vak **Regio/AFFINITEIT groep/virtuele netwerk** .
    -   In het **Opslag-Account**, selecteert u een bestaand opslag-account of een automatisch gegenereerde te selecteren.
    -   Selecteer **(geen)**in het vak **Beschikbaarheid instellen** .
    -   Lees en accepteer de prijsinformatie.

6.  Klik in de sectie **EINDPUNTEN** klikt u in de lege vervolgkeuzelijst onder **naam**, en selecteer **MSSQL** en typ vervolgens het poortnummer van het exemplaar van de Database-Engine (**1433** voor het standaardexemplaar).

7.  Uw SQL Server-VM kan ook fungeren als een IPython notitieblok Server worden geconfigureerd in een volgende stap.
    Voeg een nieuw eindpunt om op te geven van de poort wilt gebruiken voor uw server IPython notitieblok. Typ een naam in de kolom **naam** , selecteert u een poortnummer van uw keuze voor de openbare poort en 9999 voor de poort privé.

    Klik op de pijl-rechts aan de rechterkant onder om door te gaan.

    ![Selecteer MSSQL en IPython-poorten][3]

8.  Accepteer de standaardoptie voor **VM installeren agent** ingeschakeld en klik op de het selectievakje in de rechterbenedenhoek van de wizard om de inrichting van proces VM.

    `![VM definitief opties][4]

9.  Wacht terwijl Azure uw VM bereidt. Volg de stappen in de status van de VM verwachten:

    -   (Inrichting) wordt gestart
    -   Gestopt
    -   (Inrichting) wordt gestart
    -   Actief (inrichting)
    -   Uitgevoerd


##<a name="RemoteDesktop"></a>Open de virtuele machine met extern bureaublad en setup voltooien

1.  Als u inrichting hebt voltooid, klikt u op de naam van uw VM om naar de dashboardpagina te gaan. Onderaan op de pagina, klikt u op **verbinding maken**.

2.  Kies het rpd-bestand met het programma Extern bureaublad van Windows te openen (`%windir%\system32\mstsc.exe`).

3.  Klik in het dialoogvenster **Windows-beveiliging** bieden u het wachtwoord voor het lokale beheerdersaccount die u hebt opgegeven in een eerdere stap.
    (U mogelijk gevraagd om te controleren of de referenties van de virtuele machine.)

4.  De eerste keer dat u zich aanmeldt bij deze VM verschillende processen moet mogelijk hebt voltooid, inclusief de instellingen van uw desktopprinter, een Windows-updates en voltooiing van de eerste configuratietaken (sysprep) van Windows. Nadat Windows sysprep is voltooid, voltooit de installatie van SQL Server configuratietaken uit te voeren. Deze taken mogelijk een vertraging van een paar minuten terwijl ze voltooien. `SELECT @@SERVERNAME`de juiste naam mogelijk niet worden geretourneerd totdat de installatie van SQL Server is voltooid en SQL Server Management Studio is mogelijk niet visable op de startpagina.

Nadat u met de virtuele machine met extern bureaublad van Windows verbonden bent, wordt de virtuele machine werkt net zoals elke andere computer. Verbinding maken met het standaardexemplaar van SQL Server met SQL Server Management Studio (uitgevoerd op de virtuele machine) in de normale manier.


##<a name="InstallIPython"></a>IPython notitieblok en andere ondersteunende's installeren

Als u wilt configureren op uw nieuwe SQL Server-VM voor als een server IPython notitieblok dienen en installeren van extra ondersteunende hulpmiddelen voor anderen zoals AzCopy, Azure opslag Explorer en nuttige gegevens wetenschappelijke Python-pakketten, is een speciale aanpassingen-script voor u beschikbaar. Installeren:

- Met de rechtermuisknop op het pictogram **Start van Windows** en klik op de **opdrachtprompt (beheerders)**
- De volgende opdrachten kopiëren en plakken bij de opdrachtprompt.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Wanneer u wordt gevraagd, typt u een wachtwoord van uw keuze voor de server IPython notitieblok.
- Het script aanpassing automatiseert verschillende na de installatie procedures, waaronder:
    + Installatie en configuratie van IPython notitieblok server
    + TCP-poorten openen in Windows firewall voor de eindpunten eerder hebt gemaakt:
    + Voor externe SQL Server-connectiviteit
    + Voor externe serverconnectiviteit IPython notitieblok
    + Voorbeeld IPython notitieblokken en SQL-scripts ophalen
    + Downloaden en installeren van nuttige gegevens wetenschappelijke Python-pakketten
    + Downloaden en installeren van Azure hulpprogramma's zoals AzCopy en Azure opslag Explorer  
<br>
- U kunt openen en IPython notitieblok uitvoeren vanaf elke gewenste lokale of externe browser via een URL van het formulier `https://<virtual_machine_DNS_name>:<port>`, waarbij de IPython openbare poort die u hebt geselecteerd bij de inrichting van de virtuele machine.
- IPython notitieblok server als achtergrond wordt uitgevoerd en worden gestart wanneer u de virtuele machine opnieuw opstart.

##<a name="Optional"></a>Gegevensschijf hebt toegevoegd indien nodig

Als uw afbeelding VM bevat geen gegevensschijven, dat wil zeggen schijven dan station C (OS schijf) en station D (tijdelijke schijf), moet u een of meer gegevensschijven opslaan van gegevens toevoegen. De afbeelding VM voor SQL Server 2012 SP2 Enterprise geoptimaliseerd voor DataWarehousing werkbelasting wordt geleverd vooraf geconfigureerde met extra schijven voor SQL Server data en logbestanden.

 > [AZURE.NOTE] Gebruik niet het station D voor de opslag van gegevens. Als de naam aangeeft, biedt deze alleen tijdelijke opslag. Deze optie biedt geen redundantie of back-up omdat deze zich niet in Azure opslag.

Als u wilt bijvoegen aanvullende gegevensschijven, volgt u de stappen in [het bijvoegen een schijf gegevens aan een virtuele Windows-computer](virtual-machines-windows-classic-attach-disk.md), die u begeleidt:

1. Lege schijven koppelen aan de virtuele machine deze is ingericht in eerdere stappen
2. Initialisatie van de nieuwe schijven in de virtuele machine


##<a name="SSMS"></a>Verbinding maken met SQL Server Management Studio en gemengde modus-verificatie inschakelen

De SQL Server-Database Engine niet Windows-verificatie gebruiken zonder domeinomgeving. Als u wilt verbinding maken met de Database-Engine vanuit een andere computer, SQL Server te configureren voor verificatie in gemengde modus. Gemengde verificatiemodus kan zowel SQL Server-verificatie en Windows-verificatie. SQL-verificatiemodus is vereist voor het nemen van de gegevens rechtstreeks uit uw VM van SQL Server-databases in de [Azure Machine Learning Studio](https://studio.azureml.net) gebruik van de module gegevens importeren.

1.  Verbinding gemaakt met de virtuele machine met behulp van extern bureaublad en gebruik het deelvenster Windows **Zoeken** en typt u **SQL Server Management Studio** (SMSS). Hiermee start de SQL Server Management Studio (SSMS). Het is raadzaam een snelkoppeling toevoegen aan SSMS op uw bureaublad voor toekomstig gebruik.

    ![SSMS starten][5]

    De eerste keer dat u Management Studio opent moet de gebruikers Management Studio-omgeving maken. Dit kan even duren.

2.  Bij het openen, worden in het dialoogvenster **verbinding maken met de Server** Management Studio weergegeven. Typ in het vak **servernaam** de naam van de virtuele machine verbinding maken met de Database-Engine met de Verkenner Object.
    (In plaats van de naam van de VM u kunt ook gebruiken **(lokaal)** of een bepaalde periode als de **servernaam**. Selecteer **Windows-verificatie**en laat ** *uw\_VM\_naam*\\uw\_lokale\_beheerder** in het vak **gebruikersnaam in te voeren** . Klik op **verbinding maken**.

    ![Verbinding maken met de Server][6]

    <br>

     > [AZURE.TIP] U kunt de SQL Server-verificatiemodus een wijziging van de registersleutel Windows of gebruiken van de SQL Server Management Studio wijzigen. Als u wilt wijzigen met behulp van wijziging van de registersleutel verificatiemodus, start een **Nieuwe Query** en het volgende script uitvoeren:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    De verificatiemodus met behulp van SQL Server management Studio wijzigen:

3.  In **SQL Server Management Studio Object Explorer**met de rechtermuisknop op de naam van het exemplaar van SQL Server (de naam van de virtuele machine) en klik vervolgens op **Eigenschappen**.

    ![Servereigenschappen van de][7]

4.  Op de pagina **beveiliging** onder **Serververificatie**, **SQL Server en Windows-verificatiemodus**selecteren en klik vervolgens op **OK**.

    ![Selecteer de verificatiemodus][8]

5.  Klik op **OK** om te bevestigen dat de vereiste opnieuw starten van SQL Server in het dialoogvenster **SQL Server Management Studio** .

6.  In **Object Explorer**met de rechtermuisknop op de server en klik op **opnieuw starten**. (Als SQL Server Agent wordt uitgevoerd, deze moet ook opnieuw worden gestart.)

    ![Opnieuw starten][9]

7.  Klik in het dialoogvenster **SQL Server Management Studio** op **Ja** te accepteren dat u opnieuw wilt laten SQL Server.

##<a name="Logins"></a>SQL Server-verificatie aanmeldingen maken

Als u wilt verbinding maken met de Database-Engine vanuit een andere computer, moet u ten minste één SQL Server-verificatie login.  

U kunt nieuwe SQL Server-aanmeldingen via programmacode maken of met de SQL Server Management Studio. Om een nieuwe gebruiker van de systeembeheerder SQL-verificatie via een programma, start u een **Nieuwe Query** en het volgende script uitvoeren. Vervang < nieuwe gebruikersnaam in te voeren\> en < nieuw wachtwoord\> met uw keuze uit de *gebruikersnaam* en *wachtwoord*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Pas de wachtwoordbeleid naar wens (de steekproef code schakelt beleid controleren en wachtwoord verlooptijd). Zie voor meer informatie over SQL Server-aanmeldingen [maken een aanmelding](http://msdn.microsoft.com/library/aa337562.aspx).  

Nieuwe SQL Server-aanmeldingen met de SQL Server Management Studio maken:

1.  Vouw de map van de server-instantie waarin u wilt maken van de nieuwe login in **SQL Server Management Studio Object Explorer**.

2.  Met de rechtermuisknop op de map **beveiliging** , wijs **Nieuw**aan en selecteer **aanmelden...**.

    ![Nieuwe aanmelding][10]

3.  Voer de naam van de nieuwe gebruiker in het vak **Login name** in het dialoogvenster **Login - nieuwe** op de pagina **Algemeen** .

4.  Selecteer **SQL Server-verificatie**.

5.  Voer in het vak **wachtwoord** een wachtwoord voor de nieuwe gebruiker. Typ dit wachtwoord opnieuw in het vak **Wachtwoord bevestigen** .

6.  Als wilt afdwingen beleid wachtwoordopties voor complexiteit en afdwingen, schakelt u **wachtwoordbeleid afdwingen** (aanbevolen). Dit is een standaardoptie wanneer u SQL Server-verificatie is geselecteerd.

7.  Selecteer om af te dwingen beleid wachtwoordopties voor verlooptijd, **bijna verlopen wachtwoorden van afdwingen** (aanbevolen). Afdwingen wachtwoordbeleid zodat dit selectievakje moet zijn ingeschakeld. Dit is een standaardoptie wanneer u SQL Server-verificatie is geselecteerd.

8.  Als u wilt dat de gebruiker een nieuw wachtwoord maken na de eerste keer dat de aanmelding wordt gebruikt, selecteert u **gebruiker moet wachtwoord bij volgende aanmelding wijzigen** (aanbevolen als deze aanmelding namens iemand anders is te gebruiken. Als de aanmelding voor eigen gebruik is, schakel deze optie.) Afdwingen bijna verlopen wachtwoorden zodat dit selectievakje moet zijn ingeschakeld. Dit is een standaardoptie wanneer u SQL Server-verificatie is geselecteerd.

9.  Selecteer een standaarddatabase voor de aanmelding in de lijst **standaard-database** . **basispagina** is de standaardinstelling voor deze optie. Als u een gebruikersdatabase nog niet hebt gemaakt, laat u deze waarde op **diamodel**.

10. Laat **standaard** als de waarde in de lijst **standaardtaal** .

    ![Meld u eigenschappen][11]

11. Als dit de eerste aanmelding die u maakt, is het raadzaam deze aanmelding aanwijzen als de beheerder van een SQL Server. Als dat het geval is, schakelt u op de pagina **Serverrollen** **systeembeheerder**.

    > [AZURE.IMPORTANT] Leden van de functie van de systeembeheerder vaste server hebt volledige controle van de Database Engine. Veiligheidsoverwegingen, moet u zorgvuldig lidmaatschap van deze rol beperken.

    ![systeembeheerder][12]

12. Klik op OK.

##<a name="DNS"></a>De naam van de DNS van de virtuele machine bepalen

Als u wilt verbinding maken met de SQL Server-Database Engine vanuit een andere computer, moet u de naam van de Domain Name System (DNS) van de virtuele machine weten.

(Dit is de naam die Internet wordt gebruikt om aan te geven van de virtuele machine. U kunt het IP-adres, maar het IP-adres kan worden gewijzigd wanneer Azure resources voor redundantie of onderhoud beweegt. De DNS-naam worden stabiele omdat deze kan worden omgeleid naar een nieuwe IP-adres.)

1.  Klik in de klassieke Azure-Portal (of uit de vorige stap), selecteert u **virtuele MACHINES**.

2.  Zoek op de pagina **VM exemplaren** in de kolom **Naam van de DNS-** en de DNS-naam voor de virtuele machine die wordt weergegeven voorafgegaan door **http://**kopiëren. (De gebruikersinterface van de volledige naam kan niet worden weergegeven, maar u kunt met de rechtermuisknop op dit en selecteer kopiëren.)

##<a name="cde"></a>Verbinding maken met de Database-Engine vanuit een andere computer

1.  Open op een computer is verbonden met internet, SQL Server Management Studio.

2.  Voer in het dialoogvenster **verbinding maken met de Server** of **verbinding maken met de Database-Engine** in het vak **servernaam** de DNS-naam van de virtuele machine (bepaald in de vorige taak) en een openbare eindpunt poortnummer in de indeling van *DNS-naam, poortnummer* zoals **tutorialtestVM.cloudapp.net,57500**.

3.  Selecteer in het vak **verificatie** **SQL Server-verificatie**.

4.  Typ de naam van een aanmelding die u hebt gemaakt in een eerdere taak in het vak **Login** .

5.  Typ in het vak **wachtwoord** het wachtwoord van de aanmelding die u in een eerdere taak maakt.

6.  Klik op **verbinding maken**.

##<a name="amlconnect"></a>Verbinding maken met de Database-Engine van Azure Machine Learning

In latere stadia van het Team gegevens wetenschap proces gebruikt u de [Azure Machine Learning Studio](https://studio.azureml.net) voor bouwen en implementeren van machine learning-modellen. Gegevens uit uw databases SQL Server VM rechtstreeks aan op Azure Machine Learning trainingen nemen of scoren, gebruikt u de module **Gegevens importeren** in een nieuwe [Azure Machine Learning Studio](https://studio.azureml.net) experiment. In dit onderwerp vindt u in meer gegevens via de koppelingen van de handleiding voor Team gegevens wetenschap proces. Zie voor een inleiding [Wat Azure Machine Learning Studio is?](machine-learning-what-is-ml-studio.md).

2.  Selecteer in het deelvenster **Eigenschappen** van de [gegevens importeren module](https://msdn.microsoft.com/library/azure/dn905997.aspx) **Azure SQL-Database** in de vervolgkeuzelijst van de **Gegevensbron** .

3.  Voer in het tekstvak **naam van de Database-server**`tcp:<DNS name of your virtual machine>,1433`

4.  Typ de naam van de SQL-gebruiker in het tekstvak **gebruikersnaam Server** .

5.  Voer het wachtwoord van de sql-gebruiker in het tekstvak **wachtwoord voor een gebruikersaccount Server** .

    ![Azure ML importgegevens][13]

##<a name="shutdown"></a>Afsluiten en toewijzing VM wanneer u niet in gebruik

Azure virtuele Machines prijs als **betalen alleen voor wat u gebruikt**. Om ervoor te zorgen dat u bent niet die worden gefactureerd wanneer u uw VM niet gebruikt, hebt u er moet in de stand **gestopt (Deallocated)** .

> [AZURE.NOTE] Als u de virtuele machine afsluit uit binnen (via power-opties voor Windows), de VM is gestopt maar blijft verdeeld. U bent niet die worden gefactureerd, zodat altijd stoppen virtuele machines in de [Klassieke Azure-Portal](http://manage.windowsazure.com/). U kunt ook de VM via Powershell stoppen door te bellen ShutdownRoleOperation met 'PostShutdownAction' gelijk is aan 'StoppedDeallocated'.

Afsluiten en de virtuele machine opheffen:

1. Meld u aan bij de [Klassieke Azure-Portal](http://manage.windowsazure.com/) met uw account.  

2. Selecteer **virtuele MACHINES** in de linkernavigatiebalk.

3. In de lijst met virtuele machines, klik op de naam van uw virtuele computer en Ga naar **de dashboardpagina** .

4. Klik onderaan op de pagina, op **Afsluiten**.

![VM afsluiten][15]

De virtuele machine wordt opgeheven maar deze niet verwijderd. U kunt uw VM opnieuw opstarten op elk gewenst moment in de klassieke Azure-Portal.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Uw VM Azure SQL Server is klaar voor gebruik: Wat is het volgende?

Uw VM is nu klaar voor gebruik in uw gegevens wetenschappelijke oefeningen. De virtuele machine is ook klaar voor gebruik als een server IPython notitieblok voor het verkennen en de verwerking van gegevens en andere taken in combinatie met Azure Machine Learning en het Team gegevens wetenschap proces (TDSP).

De volgende stappen in het proces van de wetenschappelijke gegevens in het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) zijn toegewezen, en eventueel stappen die gegevens verplaatsen naar HDInsight, verwerken en probeer het voorbereiding op leren van de gegevens met Azure Machine Learning.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
