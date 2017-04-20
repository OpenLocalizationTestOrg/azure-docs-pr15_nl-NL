<properties 
    pageTitle="Een Web-App maakt in Azure App-Service met behulp van de Azure-SDK voor Java" 
    description="Informatie over het maken van een Web-App op Azure App-Service programmacode met de SDK Azure for Java." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Een Web-App maakt in Azure App-Service met behulp van de Azure-SDK voor Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>Overzicht

Deze procedure ziet u hoe u een Azure SDK for Java-toepassing die een Web-App in [Azure App Service][]maakt maken en implementeren van een toepassing toe. Deze bestaat uit twee delen:

- Deel 1 laat zien hoe u een Java-toepassing die Hiermee maakt u een web-app kunt maken.
- Deel 2 ziet u het maken van een eenvoudige JSP "Hallo wereld"-toepassing en klik vervolgens gebruiken een FTP-client aan code implementeren naar App Service.


## <a name="prerequisites"></a>Vereisten voor

### <a name="software-installations"></a>Software-installaties

De code van de toepassing AzureWebDemo in dit artikel is geschreven met behulp van Azure Java SDK 0.7.0, die u kunt installeren met behulp van het [Installatieprogramma van de Web-Platform][] (WebPI). Daarnaast moet u de meest recente versie van de [Azure Toolkit voor Eclips][]. Nadat u de SDK hebt geïnstalleerd, de afhankelijkheden in uw project Eclips **Index bijwerken** door in te voeren **Maven opslagplaatsen**bijwerken en klik opnieuw om de meest recente versie van elke pakket in het venster **afhankelijkheden** toe te voegen. U kunt controleren of de versie van de geïnstalleerde software in Eclips door te klikken op **Help > Installatiedetails**; u moet minimaal beschikken over de volgende versies:

- Pakket voor Microsoft Azure-bibliotheken voor Java 0.7.0.20150309
- Eclips IDE voor Java EE ontwikkelaars 4.4.2.20150219


### <a name="create-and-configure-cloud-resources-in-azure"></a>Maken en configureren van Cloud Resources in Azure wordt aangegeven

Voordat u deze procedure begint, moet u een geldig Azure abonnement hebt en een standaard AD (Active Directory) op Azure instellen.


### <a name="create-an-active-directory-ad-in-azure"></a>Maken van een Active Directory (AD) in Azure wordt aangegeven

Als u nog een AD (Active Directory) voor uw abonnement op Azure, meld u aan bij de [portal van Azure klassieke][] met uw Microsoft-account. Als u meerdere abonnementen hebt, klikt u op **abonnementen** en selecteert u de standaardmap voor het abonnement dat u wilt gebruiken voor dit project. Klik vervolgens op **toepassen** om te schakelen naar de weergave van die abonnement.

1. Selecteer **Active Directory** in het menu aan de linkerkant. **Klikt u op nieuwe > Directory > aangepaste maken**.

2. Selecteer in de **Adreslijst toevoegen**, **Nieuwe map maken**.

3. Voer in het vak **naam**de naam van een map.

4. Voer in het **domein**, een domeinnaam. Dit is een eenvoudige domeinnaam die is opgenomen al dan niet standaard met uw adreslijst; Dit is het formulier `<domain_name>.onmicrosoft.com`. U kunt een naam geven op basis van de mapnaam of een andere domeinnaam dat u eigenaar bent. U kunt een andere domeinnaam die uw organisatie al gebruikmaakt van later, kunt toevoegen.

5. Selecteer uw land in **land of regio**.

Zie [Wat is een Azure AD-map][]voor meer informatie over AD?


### <a name="create-a-management-certificate-for-azure"></a>Een certificaat Management voor Azure maken

De SDK Azure for Java gebruikt management certificaten om te verifiëren met Azure-abonnement. Hierna ziet u X.509 v3-certificaten die u gebruiken om te verifiëren van een clienttoepassing die gebruikmaakt van de Service Management API fungeert namens de eigenaar van het abonnement abonnement bronnen beheren.

