<properties
    pageTitle="Veelgestelde vragen over Azure meervoudige verificatie"
    description="Hier vindt u een lijst met veelgestelde vragen en antwoorden die betrekking hebben op Azure meervoudige verificatie. Meervoudige verificatie is een methode voor het verifiëren van de identiteit van een gebruiker die meer dan een gebruikersnaam en wachtwoord vereist. Een extra beveiliging op aanmelding en transacties krijgen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Veelgestelde vragen over Azure meervoudige verificatie


Deze Veelgestelde vragen over antwoorden op veelgestelde vragen over Azure meervoudige verificatie en het gebruik van de meervoudige verificatie-service, inclusief vragen over de facturering model en bruikbaarheid.

## <a name="general"></a>Algemene

**V: hoe wordt gebruikersgegevens door Azure meervoudige verificatieserver worden verwerkt?**

Gebruikersgegevens is opgeslagen met meervoudige verificatie-Server, alleen op de on-premises implementatie-servers. Geen permanente gebruikersgegevens wordt opgeslagen in de cloud. Wanneer de gebruiker verificatie in twee stappen uitvoert, verzendt meervoudige verificatieserver gegevens naar de cloud-service van de Azure meervoudige verificatie voor verificatie. Communicatie tussen meervoudige verificatie-Server en de meervoudige verificatie-cloudservice gebruikt Secure Sockets Layer (SSL) of beveiliging TLS (Transport Layer) via poort 443 uitgaande.

Als verificatieaanvragen worden verzonden naar de cloudservice, gegevens worden verzameld voor verificatie en het gebruik van rapporten. Gegevensvelden die zijn opgenomen in twee stappen verificatie logboeken zijn als volgt:

- **Unieke ID** (beide gebruikers naam of on-premises meervoudige verificatie Server-ID)
- **En achternaam** (optioneel)
- **E-mailadres** (optioneel)
- **Telefoonnummer** (als u een telefoongesprek of SMS-verificatie)
- **Apparaat Token** (als u de mobiele app verificatie)
- **Verificatiemodus**
- **Verificatieresultaat**
- **Meervoudige verificatie-servernaam**
- **Meervoudige verificatieserver IP**
- **Client IP** (indien beschikbaar)

De optionele velden kunnen worden geconfigureerd in meervoudige verificatie-Server.

Het resultaat verificatie (slagen of geweigerd) en de reden als deze is geweigerd, wordt met de verificatiegegevens is opgeslagen en beschikbaar is in verificatie-en gebruiksrapporten.


## <a name="billing"></a>Facturering

