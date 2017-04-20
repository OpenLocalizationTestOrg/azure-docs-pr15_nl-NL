<properties 
    pageTitle="Eenmalige aanmelding naar toepassingen die niet zijn opgenomen in de galerie met Azure Active Directory-toepassing configureren | Microsoft Azure" 
    description="Leer hoe-apps met Azure Active Directory met SAML en op basis van wachtwoorden eenmalige aanmelding bij self-service verbinden" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Eenmalige aanmelding naar toepassingen die niet zijn opgenomen in de galerie met Azure Active Directory-toepassing configureren

In dit artikel gaat over een functie waarmee beheerders voor het configureren van eenmalige aanmelding naar toepassingen niet aanwezig zijn in de Azure Active Directory-app galerie *-code te schrijven*. Deze functie is uitgebracht van technische bètaversie van op 18e November 2015 en deel uitmaakt van [Azure Active Directory Premium](active-directory-editions.md). Als u in plaats daarvan ontwikkelaars instructies over het aangepaste apps integreren met Azure AD tot en met code zoekt, raadpleegt u [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md).

De galerie met Azure Active Directory-toepassing biedt een overzicht van toepassingen waarvan u weet ter ondersteuning van een vorm van eenmalige aanmelding met Azure Active Directory, zoals wordt beschreven in [dit artikel](active-directory-appssoaccess-whatis.md). Als u (als een IT specialist of systeem integrator in uw organisatie) de toepassing die u wilt verbinden hebt gevonden, u kunt aan de slag door Volg de stapsgewijze instructies in de beheerportal Azure gepresenteerd om in te schakelen eenmalige aanmelding.

Klanten met [Azure Active Directory Premium](active-directory-editions.md) licenties ook toegevoegd aan deze extra mogelijkheden:

