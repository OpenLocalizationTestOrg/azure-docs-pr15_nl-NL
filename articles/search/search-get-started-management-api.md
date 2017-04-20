<properties 
    pageTitle="Aan de slag met Azure zoeken Management REST API | Microsoft Azure | De zoekservice gehoste cloud" 
    description="Uw gehoste cloud Azure zoekservice met een Management REST API beheren" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="get-started-with-azure-search-management-rest-api"></a>Aan de slag met Azure zoeken Management REST API
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST API](search-get-started-management-api.md)

Het beheer van Azure zoeken REST API is een programma alternatief voor het uitvoeren van beheertaken in de portal. Service management bewerkingen bevatten maken of verwijderen van de service, schaalbaarheid van de service en beheren van toetsen. Deze zelfstudie wordt geleverd met de clienttoepassing van een steekproef waarop u de API voor het servicebeheer. Daarnaast moet worden uitgevoerd in het voorbeeld in uw lokale ontwikkelomgeving volgende configuratiestappen uit.

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

- Visual Studio 2012 of 2013
- het voorbeeld client toepassing downloaden

In de loop van de zelfstudie is voltooid, twee services ingericht: Azure zoeken en Azure Active Directory (AD). Bovendien maakt u een AD-toepassing die vertrouwensrelatie tussen uw clienttoepassing en het eindpunt van de manager resource te worden ingesteld in Azure wordt aangegeven.

Moet u een Azure-account om te voltooien van deze zelfstudie.


##<a name="download-the-sample-application"></a>Downloaden van de steekproef-toepassing

Deze zelfstudie is gebaseerd op een Windows-console-toepassing geschreven in C#, die u kunt bewerken en uitvoeren in Visual Studio 2012 of 2013