De code in deze procedure wordt een zelfondertekend certificaat gebruikt om te verifiëren met Azure. Voor deze procedure moet u een digitaal certificaat maken en uploadt dit naar de [portal van Azure klassieke][] vooraf. Dit omvat de volgende stappen:

- Een PFX-bestand dat staat voor het certificaat genereren en lokaal opslaan.
- Genereer een management-certificaat (CER-bestand) van het PFX-bestand.
- Het CER-bestand uploaden naar uw Azure-abonnement.
- Het PFX-bestand converteren naar JKS, omdat Java die opmaak gebruikt om te verifiëren met certificaten.
- Schrijf de verificatiecode van de toepassing, die naar het lokale JKS-bestand verwijst.

Wanneer u deze procedure hebt voltooid, het certificaat CER zich bevinden in uw Azure-abonnement en het certificaat JKS zich bevinden op uw lokale harde schijf. Zie voor meer informatie over het beheer van certificaten, [maken en uploaden van een certificaat Management voor Azure][].


#### <a name="create-a-certificate"></a>Een digitaal certificaat maken

Als u wilt uw eigen zelfondertekend certificaat maken, opent u een opdrachtenconsole op uw besturingssysteem en voer de volgende opdrachten.

> **Notitie:**  De computer waarop u deze opdracht uitvoert, moet de JDK is geïnstalleerd. Het pad naar het hulpprogramma Keytool gebruikt is ook, afhankelijk van de locatie waar u de JDK installeren. Zie voor meer informatie [sleutel en certificaat Beheerhulpmiddel (hulpprogramma Keytool gebruikt)][] in de online Java-documenten.

De .pfx-bestand maken:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

De .cer-bestand maken:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

waarbij geldt:

- `<java-install-dir>`is het pad naar de map waarin u Java hebt geïnstalleerd.
- `<keystore-id>`is de keystore item-id (bijvoorbeeld `AzureRemoteAccess`).
- `<cert-store-dir>`het pad naar de map waarin u wilt opslaan van certificaten is (bijvoorbeeld `C:/Certificates`).
- `<cert-file-name>`is de naam van het certificaatbestand (bijvoorbeeld `AzureWebDemoCert`).
- `<password>`is het wachtwoord dat u wilt beveiligen van het certificaat; het moet ten minste 6 tekens bevatten. Hoewel dit wordt niet aanbevolen, kunt u geen wachtwoord invoeren.
- `<dname>`is de X.500 DN-naam moet worden gekoppeld aan de alias en wordt gebruikt als de uitgever en het onderwerp velden in het zelfondertekend certificaat.

Zie voor meer informatie [maken en uploaden van een certificaat Management voor Azure][].


#### <a name="upload-the-certificate"></a>Het certificaat uploaden

Als u wilt een zelfondertekend certificaat uploaden naar Azure, Ga naar de pagina **Instellingen** in de klassieke portal en klik op het tabblad **Management certificaten** . Klik op **uploaden** onder aan de pagina en navigeer naar de locatie van het CER-bestand dat u hebt gemaakt.


#### <a name="convert-the-pfx-file-into-jks"></a>Het PFX-bestand converteren naar JKS

In de opdrachtprompt van Windows (met als beheerder), cd naar de map met de certificaten en voer de volgende opdracht, waar `<java-install-dir>` is de map waarin u Java op uw computer hebt geïnstalleerd:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Wanneer u wordt gevraagd, voert u het wachtwoord van de bestemming keystore; Dit is het wachtwoord voor het bestand JKS.

2. Wanneer u wordt gevraagd, voert u het wachtwoord van de bron keystore; Dit is het wachtwoord die u hebt opgegeven voor het PFX-bestand.

De twee wachtwoorden hebben geen moeten overeenkomen. Hoewel dit wordt niet aanbevolen, kunt u geen wachtwoord invoeren.


## <a name="build-a-web-app-creation-application"></a>Een Web App maken-servicetoepassing maken

### <a name="create-the-eclipse-workspace-and-maven-project"></a>De werkruimte Eclips en Maven Project maken

In dit gedeelte maakt u een werkruimte en een project Maven voor de web-app maken toepassing, met de naam AzureWebDemo.

