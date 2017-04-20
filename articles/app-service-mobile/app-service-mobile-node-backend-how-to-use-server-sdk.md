<properties
    pageTitle="Het werken met Node.js endserver SDK voor Mobile-Apps | Azure App-Service"
    description="Informatie over het werken met Node.js endserver SDK voor Azure App Service Mobile-Apps."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Het gebruik van de Azure Mobile-Apps Node.js SDK

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

In dit artikel vindt u gedetailleerde informatie en voorbeelden ziet u hoe u werkt met een backend Node.js in Azure App Service Mobile-Apps.

## <a name="Introduction"></a>Inleiding

Azure App Service Mobile-Apps biedt de mogelijkheid een geoptimaliseerde mobiele gegevenstoegang Web API toevoegen aan een webtoepassing.  De SDK van Azure App Service Mobile-Apps is opgegeven voor ASP.NET en Node.js webtoepassingen.  De SDK biedt de volgende bewerkingen:

- Tabelbewerkingen (lezen, invoegen, Update, Delete) voor toegang tot gegevens
- Aangepaste API-bewerkingen

Beide bewerkingen voorzien voor verificatie via alle identiteitsprovider toegestaan door Azure App-Service, met inbegrip van de sociale identiteitsprovider zoals Facebook, Twitter, Google en Microsoft, evenals Azure Active Directory voor enterprise identiteit.

Voorbeelden vindt u voor elke use-case in de [map samples op GitHub].

## <a name="supported-platforms"></a>Ondersteunde platformen

De Azure Mobile-Apps knooppunt SDK ondersteunt de huidige LTS-versie van knooppunt en hoger.  Op schrijven is de meest recente versie van LTS knooppunt v4.5.0.  Andere versies van knooppunt werken mogelijk, maar worden niet ondersteund.

De Azure Mobile-Apps knooppunt SDK ondersteunt twee databasestuurprogramma: het knooppunt mssql-stuurprogramma ondersteunt SQL Azure wordt aangegeven en lokale SQL Server-exemplaren.  Het stuurprogramma sqlite3 ondersteunt SQLite databases op slechts één exemplaar.

### <a name="howto-cmdline-basicapp"></a>Hoe u: een eenvoudige Node.js backend via de opdrachtregel maken

Elke backend Azure App Service mobiele App Node.js wordt gestart als een toepassing ExpressJS.  ExpressJS is het beschikbaar voor Node.js populairste web service framework.  U kunt een basic maken [Express] toepassing als volgt:

1. Maak een map voor uw project in een opdracht of PowerShell-venster.

        mkdir basicapp

2. Initialisatie van de npm als u wilt de pakketstructuur geïnitialiseerd uitvoeren.

        cd basicapp
        npm init

    De opdracht npm-initialisatie wordt gevraagd om een reeks vragen initialisatie van het project.  Zie de voorbeeld-uitvoer:

    ![De uitvoer van de initialisatie npm][0]

3. Installeer de bibliotheken express en azure-mobile-apps uit de bibliotheek npm.

        npm install --save express azure-mobile-apps

4. Een bestand app.js implementatie van de eenvoudige mobiele server maken.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Deze toepassing Hiermee maakt u een geoptimaliseerde mobiele WebAPI met een enkelvoudig eindpunt (`/tables/TodoItem`) die niet-geverifieerde toegang tot een onderliggende SQL-gegevensopslag met behulp van een dynamische schema bevat.  Het is geschikt voor het volgen van de client bibliotheek snel aan de slag:

- [Snelstartgids voor android-Client]
- [Apache Cordova Client QuickStart]
- [iOS-Client QuickStart]
- [Windows Store-Client QuickStart]
- [QuickStart Xamarin.iOS-Client]
- [QuickStart Xamarin.Android-Client]
- [QuickStart Xamarin.Forms-Client]

U vindt de code voor deze eenvoudige toepassing in de [steekproef basicapp op GitHub].

### <a name="howto-vs2015-basicapp"></a>Hoe u: Maak een backend knooppunt met Visual Studio-2015

Visual Studio-2015 vereist een uitbreiding van Node.js toepassingen IDE ontwikkelen.  Als u wilt beginnen, installeert u de [Node.js Tools 1.1 voor Visual Studio].  Zodra de Node.js 's voor Visual Studio zijn geïnstalleerd, kunt u een Express 4.x-toepassing maken:

1. Open het dialoogvenster **Nieuw Project** (uit **bestand** > **Nieuw** > **Project...**).

2. **Sjablonen** > **JavaScript** > **Node.js**.

3. Selecteer de **eenvoudige Azure Node.js Express 4-toepassing**.

4. Voer de naam van het project.  Klik op *OK*.

    ![Nieuw Project Visual Studio-2015][1]

5. Met de rechtermuisknop op het knooppunt **npm** en selecteer **nieuwe installeren npm-pakketten...**.

6. Mogelijk moet u de catalogus npm over het maken van uw eerste Node.js-toepassing te vernieuwen.  Klik zo nodig, klikt u op **vernieuwen** .

7. Voer _azure-mobile-apps_ in het zoekvak.  Klik op het pakket **azure-mobile-apps 2.0.0** en klik op **Installeren pakket**.

    ![Nieuwe npm-pakketten installeren][2]

8. Klik op **sluiten**.

