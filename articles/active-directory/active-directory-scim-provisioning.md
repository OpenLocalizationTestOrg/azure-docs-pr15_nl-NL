<properties
    pageTitle="Gebruikmaken van SCIM waarmee automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen | Microsoft Azure"
    description="Gebruikers en groepen naar een toepassing of identiteit store die door een webservice is fronted met de interface die is gedefinieerd in de specificatie SCIM protocol kunt automatisch inrichten van Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Gebruikmaken van SCIM waarmee automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen

##<a name="overview"></a>Overzicht

Azure Active Directory kunt automatisch inrichten van gebruikers en groepen naar een toepassing of identiteit store die door een webservice is fronted met de interface die is gedefinieerd in de [specificatie van SCIM 2.0-protocol](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory kunnen aanvragen maken, wijzigen en verwijderen van de toegewezen gebruikers en groepen om over te deze webservice die u deze aanvragen vervolgens in bewerkingen na het doel identiteit archief vertalen kunt verzenden. 

![][1]
*Afbeelding: De inrichting van Azure Active Directory naar een identiteit store via een webservice*

Deze mogelijkheid kan worden gebruikt in combinatie met de mogelijkheid "[doen om uw eigen app](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" in Azure AD voor eenmalige aanmelding en automatische gebruikersnaam voor toepassingen die geven of door een webservice SCIM zijn fronted is geïnstalleerd.

Er zijn twee gebruik dozen voor SCIM in Azure Active Directory:

* **Inrichten van gebruikers en groepen naar toepassingen die SCIM ondersteunen** - toepassingen die ondersteuning SCIM 2.0 en het gebruik van OAuth vruchtdragende tokens voor verificatie werkt met Azure AD van het vak.

* **Maak uw eigen inrichten oplossing voor toepassingen die ondersteuning voor andere API gebaseerde inrichting** - voor niet-SCIM-toepassingen, kunt u een SCIM eindpunt maken om te vertalen tussen Azure AD SCIM eindpunt en datgene API de toepassing die ondersteuning biedt voor het inrichten van de gebruiker.  Hulp bij de ontwikkeling van een eindpunt SCIM, bieden we CLI bibliotheken samen met de voorbeelden van de code u zien hoe een eindpunt SCIM bieden en vertalen SCIM berichten.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Inrichten van gebruikers en groepen naar toepassingen die ondersteuning bieden voor SCIM

Azure Active Directory kan worden geconfigureerd automatisch toegewezen om gebruikers te richten en groepen om over te geïmplementeerd van een [systeem voor andere domeinen identiteitsbeheer 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) Web service en accepteer OAuth vruchtdragende tokens voor verificatie. Binnen de SCIM 2.0-specificatie, toepassingen moeten voldoen aan deze vereisten is voldaan:

* Ondersteunt het maken van gebruikers en/of groepen aan de hand van sectie 3.3 van het SCIM-protocol.  

* Ondersteunt het wijzigen van gebruikers en/of groepen met patches aanvragen aan de hand van sectie 3.5.2 van het SCIM-protocol.  

* Ondersteunt het ophalen van een bekende resource aan de hand van sectie 3.4.1 van het SCIM-protocol.  

*  Ondersteunt query's uitvoeren/gebruikers of groepen aan de hand van sectie 3.4.2 van het SCIM-protocol.  Standaard gebruikers gevraagd door externalId en groepen worden opgevraagd door weergavenaam.  

* Ondersteunt query's uitvoeren gebruiker op ID en op de manager aan de hand van sectie 3.4.2 van het SCIM-protocol.  

* Ondersteunt query's uitvoeren groepen op ID en klik op lid aan de hand van sectie 3.4.2 van het SCIM-protocol.  

* OAuth vruchtdragende tokens voor een vergunning aan de hand van sectie 2.1 van het protocol SCIM accepteert.

U moet controleren met uw toepassingsprovider of de documentatie van uw toepassingsprovider voor overzichten van compatibiliteit met deze vereisten is voldaan.
 
###<a name="getting-started"></a>Aan de slag

Toepassingen die ondersteuning bieden voor het hierboven beschreven SCIM-profiel kunnen worden verbonden met Azure Active Directory met de functie 'aangepaste'-app in de galerie met Azure AD-toepassing. Zodra u verbinding hebt, voert Azure AD een synchronisatieproces elke 5 minuten waar deze query's van de toepassing SCIM eindpunt voor toegewezen gebruikers en groepen, en maakt of wijzigt deze op basis van de details van de toewijzing.

**Verbinding met het maken van een toepassing die ondersteuning biedt voor SCIM:**

1.  Start de Azure management-portal op https://manage.windowsazure.com in een webbrowser.
2.  Blader naar **Active Directory > Directory > [Your Directory] > toepassingen**, en selecteer **toevoegen > een toepassing toevoegen vanuit de galerie**.
3.  Selecteer het tabblad **aangepast** aan de linkerkant, voer een naam voor uw toepassing en klik op het pictogram vinkje als een app-object wilt maken.

![][2]

4.  Selecteer de tweede **configureren account inrichting** knop in de resulterende scherm.
5.  Voer in het veld **Inrichting eindpunt URL** de URL van van de toepassing SCIM eindpunt.
6.  Als het eindpunt SCIM vereist is voor een OAuth-dragertoken van een uitgever dan Azure AD, kopieert u de vereiste OAuth-dragertoken vervolgens naar het veld **Verificatie-Token (optioneel)** . Is dit veld leeg is, en vervolgens Azure AD bevat een OAuth-dragertoken van Azure AD met elk verzoek om een uitgegeven. Apps die gebruikmaken van Azure AD, zoals een provider idenity deze Azure AD valideren kunt-token uitgegeven.
7.  Klik op **volgende**en klik op de knop **Test starten** met Azure Active Directory verbinding maakt met het eindpunt SCIM. Als de pogingen mislukken, wordt diagnostische informatie weergegeven.  
8.  Als de verbinding aan het succesvol toepassing probeert te klik vervolgens op **volgende** op de resterende schermen en klik op **Voltooien** om het dialoogvenster te sluiten.
9.  Selecteer de derde **Toewijzen rekeningen** -knop in de resulterende scherm. Toewijzen in de resulterende sectie gebruikers en groepen, de gebruikers of groepen die u wilt inrichten met de toepassing.
10. Als gebruikers en groepen zijn toegewezen, klikt u op het tabblad **configureren** Klik boven aan het scherm.
11. Klik onder **Account inrichting**, bevestig dat de Status is ingesteld op aan. 
12. Klik onder **Hulpmiddelen voor**klikt u **opnieuw de inrichting van account** op begin het inrichten.

Houd er rekening mee dat 5-10 minuten verstrijken mogelijk voordat het inrichten aanvragen verzenden naar het eindpunt SCIM wordt gestart.  Een overzicht van verbindingen wordt weergegeven op het tabblad van de Dashboard van de toepassing en een rapport van inrichten activiteit zowel inrichten fouten kunnen worden gedownload van de tabblad rapporten van de map.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>Samenstellen van uw eigen oplossing voor een toepassing is geïnstalleerd

Als u een SCIM-webservice die interfaces met Azure Active Directory, kunt u één gebruiker voor eenmalige aanmelding en automatische voor vrijwel elke toepassing waarmee een inrichting API REST of SOAP-gebruiker is geïnstalleerd.

Hier volgt hoe dit werkt:

1.  Azure AD biedt een algemene taal infrastructuur-bibliotheek met de naam [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systeemintegrators en ontwikkelaars kunnen deze bibliotheek maken en implementeren van een SCIM gebaseerde web service-eindpunt Azure AD naar van een toepassing identiteit store verbinding kan gebruiken.
2.  Toewijzingen zijn geïmplementeerd in de webservice het gebruikersschema gestandaardiseerde toewijzen aan de gebruikersschema en het protocol dat is vereist door de toepassing.
3.  De URL van het eindpunt is geregistreerd in Azure AD als onderdeel van een maatwerktoepassing op in de galerie toepassing.
4.  Gebruikers en groepen worden toegewezen aan deze toepassing in Azure AD. Na het toewijzing, worden ze plaatst in wachtrij moeten worden gesynchroniseerd naar de doeltoepassing instellen. De synchronisatieproces de wachtrij voor de verwerking elke 5 minuten wordt uitgevoerd.

###<a name="code-samples"></a>Voorbeelden van programmacode

Om dit proces vereenvoudigen, zijn een aantal [voorbeelden code](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) voorwaarde die een SCIM web service-eindpunt maken en demonstreren automatisch inrichten. Een voorbeeld is van een provider die onderhoudt een bestand met rijen met door komma's gescheiden waarden dat staat voor gebruikers en groepen.  De andere is van een provider die wordt toegepast op de service Amazon Web Services-id en toegangsbeheer.  

**Vereisten voor**

* Visual Studio 2013 of hoger
* [Azure SDK voor .NET](https://azure.microsoft.com/downloads/)
* Windows-computer die ondersteuning biedt voor het ASP.NET-framework 4.5 moet worden gebruikt als het eindpunt SCIM. Deze computer moet toegankelijk zijn vanuit de cloud
* [Een Azure-abonnement met een proefabonnement of gelicentieerde versie van Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* De steekproef Amazon AWS vereist bibliotheken van de [AWS Toolkit voor Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Zie het Leesmij-bestand dat is inbegrepen in de steekproef voor meer informatie

###<a name="getting-started"></a>Aan de slag

De eenvoudigste manier om een SCIM-eindpunt dat u kunt akkoord gaan met inrichten aanmeldingsaanvragen van Azure AD implementeren is voor bouwen en implementeren van de steekproef code die Hiermee kunt u de ingerichte gebruikers naar een bestand met door komma's gescheiden waarden (.csv).

**Een voorbeeld SCIM om eindpunt te maken:**

1.  Het pakket van de steekproef code op [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) downloaden
2.  Het pakket pak en plaats het op uw Windows-computer op een locatie, zoals C:\AzureAD-BYOA-Provisioning-Samples\.
3.  In deze map, start u de oplossing FileProvisioningAgent in Visual Studio.
4.  Selecteer **Hulpmiddelen > bibliotheek Package Manager > Package Manager Console**, en de opdrachten onder voor het project FileProvisioningAgent voor het oplossen van de oplossing verwijzingen uitvoeren:

    Installatie-pakket Microsoft.SystemForCrossDomainIdentityManagement-installatiepakket Microsoft.IdentityModel.Clients.ActiveDirectory-installatiepakket Microsoft.Owin.Diagnostics-installatiepakket Microsoft.Owin.Host.SystemWeb

5.  Het project FileProvisioningAgent maakt.
6.  Start de toepassing opdrachtprompt in Windows (als een beheerder) en de opdracht **cd** gebruiken om te wijzigen van de map naar de map **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** .
7.  Hiermee voert u de opdracht onder de naam van het IP- of domein van de Windows-computer kunt < ip-adres > vervangen.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  In Windows onder **Windows-instellingen > netwerk en instellingen voor Internet**, selecteer de **Windows Firewall > Geavanceerde instellingen**, en een **Binnenkomende regel** voor binnenkomende toegang tot poort 9000 maken.
9.  Als de Windows-computer zich achter een router, moet de router worden geconfigureerd voor het uitvoeren van netwerk Access vertaling tussen de poort 9000 die wordt blootgesteld aan internet en poort 9000 op de Windows-computer. Dit is vereist voor Azure AD kunnen voor toegang tot dit eindpunt in de cloud.


**Het voorbeeld SCIM eindpunt registreren in Azure AD:**

1.  Start de Azure management-portal op https://manage.windowsazure.com in een webbrowser.
2.  Blader naar **Active Directory > Directory > [uw map] > toepassingen**, en selecteer **toevoegen > een toepassing toevoegen vanuit de galerie**.
3.  Selecteer het tabblad **aangepast** aan de linkerkant, voert u een naam, zoals "SCIM Test App" en klik op het pictogram vinkje als een app-object wilt maken. Houd er rekening mee dat het application-object gemaakt is wilt de doel-app toe worden inrichting naar en implementeren van eenmalige aanmelding voor en niet alleen het eindpunt SCIM vertegenwoordigen.

![][2]

4.  Selecteer de tweede **configureren account inrichting** knop in de resulterende scherm.
5.  Voer in het dialoogvenster de internet blootgesteld URL en poort van uw SCIM-eindpunt. Dit zou zijn zoals http://testmachine.contoso.com:9000 of http://<ip-address>:9000/, waar < ip-adres > internet is die worden aangeboden IP address.  
6.  Klik op **volgende**en klik op de knop **Test starten** met Azure Active Directory verbinding maakt met het eindpunt SCIM. Als de pogingen mislukken, wordt diagnostische informatie weergegeven.  
7.  Als de verbinding met uw webservice probeert te worden voltooid, klikt u op **volgende** op de overige schermen en klik op **Voltooien** om het dialoogvenster te sluiten.
8.  Selecteer de derde **Toewijzen rekeningen** -knop in de resulterende scherm. Toewijzen in de resulterende sectie gebruikers en groepen, de gebruikers of groepen die u wilt inrichten met de toepassing.
9.  Als gebruikers en groepen zijn toegewezen, klikt u op het tabblad **configureren** Klik boven aan het scherm.
10. Klik onder **Account inrichting**, bevestig dat de Status is ingesteld op aan. 
11. Klik onder **Hulpmiddelen voor**klikt u **opnieuw de inrichting van account** op begin het inrichten.

Houd er rekening mee dat 5-10 minuten verstrijken mogelijk voordat het inrichten aanvragen verzenden naar het eindpunt SCIM wordt gestart.  Een overzicht van verbindingen wordt weergegeven op het tabblad van de Dashboard van de toepassing en zowel een rapport van inrichten activiteit en inrichten fouten kunnen worden gedownload van de map rapporten tabblad.

De laatste stap bij het controleren van de steekproef is het bestand te openen TargetFile.csv in de map \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug op uw Windows-computer. Nadat het inrichten wordt uitgevoerd, wordt dit bestand bevat de details van alle toegewezen en deze is ingericht gebruikers en groepen.

###<a name="development-libraries"></a>Bibliotheken van ontwikkeling

Als u wilt uw eigen-webservice die aan de specificatie SCIM voldoet ontwikkelt, eerst Maak uzelf vertrouwd met de volgende bibliotheken die is verstrekt door Microsoft om u te helpen versnellen van het ontwikkelingsproces: 

**1:**  Algemene taal infrastructuur bibliotheken zijn beschikbaar voor gebruik met talen die op basis van deze infrastructuur, zoals C#.  Een van deze bibliotheken, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), een interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, dat is weergegeven in de onderstaande afbeelding gedeclareerd.  Een ontwikkelaar met de bibliotheken zou die interface met een klasse die mogelijk worden hierna, algemeen, als een provider implementeren.  De bibliotheken inschakelen de ontwikkelaar om te implementeren eenvoudig een webservice die voldoet aan de specificatie SCIM, hetzij vanuit Internet Information Services of een uitvoerbare algemene taal infrastructuur constructie die worden gehost.  Aanvragen voor die service worden vertaald in oproepen naar van de provider methoden, die door de ontwikkelaar programmeren zou in sommige store identiteit.    

![][3]

**2:** [Express route handlers](http://expressjs.com/guide/routing.html) zijn beschikbaar voor parseren node.js verzoek objecten in een node.js webservice dat staat voor bellen (zoals gedefinieerd door de specificatie SCIM), aangebracht.     

###<a name="building-a-custom-scim-endpoint"></a>Samenstellen van een aangepaste SCIM-eindpunt

Met de bovenstaande bibliotheken, kunnen ontwikkelaars die bibliotheken met hun services binnen een uitvoerbare algemene taal infrastructuur combinatie, of Internet Information Services hosten.  Hier ziet u voorbeelden van code voor een hostingservice binnen uitvoerbare assemblies, op het adres http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Het is belangrijk dat deze service moet beschikken over een HTTP-mailadres en server certificaat voor serververificatie waarvan de basiscertificeringsinstantie een van de volgende opties is: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Een certificaat voor serververificatie kan worden gekoppeld aan een poort op een Windows-host met het hulpprogramma voor netwerk-shell, zoals in dit voorbeeld: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
Hier is de waarde die is opgegeven voor het argument certhash de vingerafdruk van het certificaat, terwijl de waarde die is opgegeven voor het argument toepassings-id een willekeurige globaal unieke id is.  

Als u wilt de service binnen Internet Information Services hosten, zou een ontwikkelaar maken op een gemeenschappelijke taal infrastructuur code bibliotheek constructie met een klasse met de naam opstarten in de standaard-naamruimte van de vergadering.  Hier volgt een voorbeeld van dergelijke groep: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Eindpunt verificatie voor de verwerking

Aanmeldingsaanvragen van Azure Active Directory bevatten een 2.0 OAuth-dragertoken.   Elke service die de aanvraag ontvangt, moet de uitgever Azure Active Directory die namens de verwachte Azure Active Directory-tenant, voor toegang tot Azure Active Directory-webservice Graph verifiëren.  In het token, de uitgever wordt aangegeven door een claim iss, zoals "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  In dit voorbeeld het basisadres van de waarde van de claim, https://sts.windows.net, geeft aan wat Azure Active Directory als de uitgever, terwijl het segment relatief adres, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is een unieke id van de Azure Active Directory-tenant namens waarin het token is uitgegeven.  Als het token is verleend voor toegang tot de Azure Active Directory Graph-webservice, klikt u vervolgens moet de id van die service, 00000002-0000-0000-c000-000000000000, in de waarde van van het token aud claim.  

Gebruik van de algemene taal infrastructuur bibliotheken geleverd door Microsoft voor het samenstellen van een service SCIM ontwikkelaars kunnen verifiëren aanmeldingsaanvragen van Azure Active Directory het pakket Microsoft.Owin.Security.ActiveDirectory gebruiken door deze stappen uit: 

**1:**  Implementeren in een provider, de eigenschap Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior doordat deze retourneren van een methode om te worden aangeroepen wanneer de service wordt gestart: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  De volgende code toevoegen aan deze methode hebt elk verzoek aan van de service-eindpunten geverifieerd als voorzien van een token uitgegeven door Azure Active Directory namens een opgegeven tenant, voor toegang tot Azure Active Directory-webservice Graph: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Gebruiker en groep Schema

Azure Active Directory kunt inrichten van twee soorten resources met SCIM-webservices.  Deze soorten resources zijn gebruikers en groepen.  

Gebruikersresources zijn die wordt aangeduid met het schema-id, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, die is opgenomen in deze protocolspecificatie: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  De standaard-koppeling van de kenmerken van gebruikers in Azure Active Directory in de kenmerken van resources urn: ietf:params:scim:schemas:extension:enterprise:2.0:User hieronder vindt u in de tabel 1.  

Groep resources zijn die wordt aangeduid met het schema-id, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabel 2, onder, ziet u de standaard-koppeling van de kenmerken van groepen in Azure Active Directory in de kenmerken van http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.  

###<a name="table-1-default-user-attribute-mapping"></a>Tabel 1: Standaard gebruiker-kenmerk-koppeling

| Azure Active Directory-gebruiker | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | actieve |
| Weergavenaam | Weergavenaam |
| Fax-TelephoneNumber | phoneNumbers [type eq "fax"] .value |
| givenName | name.givenName |
| Functie | titel |
| e-mail | e-mailberichten [type eq 'werk'] .value |
| mailNickname | externalId |
| Manager | Manager |
| mobiele | phoneNumbers [type eq "mobile"] .value |
| object-id | ID |
| Postcode | adressen [type eq 'werk'] .postalCode |
| proxyadressen | e-mailberichten [type eq, "Overig"]. Waarde |
| fysieke-bezorging-OfficeName | adressen [type eq, "Overig"]. Opgemaakt |
| streetAddress | adressen [type eq 'werk'] .streetAddress |
| Achternaam | name.familyName |
| Telefoonnummer | phoneNumbers [type eq 'werk'] .value |
| gebruiker-PrincipalName | Gebruikersnaam |


###<a name="table-2-default-group-attribute-mapping"></a>Tabel 2: Standaard groepen kenmerk vergelijken

| Azure Active Directory-groep | http://schemas.Microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| Weergavenaam | externalId |
| e-mail | e-mailberichten [type eq 'werk'] .value |
| mailNickname | Weergavenaam |
| leden | leden |
| object-id | ID |
| proxyAddresses | e-mailberichten [type eq, "Overig"]. Waarde |


##<a name="user-provisioning-and-de-provisioning"></a>Inrichting van de gebruiker en verwijderen van gegevens

De afbeelding hieronder ziet u de berichten Azure Active Directory worden verzonden naar een SCIM-service voor het beheren van de levenscyclus van een gebruiker in een andere identiteit worden opgeslagen.  Het diagram ziet u ook een SCIM-service geïmplementeerd met behulp van de algemene taal infrastructuur bibliotheken geleverd door Microsoft voor het samenstellen van deze services wordt hoe deze aanvragen vertalen in oproepen naar de methoden van een provider.  

![][4]
*Afbeelding: Inrichting van de gebruiker en reeks verwijderen van gegevens*

**1:**  Azure Active Directory, wordt de service voor een gebruiker query met een externalId kenmerkwaarde die overeenkomen met de kenmerkwaarde mailNickname van een gebruiker in Azure Active Directory.  De query weergegeven als een aanvraag Hypertext Transfer Protocol zoals dit item, waarin jyoung een voorbeeld van een mailNickname van een gebruiker in Azure Active Directory is: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Als de service is gemaakt met de algemene taal infrastructuur bibliotheken geleverd door Microsoft voor de uitvoering van SCIM services, wordt het verzoek worden omgezet in een oproep door naar de methode Query van de provider van de service.  Hier ziet u de handtekening van deze methode: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Hier ziet u de definitie van de interface Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

Als het voorgaande voorbeeld van een query voor een gebruiker met een bepaalde waarde voor het kenmerk externalId waarden van de argumenten doorgegeven aan de Query-methode worden als volgt: 

* parameters. AlternateFilters.Count: 1
* parameters. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* parameters. AlternateFilters.ElementAt(0). Vergelijkingsoperator: ComparisonOperator.Equals
* parameters. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Aanvraag-id"] 

**2:**  Als het antwoord op een query toevoegen aan de service voor een gebruiker met een externalId kenmerkwaarde die overeenkomen met de kenmerkwaarde mailNickname van een gebruiker in Azure Active Directory niet alle gebruikers, klikt u vervolgens Azure Active Directory wordt ook zelf aanvragen dat de service inrichten van een gebruiker die overeenkomt met een in Azure Active Directory.  Hier volgt een voorbeeld van een aanvraag voor: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

De algemene taal infrastructuur bibliotheken geleverd door Microsoft voor de uitvoering van SCIM services zou die aanvraag vertalen in een gesprek met de methode maken van de provider van de service.  De methode Create heeft deze handtekening: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Een aanvraag voor het inrichten van een gebruiker, wordt de waarde van de argumenten van de resource een exemplaar van de Microsoft.SystemForCrossDomainIdentityManagement worden. Core2EnterpriseUser klasse is gedefinieerd in de bibliotheek Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  Als de aanvraag voor het inrichten van de gebruiker is geslaagd, wordt de implementatie van de methode naar verwachting om terug te keren een exemplaar van de de Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser klasse, met de waarde van de eigenschap id is ingesteld op de unieke id van de gebruiker zojuist-deze is ingericht.  

**3:**  Als u wilt bijwerken van een gebruiker bekend in een identiteit store fronted door een SCIM, voortgezet Azure Active Directory aanvragen van de huidige status van die gebruiker van de service met een verzoek om zoals dit: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

In een service die is gemaakt met de algemene taal infrastructuur bibliotheken geleverd door Microsoft voor de uitvoering van SCIM services, wordt het verzoek worden omgezet in een gesprek met de methode ophalen van de provider van de service.  Hier ziet u de handtekening van de methode ophalen: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

In het voorgaande voorbeeld van een aanvraag om op te halen van de huidige status van een gebruiker de waarden van de eigenschappen van het object die is opgegeven als de waarde van het argument parameters worden als volgt: 

* -ID: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Als een verwijzing-kenmerk worden bijgewerkt, en vervolgens Azure Active Directory wordt de service om te bepalen of de huidige waarde van het kenmerk verwijzing in de winkel identiteit op de al fronted door de service query komt overeen met de waarde van het desbetreffende kenmerk in Azure Active Directory.  In het geval van gebruikers is het alleen-kenmerk van de plaats waar de huidige waarde wordt gezocht op deze manier de manager-kenmerk.  Hier volgt een voorbeeld van een aanvraag om te bepalen of het kenmerk manager van een bepaald gebruikersobject momenteel een bepaalde waarde heeft: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

De waarde van de queryparameter kenmerken id duidt op die als een gebruikersobject bestaat die overeenkomt met de uitdrukking die is opgegeven als de waarde van de parameter van de query filter en klik op de service wordt naar verwachting reageren met een resource urn: ietf:params:scim:schemas:core:2.0:User of urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, met inbegrip van alleen de waarde van de desbetreffende resource-id-kenmerk.  Natuurlijk de waarde van het kenmerk id bekend is verstuurd, wordt opgenomen in de waarde van de query-parameter van het filter; het doel van het verzoek om deze is werkelijk aanvragen van een minimale weergave van een resource die voldoen aan de filterexpressie als vermelding van het al dan niet een dergelijke object bestaat.   

Als de service is gemaakt met de algemene taal infrastructuur bibliotheken geleverd door Microsoft voor de uitvoering van SCIM services, wordt het verzoek worden omgezet in een oproep door naar de methode Query van de provider van de service.  De waarde van de eigenschappen van het object die is opgegeven als de waarde van het argument parameters worden als volgt: 

* parameters. AlternateFilters.Count: 2
* parameters. AlternateFilters.ElementAt(x). AttributePath: "id"
* parameters. AlternateFilters.ElementAt(x). Vergelijkingsoperator: ComparisonOperator.Equals
* parameters. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* parameters. AlternateFilters.ElementAt(y). AttributePath: "manager"
* parameters. AlternateFilters.ElementAt(y). Vergelijkingsoperator: ComparisonOperator.Equals
* parameters. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* parameters. RequestedAttributePaths.ElementAt(0): "id"
* parameters. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Hier de waarde van de index x mogelijk 0 en de waarde van y index mogelijk 1, of de waarde van x mogelijk 1 en de waarde van y mogelijk 0, afhankelijk van de volgorde van de expressies van de queryparameter filter.   

**5:**  Hier volgt een voorbeeld van een aanvraag van Azure Active Directory op een service SCIM bijwerken van een gebruiker: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

De bibliotheken Microsoft Common Language Interface voor de uitvoering van SCIM services zou het verzoek vertalen in een oproep door naar de methode Update van de provider van de service.  Hier ziet u de handtekening van deze methode: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



In het voorgaande voorbeeld van een aanvraag voor het bijwerken van een gebruiker heeft het object die is opgegeven als de waarde van het argument patch waarden van deze eigenschappen: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (PatchRequest als PatchRequest2). Operations.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Path.AttributePath: "manager"
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.Count: 1
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Verwijzing: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (PatchRequest als PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Waarde: 2819c223-7f76-453a-919d-413861904646

**6:**  Als u wilt deactiveren inrichten van een gebruiker van een identiteit store fronted door een SCIM-service, stuurt Azure Active Directory een aanvraag zoals dit: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Als de service is gemaakt met de algemene taal infrastructuur bibliotheken geleverd door Microsoft voor de uitvoering van SCIM services, wordt het verzoek worden omgezet in een gesprek met de methode verwijderen van de provider van de service.   Deze methode heeft deze handtekening: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Het object die is opgegeven als de waarde van het argument resourceIdentifier heeft de waarden van deze eigenschappen in het voorgaande voorbeeld van een aanvraag voor het inrichten van een gebruiker verwijderen: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Groep inrichting en verwijderen van gegevens

De afbeelding hieronder ziet u de berichten Azure Active Directory worden verzonden naar een SCIM-service voor het beheren van de levenscyclus van een groep in een andere identiteit worden opgeslagen.  Deze berichten verschillen van de berichten die betrekking hebben op gebruikers op drie manieren: 

* Het schema van een resource groep worden geïdentificeerd als http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Verzoeken om op te halen groepen zal aangeven dat het kenmerk leden moet worden uitgesloten uit alle bronnen die beschikbaar zijn in antwoord op de aanvraag is.  
* Om te bepalen of een verwijzing-kenmerk een bepaalde waarde heeft worden aanvragen over het kenmerk leden.  

![][5]
*Afbeelding: Inrichting van de groep en reeks verwijderen van gegevens*

##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Automatiseren gebruiker inrichting/Deprovisioning aan SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Kenmerktoewijzingen voor het inrichten van de gebruiker aan te passen](active-directory-saas-customizing-attribute-mappings.md)
- [Expressies voor het toewijzen van kenmerken schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Een bereik van Filters voor het inrichten van de gebruiker instellen](active-directory-saas-scoping-filters.md)
- [Account inrichting van meldingen](active-directory-saas-account-provisioning-notifications.md)
- [Lijst met zelfstudies over het integreren van SaaS Apps](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