1. Maak een nieuwe Maven-project. Klik op **Bestand > Nieuw > Maven Project**. Selecteer **een eenvoudige project maken** en **de werkruimte standaardlocatie gebruiken**in **Nieuwe Maven Project**.

2. Klik op de tweede pagina van de **Nieuwe Maven Project**, geef het volgende:

    - Groeps-ID:`com.<username>.azure.webdemo`
    - Onderdeel-ID: AzureWebDemo
    - Versie: 0.0.1-SNAPSHOT
    - Het verpakken: oppervlak
    - Naam: AzureWebDemo

    Klik op **Voltooien**.

3. Open het nieuwe project pom.xml bestand in Projectverkenner. Selecteer het tabblad **afhankelijkheden** . Dit is een nieuw project, staan geen pakketten vermeld nog.

4. Open de Maven opslagplaatsen. **Klik op Venster > weergave weergeven > andere > Maven > Maven opslagplaatsen** en klik op **OK**. De weergave **Maven opslagplaatsen** wordt weergegeven onder aan de IDE.

5. **Globale opslagplaatsen**openen, met de rechtermuisknop op de **centrale** opslagplaats en selecteer **Index opnieuw opbouwen**.

    ![][1]
    
    Deze stap kan enkele minuten duren afhankelijk van de snelheid van de verbinding. Wanneer de index wordt opnieuw gemaakt, ziet u de Microsoft Azure-pakketten in de **centrale** opslagplaats voor Maven.

6. In **afhankelijkheden**, klikt u op **toevoegen**. Voer in **De groep Enter ID...** `azure-management`. Selecteer de pakketten voor grondtal beheer en beheer van de App Service Web Apps:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Notitie:** Als u de afhankelijkheden na een nieuwe versie bijwerkt, moet u elk van de afhankelijkheden in deze lijst opnieuw toe te voegen.
    > Nadat u klikt u op **toevoegen** en elke afhankelijkheid selecteert, wordt deze weergegeven met het nieuwe versienummer in de lijst **afhankelijkheden** .

Klik op **OK**. De Azure-pakketten worden vervolgens weergegeven in de lijst met **afhankelijkheden** .


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Java-Code voor het maken van een Web-App door te bellen van de Azure SDK schrijven

Schrijf vervolgens de code die roept API's in de SDK Azure for Java om de App Service WebApp te maken.

1. Maak een Java-klasse aanduiding van de hoofdvermelding punt-code. Klik in de Projectverkenner met de rechtermuisknop op het projectknooppunt en selecteer **Nieuw > Class**.

2. In de **Nieuwe Java-klasse**, naam op voor de klas `WebCreator` en schakel het selectievakje **openbare statische void Hoofdgegeven** . De selecties ziet er als volgt uit:

    ![][2]

3. Klik op **Voltooien**. Het bestand WebCreator.java wordt weergegeven in Projectverkenner.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Bellen van de Azure API als u wilt maken van een App Service Web-App


#### <a name="add-necessary-imports"></a>Nodig invoer toevoegen

In WebCreator.java, toevoegen de volgende invoer; deze invoer bieden toegang tot klassen in de management bibliotheken voor het gebruik van Azure-API's:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>De klasse hoofdvermelding point definiëren

De belangrijkste klasse omdat het doel van de toepassing AzureWebDemo is het opzetten van een App-Service Web-App, een naam voor deze toepassing `WebAppCreator`. Deze klasse biedt de hoofdvermelding punt-streepjescode die door de API Azure-Service Management om te maken van de WebApp-oproepen.

Voeg de volgende parameterdefinities voor de WebApp en webspace. Moet u uw eigen Azure abonnement-ID en certificaat informatie te verstrekken.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

waarbij geldt:

