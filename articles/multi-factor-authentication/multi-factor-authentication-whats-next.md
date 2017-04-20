<properties
    pageTitle="Azure meervoudige verificatie - volgende stappen"
    description="Dit is de pagina van de Azure meervoudige verificatie waarmee wordt beschreven wat u moet het volgende doen met MFA.  Dit geldt ook voor rapporten, fraude melding, eenmalige overslaan, aangepaste ingesproken berichten, caching, vertrouwde IP-adressen en app wachtwoorden."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Azure meervoudige verificatie configureren

In dit artikel kunt u beheren Azure meervoudige verificatie nu dat u goed werken.  Deze bedekt tal van onderwerpen die u helpen bij het optimaal gebruikmaken van Azure meervoudige verificatie.  Niet al deze functies zijn beschikbaar in elke versie van Azure meervoudige verificatie.

De configuratie voor enkele van de functies die hieronder vindt u in de beheerportal van Azure meervoudige verificatie. Er zijn twee verschillende manieren waartoe u toegang hebt de beheerportal MFA, die beide via de portal van Azure worden uitgevoerd. De eerste is door het beheer van een meervoudige Auth Provider als verbruik gebaseerde MFA gebruikt. Het tweede is via de service-instellingen van MFA. De tweede optie vereist een meervoudige Auth-Provider of een Azure MFA, Azure AD Premium of Enterprise mobiliteit Suite-licentie.

Voor toegang tot de beheerportal MFA via een Azure meervoudige Auth-Provider, meld u aan bij de Azure-portal als beheerder en selecteer de optie Active Directory. Klik op het tabblad **Meervoudige Auth Providers** , selecteer uw adreslijst en klik op de knop **beheren** onder.

Voor toegang tot de beheerportal MFA via de pagina Service-instellingen van MFA, meld u aan bij de Azure-portal als beheerder en selecteer de optie Active Directory. Klik op de map en klik vervolgens op het tabblad **configureren** . Selecteer onder de sectie meervoudige verificatie **beheren service-instellingen**. Klik onder aan de pagina Service-instellingen van MFA, op de koppeling **Ga naar de portal** .


