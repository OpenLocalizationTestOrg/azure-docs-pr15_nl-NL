<properties 
    pageTitle="De mobiele App-webservice van MFA Server aan de slag"
    description="De App Azure meervoudige verificatie biedt een optie Extra out-van-band-verificatie.  Dit kan de server MFA gebruik van push-meldingen aan gebruikers."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="getting-started-the-mfa-server-mobile-app-web-service"></a>De mobiele App-webservice van MFA Server aan de slag

De App Azure meervoudige verificatie biedt een optie Extra out-van-band-verificatie. In plaats van een geautomatiseerde telefoongesprek of SMS aan de gebruiker plaatsen tijdens het aanmelden, geeft Azure meervoudige verificatie een melding naar de App Azure meervoudige verificatie op van de gebruiker smartphone of tablet. De gebruiker gewoon tikt op "Verifiëren" (of een PINCODE invoert en tikt op "Verifiëren") in de app aan te melden.

Pas de Azure meervoudige verificatie-App gebruiken, het volgende is vereist zodat de app met de mobiele App-webservice communiceren kunt:

- Zie Hardware en Software Requirements voor hardware en software vereisten
- U moet gebruiken v6.0 of hoger van de Server Azure meervoudige verificatie
- Mobile-App-webservice moeten worden geïnstalleerd op een webserver internetgerichte is met Microsoft® Internet Information Services (IIS) IIS 7.x of hoger.  Zie voor meer informatie over IIS [IIS.NET](http://www.iis.net/).
- Controleer ASP.NET v4.0.30319 is geïnstalleerd, geregistreerd en instellen op toegestaan
- Vereiste rolservices opnemen ASP.NET en IIS 6 Metabase compatibiliteit
- Mobile-App-webservice moet toegankelijk zijn via een openbare URL
- Mobile-App-webservice moeten worden beveiligd met een SSL-certificaat.
- De SDK van Azure meervoudige verificatie Web Service moet zijn geïnstalleerd in IIS 7.x of hoger nodig voor de server die de Server Azure meervoudige verificatie
- De SDK van Azure meervoudige verificatie Web Service moeten worden beveiligd met een SSL-certificaat.
- Mobile-App-webservice moet kunnen verbinding maken met de SDK van Azure meervoudige verificatie Web Service via SSL
- Mobile-App-webservice moet toegang hebben tot de Azure meervoudige verificatie Web Service SDK met de referenties van een service-account die deel uitmaakt van een beveiligingsgroep 'PhoneFactor beheerders' genoemd. Deze serviceaccount en de groep bestaat niet in Active Directory als de Server Azure meervoudige verificatie wordt uitgevoerd op een server domein behoren. Deze serviceaccount en de groep bestaat lokaal op de Server Azure meervoudige verificatie als deze niet met een domein verbonden is.


Installatie van de gebruiker-portal op een server dan de Server Azure meervoudige verificatie vereist de volgende drie stappen uitvoeren:

1. De webservice SDK installeren
2. De mobiele app-webservice installeren
3. De mobiele app-instellingen configureren in de Server Azure meervoudige verificatie
4. De App Azure meervoudige verificatie voor eindgebruikers activeren

## <a name="install-the-web-service-sdk"></a>De webservice SDK installeren

Als de SDK van Azure meervoudige verificatie Web Service niet al is geïnstalleerd op de Server Azure meervoudige verificatie, gaat u naar die server en opent u de Server Azure meervoudige verificatie. Klik op het pictogram Web Service SDK, klikt u op de SDK Web Service installeren... knop en volg de aanwijzingen. De SDK van de Service Web moeten worden beveiligd met een SSL-certificaat. Een zelfondertekend certificaat dat hiervoor in orde is, maar er worden geïmporteerd in de store "Vertrouwde certificeringsinstanties die basiscertificaten" van de lokale Computer-account op de webserver User Portal zodat deze dat certificaat vertrouwt wanneer de SSL-verbinding tot stand brengen.

<center>![Setup](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)</center>

## <a name="install-the-mobile-app-web-service"></a>De mobiele app-webservice installeren
Voordat u de mobiele app-webservice installeert, worden op de hoogte van de volgende opties:

- Als de Portal met Azure meervoudige verificatie-gebruiker is al geïnstalleerd op de server internetgerichte, kunnen de gebruikersnaam, wachtwoord en URL voor de Web-Service SDK worden gekopieerd van de gebruiker-Portal web.config-bestand.
- Is het handig om open een webbrowser op de webserver internetgerichte en navigeer naar de URL van de SDK van de Web-Service die is ingevoerd in het bestand web.config. Als de browser kunt u naar de webservice is, moet deze u wordt gevraagd om referenties. Voer de gebruikersnaam en wachtwoord die zijn ingevoerd in het bestand web.config precies zoals deze wordt weergegeven in het bestand. Zorg ervoor dat er geen certificaat waarschuwingen of de fouten worden weergegeven.
- Als een reverse-proxy of firewall zich vóór de webserver Mobile-App-webservice en uitvoering SSL offloading, kunt u de mobiele App-webservice web.config-bestand bewerken en toevoegen van de volgende toets om de <appSettings> sectie zodat de mobiele App-webservice http in plaats van https gebruiken kunt. SSL is echter nog steeds verplicht uit de Mobile-App voor de firewall/reverse-proxyserver. <add key="SSL_REQUIRED" value="false"/>

### <a name="to-install-the-mobile-app-web-service"></a>De mobiele app-webservice installeren

<ol>
<li>Open Windows Verkenner op de Server Azure meervoudige verificatie en navigeer naar de map waarin de Server Azure meervoudige verificatie is geïnstalleerd (bijvoorbeeld C:\Program Files\Azure meervoudige verificatie). De 32-bits of 64-bits versie van de Azure meervoudige AuthenticationPhoneAppWebServiceSetup installatiebestand afhankelijk van de server die Mobile-App-webservice wordt geïnstalleerd op kiezen. Kopieer het bestand voor installatie op de server internetgerichte.</li>

<li>Klik op de webserver internetgerichte, moet het installatiebestand worden uitgevoerd met beheerdersrechten. De eenvoudigste manier hiervoor is om open een opdrachtpromptvenster als beheerder en navigeer naar de locatie waar het bestand voor installatie is gekopieerd.</li>  

<li>Het installatiebestand meervoudige AuthenticationMobileAppWebServiceSetup uitvoeren, de Site desgewenst wijzigen en wijzig de virtuele map in een korte naam, zoals "PA". De naam van een korte virtuele map wordt aanbevolen, omdat gebruikers URL van de mobiele App webservice in het mobiele apparaat tijdens de activering invoeren moeten.</li>

<li>Nadat de installatie van de Azure meervoudige AuthenticationMobileAppWebServiceSetup is voltooid, blader naar C:\inetpub\wwwroot\PA (of de juiste map op basis van de naam van de virtuele map) en het bestand web.config bewerken.</li>  

<li>Zoek de toetsen WEB_SERVICE_SDK_AUTHENTICATION_USERNAME en WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD en stel de waarden op de gebruikersnaam en wachtwoord van de serviceaccount die deel uitmaakt van de beveiliging PhoneFactor Admins groeperen (Zie het gedeelte vereisten voor hierboven). Dit kan zijn hetzelfde account wordt gebruikt als de identiteit van de Portal Azure meervoudige verificatie gebruiker als die eerder is geïnstalleerd. Zorg ervoor dat u de gebruikersnaam en wachtwoord invoeren tussen de aanhalingstekens aan het einde van de regel (waarde = "" / >). Het wordt aanbevolen voor het gebruik van een gekwalificeerde gebruikersnaam (bijvoorbeeld domein\gebruikersnaam of machine\username).</li>  

<li>Zoek de instelling van de App Web Service_pfwssdk_PfWsSdk pfMobile en wijzig de waarde van 'http://localhost:4898/PfWsSdk.asmx' naar de URL van de Web-Service SDK die wordt uitgevoerd op de Server Azure meervoudige verificatie (bijvoorbeeld https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Aangezien SSL wordt gebruikt voor deze verbinding, moet u de SDK van de Service Web door de servernaam van de en niet IP-adres verwijzen naar aangezien het SSL-certificaat wordt zijn verleend voor naam van de server en de URL die wordt gebruikt, moet overeenkomen met de naam op het certificaat. Als de naam van de server niet is opgelost naar een IP-adres van de server internetgerichte, moet u een fragment toevoegen aan het hostsbestand op die server naar de naam van de Server Azure meervoudige verificatie toewijzen aan het IP-adres. Sla het bestand web.config nadat u wijzigingen hebt aangebracht.</li>  

<li>Als de website van dat Mobile-App-webservice onder (bijvoorbeeld standaardwebsite) is geïnstalleerd al niet is binded met een certificaat openbaar zijn aangemeld, het certificaat installeren op de server als dat niet al is geïnstalleerd, opent u IIS-beheer en afhankelijk van het certificaat naar de website.</li>  

<li>Open een webbrowser vanaf elke computer en Ga naar de URL waarop webservice Mobile-App is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/PA). Zorg ervoor dat er geen certificaat waarschuwingen of de fouten worden weergegeven.</li>

### <a name="configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De mobiele app-instellingen configureren in de Server Azure meervoudige verificatie
Nu de mobiele app-webservice is geïnstalleerd, moet u voor het configureren van de Server Azure meervoudige verificatie voor gebruik met de portal.

#### <a name="to-configure-the-mobile-app-settings-in-the-azure-multi-factor-authentication-server"></a>De mobiele app-instellingen configureren in de Server Azure meervoudige verificatie

1. Klik op het User Portal-pictogram in de Server Azure meervoudige verificatie. Als gebruikers kunnen hun verificatiemethoden beheren op het tabblad instellingen onder gebruikers toestaan deel te selecteren, schakelt u Mobile-App. Zonder deze functie is ingeschakeld, eindgebruikers moet contact opnemen met uw helpdesk om activering voor de Mobile-App te voltooien.
2. Controleer de gebruikers toestaan om te activeren vak Mobile-App.
3. Schakel het selectievakje toestaan dat gebruikers registratie.
4. Klik op het pictogram Mobile-App.
5. Voer de URL die wordt gebruikt met de virtuele map die is gemaakt tijdens de installatie van de Azure meervoudige AuthenticationMobileAppWebServiceSetup. De naam van een Account kan worden ingevoerd in de ruimte. Deze bedrijfsnaam wordt weergegeven in de mobiele toepassing. Als leeg laat, wordt de naam van uw meervoudige Auth Provider die is gemaakt in de beheerportal Azure worden weergegeven.



<center>![Setup](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)</center>