- `<subscription-id>`is het Azure abonnements-ID die u wilt maken van de resource.
- `<certificate-store-path>`is het pad en de bestandsnaam naar het bestand JKS in het telefoonboek van uw lokale certificaat store. Bijvoorbeeld `C:/Certificates/CertificateName.jks` voor Linux en `C:\Certificates\CertificateName.jks` voor Windows.
- `<certificate-password>`is het wachtwoord die u hebt opgegeven toen u uw JKS-certificaat hebt gemaakt.
- `webAppName`een naam die u kiest, kan zijn de naam van deze procedure wordt gebruikt `WebDemoWebApp`. De volledige domeinnaam is de `webAppName` met de `domainName` toegevoegd, dus in dit geval het volledige domein is `webdemowebapp.azurewebsites.net`.
- `domainName`moeten worden opgegeven, zoals hierboven.
- `webSpaceName`moet een van de waarden die zijn gedefinieerd in de klas [WebSpaceNames][] .
- `appServicePlanName`moeten worden opgegeven, zoals hierboven.

> **Notitie:** Telkens wanneer u bij het uitvoeren van deze toepassing, moet u de waarde van wijzigen `webAppName` en `appServicePlanName` (of verwijderen van de web-app op de Portal Azure) voordat u de toepassing opnieuw uitvoert. Uitvoering, anders mislukt omdat dezelfde resource al op Azure bestaat.


#### <a name="define-the-web-creation-method"></a>Deze methode maken definiëren

Definieer vervolgens een methode voor het maken van de web-app. Deze methode `createWebApp`, de parameters van de web-app en de webspace. Ook wordt gemaakt en de client voor het beheer van App Service Web Apps, die is gedefinieerd door het object [WebSiteManagementClient][] configureert. De client management is heel belangrijk voor het maken van Web Apps. Deze biedt RESTful webservices waarmee toepassingen voor het beheren van WebApps (bewerkingen uitvoert, zoals maken, bijwerken en verwijderen) door de ondersteuning voor de API voor het servicebeheer.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

De code de HTTP-status van het antwoord dat aangeeft slagen of mislukken wordt uitgevoerd en als dit lukt, wordt de naam van de gemaakte WebApp uitvoeren.


#### <a name="define-the-main-method"></a>De methode main() definiëren

Geef de main() methode-streepjescode die door createWebApp() als u wilt maken van de WebApp-oproepen.

