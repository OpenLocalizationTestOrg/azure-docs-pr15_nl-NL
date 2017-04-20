<properties 
    pageTitle="De gebruiker-portal implementeren voor de Server Azure meervoudige verificatie"
    description="Dit is de pagina van de Azure meervoudige verificatie die wordt beschreven hoe u aan de slag met Azure MFA en de gebruiker-portal."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="deploying-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>De gebruiker-portal implementeren voor de Server Azure meervoudige verificatie

De Portal van de gebruiker kan de beheerder installeren en configureren van de Portal met Azure meervoudige verificatie-gebruiker. De gebruiker-Portal is een IIS-website waarmee gebruikers kunnen inschrijven Azure meervoudige verificatie en onderhouden van hun accounts. Een gebruiker kan hun telefoonnummer wijzigen, hun PINCODE wijzigen of Azure meervoudige verificatie overslaan tijdens hun volgende aanmeldingsgegevens op.

Gebruikers wordt Meld u aan bij de gebruiker-Portal met hun normale gebruikersnaam en wachtwoord en wordt een oproep Azure meervoudige verificatie voltooien of beveiliging vragen om te voltooien hun verificatie beantwoorden. Als registratie van de gebruiker is toegestaan, configureert een gebruiker hun telefoonnummer en PINCODE voor de eerste keer dat ze zich bij de Portal van de gebruiker aanmelden.

Beheerders van de Portal gebruikers mogelijk instellen en de machtiging nieuwe gebruikers toevoegen en bijwerken van bestaande gebruikers.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## <a name="deploying-the-user-portal-on-the-same-server-as-the-azure-multi-factor-authentication-server"></a>De gebruiker-portal op dezelfde server als de Server Azure meervoudige verificatie implementeren

De volgende minimumvereisten zijn vereist voor het installeren van de gebruikers-Portal op dezelfde server als de Server Azure meervoudige verificatie:

- IIS moet zijn geïnstalleerd met inbegrip van asp.net en IIS 6 metagegevens grondtal compatibiliteit (voor IIS-7 of hoger)
- Aangemelde gebruiker beheerder rechten voor de computer en domein moet hebben, indien van toepassing.  Dit is omdat het account machtigingen moet voor het maken van Active Directory-beveiligingsgroepen.

### <a name="to-deploy-the-user-portal-for-the-azure-multi-factor-authentication-server"></a>Naar het implementeren van de gebruiker-portal voor de Server Azure meervoudige verificatie

1. Binnen de Azure meervoudige verificatieserver: klik op gebruiker Portal-pictogram in het linkermenu, klikt u op de knop installeren User Portal.
1. Klik op volgende.
1. Klik op volgende.
1. Als de computer deel van een domein uitmaakt en de Active Directory-configuratie voor het beveiligen van de communicatie tussen de gebruiker-Portal en de Azure meervoudige verificatie-service niet volledig is, worden de Active Directory-stap weergegeven. Klik op de knop Volgende om deze configuratie automatisch aanvullen.
1. Klik op volgende.
1. Klik op volgende.
1. Klik op sluiten.
1. Open een webbrowser vanaf elke computer en Ga naar de URL waarin gebruiker Portal is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/MultiFactorAuth). Zorg ervoor dat er geen certificaat waarschuwingen of de fouten worden weergegeven.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## <a name="deploying-the-azure-multi-factor-authentication-server-user-portal-on-a-separate-server"></a>De Portal Azure meervoudige verificatie Server gebruiker op een afzonderlijke Server implementeren

Pas de Azure meervoudige verificatie-App gebruiken, het volgende is vereist zodat de app met User Portal communiceren kunt:

Zie Hardware en Software Requirements voor hardware en software vereisten:

- U moet gebruiken v6.0 of hoger van de Server Azure meervoudige verificatie.
- User Portal moet worden geïnstalleerd op een webserver internetgerichte is met Microsoft® Internet Information Services (IIS) 6.x IIS 7.x of hoger.
- Bij gebruik van IIS 6.x, Controleer ASP.NET v2.0.50727 is geïnstalleerd, geregistreerd en instellen op toegestaan.
- Rolservices vereist bij gebruik van IIS 7.x of hoger, bevatten ASP.NET en IIS 6 Metabase compatibiliteit.
- User Portal moet worden beveiligd met een SSL-certificaat.
- De SDK van Azure meervoudige verificatie Web Service moet zijn geïnstalleerd in IIS 6.x IIS 7.x of hoger nodig voor de server die op de Server Azure meervoudige verificatie is geïnstalleerd.
- De SDK van Azure meervoudige verificatie Web Service moeten worden beveiligd met een SSL-certificaat.
- User Portal moet kunnen verbinding maken met de SDK van Azure meervoudige verificatie Web Service via SSL.
- User Portal moet toegang hebben tot de Azure meervoudige verificatie Web Service SDK met de referenties van een service-account die deel uitmaakt van een beveiligingsgroep 'PhoneFactor beheerders' genoemd. Deze serviceaccount en de groep bestaat niet in Active Directory als de Server Azure meervoudige verificatie wordt uitgevoerd op een server domein behoren. Deze serviceaccount en de groep bestaat lokaal op de Server Azure meervoudige verificatie als deze niet met een domein verbonden is.

