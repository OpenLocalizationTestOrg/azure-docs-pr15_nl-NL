<properties
    pageTitle="Een Java-web-app maakt in Azure App Service | Microsoft Azure"
    description="Deze zelfstudie ziet u hoe u een Java-web-app implementeren naar Azure App-Service."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Een Java-web-app maakt in Azure App-Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Deze zelfstudie wordt getoond hoe u een Java- [WebApp in Azure App Service] maken met behulp van de [Azure-Portal]. De Portal Azure is een webservice-interface die u gebruiken kunt voor het beheren van uw Azure resources.

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Microsoft Azure-account. Als u geen account hebt, kunt u [zich registreren voor een gratis proefversie]of [activeren van de voordelen van uw Visual Studio-abonnee] .
>
> Als u aan de slag met Azure App Service wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [De App-Service probeert]. Er, kunt u direct een tijdelijk starter in de browser maken in de App-Service; geen creditcard is vereist, en geen verplichtingen.

## <a name="java-application-options"></a>Java-Toepassingsopties

Er zijn verschillende manieren die kunt u een Java-toepassing in een App Service web-app instellen. 

1. Een app maken en klik vervolgens configureren **Toepassingsinstellingen**.

    App-Service biedt diverse Tomcat en Jetty versies, standaardconfiguratie. Als de toepassing die u moet hosten met een van de ingebouwde versies werkt, deze methode voor het instellen van een container web het eenvoudigst en is ideaal wanneer u wilt doen, is een war-bestand uploaden naar een container web. Voor deze methode kunt u een app maken in de Portal Azure en ga vervolgens naar het blad **Toepassingsinstellingen** voor de app uw versie van Java samen met de gewenste Java web container kiezen. Wanneer u deze methode te gebruiken zijn zowel Java en uw web-container uit Program Files worden uitgevoerd. De andere methoden plaatst u de container web en potentieel JVM op uw schijfruimte. Wanneer u dit model gebruikt, hebt u geen toegang voor het bewerken van bestanden in dit gedeelte van het bestandssysteem. Dit betekent dat u geen zaken uitvoeren als het bestand *server.xml* configureert of bibliotheekbestanden in de map */lib* plaatsen. Zie voor meer informatie de [maken en configureren van een WebApp Java](#appsettings) sectie verderop in deze zelfstudie.
    
2. Gebruik een sjabloon van Azure Marketplace.

    De Azure Marketplace bevat sjablonen die automatisch maken en configureren van Java-WebApps met Tomcat of Jetty web containers. De web-containers die de sjablonen maakt, worden geconfigureerd. Zie de sectie [met een Java-sjabloon van Azure Marketplace](#marketplace) van deze zelfstudie voor meer informatie.
  
3. Een app maken en klik vervolgens handmatig kopiëren en configuratiebestanden bewerkt 

    U kunt een aangepaste Java-toepassing die geen in een van de web-containers verstrekt door App Service implementeert hosten. Bijvoorbeeld:
    
    * Uw Java-toepassing vereist een versie van Tomcat of Jetty die rechtstreeks wordt niet ondersteund door de App Service of die beschikbaar zijn in de galerie.
    * Uw Java-toepassing HTTP-aanvragen wordt ingevoegd en wordt niet als een WAR implementeren naar een bestaande web container.
    * U wilt configureren van de container web helemaal zelf. 
    * U wilt gebruiken van een versie van Java die niet wordt ondersteund in App Service en uploadt dit zelf wilt.

    Voor zulke gevallen kunt u een app met behulp van de Azure-Portal maken en geef de juiste runtime-bestanden handmatig. In dit geval worden de bestanden die geteld ten opzichte van de ruimte de opslagquota voor uw App serviceplan. Zie [een aangepaste Java web-app naar Azure uploaden]voor meer informatie.

## <a name="portal"></a>Maken en configureren van een Java-web-app

In dit gedeelte leert hoe u een WebApp maken en configureren voor Java met het blad **Toepassingsinstellingen** van de portal.

1. Meld u aan bij de [Portal van Azure].

2. Klik op **Nieuw > Web + Mobile > Web App**.

    ![Nieuwe Web-App][newwebapp]

4. Voer een naam voor de web-app in het vak **WebApp** .

    Deze naam moet uniek zijn in het domein azurewebsites.net omdat de URL van de web-app {naam}. azurewebsites.net. Als de naam die u invoert niet uniek, is een rood uitroepteken wordt weergegeven in het tekstvak.

5. Selecteer een **Resourcegroep** of maak een nieuwe record.

    Zie voor meer informatie over resourcegroepen, [met behulp van de Azure-Portal als u wilt uw Azure resources beheren].

6. Selecteer een **App Service abonnement/locatie** of maak een nieuwe record.

    Zie [overzicht van de Azure App Service-abonnementen]voor meer informatie over App Service-abonnementen.

7. Klik op **maken**.

    ![WebApp maken][newwebapp2]
 
8. Wanneer de web-app is gemaakt, klikt u op **Web Apps > {uw web-app}**.
 
    ![Selecteer WebApp][selectwebapp]

9. Klik in het blad **WebApp** op **Instellingen**.

10. Klik op **instellingen van toepassing**.

11. Kies de gewenste **Java-versie**. 

12. Kies de gewenste **Java secundaire versie**. Als u een **Nieuw**selecteert, worden uw app de nieuwste secundaire versie die beschikbaar is in App Service voor die Java primaire versie gebruikt. Het **Nieuw** item is uniek voor Java-apps die zijn gemaakt op basis van de **instellingen van toepassing**. Als u uw Java-app vanuit de galerie maken, hebt u voor het beheren van uw eigen container en JVM wijzigingen. 

12. Kies de gewenste **Web container**. Als u een waarvan de containernaam met **Nieuw begint**selecteert, wordt de meest recente versie van die web container primaire versie die beschikbaar is in App Service uw app bewaard. 

    ![Web Container versies][versions]

13. Klik op **Opslaan**.

    Binnen enkele minuten worden uw web-app Java gebaseerde en geconfigureerd voor het gebruik van de web-container die u hebt geselecteerd.

14. Klik op **Web apps > {uw nieuwe WebApp}**.

15. Klik op de **URL** om te bladeren naar de nieuwe site.

    De pagina met webonderdelen bevestigt dat u een Java gebaseerde web-app hebt gemaakt.

## <a name="marketplace"></a>Gebruik een Java-sjabloon van Azure Marketplace

Hier vindt u het gebruik van de Azure Marketplace om een WebApp Java te maken. De dezelfde opbouw kan ook worden gebruikt om een Java gebaseerde mobile of API-app te maken. 

1. Meld u aan bij de [Portal van Azure]

2. Klik op **Nieuw > Marketplace**.

    ![Nieuwe Marketplace][newmarketplace]

3. Klik op **Web + Mobile**.

    Mogelijk moet u schuiven van links om te zien van het blad **Marketplace** waarin u **Web + Mobile**kunt selecteren.

4. Voer de naam van een Java-toepassingsserver, zoals **Apache Tomcat** of **Jetty**, in het vak tekst zoeken en druk op Enter.

5. Klik in de lijst met zoekresultaten op de Java-toepassingsserver.

    ![Mobiele Jetty Web][webmobilejetty]

6. Klik in het eerste **Apache Tomcat** of **Jetty** blad, klikt u op **maken**.

    ![Jetty Portal Blade][jettyblade]

7. Voer in het volgende **Apache Tomcat** of **Jetty** blad, een naam voor de web-app in het vak **WebApp** .

    Deze naam moet uniek zijn in het domein azurewebsites.net omdat de URL van de web-app {naam}. azurewebsites.net. Als de naam die u invoert niet uniek, is een rood uitroepteken wordt weergegeven in het tekstvak.

8. Selecteer een **Resourcegroep** of maak een nieuwe record.

    Zie voor meer informatie over resourcegroepen, [met behulp van de Azure-Portal als u wilt uw Azure resources beheren].

9. Selecteer een **App Service abonnement/locatie** of maak een nieuwe record.

    Zie [overzicht van de Azure App Service-abonnementen]voor meer informatie over App Service-abonnementen.

10. Klik op **maken**.

    ![Jetty Portal maken][jettyportalcreate2]

    In een korte tijdnotatie, meestal minder dan ongeveer een minuut eindigt Azure maken van de nieuwe web-app.

11. Klik op **Web apps > {uw nieuwe WebApp}**.

12. Klik op de **URL** om te bladeren naar de nieuwe site.

    ![Jetty URL][jettyurl]

    Tomcat wordt geleverd met een standaardset met pagina's. Als u Tomcat kiest, ziet u een pagina die vergelijkbaar is met het volgende voorbeeld.

    ![WebApp via Apache Tomcat][tomcat]

    Als u Jetty kiest, ziet u een pagina die vergelijkbaar is met het volgende voorbeeld. Jetty geen een reeks pagina, dus hier opnieuw de dezelfde JSP die wordt gebruikt voor een lege Java-site wordt gebruikt.

    ![WebApp via Jetty][jetty]

Nu dat u de web-app hebt gemaakt met een container app, raadpleegt u de [volgende stappen](#next-steps) sectie voor informatie over het uploaden van uw toepassing naar de web-app.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een Java-toepassingsserver uitgevoerd in uw web-app in Azure App-Service. Als u wilt uw eigen code implementeren naar de web-app, raadpleegt u [toepassing toevoegen of webpagina naar uw Java-web-app].

Zie voor meer informatie over het ontwikkelen van toepassingen Java in Azure wordt aangegeven, het [Java Developer Center].

<!-- URL List -->

[Een toepassing of een webpagina toevoegen aan uw Java-web-app]: ./web-sites-java-add-app.md
[Overzicht van het abonnement Azure App-Service]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure-Portal]: https://portal.azure.com/
[de voordelen van uw Visual Studio-abonnee activeren]: http://go.microsoft.com/fwlink/?LinkId=623901
[registreren voor een gratis proefversie]: http://go.microsoft.com/fwlink/?LinkId=623901
[Probeer de App-Service]: http://go.microsoft.com/fwlink/?LinkId=523751
[WebApp in Azure App-Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Java Developer Center]: /develop/java/
[Met behulp van de Azure-Portal voor het beheren van uw Azure resources]: ../azure-portal/resource-group-portal.md
[Een aangepaste Java-web-app uploaden naar Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
