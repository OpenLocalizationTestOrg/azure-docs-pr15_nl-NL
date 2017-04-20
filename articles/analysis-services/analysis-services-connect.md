<properties
   pageTitle="Gegevens ophalen uit Analysis Services van Azure | Microsoft Azure"
   description="Leer hoe u verbinding maken met en gegevens ophalen uit een Analysis Services-server in Azure wordt aangegeven."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Gegevens ophalen uit Analysis Services van Azure
Nadat u hebt gemaakt van een server in Azure wordt aangegeven en een tabelmodel geïmplementeerd in deze, zich gebruikers in uw organisatie verbinding maken en begin met het verkennen van gegevens.

Azure Analysis Services ondersteunt clientverbindingen met [bijgewerkt objectmodellen](#client-libraries). TOM, AMO, Adomd.Net of MSOLAP, via xmla op de server te verbinden. Bijvoorbeeld Power BI, Power BI Desktop, Excel of een derde partij clienttoepassing die ondersteuning biedt voor de objectmodellen.

## <a name="server-name"></a>De naam van server
Als u een Analysis Services-server in Azure maakt, geeft u een unieke naam en de regio waar de server onderdeel is moet worden gemaakt. Wanneer u de servernaam opgeeft in een verbinding, is het schema voor de naamgeving server:
```
<protocol>://<region>/<servername>
```
 Hier protocol tekenreeks **asazure**, de regio is de Uri van de regio waar de server is gemaakt (bijvoorbeeld voor West US, westus.asazure.windows.net) en de servernaam is de naam van uw unieke server in het gebied.

## <a name="get-the-server-name"></a>De servernaam verkrijgen
Voordat u verbinding maakt, moet u de naam van de server. In **de portal van Azure** > server > **Overzicht** > **servernaam**de naam van de hele server kopiëren. Als andere gebruikers in uw organisatie zijn ook verbinding met deze server, moet u deze servernaam deelt met deze wilt. Wanneer u een servernaam opgeeft, kan het volledige pad moet worden gebruikt.

![Servernaam ophalen in Azure wordt aangegeven](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Verbinding maken in Power BI Desktop

> [AZURE.NOTE] Deze functie is Preview.

1. Klik op **Gegevens ophalen**in [Power BI Desktop](https://powerbi.microsoft.com/desktop/) > **Databases** > **Azure Analysis Services**.

2. Plak in het vak **Server**de servernaam vanaf het Klembord.

3. In de **Database**, als u de naam van het tabellaire modeldatabase of perspectief die u verbinding wilt maken, plak deze hier. Anders kunt u dit veld leeg laten. U kunt een database of het perspectief in het volgende scherm selecteren.

4. Laat het selectievakje standaard **verbinding maken met live** geselecteerd en klik op **verbinding maken**. Als u wordt gevraagd om in te voeren van een account, voert u uw organisatieaccount.

5. Vouw de server, in **Navigator**en vervolgens selecteert u het model of perspectief die u verbinding wilt maken en klik vervolgens op **verbinding maken**. Één klik op een model of perspectief ziet u alle objecten voor deze weergave.


## <a name="connect-in-power-bi"></a>Verbinding maken in Power BI
1. Maak een Power BI Desktop-bestand met een live verbinding met uw model op de server.

2. Klik op **Gegevens ophalen**in [Power BI](https://powerbi.microsoft.com), > **bestanden**. Zoek en selecteer het bestand.


## <a name="connect-in-excel"></a>Verbinding maken in Excel
Verbinding maken met Azure Analysis Services-server in Excel wordt ondersteund door de gegevens ophalen in Excel 2016 of Power Query in eerdere versies gebruiken. [Msolap.7-provider](https://aka.ms/msolap) is vereist. Verbinding maken met behulp van de Wizard tabel importeren in PowerPivot wordt niet ondersteund.

1. Klik in Excel 2016, klik op het lint **gegevens** op **Externe gegevens ophalen** > **Uit een andere bron** > **Van Analysis Services**.

2. Plak in de Wizard Gegevensverbinding in het vak **servernaam**de naam van de server vanaf het Klembord. **Aanmeldingsreferenties**, selecteert u in **de volgende gebruikersnaam en wachtwoord**, en typ de naam van de organisatie-gebruiker, bijvoorbeeld nancy@adventureworks.com, en wachtwoord.

    ![Verbinding maken in Excel aanmelding](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. Selecteer de database en het model of het perspectief in de **Database en tabel selecteren**, en klik vervolgens op **Voltooien**.

    ![Verbinding maken in Excel, selecteer model](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Verbindingsreeks
Wanneer u verbinding maakt met Azure Analysis Services met het in tabelvorm objectmodel, gebruikt u de volgende verbinding tekenreeks indelingen:

###### <a name="integrated-azure-active-directory-authentication"></a>Geïntegreerde Azure Active Directory-verificatie
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Geïntegreerde verificatie gewoon de Azure Active Directory referentiecache indien beschikbaar. Als dit niet het geval is, wordt het aanmeldingsvenster van Azure wordt weergegeven.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory-verificatie met gebruikersnaam en wachtwoord
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Clientbibliotheken
Wanneer u verbinding maakt met Azure Analysis Services uit Excel of andere interfaces zoals TOM, AsCmd, ADOMD.NET, moet u mogelijk de meest recente bibliotheken van de provider-client installeren. De meest recente ophalen:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Volgende stappen
[Uw server beheren](analysis-services-manage.md)