De meeste billing vragen kunnen worden beantwoord door te verwijzen naar de [pagina meervoudige verificatie prijzen](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

**V: is mijn organisatie betalen voor telefoongesprekken of SMS-berichten die worden gebruikt om te verifiëren van mijn gebruikers?**

Organisaties worden geen btw geheven voor afzonderlijke telefoongesprekken geplaatst of tekst aan gebruikers via Azure meervoudige verificatie verzonden berichten. Eigenaren van de telefoon mogelijk betalen voor de telefoongesprekken of de SMS-berichten die ze, op basis van hun telefoon-service ontvangen.

**V: doet de per gebruiker billing model boete op basis van het aantal gebruikers die zijn geconfigureerd voor meervoudige verificatie of het aantal gebruikers dat controles uitvoeren?**

Facturering is gebaseerd op het aantal gebruikers dat is geconfigureerd voor gebruik van meervoudige verificatie.

**V: hoe werkt facturering meervoudige verificatie?**

Wanneer u gebruikt de "per gebruiker" of "per verificatie" model, Azure MFA een resource verbruik gebaseerde is. Eventuele kosten zijn gefactureerd aan van de organisatie Azure abonnement net als virtuele machines, websites, enzovoort.

Wanneer u het licentiemodel gebruikt, zijn Azure meervoudige verificatie licenties hebt gekocht en vervolgens toegewezen aan gebruikers, net als voor Office 365 en andere producten abonnement.

**V: is er een gratis versie van Azure meervoudige verificatie voor beheerders?**

In sommige gevallen, Ja. Meervoudige verificatie voor beheerders van Azure biedt een subset van Azure MFA functies gratis. Deze aanbieding is van toepassing op leden van de groep Azure globale beheerders in Azure Active Directory-processen die niet zijn gekoppeld aan een Azure meervoudige verificatie op basis van verbruik van provider. Alle beheerders en gebruikers in de map die de volledige versie van Azure meervoudige verificatie met meervoudige verificatie zijn geconfigureerd met een meervoudige verificatie-provider worden bijgewerkt.

**V: is er een gratis versie van Azure meervoudige verificatie voor Office 365-gebruikers?**

In sommige gevallen, Ja. Meervoudige verificatie voor Office 365 biedt een subset van Azure MFA functies gratis. Deze aanbieding geldt voor gebruikers met een Office 365-licentie is toegewezen, wanneer een Azure meervoudige verificatie op basis van verbruik van provider is niet gekoppeld aan het bijbehorende exemplaar van Azure Active Directory. Alle beheerders en gebruikers in de map die de volledige versie van Azure meervoudige verificatie met meervoudige verificatie zijn geconfigureerd met de meervoudige verificatie-provider worden bijgewerkt.

**V: kan mijn organisatie schakelen tussen per gebruiker en per verificatie billing verbruik-modellen op elk gewenst moment?**

Uw organisatie kiest een factureringsbeheerder model bij het maken van een resource. U kunt een factureringsbeheerder model niet meer wijzigen nadat de resource is ingericht. U kunt echter een andere meervoudige verificatie resource als u wilt vervangen van de oorspronkelijke maken. Gebruikersinstellingen en configuratieopties kunnen niet worden doorgeschakeld naar de nieuwe resource.

**V: kan mijn organisatie schakelen tussen de verbruik facturering en het licentiemodel op elk gewenst moment?**

Wanneer licenties zijn toegevoegd aan een map die beschikt over een provider van de Azure meervoudige verificatie per gebruiker, is verbruik gebaseerde facturering verlaagd met het aantal licenties in bezit. Als alle gebruikers geconfigureerd voor gebruik van meervoudige verificatie toegewezen licenties hebt, kan de beheerder de provider Azure meervoudige verificatie verwijderen.

U kunt geen per verificatie verbruik facturering met een licentiemodel combineren. Wanneer een provider van de meervoudige verificatie per verificatie is gekoppeld aan een map, wordt de organisatie gefactureerd voor alle meervoudige verificatie verificatieaanvragen, ongeacht eventuele licenties in bezit.

**V: Mijn organisatie hoeft te gebruiken en identiteiten als u wilt gebruiken Azure meervoudige verificatie synchroniseren?**

Als een organisatie gebruikmaakt van een verbruik gebaseerde billing model, is Azure Active Directory niet vereist. Een provider meervoudige verificatie koppelen aan een map is optioneel. Als uw organisatie is niet gekoppeld aan een adreslijst, kunt deze Azure meervoudige verificatieserver of de Azure meervoudige verificatie SDK on-premises implementeren.

Azure Active Directory is vereist voor het licentiemodel omdat licenties zijn toegevoegd aan de map wanneer u aanschaffen en aan gebruikers in de adreslijst toewijzen.


## <a name="usability"></a>Bruikbaarheid

**V: wat doet een gebruiker als ze geen ontvangt een antwoord op hun telefoon of als de telefoon is niet beschikbaar voor de gebruiker?**

Als de gebruiker heeft een back-telefoon geconfigureerd, moeten ze probeer het opnieuw en selecteert u die telefoon wanneer u wordt gevraagd op de pagina aanmelden. Als de gebruiker geen een andere methode die zijn geconfigureerd, kan de beheerder van de organisatie het getal dat is toegewezen aan de primaire telefoonnummer van de gebruiker kunt bijwerken.


**V: wat doet de beheerder als een gebruiker met de beheerder voor informatie over een account dat de gebruiker geen toegang meer contactpersonen?**

De beheerder kan opnieuw instellen van account van de gebruiker door het verzoek om het registratieproces opnieuw doorlopen. Meer informatie over het [beheren van gebruikers en apparaatinstellingen met Azure meervoudige verificatie in de cloud](multi-factor-authentication-manage-users-and-devices.md).

**V: wat doet een beheerder van een gebruiker telefoon die is gebruik van appwachtwoorden is verlies of diefstal?**

De beheerder kan de appwachtwoorden van de gebruiker om te voorkomen dat onbevoegden verwijderen. Nadat de gebruiker een vervangende-apparaat heeft, kan de gebruiker de wachtwoorden opnieuw. Meer informatie over het [beheren van gebruikers en apparaatinstellingen met Azure meervoudige verificatie in de cloud](multi-factor-authentication-manage-users-and-devices.md).

**V: Wat gebeurt er als de gebruiker niet aanmelden bij browser-apps?**

Een gebruiker die is geconfigureerd voor gebruik van meervoudige verificatie is vereist een appwachtwoord te melden bij sommige apps niet-browser. Een gebruiker moet (verwijderen) aanmeldingsproblemen wilt verwijderen, start de app en aanmelden met behulp van hun naam en app gebruikerswachtwoord.

Meer informatie over het maken van de appwachtwoorden en andere [helpen met appwachtwoorden](multi-factor-authentication-end-user-app-passwords.md)vinden.


>[AZURE.NOTE] Moderne verificatie voor Office 2013-clients
>
> Nieuwe verificatieprotocollen wordt ondersteund door Office 2013-clients (met inbegrip van Outlook). U kunt Office 2013 voor meervoudige verificatie configureren. Nadat u Office 2013 hebt geconfigureerd, zijn niet appwachtwoorden vereist voor Office 2013-clients. Voor meer informatie raadpleegt u de [aankondiging van Office 2013 moderne verificatie openbare preview](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**V: wat doet een gebruiker als de gebruiker niet een SMS-bericht ontvangt of als de gebruiker een tweerichtingsvertrouwensrelatie tussen SMS-bericht beantwoordt, maar de verificatie treedt er een time-out?**

Bezorgen van SMS-berichten en ontvangst van antwoorden in twee richtingen SMS is niet gegarandeerd omdat er hebt factoren die van invloed kunnen zijn op de betrouwbaarheid van de service. Deze factoren zijn de land van de bestemming en de mobiele telefoon carrier signaalsterkte.

Gebruikers die problemen ondervindt betrouwbaar tekstberichten ontvangen moeten in plaats daarvan de mobiele app of telefoongesprek methode selecteren. De mobiele app kunt meldingen zowel via mobiel en Wi-Fi-verbindingen ontvangen. Bovendien kunt de mobiele app verificatie codes genereren, zelfs wanneer het apparaat geen signaal helemaal heeft. De app Microsoft Authenticator is beschikbaar voor [Windows Phone-](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Als u SMS-berichten gebruiken moet, wordt u aangeraden met enkelvoudige SMS in plaats van met twee variabelen SMS indien mogelijk. Enkelvoudige SMS meer betrouwbaar is en deze voorkomen dat gebruikers dat globale SMS-gesprekskosten zijn van het beantwoorden van een SMS-bericht dat is verzonden door een ander land.


**V: kan ik Hardwaretokens gebruiken met Azure meervoudige verificatieserver?**

Als u Azure meervoudige verificatieserver gebruikt, kunt u importeren van derden openen verificatie (EED) op basis van tijd en eenmalig wachtwoord (TOTP) tokens en deze vervolgens gebruiken voor verificatie in twee stappen.

U kunt ActiveIdentity tokens die EED TOTP tokens zijn als u het geheime belangrijke bestand heb in een CSV-bestand opgeslagen en met Azure meervoudige verificatie-Server importeren. U kunt EED tokens gebruiken met Active Directory Federation Services (ADFS), verificatie inbellen gebruiker Service RADIUS (Remote) wanneer access uitdaging antwoorden kan worden verwerkt door het clientsysteem en Internet Information Server (IIS) op formulieren gebaseerde verificatie.

U kunt importeren derden EED TOTP tokens met de volgende indelingen:  
- Portable Symmetric belangrijke Container (PSKC)  
- CSV als het bestand een serieel getal, een geheime sleutel in de basis 32-indeling en een tijdsinterval bevat  

**V: kan ik Azure meervoudige verificatieserver beveiligen Terminal Services gebruiken?**

Ja, maar als u via Windows Server 2012 R2 of hoger gebruikt, alleen via Extern bureaublad-Gateway (RD-Gateway).

Wijzigingen in de beveiliging in Windows Server 2012 R2 zijn de manier waarop Azure meervoudige verificatieserver verbinding met het beveiligingspakket lokale beveiliging (LSA) in Windows Server 2012 en eerdere versies worden gewijzigd. Voor de versies van Terminal Services in Windows Server 2012 of eerder kunt u [een toepassing met Windows-verificatie secure](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Als u Windows Server 2012 R2 gebruikt, moet u de RD-Gateway.

**V: Waarom zou een gebruiker een oproep meervoudige verificatie ontvangen van een anonieme beller na het instellen van de beller-ID?**

Wanneer meervoudige verificatie-oproepen via de openbare telefoonnetwerk worden geplaatst, worden soms ze gerouteerd via een luchtvaartmaatschappij die biedt geen ondersteuning voor beller-ID. Reden is beller-ID niet gegarandeerd, hoewel het systeem meervoudige verificatie altijd verzonden.


## <a name="errors"></a>Fouten

**V: Wat doe gebruikers als ze een foutbericht 'verificatie-aanvraag is voor een geactiveerde account' wordt weergegeven wanneer u een mobiele app meldingen gebruikt?**

Uitleggen dat ze moeten Volg deze procedure om hun account vanuit de mobiele app verwijderen en opnieuw toevoegen:

1. Ga naar [uw Azure portal profiel](https://account.activedirectory.windowsazure.com/profile/) en meld u aan met uw organisatie-account.
2. Selecteer **Extra beveiliging verificatie**.
4. Verwijder de bestaande account vanuit de mobiele app.
5. Klik op **configureren**en volg de instructies om te configureren, de mobiele app.


**V: Wat doe gebruikers als ze een 0x800434D4L foutbericht wordt weergegeven tijdens het aanmelden bij een toepassing voor de niet-browser?**

Een gebruiker kunt momenteel verificatie voor extra beveiliging alleen voor gebruik met toepassingen en services die de gebruiker toegang tot en met een browser. Niet-browser-toepassingen (ook wel *uitgebreide clienttoepassingen*genoemd) die zijn geïnstalleerd op een lokale computer, zoals Windows PowerShell, werkt niet met accounts waarvoor verificatie voor extra beveiliging. De toepassing genereert een fout 0x800434D4L ziet in dit geval van de gebruiker.

Een tijdelijke oplossing voor dit is om te laten afzonderlijke accounts voor beheerder-gerelateerde en niet-beheerders bewerkingen. Later, kunt u postvakken tussen uw beheerdersaccount en niet-beheerders account koppelen, zodat u bij Outlook aanmelden kunt met uw account niet-beheerders. Lees voor meer informatie over dit, geeft u [een beheerder de mogelijkheid om te openen en de inhoud van het postvak van een gebruiker bekijken](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Volgende stappen

Als uw vraag hier niet wordt beantwoord, laat u dit in de opmerkingen onder aan de pagina. Of, Hier volgen enkele aanvullende opties voor het Help-informatie opvragen:


**V: hoe kan ik hulp bij Azure meervoudige verificatie krijgen?**

- Zoeken in de [Microsoft Support Knowledge Base](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) voor oplossingen voor veelvoorkomende technische problemen.

- Zoeken naar en blader technische vragen en antwoorden van de community of uw eigen vraag stellen in de [Azure Active Directory-forums](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Als u een oudere PhoneFactor-klant bent en u vragen hebt of hulp bij het opnieuw instellen van een wachtwoord, gebruikt u de koppeling [wachtwoord opnieuw instellen](mailto:phonefactorsupport@microsoft.com) om een melding voor ondersteuning.

- Neem contact op met een ondersteuningsmedewerker tot en met de [ondersteuning van Azure meervoudige verificatieserver (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Wanneer het contact met ons opnemen, is het handig als u kunt zo veel mogelijk informatie over het probleem mogelijk opnemen. Informatie die u kunt opgeven bevat de pagina waar u de fout, de specifieke foutcode de specifieke sessie-ID en de ID van de gebruiker die de fout hebt gezien hebt gezien.