9. Open het bestand _app.js_ om toe te voegen ondersteuning voor de SDK van Azure Mobile-Apps.  Op regel 6 at onder aan de bibliotheek vereisen overzichten, voeg de volgende code:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Toevoegen aan de regel ongeveer 27 na de overige app.use-vermeldingen, de volgende code:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Sla het bestand.

10. U voert u de toepassing lokaal (de API wordt bediend op http://localhost:3000) of publiceren naar Azure.

### <a name="create-node-backend-portal"></a>Hoe u: een Node.js backend met behulp van de Azure portal maken

U kunt een rechts van de backend Mobile-App maken in de [portal van Azure]. U kunt de volgende stappen of maakt een clients en servers samen volgens de zelfstudie voor het [maken van een mobiele app](app-service-mobile-ios-get-started.md) . De zelfstudie bevat een vereenvoudigde versie van deze instructies en meest geschikt is voor aankoopbewijs concept projecten.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Terug in het blad _aan de slag_ , klik onder **een tabel API maken**, kiest u **Node.js** als de **Backend-taal**. Schakel het vakje in voor het "**ik bevestig dat Hiermee worden overschreven alle inhoud van site.**" en klik op **de tabel TodoItem maken**.

### <a name="download-quickstart"></a>Hoe: het Node.js backend quickstart CodeProject cijfer met downloaden

Wanneer u een backend Node.js Mobile-App via de portal **snel aan de slag** blade maakt, is een project Node.js voor u gemaakt en geïmplementeerd op uw site. U kunt tabellen en API's toevoegen en bewerken van codebestanden voor de backend Node.js in de portal. U kunt ook verschillende implementatieprogramma gebruiken om te downloaden van het project backend zodat u kunt toevoegen of wijzigen van tabellen en API's en vervolgens opnieuw publiceren van het project. Zie de [Implementatiehandleiding van Azure App Service]voor meer informatie. de volgende procedure wordt een cijfer opslagplaats downloaden van de quickstart project-code.

1. Installeer cijfer, als u dat nog niet had gedaan. De benodigde stappen voor het installeren van cijfer verschillen tussen besturingssystemen. Zie [Cijfer installeren](http://git-scm.com/book/en/Getting-Started-Installing-Git) voor besturingssysteem / regiospecifieke onderzoeken en installatie-instructies.

2. Volg de stappen in [de App Service app opslagplaats inschakelen](../app-service-web/app-service-deploy-local-git.md#Step3) zodat de opslagplaats cijfer voor uw back-end-site, waardoor een notitie van de implementatie-gebruikersnaam en wachtwoord.

3. Klik in het blad voor de backend Mobile-App, noteer de **cijfer klonen URL** -instelling.

4. Uitvoeren de `git clone` opdracht met het cijfer klonen URL, het invoeren van uw wachtwoord als vereist, zoals in het volgende voorbeeld:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Blader naar de lokale map, namelijk in het voorgaande voorbeeld /todolist, waarna u ziet dat project-bestanden zijn gedownload. Zoek de `todoitem.json` bestand de `/tables` directory.  Dit bestand definieert machtigingen op de tabel.  Ook vinden het `todoitem.js` bestand in dezelfde map, waarin deze CRUD-bewerkingen scripts voor de tabel.

6. Nadat u wijzigingen hebt aangebracht in projectbestanden, de volgende opdrachten toevoegen, doorvoeren en de wijzigingen te uploaden naar de site te kunnen uitvoeren:

        $ git commit -m "updated the table script"
        $ git push origin master

    Wanneer u nieuwe bestanden aan het project toevoegen, moet u eerst uitvoeren de `git add .` opdracht.

De site is gepubliceerd telkens wanneer een nieuwe set doorvoercoördinatie is verplaatst naar de site.

### <a name="howto-publish-to-azure"></a>Hoe u: uw backend Node.js publiceren naar Azure

Microsoft Azure biedt veel methoden voor het publiceren van uw backend Azure App Service Mobile-Apps Node.js bij de Azure-service.  Hierbij gebruik van implementatieprogramma geïntegreerd in Visual Studio, opdrachtregel hulpmiddelen en continue distributieopties op basis van een besturingselement voor gegevensbronnen.  Zie de [Implementatiehandleiding van Azure App Service]voor meer informatie over dit onderwerp.

Azure App-Service heeft specifieke wijze Raad voor Node.js-toepassing die u voordat u implementeert controleren moet:

- [De versie van het knooppunt] opgeven
- Het [gebruik van knooppunt modules]

### <a name="howto-enable-homepage"></a>Hoe u: een startpagina voor uw toepassing inschakelen

Veel toepassingen zijn een combinatie van web en mobiele apps en het framework ExpressJS kunt u de twee aspecten combineren.  Soms echter, kunt u alleen een mobiele interface implementeren.  Het is handig om te leveren een aantekening toevoegen om ervoor te zorgen de app-service actief is.  U kunt uw eigen introductiepagina Geef of een tijdelijke startpagina inschakelen.  Als u wilt dat een tijdelijke startpagina, gebruikt u de volgende exemplaar Azure Mobile-Apps te maken:

    var mobile = azureMobileApps({ homePage: true });

Als u alleen deze optie beschikbaar wilt bij het ontwikkelen van lokaal, kunt u deze instelling toevoegen uw `azureMobile.js` bestand.

## <a name="TableOperations"></a>Tabelbewerkingen 

De mobile-azure-apps Node.js Server SDK biedt methoden om gegevenstabellen die zijn opgeslagen in Azure SQL-Database als een WebAPI weer te geven.  Vijf operations worden gegeven.

| Bewerking | Beschrijving |
| --------- | ----------- |
| /Tables/_tabelnaam_ ophalen | Alle records in de tabel ophalen |
| /Tables/_tabelnaam_/:id ophalen | Een specifieke record in de tabel ophalen |
| BERICHT /tables/_tabelnaam_ | Een record in de tabel maken |
| PATCH /tables/_tabelnaam_/:id | Een record in de tabel wordt bijgewerkt |
| /Tables/_tabelnaam_/:id verwijderen | Een record in de tabel verwijderen |

Deze WebAPI ondersteunt [OData] en breidt de mogelijkheden het tabelschema om te ondersteunen [offline gegevens synchroniseren].

### <a name="howto-dynamicschema"></a>Hoe u: tabellen met een dynamische schema definiëren

Voordat u een tabel kan worden gebruikt, moeten worden gedefinieerd.  Tabellen kunnen deze velden worden gedefinieerd met een statische schema (waar de ontwikkelaar definieert voor de kolommen in het schema) of dynamisch (waar de SDK besturingselementen voor het schema op basis van verzoeken voor oproepen). De ontwikkelaar kunt bepaalde aspecten van de WebAPI verder bepalen door Javascript-code toe te voegen aan de definitie.

Als een goede gewoonte, moet u elke tabel definiëren in een Javascript-bestand in de adreslijst tabellen en vervolgens de tables.import()-methode gebruiken om de tabellen te importeren.  De basic-app uitbreidt, zou het bestand app.js worden aangepast:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

De tabel in definiëren. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabellen gebruiken dynamische schema al dan niet standaard.  Als u wilt uitschakelen ingesteld dynamische schema globaal, de instelling van App- **MS_DynamicSchema** op onwaar binnen de Azure-portal.

U kunt een volledig voorbeeld vinden in de [steekproef doen op GitHub].

### <a name="howto-staticschema"></a>Hoe u: tabellen met een statische schema definiëren

U kunt de kolommen om weer te geven via de WebAPI expliciet definiëren.  De mobile-azure-apps Node.js SDK wordt automatisch toegevoegd eventuele extra kolommen die zijn vereist voor offline gegevens synchroniseren met de lijst die u opgeeft.  Bijvoorbeeld de clienttoepassingen QuickStart vereisen voor een tabel met twee kolommen: tekst (tekenreeks) en (een Booleaanse waarde) te voltooien.  
De tabel kan worden gedefinieerd in de tabel definitie JavaScript-bestand (zich in de adreslijst tabellen) als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Als u tabellen statisch definieert, moet u ook de methode tables.initialize() als u wilt maken van het databaseschema bij het opstarten bellen.  De methode tables.initialize() retourneert een [toegezegd] zodat de webservice dienen geen aanvragen voordat u de database wordt geïnitialiseerd.

### <a name="howto-sqlexpress-setup"></a>Hoe u: Gebruik SQL Express als een gegevensopslag ontwikkeling op uw lokale computer

De Azure Mobile-Apps de AzureMobile Apps knooppunt SDK biedt drie opties voor het serveren van gegevens uit het vak: SDK biedt drie opties voor het serveren van gegevens uit het vak:

- Het stuurprogramma **geheugen** gebruiken om te leveren van een niet-permanente voorbeeld-winkel
- Het stuurprogramma **mssql** gebruiken om u te bieden een gegevensopslag SQL Express voor ontwikkeling
- Het stuurprogramma **mssql** gebruiken om u te bieden een gegevensopslag Azure SQL-Database voor productie

De Azure Mobile-Apps Node.js SDK gebruikt de [mssql Node.js pakket] om te definiëren en gebruiken van een verbinding met zowel SQL Express als SQL-Database.  Dit pakket is vereist dat u TCP-verbindingen in uw SQL Express-exemplaar inschakelen.

> [AZURE.TIP]Het stuurprogramma geheugen biedt geen een volledige reeks voorzieningen voor het testen.  Als u testen van uw backend lokaal wilt, wordt aangeraden het gebruik van een gegevensopslag SQL Express en het stuurprogramma mssql.

1. Download en installeer [Microsoft SQL Server 2014 Express].  Controleer of dat u de SQL Server 2014 Express installeren met hulpmiddelen voor edition.  Tenzij u de 64-bits ondersteuning expliciet vereist, verbruikt de 32-bits versie minder geheugen uitgevoerd.

2. Voer de SQL Server 2014 Configuration Manager.

  1. **SQL Server-netwerkconfiguratie** Vouw in het menu linkerkant structuur.
  2. Klik op **protocollen voor SQLEXPRESS**.
  3. Met de rechtermuisknop op **TCP/IP** en selecteer **inschakelen**.  Klik op **OK** in het pop-upvenster.
  4. Met de rechtermuisknop op **TCP/IP** en selecteer **Eigenschappen**.
  5. Klik op het tabblad **IP-adressen** .
  6. Het knooppunt **IPAll** vinden.  Voer in het veld **TCP-poort** **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Klik op **OK**.  Klik op **OK** in het pop-upvenster.
  8. **SQL Server Services** Klik in het menu linkerkant structuur.
  9. Met de rechtermuisknop op **SQL Server (SQLEXPRESS)** en selecteer **opnieuw opstarten**
  10. Sluit de SQL Server 2014 Configuration Manager.

3. De SQL Server 2014 Management Studio uitvoeren en verbinding maken met uw lokale SQL Express-exemplaar

  1. Met de rechtermuisknop op uw exemplaar in de Verkenner Object en selecteer **Eigenschappen**
  2. Selecteer het tabblad **beveiliging** .
  3. Controleer of dat de **SQL Server en Windows-verificatiemodus** is ingeschakeld
  4. Klik op **OK**

        ![Express SQL-verificatie configureren][4]

  5. **Beveiliging** > **aanmeldingen** in de Objectverkenner
  6. Met de rechtermuisknop op **aanmeldingen** en selecteer **Nieuwe aanmelding...**
  7. Voer een naam Login.  Selecteer **SQL Server-verificatie**.  Een wachtwoord, voer vervolgens hetzelfde wachtwoord bij **wachtwoord bevestigen**.  Het wachtwoord moet voldoen aan vereisten voor complexiteit van Windows.
  8. Klik op **OK**

        ![Een nieuwe gebruiker toevoegen aan het SQL Express][5]

  9. Met de rechtermuisknop op uw nieuwe aanmelding en selecteer **Eigenschappen**
  10. Selecteer de pagina **Serverrollen**
  11. Schakel het selectievakje naast de rol van de server **dbcreator**
  12. Klik op **OK**
  13. Sluit de SQL Server 2015 Management Studio

Controleer of dat u opneemt de gebruikersnaam en wachtwoord die u hebt geselecteerd.  Mogelijk moet u machtigingen afhankelijk van uw Databasevereisten voor specifieke of extra servertypen rollen toewijzen.

De toepassing Node.js leest de omgevingsvariabele **SQLCONNSTR_MS_TableConnectionString** voor de verbindingsreeks voor deze database.  U kunt deze variabele instellen in uw omgeving.  U kunt bijvoorbeeld PowerShell gebruiken voor het instellen van deze omgevingsvariabele:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Toegang tot de database via een TCP/IP-verbinding en geef een gebruikersnaam en wachtwoord voor de verbinding.

### <a name="howto-config-localdev"></a>Hoe u: uw project voor lokale ontwikkeling configureren

Azure Mobile-Apps leest een JavaScript-bestand _azureMobile.js_ aangeroepen vanuit het lokale bestandssysteem.  Gebruik dit bestand niet naar de SDK van Azure mobiele Apps configureren in productie - App-instellingen van de [Azure-portal] in plaats daarvan gebruiken.  Het bestand _azureMobile.js_ moet een configuratieobject exporteren.  De meest voorkomende instellingen zijn:

- Database-instellingen
- Instellingen voor diagnostische gegevens vastleggen
- Alternatieve CORS-instellingen

Voert u een voorbeeldbestand _azureMobile.js_ de uitvoering van de voorgaande database-instellingen:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Het is raadzaam dat u _azureMobile.js_ aan uw bestand _.gitignore toevoegen_ (of andere broncode beheren negeren bestand) om te voorkomen dat wachtwoorden worden opgeslagen in de cloud.  Altijd productie-instellingen configureren in de App-instellingen van de [Azure-portal].

### <a name="howto-appsettings"></a>Procedure: App-instellingen voor de mobiele App configureren

De meeste instellingen in het bestand _azureMobile.js_ hebben een equivalente App-instelling in de [portal van Azure].  Gebruik de volgende lijst voor het configureren van uw app in de App-instellingen:

| App-instelling                 | _azureMobile.js_ -instelling  | Beschrijving                               | Geldige waarden                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | naam                      | De naam van de app                       | tekenreeks                                      |
| **MS_MobileLoggingLevel**   | Logging.level             | Minimale logboekniveau van berichten aan te melden      | fout, waarschuwing, info, uitgebreide, foutopsporing, grappige |
| **MS_DebugMode**            | fouten opsporen                     | Inschakelen of uitschakelen foutopsporingsmodus              | waar, ONWAAR                                 |
| **MS_TableSchema**          | Data.schema               | Naam van de standaard-schema voor SQL-tabellen        | tekenreeks (standaard: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Inschakelen of uitschakelen foutopsporingsmodus              | waar, ONWAAR                                 |
| **MS_DisableVersionHeader** | versie (ingesteld tot ongedefinieerd)| Schakelt de kop van de X-ZUMO-Server-versie | waar, ONWAAR                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Schakelt u de controle van de versie client-API     | waar, ONWAAR                                 |

Een App instellen:

1. Meld u aan bij de [portal van Azure].
2. Selecteer **alle resources** of **App Services** Klik op de naam van de Mobile-App.
3. Het blad instellingen standaard wordt geopend. Als dit niet het geval is, klikt u op **Instellingen**.
4. **Klik in het menu Algemeen.**
5. Blader naar de sectie instellingen van de App.
6. Als uw app instelt, al bestaat, klikt u op de waarde van de app-instelling om de waarde te bewerken.
7. Als de instelling van uw app niet bestaat, voert u de App-instelling in het vak sleutel en de waarde in het vak waarde.
8. Als u voltooid zijn, klikt u op **Opslaan**.

De meeste app-instellingen wijzigen, is een herstart service vereist.

### <a name="howto-use-sqlazure"></a>Hoe u: SQL-Database gebruiken als uw productie gegevensopslag

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Azure SQL-Database gebruiken als een gegevensopslag identiek is in alle Azure App Service-toepassingstypen. Als u dit nog niet hebt gedaan, volgt u deze stappen om u te maken van een backend Mobile-App.

1. Meld u aan bij de [portal van Azure].

2. In de bovenste links van het venster, klikt u op de knop **+ Nieuw** > **Web + Mobile** > **Mobile-App**, Geef een naam voor uw backend Mobile-App.

3. Voer in het vak **Resourcegroep** dezelfde naam als de app.

4. Het abonnement dat standaard-App is geselecteerd.  Als u wijzigen van uw App Service-abonnement wilt, kunt u doen door te klikken op het App-abonnement > **+ nieuwe maken**.  Geef een naam op van het nieuwe App Service-abonnement en selecteer de juiste locatie.  Klik op de laag prijzen en selecteer een juiste prijzen laag voor de service. Selecteer **Alles weergeven** om weer te geven meer prijzen opties, zoals **gratis** en **gedeeld**.  Zodra u de prijzen laag hebt geselecteerd, klikt u op de knop **selecteren** .  Klik op **OK**in het blad **App serviceplan** .

5. Klik op **maken**. Een mobiele App backend inrichting kan enkele minuten duren.  Zodra de Mobile-App-end is ingericht, wordt het blad **Instellingen** voor de Mobile-App-end geopend in de portal.

Nadat de backend Mobile-App is gemaakt, kunt u een nieuwe SQL-database maken of een bestaande SQL-database verbinden met uw backend Mobile-App.  In dit gedeelte maken we een SQL-database.

> [AZURE.NOTE]Als u al een database op dezelfde locatie als de backend mobiele app, kunt u in plaats daarvan kiest u **een bestaande database gebruiken** en selecteer vervolgens die database. Het gebruik van een database op een andere locatie wordt niet aanbevolen vanwege hoger vertragingstijden.

6. Klik op **Instellingen**in de nieuwe Mobile-App-end > **Mobile-App** > **gegevens** > **+ toevoegen**.

7. Klik in het blad **gegevensverbinding toevoegen** op **SQL-Database - vereiste instellingen configureren** > **een nieuwe database maken**.  Voer de naam van de nieuwe database in het veld **naam** .

8. Klik op de **Server**.  In het blad **nieuwe server** , voert u een unieke naam in het veld **naam van de Server** en geef een geschikte **Server admin aanmelden** en een **wachtwoord**.  Controleer of **dat toestaan azure-services voor toegang tot de server** is ingeschakeld.  Klik op **OK**.

    ![Een Azure SQL-Database maken][6]

9. Klik op het blad **nieuwe database** , klikt u op **OK**.

10. Weer op het blad **gegevensverbinding toevoegen** , selecteer **verbindingsreeks**, voer het aanmelden en het wachtwoord die u hebt opgegeven bij het maken van de database.  Als u een bestaande database gebruikt, vindt u de aanmeldingsreferenties voor die database.  Zodra u hebt ingevoerd, klikt u op **OK**.

11. Opnieuw weer op het blad **gegevensverbinding toevoegen** , klikt u op **OK** om de database te maken.

<!--- END OF ALTERNATE INCLUDE -->

Het maken van de database kan een paar minuten duren.  Gebruik het gebied voor **meldingen** aan de voortgang van de implementatie.  Voortgang niet totdat de database is geïmplementeerd.  Nadat met succes wordt geïmplementeerd, wordt een verbindingsreeks gemaakt voor het exemplaar van de SQL-Database in de mobiele backend App-instellingen.  U kunt deze instelling app in de **Instellingen**zien > **Toepassingsinstellingen** > **tekenreeksen met de verbinding**.

### <a name="howto-tables-auth"></a>Hoe u: verificatie vereisen voor toegang tot tabellen

Als u gebruiken van App-Service verificatie met het eindpunt van de tabellen wilt, moet u eerst verificatie van de App-Service configureren in de [portal van Azure] .  Controleer de configuratie-handleiding voor de identiteitsprovider die u wilt gebruiken voor meer informatie over het configureren van verificatie in een App-Service van Azure:

- [Het configureren van Azure Active Directory-verificatie]
- [Het configureren van Facebook-verificatie]
- [Het configureren van Google-verificatie]
- [Het configureren van Microsoft Authentication]
- [Het configureren van Twitter-verificatie]

Elke tabel heeft een access-eigenschap die kan worden gebruikt om toegang tot de tabel.  In het onderstaande voorbeeld ziet u een statisch gedefinieerde tabel met verificatie vereist.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

De eigenschap access kunt drie waarden uitvoeren

  - *anonieme* geeft aan dat de clienttoepassing kan gegevens zonder verificatie lezen
  - *geverifieerd* wordt aangegeven dat de clienttoepassing een verificatietoken geldige met de aanvraag moet verzenden
  - *uitgeschakeld* geeft aan dat deze tabel is uitgeschakeld

Als de eigenschap access ongedefinieerd, is niet-geverifieerde toegang toegestaan.

### <a name="howto-tables-getidentity"></a>Hoe u: verificatie claims gebruiken met tabellen

U kunt verschillende claims die worden opgevraagd wanneer de verificatie is ingesteld instellen.  Deze beweringen zijn niet normaal gesproken beschikbaar via de `context.user` object.  Echter ze kunnen worden opgehaald met de `context.user.getIdentity()` methode.  De `getIdentity()` methode geeft als resultaat een toegezegd die wordt omgezet in een object.  Het object wordt ingeschakeld door de verificatiemethode (facebook, google, twitter, microsoftaccount of aad).

Als u de verificatie van Microsoft-Account en het verzoek om die de e-mailadressen claimen hebt ingesteld, kunt u bijvoorbeeld het e-mailadres toevoegen aan de record met de volgende tabel controller:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Als u wilt zien welke claims beschikbaar zijn, kunt u een webbrowser gebruiken om weer te geven de `/.auth/me` eindpunt van de site.

### <a name="howto-tables-disabled"></a>Hoe u: toegang tot een bepaalde tabelbewerkingen uitschakelen

Naast het op de tabel wordt weergegeven, kan de eigenschap access worden gebruikt om te bepalen van afzonderlijke bewerkingen.  Er zijn vier bewerkingen:

  - *lezen* , is de bewerking RESTful krijgen in de tabel
  - de bewerking RESTful posten op de tabel is *Invoegen*
  - *bijwerken* is de bewerking RESTful PATCH in de tabel
  - de bewerking RESTful verwijderen op de tabel is *verwijderen*

U kunt bijvoorbeeld een alleen-lezen niet-geverifieerde tabel bieden:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Hoe u: de query die wordt gebruikt met de tabelbewerkingen aanpassen

Er is een algemene vereiste voor tabelbewerkingen op te geven van een beperkte weergave van de gegevens.  U kunt bijvoorbeeld opgeven dat een tabel die is gelabeld met de geverifieerde gebruikers-ID, zodat u kunt alleen lezen of uw eigen records bijwerken.  De definitie van de volgende tabel bevat deze functionaliteit:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Bewerkingen die normaal uitvoeren van een query de eigenschap van een query die u met een positie aanpassen kunt hebt component. De eigenschap van de query is een [QueryJS] -object dat wordt gebruikt voor het converteren van een OData-query naar iets dat de backend gegevens kan worden verwerkt.  Voor eenvoudige gelijke gevallen (zoals de bovenstaande afbeelding), kan een kaart worden gebruikt. U kunt ook specifieke SQL-componenten toevoegen:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Hoe u: vloeiende verwijderen configureren op een tabel

Zachte verwijderen werkelijk verwijdert u records.  In plaats daarvan markeren het als verwijderd in de database door in te stellen van de verwijderde kolom op waar.  Records vloeiende verwijderde verwijderd de SDK van Azure Mobile-Apps automatisch uit de resultaten tenzij de mobiele Client SDK IncludeDeleted() wordt gebruikt.  Instellen om te configureren in een tabel voor vloeiende verwijderen, de `softDelete` eigenschap in de tabel definitiebestand:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Een methode voor het verwijderen van records - hetzij van een clienttoepassing, via een WebJob, Azure-functie of via een aangepaste API, moet u eerst.

### <a name="howto-tables-seeding"></a>Hoe u: de database met gegevens vullen

Wanneer u een nieuwe toepassing maakt, kunt u een tabel met gegevens vullen.  Dit u kunt doen vanuit de tabel definitie JavaScript-bestand als volgt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Seeding van gegevens is alleen worden uitgevoerd wanneer de tabel wordt gemaakt door de SDK van Azure Mobile-Apps.  Als de tabel al in de database bestaat, wordt er geen gegevens in de tabel toegevoegd.  Als u dynamische schema is ingeschakeld, klikt u vervolgens het schema afgeleid van de vooraf ingestelde gegevens.

Het is raadzaam dat u expliciet belt de `tables.initialize()` methode voor het maken van de tabel wanneer de service wordt gestart.

### <a name="Swagger"></a>Hoe u: Swagger ondersteuning inschakelen

Azure App Service Mobile-Apps wordt geleverd met ingebouwde [Swagger] ondersteuning.  Swagger als ondersteuning wilt inschakelen, moet u eerst de swagger-gebruikersinterface installeren als een afhankelijkheid:

    npm install --save swagger-ui

Zodra u hebt geïnstalleerd, kunt u Swagger-ondersteuning in de Mobile-Apps Azure-constructor inschakelen:

    var mobile = azureMobileApps({ swagger: true });

U waarschijnlijk alleen wilt inschakelen Swagger-ondersteuning in ontwikkeling edities.  U kunt dit doen met behulp van de `NODE_ENV` instelling app:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Het eindpunt swagger bevindt zich op http://_uw site_.azurewebsites.net/swagger.  U hebt toegang tot de gebruikersinterface Swagger via de `/swagger/ui` eindpunt.  Als u besluit om verificatie vereist in uw hele toepassing, Swagger, treedt een fout.  Kies voor de beste resultaten toe te staan dat niet-geverifieerde aanvragen tot en met in de verificatie van Azure App Service / autorisatie-instellingen, klikt u vervolgens control verificatie met behulp van de `table.access` eigenschap.

U kunt ook toevoegen de optie Swagger voor uw `azureMobile.js` bestand als u wilt dat alleen Swagger ondersteuning bij het ontwikkelen van lokaal.

## <a name="a-namepushpush-notifications"></a><a name="push">Push-meldingen

Mobile-Apps is geïntegreerd met Azure melding Hubs zodat u kunt gerichte push-meldingen verzenden naar miljoenen apparaten voor alle primaire platforms. U kunt met behulp van de melding Hubs push-meldingen verzenden naar iOS, Android en Windows-apparaten. Voor meer informatie over alle kunt doen met melding Hubs, Zie [Overzicht van de melding Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Hoe u: verzenden push-meldingen

De volgende code ziet u hoe u een melding push uitzenden naar geregistreerde iOS-apparaten verzenden via het push-object:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Maakt een sjabloon push registratie van de client, kunt u in plaats daarvan een sjabloon push-bericht verzenden naar apparaten op alle ondersteunde platforms. De volgende code ziet u hoe u een melding sjabloon verzendt:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Hoe u: push-meldingen verzenden naar een geverifieerde gebruiker-labels gebruiken

Als een geverifieerde gebruiker voor push-meldingen registreert, wordt automatisch een gebruikers-ID-tag toegevoegd aan de registratie. U kunt met behulp van dit label, push-meldingen verzenden naar alle apparaten die een specifieke gebruiker zijn geregistreerd. De volgende code wordt de beveiligings-id van de gebruiker die de aanvraag en stuurt een melding van de push sjabloon aan elke apparaatregistratie voor die gebruiker:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Wanneer u registreert voor push-meldingen uit een geverifieerde client, zorg dat de verificatie is voltooid voordat u probeert de registratie.

## <a name="CustomAPI"></a>Aangepaste API 's

###  <a name="howto-customapi-basic"></a>Hoe u: een aangepaste API definiëren


Naast de toegang tot de API via het eindpunt /tables, kan aangepaste API dekking worden verstrekt door Azure Mobile-Apps.  Aangepaste API's zijn gedefinieerd in een soortgelijke manier om de tabeldefinities en toegang hebt tot dezelfde faciliteiten, met inbegrip van verificatie.

Als u wilt de verificatie van de App-Service met een aangepaste-API gebruiken, moet u eerst verificatie van de App-Service configureren in de [portal van Azure] .  Controleer de configuratie-handleiding voor de identiteitsprovider die u wilt gebruiken voor meer informatie over het configureren van verificatie in een App-Service van Azure:

- [Het configureren van Azure Active Directory-verificatie]
- [Het configureren van Facebook-verificatie]
- [Het configureren van Google-verificatie]
- [Het configureren van Microsoft Authentication]
- [Het configureren van Twitter-verificatie]

Aangepaste API's zijn gedefinieerd in de grotendeels net zoals als de API tabellen.

1. Een **api** -map maken
2. Maak een API definitie JavaScript-bestand in de adreslijst **api** .
3. Gebruik de methode importeren naar de map **api** importeren.

Hier ziet u de definitie van prototype api op basis van de steekproef van de basic-app die we eerder hebt gebruikt.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

U gaat nu een voorbeeld API die resulteert in de datum van de server met behulp van de methode _Date.now()_ .  Dit is het api/date.js-bestand:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Elke parameter is een van de standaard RESTful bewerkingen - ophalen, bericht, PATCH of verwijderen.  De methode is een standaard [ExpressJS Middleware] -functie die de vereiste uitvoer stuurt.

### <a name="howto-customapi-auth"></a>Hoe u: verificatie vereisen voor toegang tot een aangepaste API

Azure Mobile-Apps SDK implementeert verificatie op dezelfde manier voor het eindpunt van de tabellen en de aangepaste API's.  Als wilt toevoegen verificatie tot de API in de vorige sectie ontwikkeld, voegt u een **access** -eigenschap:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

U kunt ook verificatie op specifieke bewerkingen opgeven:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Het dezelfde token die wordt gebruikt voor het eindpunt van de tabellen moet worden gebruikt voor aangepaste API's waarvoor verificatie is vereist.

### <a name="howto-customapi-auth"></a>Hoe u: grote bestandsuploads verwerken

De [hoofdtekst-parser middleware](https://github.com/expressjs/body-parser) SDK van Azure Mobile-Apps gebruikt om te accepteren en de inhoud van de hoofdtekst in uw indiening decoderen.  U kunt vooraf hoofdtekst-parser accepteer groter bestandsupload configureren:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Het bestand is base-64-codering vóór de overdracht.  Hiermee wordt de grootte van de werkelijke upload verhoogd (en dus de grootte u moet u rekening houden).

### <a name="howto-customapi-sql"></a>Hoe u: aangepaste SQL-instructies uitvoeren

De SDK van Azure mobiele Apps voor toegang tot de volledige Context via het verzoekobject, zodat u eenvoudig met parameters SQL-instructies aan de gedefinieerde gegevensprovider uitvoeren:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Foutopsporing, eenvoudig tabellen en eenvoudig API 's

### <a name="howto-diagnostic-logs"></a>Hoe u: fouten opsporen, vaststellen en oplossen van Azure Mobile-apps

De Azure App-Service biedt diverse foutopsporing en probleemoplossing van technieken voor Node.js-toepassingen.
Raadpleeg de volgende artikelen voor het oplossen van problemen handig uw backend Node.js Mobile aan de slag:

- [Een Azure-App-Service controleren]
- [Diagnostische gegevens vastleggen in Azure App-Service]
- [Problemen met een Azure App-Service in Visual Studio]

Node.js toepassingen hebben toegang tot een breed scala van diagnostische logboeken hulpmiddelen.  Intern, gebruikt de Azure Mobile-Apps Node.js SDK [Winston] voor diagnostische gegevens vastleggen.  Logboekregistratie is standaard ingeschakeld doordat foutopsporingsmodus of door in te stellen van de instelling van de app **MS_DebugMode** op waar in de [portal van Azure]. Gegenereerde logboeken worden weergegeven in de diagnostische logboeken op de [Azure-portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Hoe u: werken met eenvoudige tabellen in de portal van Azure

Eenvoudig tabellen in de portal kunnen u maken en werken met tabellen rechts in de portal. U kunt zelfs tabelbewerkingen met de App-Service-Editor bewerken.

Wanneer u **gemakkelijk tabellen** in de instellingen van uw back-end-site klikt, kunt u toevoegen, wijzigen of verwijderen van een tabel. U ziet ook de gegevens in de tabel.

![Werken met eenvoudige tabellen](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

De volgende opdrachten zijn beschikbaar op de balk met opdrachten voor een tabel:

+ **Machtigingen wijzigen** - wijzigen van de machtiging voor lezen, invoegen, bijwerken en verwijderen op de tabel. 
  Opties zijn om anonieme toegang te krijgen, is verificatie vereist, of alle toegang tot de bewerking uitschakelen. 
+ **Bewerken van script** - de script-bestand voor de tabel, wordt in de App Service Editor geopend.
+ **Beheren schema** - toevoegen of kolommen verwijderen of wijzigen van de tabelindex.
+ **Tabel wissen** - kapt een bestaande tabel af als u alle gegevensrijen, maar het schema blijft ongewijzigd.
+ **Rijen verwijderen** - verwijderen afzonderlijke rijen met gegevens.
+ **Weergave logboeken streaming** - maakt u verbinding met de streaming log-service voor uw site.

###<a name="work-easy-apis"></a>Hoe u: werken met eenvoudige API's in de portal van Azure

Eenvoudig API's in de portal kunt u maken en werken met aangepaste API's rechtstreeks in de portal. API-scripts met de App-Service-Editor, kunt u bewerken.

Wanneer u **Eenvoudig API's** in uw backend site-instellingen klikt, kunt u toevoegen, wijzigen of verwijderen van een aangepaste API-eindpunt.

![Werken met eenvoudige API 's](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Klik in de portal kunt u de access-machtigingen voor een bepaalde HTTP-actie wijzigen, de API-script-bestand in de App Service Editor bewerken of de streaming logboeken weergeven.

###<a name="online-editor"></a>Hoe u: code in de App Service Editor bewerken

De portal van Azure kunt u uw Node.js backend scriptbestanden in de App Service Editor bewerken zonder dat u moet het project te downloaden naar uw lokale computer. Om scriptbestanden te bewerken in de online-editor:

1. Klik in de Mobile-App backend blade, op **alle instellingen** > **eenvoudig tabellen** of **Eenvoudig API's**, klikt u op een tabel of de API en klik op **script bewerken**. Het scriptbestand wordt geopend in de App Service Editor.

    ![Editor voor App-Service](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Uw wijzigingen aanbrengen in de codebestand in de online-editor. Wijzigingen worden automatisch opgeslagen terwijl u typt.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Snelstartgids voor android-Client]: app-service-mobile-android-get-started.md
[Apache Cordova Client QuickStart]: app-service-mobile-cordova-get-started.md
[iOS-Client QuickStart]: app-service-mobile-ios-get-started.md
[QuickStart Xamarin.iOS-Client]: app-service-mobile-xamarin-ios-get-started.md
[QuickStart Xamarin.Android-Client]: app-service-mobile-xamarin-android-get-started.md
[QuickStart Xamarin.Forms-Client]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store-Client QuickStart]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[offline gegevens synchroniseren]: app-service-mobile-offline-data-sync.md
[Het configureren van Azure Active Directory-verificatie]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Het configureren van Facebook-verificatie]: app-service-mobile-how-to-configure-facebook-authentication.md
[Het configureren van Google-verificatie]: app-service-mobile-how-to-configure-google-authentication.md
[Het configureren van Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Het configureren van Twitter-verificatie]: app-service-mobile-how-to-configure-twitter-authentication.md
[Implementatiehandleiding van Azure App-Service]: ../app-service-web/web-sites-deploy.md
[Een Azure-App-Service controleren]: ../app-service-web/web-sites-monitor.md
[Diagnostische gegevens vastleggen in Azure App-Service]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Problemen met een Azure App-Service in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Geef het knooppunt-versie]: ../nodejs-specify-node-version-azure-apps.md
[Knooppunt modules gebruiken]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Toegezegd]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Voorbeeld van de basicapp op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[Voorbeeld van de taak op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[de map Samples op GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js hulpprogramma's voor 1.1 voor Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js pakket]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
