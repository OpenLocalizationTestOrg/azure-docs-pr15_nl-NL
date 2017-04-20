<properties
    pageTitle="Een Web-Apps Resource-Provider toevoegen aan Azure stapel | Microsoft Azure"
    description="Gedetailleerde richtlijnen voor het Web-Apps Azure gestapelde implementeren"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Een provider van de resource Web Apps toevoegen aan Azure-Stack

> [AZURE.NOTE] De volgende informatie is alleen van toepassing op Azure stapel TP1 implementaties.

Een Web-Apps Resource Provider toevoegen aan de stapel Azure heeft zeven stappen:

1.  Vereiste onderdelen downloaden.
2.  Certificaten moet worden gebruikt in WebApps van Azure stapel hebt gemaakt.
3.  Gebruik het installatieprogramma downloaden, fase en Azure stapel Web-Apps installeren. 
4.  Valideer de Web Apps-installatie.
5.  DNS-records maken voor de front-end en netwerktaakverdelers Management Server.
6.  De zojuist geïmplementeerd WebApps resource provider met ARM registreren.
7.  Uitproberen de Web-Apps Resource-Provider.

## <a name="download-required-components"></a>Vereiste onderdelen downloaden

1.  Download het [installatieprogramma van Azure stapel App Service preview](http://aka.ms/azasinstaller). 
2.  Download de [Azure stapel App Service implementatie helper scripts](http://aka.ms/azashelper). 
3.  Pak de bestanden in het Help-scripts zip-bestand, moet er drie scripts:
    - Maak AppServiceCerts.ps1
    - Maak AppServiceDnsRecords.ps1
    - Register-AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Certificaten moet worden gebruikt in WebApps van Azure stapel maken

Deze eerste script werkt met de certificeringsinstantie Azure stapel 3 certificaten die nodig zijn voor WebApps maken. Het script uitvoeren op de ClientVM ervoor zorgen dat u PowerShell als azurestack\administrator uitvoert:
1.  In een PowerShell-sessie wordt uitgevoerd met **azurestack\administrator**, moet u de **Maken-AppServiceCerts.ps1** -script uitvoeren.  Hiermee maakt u drie certificaten, in dezelfde map als het script, die nodig zijn voor de Web Apps.
2.  Een wachtwoord invoeren om te beveiligen van de pfx-bestanden en noteer ervan zoals moet u die naam opgeven in het installatieprogramma van Azure stapel Web Apps.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Het installatieprogramma downloaden en installeren van Azure stapel Web Apps gebruiken

Het installatieprogramma van appservice.exe zal het volgende doen:
1.  De gebruiker gevraagd om de Microsoft en derden gebruiksrechtovereenkomsten accepteren.
2.  Informatie over de implementatie van Azure stapel verzamelen.
3.  Maak een container blob in het opgegeven Azure stapel opslag-account.
4.  Download de bestanden nodig voor het installeren van de provider van de resource Azure stapel Web App.
5.  Bereid de installatie te implementeren van de provider van de resource Web App in de stapel Azure-omgeving.
6.  Upload de bestanden aan de App-opslag-account.
7.  Gegevens die nodig zijn op de sjabloon Azure resourcemanager gang presenteren.

De volgende stappen helpt u bij de installatie-fasen:

>[AZURE.NOTE] Het installatieprogramma uitvoeren, moet u eerst een verhoogde account (lokaal of domein administrator) gebruiken. Als u zich als azurestack\azurestackuser aanmelden, wordt u gevraagd om verhoogde referenties. 

1.  Appservice.exe als **azurestack\administrator**worden uitgevoerd. 
2.  Klik op **Deploy Azure Resource Manager gebruiken**.

![Azure stapel App Service Technical Preview 1 Installer][1]

3.  Lees en accepteer de licentievoorwaarden voor voorlopige versie van Microsoft-Software en klik op **volgende**.
4.  Bekijk akkoord met de derde partylicense voorwaarden en klik vervolgens op **volgende**.
5.  Controleer de gegevens van de configuratie van App-Cloudservice en klik op **volgende**.

![Azure stapel App Service Technical Preview 1 App Service Cloud configuratie][2]

6. Klik op **verbinding maken met** (naast het vak Azure stapel abonnementen).

![Azure stapel App Service Technical Preview 1 App Service Cloud Configuratiescherm twee][3]

7.  Geef in het venster Azure stapel verificatie uw **servicebeheerder van Azure Active Directory-account** en **wachtwoord**en klik vervolgens op **Aanmelden**.
**Notitie:** Dit is het Azure Active Directory-account dat u hebt opgegeven toen u Azure stapel geïmplementeerd.
    - Klik op de **Pijl-omlaag** aan de rechterkant van het vak naast **Azure stapel abonnementen** en selecteer vervolgens uw abonnement.

![Azure stapel App Service Technical Preview 1 abonnement selectie][5]

8.  Klik op de **Pijl-omlaag** aan de rechterkant van het vak naast **Azure stapel locaties**.
    - Selecteer **lokale**.
9. Voer de **naam** voor de beheerder.
10. Typ een **wachtwoord** voor de beheerder.
11. Controleer de **details van SQL Server** en wijzig indien nodig.
12. Bekijk de **Systeembeheerder aanmeldingsaccount** en wijzig indien nodig.
13. Voer het **Wachtwoord van de systeembeheerder**.
14. Klik op **volgende**.  Op dit moment het installatieprogramma wordt nu Controleer of de verbindingsgegevens voor SQL Server is opgegeven.

![Azure stapel App Service Technical Preview 1 abonnement selectie][4]    

15. Klik op **Bladeren** naast het **Certificaatbestand voor SSL van Web Apps-standaard** en navigeer naar de **-webapps. AzureStack.Local** certificaat [eerder hebt gemaakt](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
16. Voer het **wachtwoord voor het certificaat** dat u hebt ingesteld wanneer u de certificaten hebt gemaakt.
17. Klik op **Bladeren** naast het **Bestand Resource Provider SSL-certificaat** en Ga naar het **-beheer. AzureStack.Local** certificaat [eerder hebt gemaakt](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
18. Voer het **wachtwoord voor het certificaat** dat u hebt ingesteld wanneer u de certificaten hebt gemaakt.
19. Klik op **Bladeren** naast het **Bronbestand Provider hoofdsite certificaat** en Ga naar het **-beheer. AzureStack.Local** certificaat [eerder hebt gemaakt](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
20. Klik op **volgende** het installatieprogramma van het certificaatwachtwoord dat wordt gecontroleerd.

![Azure stapel App Technical Preview 1 certificaat servicedetails][6]

De implementatie duurt ongeveer 45-60 minuten om te voltooien.

![Voortgang van de Azure stapel App Service Technical Preview-1-installatie][7]

21. Klik op **Afsluiten**nadat het installatieprogramma van succes is voltooid.

## <a name="validate-web-apps-installation"></a>Valideer de Web Apps-installatie

1.  Open op uw **Azure stapel Host Machine** **Hyper-V-beheer**.
2.  Zoek de **CN0-VM** en **verbinding maken** met de VM.
![Azure stapel App Technical Preview 1 Hyper-V servicebeheer][8]
3.  Klik op het bureaublad van deze VM dubbelklikt u op het **Web Cloud Management Console**.
![Beheerconsole van Azure stapel App Service Technical Preview 1][9]
4.  Navigeer naar de **beheerde Servers**.
5.  Wanneer alle computers bent **Gereed** Ga verder met de volgende stap. 
![Azure stapel App Technical Preview 1 beheerde Servers servicestatus][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>DNS-records maken voor de Management Server en de front-end van netwerktaakverdelers
1.  Open een PowerShell-exemplaar als **azurestack\administrator**.
2.  Navigeer naar de locatie van de scripts gedownload en uitgepakt in de [vereiste stap](#Download-Required-Components).
3.  Het script **Maken-AppServiceDnsRecords.ps1** uitvoert, maakt Hiermee u DNS entries om in te schakelen van de portal en web app-aanvragen naar de Front-End-Servers worden doorgestuurd.  Tijdens de ARM implementatie van Web-Apps, netwerktaakverdelers twee Software (SLBs), zijn gemaakt in de resource netwerkprovider. Ze wijst u de Servers voor het beheer en de front-end-servers. De portal en ARM gebaseerde aanvragen voor de provider van de resource Azure stapel App Service gaat u naar de Server.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>De zojuist geïmplementeerd Azure stapel Web Apps-provider met ARM registreren
1.  Open een PowerShell-exemplaar als **azurestack\administrator**.
2.  Navigeer naar de locatie van de scripts gedownload en uitgepakt in de [vereiste stap](#Download-Required-Components).
3.  De **Register-AppServiceResourceProvider.ps1** -script uitvoeren. 

>[AZURE.NOTE] Typ de gebruikersnaam en wachtwoord **exact (inclusief hoofdletters en kleine letters)** zoals deze voor de **Beheerder van de Virtual Machine(s)** en het **wachtwoord** velden in de installatie is ingevoerd of uw provider resource registreren, mislukt.

## <a name="test-drive-azure-stack-web-apps"></a>Test station Azure stapel WebApps

Nu dat u hebt geïmplementeerd en de provider van de resource-WebApps geregistreerd, kunt u het testen om ervoor te zorgen dat tenants WebApps kunnen implementeren.

1.  Klik in de portal Azure stapel klikt u op Nieuw, klik op Web + Mobile en op Web App.
2.  Typ een naam in het vak Web-app in het blad Web App.
3.  Klik onder resourcegroep, klikt u op Nieuw en typ vervolgens een naam in het vak resourcegroep. 
4.  Klik op App-Service plannen/locatie en klik op Nieuw.
5.  Typ een naam in het vak App-abonnement in het blad App Service-abonnement.
6.  Klik op prijzen laag op gratis gedeeld of gedeeld-gedeeld, klik op selecteren, klikt u op OK, en klik vervolgens op maken.
7.  In onder minuten, wordt een tegel voor de nieuwe web-app weergegeven op het Dashboard. Klik op de tegel.
8.  Klik in het blad web-app op Bladeren om weer te geven van de standaardwebsite voor deze app.


**(Optioneel) een WordPress, DNN of Django website implementeren**

1. Klik in de portal Azure stapel op "+", gaat u naar de Azure Marketplace, implementatie van een website Django en wacht voor een succesvolle afronding. Het Django webplatform beschikt over een bestand gebaseerde database en eventuele aanvullende resource-mailproviders zoals SQL of MySQL geen nodig.  

2. Als u ook een provider van de resource MySQL geïmplementeerd, kunt u een website WordPress van Marketplace implementeren. Wanneer u wordt gevraagd voor databaseparameters, de naam van de gebruiker als invoer *User1@Server1* (met de gebruikersnaam in te voeren en de servernaam van uw keuze).

3. Als u ook een resource-provider voor SQL Server geïmplementeerd, kunt u een website DNN van Marketplace implementeren. Wanneer u wordt gevraagd voor databaseparameters, kiest u een database in de computer met SQL Server die is gekoppeld aan de provider van de resource.

## <a name="next-steps"></a>Volgende stappen

U kunt ook andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md), zoals de [resource-provider voor SQL Server](azure-stack-sql-rp-deploy-short.md) en [MySQL resource provider](azure-stack-mysql-rp-deploy-short.md)uitproberen.

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