U vindt de clienttoepassing op Github op [Azure zoeken .NET Management API Demo](https://github.com/Azure-Samples/search-dotnet-management-api/).


##<a name="configure-the-application"></a>De toepassing configureren

Voordat u de voorbeeldtoepassing uitvoeren kunt, moet u verificatie inschakelen zodat dat verzonden vanuit de clienttoepassing naar het eindpunt van de manager resource kunnen worden geaccepteerd. De verificatievereiste afkomstig is met [Azure resourcemanager](https://msdn.microsoft.com/library/azure/dn790568.aspx)die de basis vormt voor alle portal-gerelateerde bewerkingen aangevraagd via een API, met inbegrip van die betrekking hebben op zoekservice beheren. Het beheer van service API voor zoekprogramma's Azure is gewoon een uitbreiding van de Azure Resource Manager en kan dus overneemt bijbehorende afhankelijkheden.  

Azure resourcemanager vereist Azure Active Directory-service als de identiteitsprovider. 

Als u een toegangstoken waarmee aanvragen voor het resourcemanager hebt bereikt, bevat de clienttoepassing een codesegment dat u Active Directory belt. Het codesegment, plus de vereiste stappen voor het gebruik van het codesegment zijn geleend van dit artikel: [verificatie Azure resourcemanager aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx).

U kunt de instructies in de bovenstaande koppeling of gebruik de stappen in dit document als u liever de zelfstudie stap voor stap doorlopen.

In deze sectie, wordt u de volgende taken uitvoeren:

1. Maak een AD-service
1. Een AD-toepassing maken
1. De AD-toepassing configureren door te registreren meer informatie over de voorbeeld-clienttoepassing die u hebt gedownload
1. Laden van de steekproef-clienttoepassing met waarden die wordt gebruikt om te krijgen autorisatie voor de aanvragen

> [AZURE.NOTE] Deze koppelingen bieden achtergrond over het gebruik van Azure Active Directory voor het verifiëren van clientaanvragen voor de resourcemanager: [Azure resourcemanager](http://msdn.microsoft.com/library/azure/dn790568.aspx), [Azure resourcemanager verifiëren van aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx)en [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx).

###<a name="create-an-active-directory-service"></a>Maak een Active Directory-Service

1. Meld u aan bij de [Portal van Azure](https://manage.windowsazure.com).

2. Blader in het linker navigatiedeelvenster en klik op **Active Directory**.

4. Klik op **Nieuw** als u wilt openen **App Services** | **Active Directory**. In deze stap maakt u een nieuwe Active Directory-service. Deze service wordt gehost in de AD-toepassing die u een paar stappen vanaf nu wordt definiëren. Een nieuwe service maken Hiermee kunt u de zelfstudie vanuit andere toepassingen die u mogelijk al worden hosten in Azure wordt aangegeven.

5. Klik op **map** | **aangepaste maken**.

6. Voer een servicenaam, domein en geografische locatie. Het domein moet uniek zijn. Klik op het vinkje om te maken van de service.

     ![][5]

###<a name="create-a-new-ad-application-for-this-service"></a>Maak een nieuwe AD-toepassing voor deze service

1. Selecteer de "SearchTutorial" Active Directory-service die u zojuist hebt gemaakt.

2. Klik op het bovenste menu op **toepassingen**. 
 
3. Klik op **een App toevoegen**. Een AD-toepassing bevat informatie over de clienttoepassingen die deze wilt als een identiteitsprovider gebruiken worden.  
 
4. Kies **een toepassing het ontwikkelen van mijn organisatie toevoegen**. Deze optie biedt registratie-instellingen voor toepassingen die niet worden gepubliceerd naar de galerie toepassing. Aangezien de clienttoepassing geen deel van de toepassing-galerie uitmaakt, is dit het beste kiezen voor deze zelfstudie.

     ![][6]
 
5. Voer een naam, bijvoorbeeld "Azure-zoeken-Manager".

6. Kies **systeemeigen clienttoepassing** voor toepassingstype. Dit is juist voor de steekproef toepassing niet omzeilen. Dit gebeurt als een Windows-client (console)-toepassing, niet een webtoepassing.

     ![][7]
 
7. Voer in het vak omleiden URI "http://localhost/Azure-Search-Manager-App". Dit een URI naar welke Azure Active Directory user-agent in antwoord op een OAuth 2.0 autorisatie ontvangt leiden. De waarde niet hoeft te worden van een fysieke eindpunt, maar u moet een geldige URI. 

    Voor de toepassing van deze zelfstudie wordt de waarde kan van alles zijn, maar wat u invoert, wordt een vereiste invoer voor de administratieve verbinding in de steekproef-toepassing. 
 
7. Klik op het vinkje om de Active Directory-toepassing te maken. 'Azure-zoeken-Manager-App' ziet u in het linkernavigatiedeelvenster.

###<a name="configure-the-ad-application"></a>De AD-toepassing configureren
 
9. Klik op de toepassing AD "Azure-zoeken-Manager-App', die u zojuist hebt gemaakt. U ziet dat deze weergegeven in het linkernavigatiedeelvenster.

10. Klik op **configureren** in het bovenste menu.
 
11. Schuif omlaag naar machtigingen en selecteer **Azure Management API**. In deze stap geeft u de API (in dit geval de Azure resourcemanager API) opgeven dat de clienttoepassing toegang tot nodig heeft, samen met het toegangsniveau deze nodig.

12. Klik op de vervolgkeuzelijst overzicht en selecteert u machtigingen voor gedelegeerd **toegangsbeheer Azure-Service (Preview**).
 
     ![][8]
 
13. De wijzigingen opslaan. 

De pagina van de configuratie van toepassingen geopend houden. In de volgende stap, wordt u waarden van deze pagina kopiëren en deze invoeren voor de steekproef-toepassing.

###<a name="load-the-sample-application-program-with-registration-and-subscription-values"></a>De toepassing van de steekproef met registratie en abonnementen waarden laden

In deze sectie, kunt u de oplossing in Visual Studio, wordt vervangen door geldige waarden uit de portal opgehaald bewerken.
De waarden die u wilt toevoegen wordt weergegeven boven aan Program.cs:

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

Als u nog niet [gedownload van de steekproef toepassing Github hebt](https://github.com/Azure-Samples/search-dotnet-management-api/), moet u deze voor deze stap.

1. Open de **ManagementAPI.sln** in Visual Studio.

2. Open Program.cs.

3. Geef `ClientId`. Uit de AD-toepassing configuratie pagina links openen vanuit de vorige stap, de Client-ID van de pagina AD-toepassing configuratie in de portal kopiëren en plak deze in Program.cs.

4. Geef `RedirectUrl`. URI omleiden uit de dezelfde portalpagina Kopieer en plak deze in Program.cs.

    ![][9]

5. Bieden`TenantID.` 
    - Ga terug naar Active Directory | SearchTutorial (service). 
    - Klik op **toepassingen** uit de bovenste balk. 
    - Klik op **Weergave eindpunten** onder aan de pagina. 
    - Kopieer het OAUTH 2.0 autorisatie eindpunt onderaan in de lijst. 
    - Plak het eindpunt in TenantID, de waarde van alle URI inkorten parameters, met uitzondering van de tenant-ID.

    Gegeven "https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0", Verwijder alles, behalve '55e324c7-1656-4afe-8dc3-43efcd4ffa50'.

    ![][10]

6. Geef `SubscriptionID`.
    - Ga naar de hoofdpagina van de portal.
    - Klik op **Instellingen** onderaan in het linkernavigatiedeelvenster.
    - Op het tabblad abonnementen de abonnements-ID Kopieer en plak deze in Program.cs.

7. Sla en stelt u vervolgens de oplossing samen.


##<a name="explore-the-application"></a>De toepassing verkennen

Een onderbrekingspunt toevoegen aan de eerste methode oproep zodat u kunt het programma stapsgewijs. Druk op **F5** om de toepassing te starten en druk op **F11** om de code te doorlopen.

De toepassing van de steekproef Hiermee maakt u een gratis service van Azure zoeken voor een bestaand Azure-abonnement. Als er al een gratis service voor uw abonnement bestaat, mislukt de voorbeeldtoepassing. Slechts één gratis zoekservice per abonnement is toegestaan.

1. Program.cs openen vanuit de Solution Explorer en Ga naar de functie Hoofdgegeven (tekenreeks [] void). 
 
3. Zoals u ziet dat **ExecuteArmRequest** wordt gebruikt voor het uitvoeren van aanvragen ten opzichte van het eindpunt Azure resourcemanager, `https://management.azure.com/subscriptions` voor een opgegeven `subscriptionID`. Deze methode wordt gebruikt in het hele programma via de Azure resourcemanager API of zoek management-API bewerkingen uit te voeren.

3. Vergaderverzoeken naar Azure resourcemanager moeten worden geverifieerd en gemachtigd. Dit gebeurt met de methode **GetAuthorizationHeader** aangeroepen door de methode **ExecuteArmRequest** , geleend van [Azure resourcemanager verifiëren aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx). U ziet dat u **GetAuthorizationHeader belt** `https://management.core.windows.net` om een toegangstoken.

4. U wordt gevraagd of u aan te melden met een gebruikersnaam en wachtwoord dat geschikt is voor uw abonnement.

5. Vervolgens is een nieuwe Azure-zoekservice geregistreerd bij de resourcemanager Azure-provider. Klik nogmaals dit is de methode **ExecuteArmRequest** , ditmaal gebruikt om te maken van de zoekservice op Azure voor uw abonnement via `providers/Microsoft.Search/register`. 

6. De rest van het programma gebruikt de [Azure zoeken Management REST API](http://msdn.microsoft.com/library/dn832684.aspx). U ziet dat de `api-version` voor deze API van de Azure resourcemanager api-versie verschilt. Bijvoorbeeld `/listAdminKeys?api-version=2014-07-31-Preview` ziet u de `api-version` van de Azure zoeken Management REST API.

    De volgende reeks bewerkingen ophalen de servicedefinitie u zojuist hebt gemaakt, de admin-api-toetsen, genereert u deze opnieuw en sleutels zijn opgehaald, verandert de replica en partition en ten slotte Hiermee verwijdert u de service.

    Wanneer u de service replica of partition telling wijzigt, wordt naar verwachting dat deze actie niet als u de gratis versie gebruikt. Alleen de standard edition Maak gebruik van extra partities en replica's.

    Verwijderen van de service is de laatste bewerking.

##<a name="next-steps"></a>Volgende stappen

Na het nadat deze zelfstudie is voltooid, wilt u misschien meer informatie over het servicebeheer van de of verificatie met Active Directory-service:

- Meer informatie over het integreren van een clienttoepassing met Active Directory. Zie [integratie van toepassingen in Azure Active Directory](http://msdn.microsoft.com/library/azure/dn151122.aspx).
- Meer informatie over andere bewerkingen voor het beheer van service in Azure wordt aangegeven. Zie [uw Services beheren](http://msdn.microsoft.com/library/azure/dn578292.aspx).

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->
[5]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-Service.PNG
[6]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App.PNG
[7]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App-prop.PNG
[8]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPermissions.PNG
[9]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPage.PNG
[10]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-OAuthEndpoint.PNG

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md


 