* Selfservice-integratie van een toepassing die ondersteuning biedt voor SAML 2.0-identiteitsprovider (SP-gestart of IdP worden gestart)
* Selfservice-integratie van een webtoepassing met een HTML-e-aanmeldingspagina [op basis van wachtwoorden Eenmalige](active-directory-appssoaccess-whatis.md#password-based-single-sign-on) aanmelding van
* Selfservice-verbinding van toepassingen die gebruikmaken van het protocol SCIM voor de gebruiker inrichten ([hier beschreven](active-directory-scim-provisioning.md))
* Koppelingen toevoegen aan een toepassing in het [Startprogramma voor Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of in het [deelvenster van Azure AD-toegang](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

Dit kunt niet alleen SaaS-toepassingen die u gebruikt, maar hebben geen nog is op-aangehouden de galerie met Azure AD-toepassing, maar derden webtoepassingen die uw organisatie heeft geïmplementeerd in de servers die u, in de cloud- of on-premises implementatie beheert opnemen.

Deze mogelijkheden, ook bekend als *integratie Toepassingssjablonen*, bieden gestandaardiseerde verbindingspunten voor apps die SAML, SCIM of op formulieren gebaseerde verificatie te ondersteunen, en flexibele opties en instellingen voor compatibiliteit met een groot aantal toepassingen bevatten. 

##<a name="adding-an-unlisted-application"></a>Een niet-vermelde toepassing toe te voegen

Als u wilt verbinden met een toepassing op basis van een sjabloon voor een app-integratie, meld u aan bij de Azure beheerportal met uw beheerdersaccount Azure Active Directory en bladert u naar de **Active Directory > [map] > toepassingen** sectie, selecteer **toevoegen**en selecteer **een toepassing in de galerie toevoegen**. 

![][1]

Klik in de galerie app kunt u een niet-vermelde app via de categorie **aangepast** aan de linkerkant, of door te selecteren van de koppeling **toevoegen een niet-vermelde-toepassing** die wordt weergegeven in de lijst met zoekresultaten als de gewenste app is niet gevonden toevoegen. Na het invoeren van een naam voor uw toepassing, kunt u de opties voor eenmalige aanmelding en de werking. 

**Snelle tip**: als een goede gewoonte, kunt u de functie VIND.spec gebruiken om te controleren om te zien als de toepassing al in de galerie toepassing bestaat. Als de app wordt gevonden en een beschrijving 'eenmalige aanmelding' en vervolgens de toepassing vermeldingen wordt al ondersteund voor federatieve eenmalige aanmelding. 

![][2]

Toevoegen van een toepassing op deze manier beschikt u over een vergelijkbaar ervaring met de sjabloon beschikbaar voor vooraf geïntegreerde toepassingen. Als u wilt starten, selecteert u **Configureren eenmalige aanmelding**. Het volgende scherm geeft de volgende drie opties voor het configureren van eenmalige op, die in de volgende secties worden beschreven.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding

Selecteer deze optie om SAML-gebaseerde verificatie configureren voor de toepassing. Hiervoor is vereist dat de programmaondersteuning SAML 2.0 en u informatie te over het gebruik van de SAML mogelijkheden van de toepassing verzamelen dient voordat u verdergaat. Nadat u **volgende**selecteert, wordt u gevraagd om in te voeren drie verschillende URL's die overeenkomt met de eindpunten SAML voor de toepassing. 

![][4]
 
Dit zijn:

* **Aanmelding op URL (SP-gestart alleen)** – wanneer de gebruiker naar aanmelden bij deze toepassing gaat. Als de toepassing is geconfigureerd voor het uitvoeren van service provider gestart eenmalige op, klikt u vervolgens wanneer een gebruiker naar deze URL navigeert, doet de serviceprovider de benodigde omleiding op Azure AD om te verifiëren en meld u aan de gebruiker in. Als dit veld is ingevuld, klikt u vervolgens Azure AD gebruikt deze URL start de toepassing van Office 365 en het deelvenster Azure AD-toegang. Als dit veld ommited is en Azure AD in plaats daarvan identiteitsprovider uitvoert-gestarte aanmeldingsgegevens als de app is gestart vanuit Office 365, het deelvenster Azure AD Access of de Azure AD eenmalige aanmelding URL (copiable vanaf het tabblad Dashboard).

* **Uitgever URL** - uitgever van de URL in de toepassing voor welke eenmalige uniek aanduiden wordt geconfigureerd. Dit is de waarde die Azure AD terug naar de toepassing als de parameter van de **doelgroep** van het SAML-token en de toepassing wordt verwacht dat deze valideert. Deze waarde wordt ook weergegeven als de **Entiteit-ID** in de metagegevens van een SAML verstrekt door de toepassing. Controleren op SAML-documentatie voor meer informatie over wat u van de toepassing entiteit-id of publiek waarde is. Hierna ziet u een voorbeeld van hoe de URL van het publiek wordt weergegeven in het SAML-token geretourneerd naar de toepassing is:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Antwoord URL** - het antwoord URL waar de toepassing is voor het ontvangen van het SAML-token verwacht. Dit wordt ook de **bevestiging consumenten Service (ACS) URL**genoemd. Controleren op SAML-documentatie voor meer informatie over functies zijn SAML token-antwoord URL of ACS-URL van de toepassing.
 Nadat deze zijn ingevoerd, klikt u op **volgende** om naar het volgende scherm te gaan. Dit scherm vindt u informatie over wat u worden geconfigureerd aan de kant van de toepassing moet te kunnen een SAML-token van Azure AD accepteren. 

![][5]

Waarden die zijn vereist wordt varieert afhankelijk van de toepassing, dus kijk SAML-documentatie van de toepassing voor meer informatie. De **Aanmelding** en **Afmelden** service-URL beide omzetten in hetzelfde eindpunt, dat wil het eindpunt van de aanvraag foutafhandeling SAML voor uw exemplaar van Azure AD zeggen. De URL van de uitgever is de waarde die wordt weergegeven als de "uitgever" binnen de SAML-token uitgegeven aan de toepassing. 

Nadat uw toepassing is geconfigureerd, klikt u op de knop **volgende** en klik vervolgens op de **voltooid** om het dialoogvenster te sluiten. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Gebruikers en groepen toe te wijzen aan uw SAML-toepassing 

Wanneer uw toepassing is geconfigureerd voor het gebruik van Azure AD als een SAML-gebaseerde identiteitsprovider, is het bijna klaar om te testen. Azure AD verleent als een besturingselement voor een waardepapier, niet een token zodat ze u zich aanmeldt bij de toepassing tenzij ze toegang tot een Azure AD hebben gekregen. Gebruikers kunnen toegang is verleend rechtstreeks of via een groep waarvoor u ze lid bent. 

Als u wilt toewijzen een gebruiker of groep in uw toepassing, klikt u op de knop **Gebruikers toewijzen** . Selecteer de gebruiker of groep die u wilt toewijzen en selecteer vervolgens de knop **toewijzen** . 

![][6]

Het toewijzen van een gebruiker kan Azure AD te geven van een token voor de gebruiker, evenals de oorzaak van een tegel voor deze toepassing in het deelvenster van de toegang van de gebruiker moet worden weergegeven. De tegel van een toepassing wordt ook weergegeven in het startprogramma voor Office 365-toepassing als de gebruiker is met Office 365. 

U kunt een tegel logo voor de toepassing met behulp van de knop **Uploaden Logo** op het tabblad **configureren** voor de toepassing uploaden. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>De claims uitgegeven in het SAML-token aanpassen 

Wanneer een gebruiker geverifieerd bij de toepassing, verleent Azure AD een SAML-token door naar de app met gegevens (of claims), over de gebruiker die ze heeft een unieke id. Standaard bevat dit de gebruikersnaam, e-mailadres, voornaam en achternaam van de gebruiker. 

U kunt bekijken of bewerken van de verzonden in het SAML-token met de toepassing, klik op het tabblad **kenmerken** claims. 

![][7]

Er zijn twee mogelijke redenen waarom moet u mogelijk de claims uitgegeven in het SAML-token bewerken: •de toepassing naar een andere set claimen URI's vereisen is geschreven of claimwaarden •Your toepassing is geïmplementeerd op een manier die moeten worden de NameIdentifier claimen moeten een ander nummer dan de gebruikersnaam (btw UPN) die zijn opgeslagen in Azure Active Directory. 

Voor informatie over het toevoegen en bewerken van vorderingen die voortvloeien uit deze scenario's, raadpleegt u in dit [artikel over op claims aanpassen](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>De toepassing SAML testen 

Zodra de SAML-URL's en het certificaat zijn geconfigureerd in Azure AD en in de toepassing, gebruikers of groepen zijn toegewezen aan de toepassing in Azure wordt aangegeven, en de claims zijn bekeken en eventueel bewerkt en vervolgens de gebruiker is klaar om aan te melden bij de toepassing. 

Testen, klikt u gewoon teken in de Azure AD toegang hebt tot Configuratiescherm op https://myapps.microsoft.com met een gebruikersaccount die u hebt toegewezen aan de toepassing en klik op de tegel voor de toepassing op gang van het proces voor eenmalige aanmelding. U kunt ook rechtstreeks naar de URL eenmalige aanmelding voor de toepassing en aanmeldingsproblemen bladeren daarvandaan. 

Zie het [artikel over de fouten opsporen in SAML-gebaseerde eenmalige aanmelding naar toepassingen](active-directory-saml-debugging.md) voor foutopsporing tips, 

##<a name="password-single-sign-on"></a>Eenmalige aanmelding wachtwoord

Selecteer deze optie als u wilt configureren [op basis van wachtwoorden eenmalige aanmelding](active-directory-appssoaccess-whatis.md) voor een webtoepassing met een HTML-aanmeldingspagina. Op basis van wachtwoorden SSO, ook bekend als wachtwoord vaulting, kunt u de toegang van gebruikers en wachtwoorden voor webtoepassingen die geen ondersteuning voor identiteitsfederatie bieden beheren. Het is ook handig voor scenario's waarin meerdere gebruikers delen van een één account wilt, zoals naar sociale media app-accounts van uw organisatie. 

Na het **volgende**te selecteren, wordt u gevraagd om in te voeren van de URL van de pagina van de toepassing op het web aanmelden. Houd er rekening mee dat dit de pagina met de gebruikersnaam en wachtwoord invoervelden moet zijn. Zodra u hebt ingevoerd, wordt een proces parseren van de pagina aanmelden voor een input gebruikersnaam en wachtwoord hebben invoer door Azure AD gestart. Als het proces geslaagd is, klikt u vervolgens begeleidt die u bij een alternatief proces browserextensie (hiervoor Internet Explorer, Chrome of Firefox) waarmee u om vast te leggen handmatig de velden te installeren.

Wanneer de aanmeldingspagina is vastgelegd, gebruikers en groepen kunnen zijn aangewezen en referentie beleidsregels net als normale [wachtwoord SSO apps](active-directory-appssoaccess-whatis.md)kunnen worden ingesteld.

Opmerking: U kunt een logo tegel voor de toepassing met behulp van de knop **Uploaden Logo** op het tabblad **configureren** voor de toepassing te uploaden. 

##<a name="existing-single-sign-on"></a>Bestaande eenmalige aanmelding

Selecteer deze optie om een koppeling toevoegen aan een toepassing als het deelvenster Azure AD toegang of Office 365-portal van uw organisatie. U kunt dit koppelingen toevoegen aan aangepaste WebApps die momenteel gebruikmaken van Azure Active Directory Federation Services (of een andere service Federatie) gebruiken in plaats van Azure AD voor verificatie. Of u kunt uitgebreide koppelingen toevoegen aan specifieke SharePoint-pagina's of andere webpagina's die u alleen wilt weergeven op een van de gebruiker toegang panelen. 

Na het **volgende**te selecteren, wordt u gevraagd de URL van de toepassing om de koppeling naar invoeren. Als voltooid, kunnen gebruikers en groepen worden toegewezen aan de toepassing, waardoor de toepassing moet worden weergegeven in het [Startprogramma voor Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) of in het [deelvenster van Azure AD-toegang](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) voor gebruikers.

Opmerking: U kunt een logo tegel voor de toepassing met behulp van de knop **Uploaden Logo** op het tabblad **configureren** voor de toepassing te uploaden.

## <a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Het aanpassen van Claims uitgegeven in het SAML-Token voor vooraf geïntegreerde Apps](active-directory-saml-claims-customization.md)
- [Op SAML gebaseerde eenmalige aanmelding oplossen](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