Installatie van de gebruiker-portal op een server dan de Server Azure meervoudige verificatie vereist de volgende drie stappen uitvoeren:

1. De webservice SDK installeren
2. Installeren van de gebruiker-portal
3. De Portal gebruikersinstellingen configureren in de Server Azure meervoudige verificatie


### <a name="install-the-web-service-sdk"></a>De webservice SDK installeren

Als de SDK van Azure meervoudige verificatie Web Service niet al is geïnstalleerd op de Server Azure meervoudige verificatie, gaat u naar die server en opent u de Server Azure meervoudige verificatie. Klik op het pictogram Web Service SDK, klikt u op de SDK Web Service installeren... knop en volg de aanwijzingen. De SDK van de Service Web moeten worden beveiligd met een SSL-certificaat. Een zelfondertekend certificaat dat hiervoor in orde is, maar er worden geïmporteerd in de store "Vertrouwde certificeringsinstanties die basiscertificaten" van de lokale Computer-account op de webserver User Portal zodat deze dat certificaat vertrouwt wanneer de SSL-verbinding tot stand brengen.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### <a name="install-the-user-portal"></a>Installeren van de gebruiker-portal

Voordat u de gebruiker-portal installeert op een afzonderlijke server, moet u als volgt te houden van de volgende opties:

- Is het handig om open een webbrowser op de webserver internetgerichte en navigeer naar de URL van de SDK van de Web-Service die is ingevoerd in het bestand web.config. Als de browser kunt u naar de webservice is, moet deze u wordt gevraagd om referenties. Voer de gebruikersnaam en wachtwoord die zijn ingevoerd in het bestand web.config precies zoals deze wordt weergegeven in het bestand. Zorg ervoor dat er geen certificaat waarschuwingen of de fouten worden weergegeven.
- Als een reverse-proxy of firewall zich vóór de webserver User Portal en uitvoering SSL offloading, kunt u het bestand User Portal web.config bewerken en toevoegen van de volgende toets om de <appSettings> sectie zodat de gebruiker-Portal http in plaats van https gebruiken kunt. <add key="SSL_REQUIRED" value="false"/>

#### <a name="to-install-the-user-portal"></a>Voor het installeren van de gebruiker-portal