Tot slot bellen `createWebApp` uit `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Voer de toepassing en controleer of web-app maken

Om te bevestigen dat de toepassing wordt uitgevoerd, klikt u op **uitvoeren > uitvoeren**. Wanneer de toepassing is voltooid uitgevoerd, ziet u de volgende uitvoer in de console Eclips:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Meld u aan bij de portal van Azure klassieke en klik op **Web Apps**. De nieuwe web-app moet worden weergegeven in de lijst met Web Apps binnen enkele minuten.


## <a name="deploying-an-application-to-the-web-app"></a>Een toepassing naar de Web-App implementeren

Nadat u hebt AzureWebDemo uitvoeren en de nieuwe WebApp hebt gemaakt, meld u aan bij de portal klassieke op **Web Apps**en selecteer **WebDemoWebApp** in de lijst met **Web Apps** . In de web-app dashboard-pagina, klikt u op **Bladeren** (of klik op de URL, `webdemowebapp.azurewebsites.net`) om te navigeren naar deze. U ziet een pagina lege tijdelijke aanduiding, omdat er geen inhoud is gepubliceerd naar de web-app nog.

U wordt vervolgens een "Hallo wereld"-toepassing bouwen en het dashboard implementeren naar de web-app.


### <a name="create-a-jsp-hello-world-application"></a>Een servicetoepassing JSP Hallo allemaal maken

#### <a name="create-the-application"></a>De toepassing maken

Om aan te tonen het implementeren van een toepassing op het web, ziet de volgende procedure u hoe u een eenvoudige "Hallo wereld" Java-toepassing maken en uploaden naar de Web-App voor App-Service uw toepassing is gemaakt.

1. Klik op **Bestand > Nieuw > dynamische webproject**. Noem deze `JSPHello`. U hoeft niet te wijzigen van de andere instellingen in dit dialoogvenster. Klik op **Voltooien**.

    ![][3]

2. In Projectverkenner uitvouwen van het project **JSPHello** , met de rechtermuisknop op **webinhoud**en klik op **Nieuw > JSP bestand**. Klik in het dialoogvenster Nieuw JSP-bestand een naam geven het nieuwe bestand `index.jsp`. Klik op **volgende**.

3. Selecteer **Nieuwe JSP-bestand (html)** in het dialoogvenster **JSP sjabloon selecteren** en op **Voltooien**.

4. In index.jsp, voeg de volgende code in de `<head>` en `<body>` secties markeren:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>De toepassing Hallo allemaal uitvoeren in localhost

Voordat u deze toepassing uitvoert, moet u enkele eigenschappen configureren.

1. Met de rechtermuisknop op het project **JSPHello** en selecteer **Eigenschappen**.

2. In het dialoogvenster **Eigenschappen** : Selecteer **Java bouwen pad**, selecteer het tabblad **volgorde en exporteren** , **JRE systeembibliotheek**en klik op **Omhoog** om deze te verplaatsen naar het begin van de lijst.

    ![][4]

3. Ook in het dialoogvenster **Eigenschappen** : Selecteer **Runtimes gericht** en klikt u op **Nieuw**.

4. Selecteer een server zoals **Apache Tomcat v7.0** in het dialoogvenster **Nieuwe Server Runtime-omgeving** en klik op **volgende**. Klik in het dialoogvenster **Tomcat Server** stelt u de gewenste **naam** voor `Apache Tomcat v7.0`, en stel **Tomcat installatiemap** op de map waarin u de versie van Tomcat server die u wilt gebruiken, hebt geïnstalleerd.

    ![][5]

    Klik op **Voltooien**.

5. Ga terug naar de pagina **Gericht Runtimes** van het dialoogvenster **Eigenschappen** . Selecteer **Apache Tomcat v7.0**en klik op **OK**.

    ![][6]

6. Klik in het menu Eclips **uitvoeren** op **uitvoeren**. Klik in het dialoogvenster **Uitvoeren als** Selecteer **worden uitgevoerd op de Server**. Selecteer in het dialoogvenster **worden uitgevoerd op Server** **Tomcat v7.0 Server**:

    ![][7]

    Klik op **Voltooien**.

7. Wanneer de toepassing wordt uitgevoerd, ziet u de **JSPHello** pagina weergegeven in een venster localhost van Eclips (`http://localhost:8080/JSPHello/`), het volgende bericht wordt weergegeven:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>De toepassing exporteren als een WAR

De web project-bestanden exporteren als een web-archiefbestand (WAR), zodat u het dashboard naar de web-app implementeren kunt. De volgende bestanden van de web-project bevinden zich in de map webinhoud:

    META-INF
    WEB-INF
    index.jsp

1. Met de rechtermuisknop op de map webinhoud en selecteer **exporteren**.

2. Klik in het dialoogvenster **Selecteer exporteren** op **Web > WAR** bestand en klik op **volgende**.

3. Klik in het dialoogvenster **WAR exporteren** de map src te selecteren in het huidige project en ook de naam van het WAR-bestand aan het einde. Bijvoorbeeld:

    `<project-path>/JSPHello/src/JSPHello.war`