Functie| Beschrijving| Wat zijn van toepassing op
:------------- | :------------- | :------------- |
[Waarschuwing](#fraud-alert)|Fraudewaarschuwing kan worden geconfigureerd en zo instellen dat uw gebruikers frauduleuze pogingen rapporteren kunnen voor toegang tot hun resources.|Het installeren, configureren en fraude rapporteren
[Eenmalige overslaan](#one-time-bypass) |Een eenmalige overslaan kan een gebruiker om te verifiëren van één keer "overslaat" meervoudige verificatie.|Het instellen en configureren van een eenmalige overslaan
[Aangepaste ingesproken berichten](#custom-voice-messages) |Aangepaste ingesproken berichten kunnen u uw eigen opnamen of begroetingen gebruiken met meervoudige verificatie. |Het instellen en configureren van aangepaste begroetingen en berichten
[In cache opslaan](#caching-in-azure-multi-factor-authentication)|In cache opslaan, kunt u een bepaald tijdstip periode instellen zodat pogingen verdere verificatie automatisch mislukt. |Het instellen en configureren caching van verificatie.
[Vertrouwde IP-adressen](#trusted-ips)|Vertrouwde dat IP-adressen is een functie van meervoudige verificatie waarmee beheerders van een tenant beheerde of federatieve de mogelijkheid, worden omzeild meervoudige verificatie voor gebruikers die zich wilt uit lokaal intranet van het bedrijf aanmelden.|Configureren en IP-adressen die niet moeten opgeschoond voor meervoudige verificatie worden instellen
[Appwachtwoorden](#app-passwords)|Een appwachtwoord kan een toepassing die is niet MFA hoogte naar meervoudige verificatie overslaan en doorgaan met werken.|Informatie over appwachtwoorden.
[Meervoudige verificatie bewaren voor onthouden apparaten en browsers](#remember-multi-factor-authentication-for-devices-users-trust)|Kunt u moet onthouden apparaten voor een bepaald aantal dagen nadat een gebruiker is aangemeld bij het gebruik van MFA.|Informatie over deze functie inschakelt en het aantal dagen in te stellen.
[Selecteerbaar verificatiemethoden te gebruiken](#selectable-verification-methods)|U kunt kiezen de verificatiemethoden die beschikbaar voor gebruikers zijn kunnen gebruiken.|Informatie over het in- of uitschakelen van specifieke verificatiemethoden zoals oproep of SMS ontvangt.



## <a name="fraud-alert"></a>Waarschuwing
Fraudewaarschuwing kan worden geconfigureerd en zo instellen dat uw gebruikers frauduleuze pogingen rapporteren kunnen voor toegang tot hun resources.  Gebruikers kunnen rapporteren fraude met de mobiele app of via de telefoon.

### <a name="to-set-up-and-configure-fraud-alert"></a>Instellen en configureren van fraudewaarschuwing

1.  Meld u aan bij http://azure.microsoft.com
2.  Navigeer naar de beheerportal MFA hand van de instructies boven aan deze pagina.
3.  Klik op instellingen onder de sectie configureren in de beheerportal Azure meervoudige verificatie.
4.  Controleer de gebruikers toestaan om in te dienen Fraudewaarschuwingen het selectievakje onder de sectie fraude waarschuwing van de pagina instellingen.
5.  Als u wilt dat gebruikers moeten worden geblokkeerd wanneer fraude wordt gemeld, schakel dit selectievakje in gebruiker blokkeren wanneer fraude wordt gemeld.
6.  Voer een numerieke code die kan worden gebruikt tijdens een gesprek verificatie in het tekstvak **Code naar rapport fraude tijdens aanvankelijke Begroetingsregel** . Als een gebruiker voert deze code plus # in plaats van alleen het teken #, wordt een melding voor een fraude worden gerapporteerd.
7.  Klik onderaan op opslaan.

>[AZURE.NOTE]
>Microsoft standaard voice begroetingen Geef aan gebruikers moet indrukken 0# om in te dienen een waarschuwing. Als u gebruiken van een code dan 0 wilt, moet u opnemen en upload uw eigen aangepaste voice begroetingen met instructies voor het juiste.


![Cloud](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Melding van fraude rapport
Fraudewaarschuwing kan op twee manieren worden gerapporteerd.  Hetzij via de mobiele app of via de telefoon.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Melding van fraude rapport met de mobiele app



1. Wanneer een verificatie wordt verzonden naar uw telefoon, selecteert u deze de app Microsoft Authenticator te starten.
2. Rapporteren van fraude, klikt u op de annuleren en het rapport fraude. Hiermee opent u een vak met de mededeling dat van uw organisatie IT ondersteuningspersoneel krijgt.
3. Klik op rapport fraude.
4. Klik op de app, klikt u op sluiten.

![Cloud](./media/multi-factor-authentication-whats-next/report1.png)


![Cloud](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Melding van fraude rapport met de telefoon

1. Wanneer u een verificatie-oproep ontvangt op uw telefoon, beantwoordt u deze.  
2. Als u wilt rapporteren van fraude, voert u de code die is geconfigureerd om te komen overeen met het rapporteren van fraude via de telefoon en klik vervolgens op het teken #. U wordt geïnformeerd dat een melding voor een fraude is ingediend.
3. De oproep beëindigen.

### <a name="to-view-the-fraud-report"></a>Het rapport fraude weergeven

1. Meld u aan bij [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Selecteer aan de linkerkant, Active Directory.
3. Selecteer boven meervoudige Auth Providers. Hiermee opent u een lijst met uw meervoudige Auth Providers.
4. Als u meer dan één meervoudige Auth Provider hebt, selecteert u de sectie die u wilt de waarschuwing rapport van fraude weergeven en klik op beheren onder aan de pagina. Als u alleen een hebt, klikt u op beheren. Hiermee opent u de beheerportal van Azure meervoudige verificatie.
5. Klik op de Azure meervoudige verificatie beheerportal, aan de linkerkant, onder weergave A rapport op fraude waarschuwing.
6. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook een specifieke gebruikersnamen, telefoonnummers en status van de gebruiker opgeven.
7. Klik op uitvoeren. Hiermee opent u een rapport dat vergelijkbaar is met de onderstaande. U kunt ook klikken op exporteren naar CSV als u wilt exporteren van het rapport.

## <a name="one-time-bypass"></a>Eenmalige overslaan

Een eenmalige overslaan kan een gebruiker om te verifiëren van één keer "overslaat" meervoudige verificatie. De overslaan is tijdelijk en verloopt na het opgegeven aantal seconden.  Zodat in situaties waarin de mobiele app of de telefoon geen een melding of telefoongesprek ontvangt, u een eenmalige overslaan inschakelen kunt zodat de gebruiker toegang hebt tot de gewenste resource.

### <a name="to-create-a-one-time-bypass"></a>Maken van een eenmalige overslaan

1.  Meld u aan bij http://azure.microsoft.com
2.  Navigeer naar de beheerportal MFA hand van de instructies boven aan deze pagina.
3.  Klik in de Azure meervoudige verificatie Management Portal als u de naam van de tenant of Azure MFA Provider aan de linkerkant met ziet een + ernaast, klikt u op het + raadpleegt u verschillende MFA Server herhaling groepen en de groep Azure standaard. Klik op de juiste groep.
4.  Klik onder beheer van de gebruiker op **One-Time overslaan**.
![Cloud](./media/multi-factor-authentication-whats-next/create1.png)
5.  Klik op de pagina One-Time overslaan op **Nieuwe One-Time overslaan**.
6.  Voer de gebruikersnaam, het aantal seconden dat de overslaan wordt bestaat, wordt de reden voor de overslaan en klik op **negeren**.
![Cloud](./media/multi-factor-authentication-whats-next/create2.png)
7.  Nu moet de gebruiker aanmelden voordat de eenmalige overslaan verloopt.



### <a name="to-view-the-one-time-bypass-report"></a>Weergeven van het rapport eenmalige overslaan

1. Meld u aan bij [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Selecteer aan de linkerkant, Active Directory.
3. Selecteer boven meervoudige Auth Providers. Hiermee opent u een lijst met uw meervoudige Auth Providers.
4. Als u meer dan één meervoudige Auth Provider hebt, selecteert u de sectie die u wilt de waarschuwing rapport van fraude weergeven en klik op beheren onder aan de pagina. Als u alleen een hebt, klikt u op beheren. Hiermee opent u de beheerportal van Azure meervoudige verificatie.
5. Klik op de Azure meervoudige verificatie beheerportal, aan de linkerkant, onder weergave A rapport op One-Time overslaan.
6. Geef het datumbereik dat u wilt weergeven in het rapport. U kunt ook een specifieke gebruikersnamen, telefoonnummers en status van de gebruiker opgeven.
7. Klik op uitvoeren. Hiermee opent u een rapport dat vergelijkbaar is met de onderstaande. U kunt ook klikken op exporteren naar CSV als u wilt exporteren van het rapport.

<center>![Cloud](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Aangepaste ingesproken berichten

Aangepaste ingesproken berichten kunnen u uw eigen opnamen of begroetingen gebruiken met meervoudige verificatie.  Deze kunnen worden gebruikt in aanvulling op of records voor het vervangen van Microsoft.

Voordat u begint Let van de volgende opties:

- De huidige ondersteunde bestandsindelingen zijn wav- en MP3.
- De maximale bestandsgrootte die is 5 MB.
- Het wordt aanbevolen dat voor verificatie die berichten het zijn niet meer dan 20 seconden. Iets groter is dan dit kan leiden tot de verificatie mislukt omdat de gebruiker niet reageert voordat het bericht is voltooid en time-out voor de verificatie.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Aangepaste voicemailberichten in Azure meervoudige verificatie instellen
1.  Maak een aangepaste spraakbericht met een van de ondersteunde bestandsindelingen.
2.  Meld u aan bij http://azure.microsoft.com
3.  Navigeer naar de beheerportal MFA hand van de instructies boven aan deze pagina.
4.  Klik op ingesproken berichten die onder de sectie configureren in de beheerportal Azure meervoudige verificatie.
5.  Klik op **Nieuw voicemailbericht**onder de sectie ingesproken berichten.
![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6.  Klik op de configureren: nieuwe voicemailberichten pagina, klikt u op **Geluidsbestanden beheren**.
![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7.  Klik op de configureren: geluid bestanden pagina, klikt u op **Geluid-bestand uploaden**.
![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8.  Klik op de configureren: geluidsbestand uploaden, klikt u op **Bladeren** en navigeer naar uw spraakbericht, klik op **openen**.
![Cloud](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Een beschrijving toevoegen en klik op uploaden.
10. Zodra deze is voltooid, wordt een bericht wordt bevestigd dat het bestand is geüpload.
11. Klik op ingesproken berichten die aan de linkerkant.
12. Klik op Nieuw voicemailbericht onder de sectie ingesproken berichten.
13. Selecteer een taal in de vervolgkeuzelijst taal.
14. Als dit bericht voor een specifieke toepassing is, kunt u deze in het vak opgeeft.
15. Selecteer het berichttype moet worden overschreven met onze nieuwe aangepast bericht in het Type bericht.
16. Selecteer in de vervolgkeuzelijst geluidsbestand uw geluidsbestand.
17. Klik op **maken**. Een bericht wordt bevestigd dat u een voicemailbericht hebt gemaakt.
![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>In cache opslaan in Azure meervoudige verificatie

In cache opslaan, kunt u een bepaald tijdstip periode instellen zodat pogingen verdere verificatie automatisch mislukt. Dit is vooral gebruikt wanneer on-premises implementatie-systemen zoals VPN meerdere verificatie aanvragen verzendt terwijl de eerste aanvraag nog steeds bezig is. Hiermee kunt de volgende aanvragen voor het automatisch wordt uitgevoerd nadat de gebruiker de verificatie bezig is geslaagd. Opmerking: in cache opslaan niet is bedoeld om u te worden gebruikt voor aanmeldingen naar Azure AD.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>In cache opslaan in Azure meervoudige verificatie instellen

1.  Meld u aan bij http://azure.microsoft.com
2.  Navigeer naar de beheerportal MFA hand van de instructies boven aan deze pagina.
3.  Klik op cache onder de sectie configureren in de beheerportal Azure meervoudige verificatie.
4.  Klik op de pagina caching configureren op nieuwe Cache
5.  Selecteer het type Cache en de seconden van de cache. Klik op maken.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Vertrouwde IP-adressen

Vertrouwde dat IP-adressen is een functie van meervoudige verificatie waarmee beheerders van een tenant beheerde of federatieve de mogelijkheid, worden omzeild meervoudige verificatie voor gebruikers die zich wilt uit lokaal intranet van het bedrijf aanmelden. De functies zijn beschikbaar voor Azure AD-tenants die Azure AD Premium, Enterprise mobiliteit Suite of Azure meervoudige verificatie-licentie hebben.


Type Azure AD-Tenant| Beschikbare vertrouwde IP-opties
:------------- | :------------- |
Beheerde|Specifieke IP-adresbereiken – beheerders kunnen een bereik van IP-adressen die meervoudige verificatie voor gebruikers die zich wilt vanuit het bedrijfsintranet aanmelden kunnen overslaan opgeven.
Federatieve|<li>Alle federatieve gebruikers - alle federatieve gebruikers die zich aanmelden uit binnen de organisatie kan overslaan meervoudige verificatie met een claim uitgegeven door AD FS.</li><li>Specifieke IP-adresbereiken – beheerders kunnen een bereik van IP-adressen die meervoudige verificatie voor gebruikers die zich wilt vanuit het bedrijfsintranet aanmelden kunnen overslaan opgeven.

Dit werkt alleen uit binnen een bedrijfsintranet kan overslaan. Dus bevat, als u alleen alle federatieve gebruikers hebt geselecteerd, en een gebruiker zich heeft aangemeld van buiten het bedrijfsintranet, die gebruiker bijvoorbeeld om te verifiëren met meervoudige verificatie, zelfs als de gebruiker een AD FS-claim presenteert. De volgende tabel wordt beschreven wanneer meervoudige verificatie en app wachtwoorden zijn vereist binnen uw corpnet en buiten uw corpnet wanneer vertrouwde IP-adressen is ingeschakeld.


|Vertrouwde IP-adressen ingeschakeld| Vertrouwde IP-adressen uitgeschakeld
:------------- | :------------- | :------------- |
Inside corpnet|Voor browser stromen, meervoudige verificatie niet vereist.|Voor browser stromen, meervoudige verificatie vereist
|Rich client-apps voor werken normale wachtwoorden als de gebruiker geen appwachtwoorden heeft gemaakt. Wanneer u een appwachtwoord hebt gemaakt, zijn appwachtwoorden zijn vereist.|Rich client-apps voor appwachtwoorden vereist
Buiten corpnet|Voor browser stromen, meervoudige verificatie vereist.|Voor browser stromen, meervoudige verificatie vereist.
|Rich client-apps voor appwachtwoorden vereist.|Rich client-apps voor appwachtwoorden vereist.

### <a name="to-enable-trusted-ips"></a>Vertrouwde IP's inschakelen

1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map op de map die u wilt instellen met vertrouwde IPsing op.
4. Klik op de map die u hebt geselecteerd, klikt u op configureren.
5. Klik op service-instellingen beheren in de sectie meervoudige verificatie.
6. Selecteer op de pagina Service-instellingen onder vertrouwde IP-adressen, een van de volgende opties:

    - Voor aanmeldingsaanvragen van federatieve gebruikers die afkomstig zijn van mijn intranet – alle federatieve kan gebruikers die zich wilt met het bedrijfsnetwerk aanmelden meervoudige verificatie met een claim uitgegeven door AD FS overslaan.
    - Voer voor aanvragen van een specifiek bereik van openbare IP-adressen – de IP-adressen in de vakken met CIDR-notatie. Bijvoorbeeld: xxx.xxx.xxx.0/24 voor IP-adressen in het bereik xxx.xxx.xxx.1 – xxx.xxx.xxx.254 of xxx.xxx.xxx.xxx/32 voor een enkel IP-adres. U kunt maximaal 50 IP-adresbereiken invoeren.

7. Klik op opslaan.
8. Nadat de updates zijn toegepast, klikt u op sluiten.



![Vertrouwde IP-adressen](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>Appwachtwoorden

U kunt meervoudige verificatie niet gebruiken in sommige apps, zoals Office 2010 of ouder en Apple Mail.  Als u wilt deze apps gebruiken, moet u "appwachtwoorden" gebruiken in plaats van uw traditionele wachtwoord.  Het appwachtwoord kan de toepassing meervoudige verificatie overslaan en doorgaan met werken.

>[AZURE.NOTE] Moderne verificatie voor de Office 2013-Clients
>
> Office 2013-clients (inclusief Outlook) is nu ondersteuning voor nieuwe verificatieprotocollen en kunnen worden ingeschakeld voor de ondersteuning van meervoudige verificatie.  Dit betekent dat eenmaal is ingeschakeld, appwachtwoorden niet vereist is voor gebruik met Office 2013-clients zijn.  Zie [Office 2013 moderne verificatie openbare preview aangekondigd](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)voor meer informatie.



### <a name="important-things-to-know-about-app-passwords"></a>Belangrijke dingen die u moet weten over appwachtwoorden

Hier volgt een lijst belangrijke dingen die u over appwachtwoorden weten moet.

- Gebruikers kunnen meerdere appwachtwoorden, waardoor het oppervlak voor diefstal hebben. Aangezien appwachtwoorden moeilijk zijn te onthouden, kan deze personen dit noteren aanmoedigen. Hiermee wordt niet aanbevolen en moet worden afgeraden omdat slechts één factor verplicht voor het aanmelden met appwachtwoord is.
- Apps waarin cache opslaan van wachtwoorden en deze gebruiken in on-premises implementatie scenario's kunnen eerst verbroken sinds het appwachtwoord is niet bekend buiten de organisatie-id. Een voorbeeld is Exchange e-mails die on-premises implementatie zijn, maar het gearchiveerde e-mailbericht zich in de cloud. Hetzelfde wachtwoord werkt niet.
- De werkelijke wachtwoord wordt automatisch gegenereerd en niet door de gebruiker worden verstrekt. Dit komt omdat de automatisch gegenereerde wachtwoord is moeilijker onbevoegden te raden en veiliger is.
- Er is momenteel een limiet van 40 wachtwoorden per gebruiker. U wordt gevraagd een van uw bestaande appwachtwoorden verwijderen om te maken van een nieuwe record.
- Zodra de meervoudige verificatie is ingeschakeld op een gebruikersaccount, appwachtwoorden kunnen worden gebruikt met de meeste browser-mailclients zoals Outlook en Lync, maar beheertaken kunnen niet worden uitgevoerd met behulp van appwachtwoorden tot en met niet-browser-toepassingen zoals Windows PowerShell, zelfs als die gebruiker een beheerdersaccount heeft.  Controleer of u een serviceaccount maken met een sterk wachtwoord PowerShell-scripts worden uitgevoerd en schakel geen dat account voor meervoudige verificatie.

>[AZURE.WARNING]  Appwachtwoorden werken niet in een hybride omgeving waarin clients communiceren met zowel on-premises implementatie en cloud automatisch opsporen-eindpunten. Dit is omdat domeinwachtwoorden zijn vereist voor de verificatie van on-premises implementatie en appwachtwoorden zijn vereist om te verifiëren met de cloud.


### <a name="naming-guidance-for-app-passwords"></a>Naming richtlijnen voor Appwachtwoorden
Het wordt aanbevolen dat app wachtwoord namen moeten overeenkomen met het apparaat waarop deze worden gebruikt. Bijvoorbeeld als u een laptop met niet-browser-apps zoals Outlook, Word en Excel hebt, hoeft u alleen een appwachtwoord met de naam Laptop maken en gebruiken van die appwachtwoord in alle van deze toepassingen. Hoewel u afzonderlijke wachtwoorden voor al deze toepassingen maken kunt, is het niet aanbevolen. De aanbevolen is een appwachtwoord per apparaat gebruiken.


<center>![Cloud](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Appwachtwoorden federatieve (SSO)
Azure AD ondersteunt Federatie met on-premises implementatie Windows Server Active Directory Domain Services (AD DS). Als uw organisatie federated(SSO) met Azure AD is en u gaat over het gebruik van Azure meervoudige verificatie, gebeurt het volgende vindt u belangrijke informatie dat u houden moet rekening bij gebruik van appwachtwoorden. Dit geldt alleen voor federated(SSO) klanten.

- Het wachtwoord van de App is gecontroleerd door Azure AD en dus omzeilt Federatie. Federatie alleen actief wordt gebruikt bij het instellen van Appwachtwoord.
- Voor gebruikers van federated(SSO) gaat we nooit u naar de identiteitsprovider (IdP) in tegenstelling tot de passieve stroom. De wachtwoorden zijn opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, wordt die info en organisatie-id met DirSync in realtime heeft. Account uitschakelen/verwijdering duurt maximaal drie uur synchroniseren, uitschakelen/verwijdering van Appwachtwoord in Azure AD vertragen.
- On-premises implementatie beheren in Access-Client-instellingen worden niet herkend door de Appwachtwoord
- Geen on-premises implementatie-verificatie logboekregistratie / controle van de mogelijkheid is beschikbaar voor Appwachtwoord
- Meer eindgebruikers education is vereist voor de Microsoft Lync 2013-client. Zie hoe u het wachtwoord in uw e-mailbericht naar het appwachtwoord wijzigen voor de vereiste stappen.
- Voor bepaalde geavanceerde architectuur ontwerpen kunnen vragen om een combinatie van organisatie-gebruikersnaam en wachtwoorden app gebruiken bij het gebruik van meervoudige verificatie met clients, afhankelijk van waar ze verifiëren. Voor klanten die worden geverifieerd bij een on-premises-infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor klanten die worden geverifieerd bij Azure AD, gebruikt u het appwachtwoord.

Stel dat u hebt een architectuur die uit de volgende bestaat:

- U bent uw exemplaar van de on-premises van Active Directory met Azure AD samenbrengen
- U gebruikt Exchange online
- U gebruikt Lync die specifiek on-premises is
- U gebruikt Azure meervoudige verificatie


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 In deze processen, moet u het volgende doen:

- Wanneer aanmelden bij Lync, gebruikt u de gebruikersnaam en wachtwoord van uw organisatie.
- Wanneer u probeert te krijgen tot het adresboek via een Outlook-client die is verbonden met Exchange online, gebruikt u een appwachtwoord.

### <a name="allowing-app-password-creation"></a>App wachtwoord maken toestaan
Gebruikers kunnen geen appwachtwoorden maken standaard.  Deze functie moet zijn ingeschakeld.  Als u wilt dat gebruikers de mogelijkheid om te appwachtwoorden maken, gebruikt u de volgende procedure.

#### <a name="to-enable-users-to-create-app-passwords"></a>Zodat gebruikers kunnen appwachtwoorden maken



1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder op de map voor de map voor de gebruiker die u wilt inschakelen.
4. Aan de bovenkant, klikt u op gebruikers.
5. Klik onder aan de pagina, op meervoudige Auth. beheren  
6. Aan de bovenkant van de pagina meervoudige verificatie, klikt u op Service-instellingen.
7. Zorg ervoor dat het keuzerondje naast gebruikers toestaan deel te appwachtwoorden u zich aanmeldt bij browser-toepassingen maken is geselecteerd.


![Appwachtwoorden maken](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>Appwachtwoorden maken
Gebruikers kunnen appwachtwoorden maken tijdens de registratie van hun aanvankelijke.  Ze zijn creditering aan het einde van het registratieproces waarmee ze maken.

Bovendien kunnen gebruikers ook maken appwachtwoorden later door hun instellingen in de Azure de portal Office 365-portal wijzigen of door

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Appwachtwoorden maken in de Office 365-portal
--------------------------------------------------------------------------------


1. Meld u aan bij de Office 365-portal
2. Selecteer in de rechterbovenhoek, de widget instellingen
3. Aan de linkerkant, selecteer extra beveiliging verificatie
4. Aan de rechterkant, selecteert u **Mijn telefoonnummers die wordt gebruikt voor accountbeveiliging bijwerken**
5. Selecteer op de pagina proofup aan de bovenkant, appwachtwoorden
6. Klik op **maken**
7. Voer een naam voor het appwachtwoord en klik op **volgende**
8. Het appwachtwoord naar het Klembord kopiëren en plak deze in uw app.

<center>![Cloud](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>Appwachtwoorden maken in de portal van Azure
--------------------------------------------------------------------------------
1. Meld u aan bij de portal van Azure klassieke.
3. Aan de bovenkant, met de rechtermuisknop op uw gebruikersnaam in te voeren en selecteer extra beveiliging verificatie.
5. Selecteer op de pagina proofup aan de bovenkant, appwachtwoorden
6. Klik op **maken**
7. Voer een naam voor het appwachtwoord en klik op **volgende**
8. Het appwachtwoord naar het Klembord kopiëren en plak deze in uw app.


![Appwachtwoorden](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>Appwachtwoorden maken als u nog geen een abonnement op Office 365- of Azure
--------------------------------------------------------------------------------
1. Meld u aan bij [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Aan de bovenkant, selecteer profiel.
3. Klik op uw gebruikersnaam in te voeren en selecteer extra beveiliging verificatie.
5. Selecteer op de pagina proofup aan de bovenkant, appwachtwoorden
6. Klik op **maken**
7. Voer een naam voor het appwachtwoord en klik op **volgende**
8. Het appwachtwoord naar het Klembord kopiëren en plak deze in uw app.

![Appwachtwoorden](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Meervoudige verificatie bewaren voor apparaten gebruikers vertrouwen

Meervoudige verificatie onthouden voor apparaten en browsers die gebruikers vertrouwen een gratis functie voor alle gebruikers van MFA is.  U kunt gebruikers geven de optie voor het omloopleiding MFA voor een bepaald aantal dagen na het uitvoeren van een succesvolle Meld u aan met MFA. Dit kunt verbeteren door de bruikbaarheid voor uw gebruikers.

Aangezien de gebruikers MFA onthouden voor vertrouwde apparaten mogen, kan deze functie echter accountbeveiliging verminderen. Om ervoor te zorgen accountbeveiliging, moet u meervoudige verificatie voor hun apparaten herstellen voor een van de volgende scenario's:

- Als hun zakelijke account voor meer betrouwbaar is
- Als een onthouden apparaat diefstal of

> [AZURE.NOTE] Deze functie is geïmplementeerd als een cookie browsercache. Dit werkt niet als uw browsercookies die niet zijn ingeschakeld.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Hoe u onthouden meervoudige verificatie in-/ uitschakelen

1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder Active Directory, op de map die u wilt onthouden meervoudige verificatie instellen voor apparaten.
4. Klik op de map die u hebt geselecteerd, klikt u op configureren.
5. Klik op service-instellingen beheren in de sectie meervoudige verificatie.
6. Klik op de pagina Service-instellingen onder apparaat gebruikersinstellingen beheren, selecteer/Hef de selectie van de **gebruikers toestaan deel te onthouden meervoudige verificatie op apparaten die ze vertrouwen**.
![Apparaten onthouden](./media/multi-factor-authentication-whats-next/remember.png)
8. Stel het aantal dagen dat u wilt toestaan dat geschorst. De standaardinstelling is 14 dagen.
9. Klik op opslaan.
10. Klik op sluiten.


## <a name="selectable-verification-methods"></a>Selecteerbaar verificatiemethoden te gebruiken
Klik op de cloud en de on-premises versies, kunt u kiezen welke verificatiemethoden zijn beschikbaar voor uw gebruikers. De onderstaande tabel biedt een beknopt overzicht van elke methode.

Wanneer uw gebruikers hun accounts voor MFA inschrijven, kies ze hun voorkeur verificatiemethode afmelden bij de opties die u hebt ingeschakeld. De richtlijnen voor hun registratieproces vindt u in [Mijn account voor verificatie in twee stappen instellen](multi-factor-authentication-end-user-first-time.md)

Methode|Beschrijving
:------------- | :------------- |
Bellen met telefoon |  Een geautomatiseerd spraaksysteem aanroep van de telefoon verificatie geplaatst. De gebruiker beantwoordt de oproep en op # drukt in het toetsenblok van de telefoon om te verifiëren. Dit telefoonnummer is niet gesynchroniseerd met lokale Active Directory.
SMS-bericht naar telefoon | Hiermee worden SMS-bericht met een verificatiecode in die aan de gebruiker. De gebruiker wordt gevraagd naar beide antwoord in het SMS-bericht met de verificatiecode of om de verificatiecode in de interface van aanmeldingsproblemen.
Melding tot en met de mobiele app | In deze modus is de app Microsoft Authenticator voorkomt onbevoegde toegang tot accounts en frauduleuze transacties stopt. Dit is voltooid met een push-bericht naar uw telefoon of geregistreerde apparaat. Weer te geven, de melding en als dit legitiem is u tikt op verifiëren. U mogelijk anders kiest u weigeren of ervoor kiezen om te weigeren en meldt u de frauduleuze melding. Zie voor meer informatie over het rapporteren van frauduleuze meldingen het gebruik van de weigeren en fraude rapportfunctie voor meervoudige verificatie.</br></br>De app Microsoft Authenticator is beschikbaar voor [Windows Phone-](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).|
Verificatiecode vanuit mobiele app | In deze modus kan de app Microsoft Authenticator worden gebruikt als een software-token genereren een verificatiecode EED. Deze verificatiecode in die is vervolgens samen met de gebruikersnaam en wachtwoord op te geven van de tweede vorm van verificatie toegestaan.</li><br><p> De app Microsoft Authenticator is beschikbaar voor [Windows Phone-](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### <a name="how-to-enabledisable-authentication-methods"></a>Hoe u methoden voor gebruikersverificatie in-/ uitschakelen

1. Meld u aan bij de portal van Azure klassieke.
2. Aan de linkerkant, klikt u op Active Directory.
3. Klik onder Active Directory, op de map die u wilt in- of uitschakelen van verificatiemethoden.
4. Klik op de map die u hebt geselecteerd, klikt u op configureren.
5. Klik op service-instellingen beheren in de sectie meervoudige verificatie.
6. Klik op de pagina Service-instellingen onder Opties voor verificatie, selecteer/schakelt u de opties die u wilt gebruiken.</br></br>
![Opties voor verificatie](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Klik op opslaan.
10. Klik op sluiten.