1. Open Windows Verkenner op de server Azure meervoudige verificatie en navigeer naar de map waarin de Server Azure meervoudige verificatie is geïnstalleerd (zoals C:\Program Files\Multi-Factor verificatieserver). De 32-bits of 64-bits versie van het MultiFactorAuthenticationUserPortalSetup installatiebestand zo nodig voor de server voor die gebruiker Portal wordt geïnstalleerd op kiezen. Kopieer het bestand voor installatie op de server internetgerichte.
2. Klik op de webserver internetgerichte, moet het installatiebestand worden uitgevoerd met beheerdersrechten. De eenvoudigste manier hiervoor is om open een opdrachtpromptvenster als beheerder en navigeer naar de locatie waar het bestand voor installatie is gekopieerd.
3. Het installatiebestand MultiFactorAuthenticationUserPortalSetup64 wordt uitgevoerd, naam van de Site en virtuele map desgewenst wijzigen.
4. Nadat de installatie van de Portal van de gebruiker is voltooid, blader naar C:\inetpub\wwwroot\MultiFactorAuth (of de juiste map op basis van de naam van de virtuele map) en het bestand web.config bewerken.
5. Zoek de sleutel USE_WEB_SERVICE_SDK en wijzig de waarde van false in true. Zoek de toetsen WEB_SERVICE_SDK_AUTHENTICATION_USERNAME en WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD en stel de waarden op de gebruikersnaam en wachtwoord van de serviceaccount die deel uitmaakt van de beveiliging PhoneFactor Admins groeperen (Zie het gedeelte vereisten voor hierboven). Zorg ervoor dat u de gebruikersnaam en wachtwoord invoeren tussen de aanhalingstekens aan het einde van de regel (waarde = "" / >). Het wordt aanbevolen voor het gebruik van een gekwalificeerde gebruikersnaam (bijvoorbeeld domein\gebruikersnaam of machine\username)
6. Zoek de instelling pfup_pfwssdk_PfWsSdk en wijzig de waarde van 'http://localhost:4898/PfWsSdk.asmx' naar de URL van de Web-Service SDK die wordt uitgevoerd op de Server Azure meervoudige verificatie (bijvoorbeeld https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Aangezien SSL wordt gebruikt voor deze verbinding, moet u de SDK van de Service Web door de servernaam van de en niet IP-adres verwijzen naar aangezien het SSL-certificaat wordt zijn verleend voor naam van de server en de URL die wordt gebruikt, moet overeenkomen met de naam op het certificaat. Als de naam van de server niet is opgelost naar een IP-adres van de server internetgerichte, moet u een fragment toevoegen aan het hostsbestand op die server naar de naam van de Server Azure meervoudige verificatie toewijzen aan het IP-adres. Sla het bestand web.config nadat u wijzigingen hebt aangebracht.
7. Als de website van dat User Portal onder (bijvoorbeeld standaardwebsite) is geïnstalleerd al niet is binded met een certificaat openbaar zijn aangemeld, het certificaat installeren op de server als dat niet al is geïnstalleerd, opent u IIS-beheer en afhankelijk van het certificaat naar de website.
8. Open een webbrowser vanaf elke computer en Ga naar de URL waarin gebruiker Portal is geïnstalleerd (bijvoorbeeld https://www.publicwebsite.com/MultiFactorAuth). Zorg ervoor dat er geen certificaat waarschuwingen of de fouten worden weergegeven.



## <a name="configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>De portal gebruikersinstellingen configureren in de Server Azure meervoudige verificatie
Nu de portal is geïnstalleerd, moet u voor het configureren van de Server Azure meervoudige verificatie voor gebruik met de portal.

Azure meervoudige verificatie-server biedt verschillende opties voor de gebruiker-portal.  De volgende tabel vindt u een lijst van deze opties en een verklaring van de waarvoor ze worden gebruikt.

Portal gebruikersinstellingen|Beschrijving|
:------------- | :------------- |
Gebruiker Portal-URL| Kunt u voert u de URL van waar de portal wordt gehost.
Primaire verificatie| Kunt u opgeven van het type verificatie gebruiken als u zich aanmeldt bij de portal.  Windows, de RADIUS is opgegeven of de LDAP-verificatie.
Toestaan dat gebruikers kunnen aanmelden|Kunnen gebruikers in te voeren van een gebruikersnaam en wachtwoord op de aanmeldingspagina voor de gebruiker-portal.  Als dit niet is geselecteerd, wordt de vakken grijs weergegeven.
Registratie van de gebruiker toestaan|Kan de gebruiker inschrijven voor meervoudige verificatie door deze naar een installatiescherm die ze om aanvullende informatie zoals telefoonnummer vraagt.  Prompt weergeven voor back-phone kan gebruikers een secundaire telefoonnummer opgeven.  Waarschuwen voor derden EED token kan gebruikers een 3e partijen EED token opgeven.
Gebruikers toestaan zich te initiëren One-Time overslaan| Hiermee kan gebruikers een eenmalige overslaan initiëren.  Als een gebruiker sets dit dit wordt pas van invloed zijn op de volgende keer wordt de gebruiker zich aanmeldt.  Waarschuwen voor overslaan seconden biedt de gebruiker met een vak dat zij de standaardwaarde van 300 seconden kunnen wijzigen.  De eenmalige overslaan is anders alleen goede 300 seconden.
Gebruikers toestaan zich te methode selecteren| Kunnen gebruikers hun primaire contactmethode opgeven.  Dit is telefoongesprek, SMS-bericht, de mobiele app of EED token.
Gebruikers toestaan zich te Selecteer taal|  Kan de gebruiker de taal die wordt gebruikt voor het telefoongesprek, SMS-bericht, de mobiele app of EED token wijzigen.
Gebruikers toestaan zich te activeren mobiele app| Kunnen de gebruikers om een activeringscode Voltooi de mobiele app activering die wordt gebruikt met de server te genereren.  U kunt ook het aantal apparaten die kunnen ze dit activeren op instellen.  Tussen 1 en 10.
Vragen over beveiliging gebruiken voor gebruik|Kunt u vragen over beveiliging gebruiken voor het geval meervoudige verificatie mislukt.  Het aantal beveiliging vragen die met succes moeten worden beantwoord, kunt u opgeven.
Gebruikers toestaan zich te koppelen van derden EED token| Kunnen gebruikers een derde partij EED token opgeven.
Gebruik EED token voor gebruik|Kan voor het gebruik van een token EED in het geval dat meervoudige verificatie niet geslaagd is.  U kunt ook de sessietime opgeven in minuten.
Logboekregistratie inschakelen|Logboekregistratie op de gebruiker-portal.  De logboekbestanden bevinden zich op: C:\Program Files\Multi-Factor verificatie Server\Logs.

De meeste van deze instellingen zijn zichtbaar voor de gebruiker als ze worden ingeschakeld en de gebruiker positief of negatief in de portal van de gebruiker.

![Portal gebruikersinstellingen](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### <a name="to-configure-the-user-portal-settings-in-the-azure-multi-factor-authentication-server"></a>De portal gebruikersinstellingen configureren in de Server Azure meervoudige verificatie




1. Klik op het User Portal-pictogram in de Server Azure meervoudige verificatie. Voer de URL naar de Portal van de gebruiker in het tekstvak URL van de gebruiker-Portal op het tabblad instellingen. Deze URL wordt ingevoegd in e-mailberichten die zijn verzonden naar gebruikers wanneer ze worden geïmporteerd in de Server Azure meervoudige verificatie als de functionaliteit voor e-mail is ingeschakeld.
2. Kies de instellingen die u wilt gebruiken in de Portal van de gebruiker. Bijvoorbeeld als gebruikers hun verificatiemethoden bepalen kunnen, zorg ervoor dat gebruikers toestaan deel te methode selecteren samen met de methoden ze kunnen kiezen uit is ingeschakeld.
3. Klik op de koppeling Help in de rechterbovenhoek voor hulp bij het informatie over een van de instellingen weergegeven.

<center>![Setup](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## <a name="administrators-tab"></a>Tabblad voor beheerders
Dit tabblad kunt gewoon u gebruikers die beheerdersbevoegdheden hebben toevoegen.  Bij het toevoegen van een beheerder, u kunt deze af te stellen de machtigingen die ze ontvangen.  Op deze manier u kunt Zorg ervoor dat alleen de benodigde machtigingen verlenen aan de beheerder.  Klik op de knop toevoegen en selecteer vervolgens en machtigingen van gebruikers en hun en klik vervolgens op toevoegen.

![Beheerders van de gebruikers-portal](./media/multi-factor-authentication-get-started-portal/admin.png)


## <a name="security-questions"></a>Vragen over beveiliging
Dit tabblad kunt u opgeven van de beveiliging vragen die gebruikers moeten bieden antwoorden op als de gebruik beveiliging vragen voor fallback optie is geselecteerd.  Azure meervoudige Authenticaton Server wordt geleverd met standaardvragen die u kunt gebruiken.  U kunt ook de volgorde wijzigen of uw eigen vragen toevoegen.  Wanneer u uw eigen vragen toevoegt, kunt u de taal die u deze vraag wordt weergegeven dat wilt in als u ook opgeven.

![Gebruiker portal beveiliging vragen](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## <a name="passed-sessions"></a>Sessies met doorgegeven

## <a name="saml"></a>OP SAML
Kunt u voor het instellen van de gebruiker-portal als u wilt accepteren claims van een identiteitsprovider SAML gebruiken.  U kunt de sessie time-out opgeven, geef het verificatiecertificaat en het logboek af Omleidings-URL.

![OP SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## <a name="trusted-ips"></a>Vertrouwde IP-adressen
Dit tabblad kunt u deze op één IP-adressen of IP-adresbereiken die kunnen worden toegevoegd zodat meervoudige verificatie genegeerd als een gebruiker vanaf een van deze IP-adressen aanmeldt zich, opgeven.

![Gebruiker portal vertrouwde IP-adressen](./media/multi-factor-authentication-get-started-portal/trusted.png)

## <a name="self-service-user-enrollment"></a>Selfservice-gebruiker registratie
Als u wilt dat uw gebruikers u zich aanmeldt en inschrijven moet u de gebruikers toestaan deel te aanmelding in selecteert en gebruikersopties voor de registratie toestaan. Vergeet niet dat de instellingen die u selecteert invloed is op de gebruikerservaring aanmelden.

Bijvoorbeeld wanneer een gebruiker zich bij de Portal van de gebruiker aanmelden en op de knop Log In, ze worden vervolgens die u hebt gemaakt naar de instellingspagina Azure meervoudige verificatie gebruiker.  Afhankelijk van hoe u Azure meervoudige verificatie hebt geconfigureerd, kunnen de gebruiker mogelijk naar hun verificatiemethode selecteren.  

Als ze de verificatiemethode telefoongesprek selecteren of vooraf geconfigureerde zijn gebruikt, wordt de pagina de gebruiker om in te voeren hun telefoonnummer en een toestelnummer indien van toepassing.  Ze mag ook een back-telefoonnummer invoeren.  

![Gebruiker portal vertrouwde IP-adressen](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Als de gebruiker een PINCODE gebruiken moet wanneer ze wordt geverifieerd, wordt de pagina ook vragen om een PINCODE opgeven.  Na het invoeren van hun telefoonnummer (s) en PINCODE (indien beschikbaar), wordt de gebruiker de mij nu bellen om te verifiëren knop klikt.  Azure meervoudige verificatie wordt de verificatie van een telefoongesprek naar het telefoonnummer van de gebruiker uitvoeren.  De gebruiker moet de telefoon-oproep beantwoorden en voer de PINCODE (indien van toepassing) en druk op # verplaatsen op naar de volgende stap in het zelf registratieproces.   

Als de gebruiker selecteert u de tekst van de SMS-verificatiemethode of vooraf geconfigureerde met deze methode is, wordt de pagina voor hun mobiele telefoonnummer dat de gebruiker gevraagd.  Als de gebruiker een PINCODE gebruiken moet wanneer ze wordt geverifieerd, wordt de pagina ook vragen om een PINCODE opgeven.  Na het invoeren van hun telefoonnummer en PINCODE (indien beschikbaar), wordt de gebruiker de tekst mij nu om te verifiëren knop klikt.  Azure meervoudige verificatie wordt een SMS-verificatie naar van de gebruiker mobiele telefoon uitvoeren.  De gebruiker moet ontvangen de SMS waarin een een-tijd-wachtwoordcode (OTP) en beantwoorden met het bericht dat OTP plus hun PINCODE indien van toepassing) verplaatsen op naar de volgende stap in het zelf registratieproces.

![SMS-portal van de gebruiker](./media/multi-factor-authentication-get-started-portal/text.png)   

Als de gebruiker selecteert u de mobiele app verificatiemethode of vooraf geconfigureerde met deze methode is, wordt de pagina de gebruiker voor het installeren van de Azure meervoudige verificatie-app op het apparaat en een activeringscode genereren.  Na de installatie van de Azure meervoudige verificatie-app, de gebruiker klikt op de knop Activering-Code genereren.    

>[AZURE.NOTE]Pas de app Azure meervoudige verificatie gebruiken, moet de gebruiker push-meldingen voor hun apparaat inschakelen.

De pagina geeft een activeringscode en een URL samen met een streepjescode-afbeelding.  Als de gebruiker een PINCODE gebruiken moet wanneer ze wordt geverifieerd, wordt de pagina ook vragen om een PINCODE opgeven.  De gebruiker voert de activeringscode en de URL in de app Azure meervoudige verificatie of de streepjescodescanner gebruikt om de afbeelding streepjescode te scannen en op de knop activeren.    

Wanneer de activering voltooid is, wordt de gebruiker de knop mij nu verifiëren.  Azure meervoudige verificatie wordt een verificatie van de gebruiker de mobiele app uitvoeren.  De gebruiker moet hun pincode (indien van toepassing) en druk op de knop verifiëren in hun mobiele app verplaatsen op naar de volgende stap in het zelf registratieproces.  


Als de beheerders van de Azure meervoudige verificatieserver verzamelen beveiliging vragen en antwoorden hebt geconfigureerd, is, klikt u vervolgens de gebruiker wordt naar de pagina beveiliging vragen.  De gebruiker moet Selecteer vier beveiliging vragen en antwoorden op de geselecteerde vragen bieden.    

![Gebruiker portal beveiliging vragen](./media/multi-factor-authentication-get-started-portal/secq.png)  

De gebruiker zelf registratie is voltooid en de gebruiker is aangemeld bij de Portal van de gebruiker.  Gebruikers kunnen zich aanmelden weer aan bij de Portal van de gebruiker op elk gewenst moment in de toekomst om hun telefoonnummers, pincodes, verificatiemethoden en vragen over de beveiliging als dit wordt toegestaan door de beheerder.