Zie voor meer informatie over het implementeren van WAR bestanden [toevoegen een Java-toepassing tot Azure App Service Web Apps](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>De Hallo wereld-toepassing met FTP implementeren

Selecteer een derde partij FTP-client voor het publiceren van de toepassing. Deze procedure wordt beschreven twee opties: de Kudu-console ingebouwd in Azure; en FileZilla, een populaire hulpmiddel met een handige, grafische gebruikersinterface.

> **Notitie:** De Toolkit Azure voor Eclips implementatie opslag accounts en cloudservices ondersteunt, maar niet ondersteunt momenteel implementatie tot Webapps. U kunt opslagruimte accounts en cloudservices van een Project van de implementatie van Azure gebruiken zoals beschreven in [een Hallo wereld-toepassing voor Azure in Eclips maken](http://msdn.microsoft.com/library/azure/hh690944.aspx), maar niet op het WebApps implementeren. Andere methoden zoals FTP of GitHub bestanden overbrengen naar uw web-app gebruiken.

> **Notitie:** We raden niet met FTP vanaf de opdrachtprompt van Windows (het FTP.EXE hulpprogramma die wordt geleverd met Windows). FTP-clients die gebruikmaken van actieve FTP, zoals FTP.EXE, mislukt vaak om te werken via firewalls. Actieve FTP Hiermee geeft u een interne LAN-e-mailadres, waaraan een FTP-server waarschijnlijk geen verbinding.

Zie de volgende onderwerpen voor meer informatie over de implementatie van een App Service-web-app met FTP:

- [Met een FTP-hulpprogramma implementeren](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Implementatie referenties instellen

Zorg ervoor dat u de toepassing **AzureWebDemo** om een WebApp te maken hebt uitgevoerd. U wordt bestanden overbrengen naar deze locatie.

1. Meld u aan bij de portal van klassieke en klik op **Web Apps**. Zorg ervoor dat **WebDemoWebApp** weergegeven in de lijst met WebApps en zorg ervoor dat deze wordt uitgevoerd. Klik op **WebDemoWebApp** om de **Dashboard** -pagina te openen.

2. Klik op de pagina **Dashboard** , onder **Snelle bekijken**, de op **uw implementatie-referenties instellen** (als u al implementatie referenties, dit leest **uw implementatie-referenties opnieuw instellen**).

    Implementatie referenties zijn gekoppeld aan een Microsoft-account. U moet een gebruikersnaam en wachtwoord die u gebruiken kunt om te implementeren met cijfer en FTP opgeven. U kunt deze referenties gebruiken om te implementeren naar een web-app in alle Azure abonnementen die is gekoppeld aan uw Microsoft-account. Referenties cijfer en FTP-implementatie in het dialoogvenster en opnemen van de gebruikersnaam en wachtwoord voor toekomstig gebruik.


#### <a name="get-ftp-connection-information"></a>Informatie over de databaseverbinding FTP ophalen

Als u FTP wilt implementeren toepassingsbestanden naar de zojuist gemaakte WebApp, moet u informatie over de databaseverbinding verkrijgen. Er zijn twee manieren om verbindingsinformatie te verkrijgen. Een manier is te bezoeken van **de web-app dashboardpagina;** de andere manier is om te downloaden van de webversie van de Apps publiceren profiel. Het profiel publiceren is een XML-bestand waarmee gegevens, zoals FTP-host naam en de aanmeldingsgegevens referenties voor uw web-apps in Azure App-Service. U kunt deze gebruikersnaam en wachtwoord gebruiken om te implementeren naar een web-app in alle abonnementen die is gekoppeld aan de Azure-account, niet alleen dit item.

FTP-verbindingsgegevens van de web-app blade in de [Portal van Azure][]verkrijgen:

1. Klik onder **Essentials**, zoeken en kopieer de **FTP-hostnaam**. Dit is een vergelijkbaar met URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. Klik onder **Essentials**, zoeken en kopiëren **FTP-implementatie gebruikersnaam**. Dit heeft de notatie *webappname\deployment-username*; bijvoorbeeld `WebDemoWebApp\deployer77`.

Als u informatie over de databaseverbinding FTP uit het profiel publiceren:

1. Klik in de web-app blade, op **Get publiceren profiel**. Hiermee wordt een .publishsettings-bestand downloaden naar uw lokale harde schijf.

2. Open het bestand .publishsettings in een XML-editor of teksteditor en Ga naar de `<publishProfile>` element die bevat `publishMethod="FTP"`. Deze ziet er als volgt te werk:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Houd er rekening mee dat de web-app `publishProfile` instellingen toewijzen aan de sitebeheerder FileZilla instellingen als volgt:

- `publishUrl`is hetzelfde als **de naam van de FTP-host**, de waarde die u in **Host instelt**.
- `publishMethod="FTP"`betekent dat u **Protocol** ingesteld op **FTP - File Transfer Protocol**en **versleuteling** **simpel FTP**gebruiken.
- `userName`en `userPWD` sleutels voor de werkelijke gebruikersnaam en wachtwoord waarden die u hebt opgegeven wanneer u de implementatie-referenties opnieuw instellen. `userName`is hetzelfde als **implementatie / FTP-gebruiker**. Ze toewijzen aan **gebruikers** en **wachtwoord** in FileZilla.
- `ftpPassiveMode="True"`betekent dat de FTP-site wordt gebruikt voor passieve FTP-overdracht; Selecteer **passieve** op het tabblad **Instellingen overbrengen** .


#### <a name="configure-the-web-app-to-host-a-java-application"></a>De Web-App als host voor een Java-toepassing configureren

Voordat u de toepassing publiceert, moet u enkele configuratieinstellingen wijzigen zodat de web-app een Java-toepassing hosten kunt.

1. Klik in de klassieke portal gaat u naar de web-app **dashboardpagina's** en op **configureren**. Geef de volgende instellingen op de pagina **configureren** .

2. In **Java versie** de optie is standaard **uitgeschakeld**; Selecteer de Java-versie van de doelen van uw toepassing; bijvoorbeeld 1.7.0_51. Nadat u dit doet, ook voor zorgen dat **Web container** is ingesteld op een versie van Tomcat-Server.

3. In **Documenten standaard**index.jsp toevoegen en verplaatst u deze naar de bovenkant van de lijst. (De standaard-bestand voor de WebApps is hostingstart.html.)

4. Klik op **Opslaan**.


#### <a name="publish-your-application-using-kudu"></a>Uw-toepassing via Kudu publiceren

Een manier om te publiceren van de toepassing is via de Kudu foutopsporing-console in Azure ingebouwd. Kudu bekend is stabiele en consistent uitgevoerd met de App Service Web Apps en Tomcat-Server. U openen de console voor de web-app door te bladeren naar een URL van de volgende notatie:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Voor deze procedure zich het Kudu-console bevindt op de volgende URL; Ga naar deze locatie:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Selecteer in het bovenste menu **Foutopsporingsconsole > CMD**.

3. Ga in de opdrachtregel console naar `/site/wwwroot` (of klik op `site`, klikt u vervolgens `wwwroot` in de directoryweergave boven aan de pagina):

    `cd /site/wwwroot`

4. Nadat u hebt opgegeven **Java-versie**, moet Tomcat server een map webapps maken. Ga naar de map webapps in de opdrachtregel console:

    `mkdir webapps`

    `cd webapps`

5. Sleep JSPHello.war van `<project-path>/JSPHello/src/` en zet deze neer in de weergave van de map Kudu onder `/site/wwwroot/webapps`. Sleep niet deze naar het gebied "Sleep hier om te uploaden en zip" omdat Tomcat worden behouden.

  ![][8]

Eerste JSPHello.war wordt weergegeven in het gebied directory op zichzelf:

  ![][9]

In een korte tijdnotatie (waarschijnlijk minder dan 5 minuten) wordt Tomcat Server het WAR-bestand in een uitgepakt JSPHello directory unzip. Klik op de hoofdmap om te zien of index.jsp is uitgepakt en er gekopieerd. Als dat het geval is, gaat u terug naar de map webapps om te zien of de uitgepakte JSPHello map is gemaakt. Als u deze items niet ziet, wacht en herhaalt u.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Publiceer de toepassing met FileZilla (optioneel)

Een ander hulpmiddel die kunt u de toepassing publiceren is FileZilla, een populaire derden FTP-client met een handige, grafische gebruikersinterface. U kunt downloaden en installeren van FileZilla vanaf [http://filezilla-project.org/](http://filezilla-project.org/) als u niet deze hebt nog. Zie voor meer informatie over het gebruik van de client, de [FileZilla documentatie](https://wiki.filezilla-project.org/Documentation) en dit blogbericht op [FTP-Clients - deel 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Klik in FileZilla, op **Bestand > sitebeheerder**.
2. Klik op **Nieuwe Site**in het dialoogvenster **Sitebeheerder** . Een nieuwe, lege FTP-site wordt weergegeven in **Invoer selecteert** u een naam te geven waarin u wordt gevraagd. Voor deze procedure noem deze `AzureWebDemo-FTP`.

    Geef de volgende instellingen op het tabblad **Algemeen** :
    - **Host:** Voer de **Naam van de FTP-Host** die u hebt gekopieerd vanuit het dashboard.
    - **Poort:** (Laat dit leeg is, als dit een passieve doorverbinden is en de server bepaalt de poort.)
    - **Protocol:** FTP File Transfer Protocol
    - **Versleuteling:** Gewone FTP gebruiken
    - **Type aanmelden:** Normaal
    - **Gebruiker:** Voer de implementatie / FTP-gebruiker die u hebt gekopieerd vanuit het dashboard. Dit is de volledige FTP-gebruikersnaam, waarvoor het formulier *webappname\username*.
    - **Wachtwoord:** Voer het wachtwoord dat u hebt opgegeven toen u de implementatie-referenties instellen.

    Klik op het tabblad **Instellingen overbrengen** Selecteer **passieve**.

3. Klik op **verbinding maken**. Als geslaagd, FileZilla van console weer om een `Status: Connected` bericht en probleem een `LIST` opdracht voor een overzicht van de directory-inhoud.

4. Selecteer in het deelvenster **lokale** site de bronmap waarin het JSPHello.war-bestand zich bevindt. het pad moet ongeveer als volgt uit:

    `<project-path>/JSPHello/src/`

5. Selecteer de doelmap in het deelvenster **externe** site. Implementeert u het WAR-bestand naar de `webapps` directory in de hoofdmap van de web-app. Navigeer naar `/site/wwwroot`, met de rechtermuisknop op `wwwroot`, en selecteer **de map maken**. De map naam `webapps` en voert u deze map.

6. Overbrengen JSPHello.war naar `/site/wwwroot/webapps`. Selecteer JSPHello.war in de lijst met **lokale** bestanden, met de rechtermuisknop op dit en selecteer **uploaden**. U ziet deze worden weergegeven in `/site/wwwroot/webapps`.

7. Nadat u JSPHello.war gekopieerd naar de map webapps, Tomcat-Server automatisch wordt uitpakken (pak) de bestanden in de WAR-bestand. Hoewel Tomcat Server bijna onmiddellijk uitpakken begint, een lang kan duren tijd (mogelijk uren) voor de bestanden worden weergegeven in de FTP-client.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>De toepassing Hallo allemaal worden uitgevoerd op de Web-App

1. Nadat u hebt geüpload het WAR-bestand en geverifieerd of Tomcat server heeft gemaakt met een uitgepakt `JSPHello` directory, bladert u naar `http://webdemowebapp.azurewebsites.net/JSPHello` de toepassing wilt uitvoeren.

    > **Notitie:** Als u vanuit de klassieke-portal op **Bladeren** hebt geklikt, krijgt u mogelijk de webpagina standaard zeggen "deze webtoepassing Java gebaseerd is gemaakt." Mogelijk moet u de webpagina vernieuwen om te kunnen weergeven van de toepassing-uitvoer in plaats van de standaard-webpagina.

2. Wanneer de toepassing wordt uitgevoerd, ziet u een webpagina met de volgende uitvoer:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Azure resources opschonen

Deze procedure wordt gemaakt van een App Service web-app. U wordt gefactureerd voor de resource zo lang maken als deze bestaat. Tenzij u van plan bent om door te gaan met de web-app voor testdoeleinden of ontwikkeling, moet u rekening houden met stoppen of deze te verwijderen. Een web-app is gestopt wordt nog steeds een kleine boete oplopen, maar u kunt dit op elk gewenst moment starten. Een webapp verwijderen, wist u alle gegevens die u hebt geüpload naar deze.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure App-Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Platform Installer]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure Toolkit voor Eclips]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure klassieke portal]: https://manage.windowsazure.com
[Wat is een Azure AD-map]: http://technet.microsoft.com/library/jj573650.aspx
[Maken en uploaden van een certificaat Management voor Azure]: ../cloud-services/cloud-services-certs-create.md
[Sleutel en certificaat Beheerhulpmiddel (hulpprogramma Keytool gebruikt)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure-Portal]: https://portal.azure.com
