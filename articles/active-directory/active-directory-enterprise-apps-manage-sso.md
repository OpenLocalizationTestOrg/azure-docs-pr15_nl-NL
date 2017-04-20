<properties
    pageTitle="Eén aanmelding management voor bedrijfs-apps in de voorbeeldversie Azure Active Directory | Microsoft Azure"
    description="Informatie over het beheren van eenmalige op voor bedrijfs-apps met behulp van de Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Voorbeeld: Eenmalige aanmelding voor bedrijfs-apps in de nieuwe Azure-portal beheren

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure klassieke portal](active-directory-sso-integrate-saas-apps.md)

In dit artikel wordt beschreven hoe de [Azure-portal](https://portal.azure.com) gebruiken voor het beheren van instellingen voor eenmalige aanmelding voor toepassingen, met name lijsten die zijn toegevoegd in de [galerie met Azure Active Directory (Azure AD)-toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). De ervaring voor het beheer van Azure AD voor eenmalige aanmelding is momenteel in de proefversie van openbare en in dit artikel worden de nieuwe functies, evenals een paar tijdelijke beperkingen die alleen tijdens de proefperiode ter plaatse zijn. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Uw apps zoeken in de nieuwe portal

Vanaf September 2016, kunnen alle toepassingen die zijn geconfigureerd voor één aanmelding in een map, door de beheerder van een map met de [galerie met Azure Active Directory-toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) in de [klassieke Azure-portal](https://manage.windowsazure.com), nu worden bekeken en beheerd in de portal van Azure.

Deze toepassingen vindt u in de sectie **Bedrijfstoepassingen** van de Azure-portal, een koppeling waarop kan worden gevonden in de lijst **Meer Services** in de [portal](https://portal.azure.com). Bedrijfs-apps zijn apps die zijn geïmplementeerd en worden gebruikt door gebruikers binnen uw organisatie.

![Bedrijfstoepassingen blade][1]

Selecteer **alle toepassingen** voor een lijst met alle apps die zijn geconfigureerd, met inbegrip van apps die is toegevoegd vanuit de galerie. Een app selecteren, wordt het blad resource voor deze app, waar rapporten kunnen worden bekeken voor deze app en een aantal instellingen kan worden beheerd geladen.

Als u wilt beheren instellingen voor eenmalige aanmelding, selecteert u **eenmalige aanmelding**.

![Toepassing resource blade][2]


##<a name="single-sign-on-modes"></a>Modi voor eenmalige aanmelding

Het blad **eenmalige aanmelding** begint met een menu **modus** , waarmee de modus voor eenmalige aanmelding te configureren. De beschikbare opties omvatten:

* **Op SAML gebaseerde aanmeldingsgegevens** - met deze optie is beschikbaar als de toepassing ondersteunt volledige federatieve eenmalige aanmelding met Azure Active Directory via het SAML 2.0-protocol.

* **Aanmeldingsgegevens op basis van wachtwoorden** - met deze optie is beschikbaar als Azure AD ondersteunt wachtwoord-formulier invullen van deze toepassing.

* **Gekoppelde zich aanmeldt op** - voorheen bekend als "bestaande eenmalige aanmelding', deze optie beheerders kunnen een koppeling naar deze toepassing plaatsen in hun gebruikerswachtwoord Azure AD Access Configuratiescherm of Office 365-toepassing starten.

Zie voor meer informatie over deze modi, [hoe eenmalige aanmelding met Azure Active Directory werk](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>Op SAML gebaseerde aanmeldingsgegevens

De optie **aanmelden op SAML gebaseerde** bevat een blade die in vier secties is verdeeld:

###<a name="domains-and-urls"></a>Domeinen en URL 's
Dit is waar alle meer informatie over het domein en de URL's van de toepassing worden toegevoegd aan uw adreslijst Azure AD. Alle invoer vereist om inzicht te werk voor eenmalige aanmelding app worden weergegeven rechtstreeks in het scherm dat alle optionele invoeritems kunnen worden bekeken door het selectievakje **weergeven geavanceerde instellingen van de URL** te selecteren. De volledige lijst met ondersteunde invoeritems bevat:

* **Aanmelden op URL** – waar de gebruiker naar aanmelden bij deze toepassing gaat. Als de toepassing is geconfigureerd voor het uitvoeren van service provider gestart eenmalige op, klikt u vervolgens wanneer een gebruiker naar deze URL navigeert, biedt de serviceprovider de benodigde omleiding naar Azure AD om te verifiëren en meld u aan de gebruiker. Als dit veld is ingevuld, klikt u vervolgens Azure AD gebruikt deze URL start de toepassing van Office 365 en het deelvenster Azure AD-toegang. Als dit veld wordt weggelaten, wordt en Azure AD in plaats daarvan identiteitsprovider wordt-gestarte aanmeldingsgegevens als de app is gestart vanuit Office 365, het deelvenster Azure AD Access of de Azure AD eenmalige aanmelding URL.

* **Id** - deze URI moet de toepassing uniek zijn voor voor welke enkel zich aanmeldt op is geconfigureerd. Dit is de waarde die Azure AD terug naar de toepassing als de parameter van de doelgroep van het SAML-token en de toepassing wordt verwacht dat deze valideert. Deze waarde wordt ook weergegeven als de entiteit-ID in de metagegevens van een SAML verstrekt door de toepassing.

* **Antwoord URL** - het antwoord URL waar de toepassing is voor het ontvangen van het SAML-token verwacht. Dit wordt ook de URL bevestiging consumenten Service (ACS) genoemd. Nadat deze zijn ingevoerd, klikt u op volgende om naar het volgende scherm te gaan. Dit scherm vindt u informatie over wat u worden geconfigureerd aan de kant van de toepassing moet te kunnen een SAML-token van Azure AD accepteren.

* **Relay staat** - de stand relay is een optionele parameter waarmee u de toepassing aangeeft waar de gebruiker doorsturen kunt nadat verificatie is voltooid. De waarde is meestal de URL van een geldige bij de toepassing, maar sommige toepassingen gebruik dit veld anders (Zie eenmalige van de app op documentatie voor meer informatie). De mogelijkheid om in te stellen van de stand relay is een nieuwe functie die uniek is voor de nieuwe portal van Azure.

###<a name="user-attributes"></a>Gebruikerskenmerken
Dit is waar beheerders kunnen bekijken en bewerken van de kenmerken die zijn verzonden in het SAML-token die Azure AD met de toepassing elke problemen keer gebruikers zich aanmelden.

Voor de eerste preview-versie is het alleen bewerkbaar kenmerk ondersteund het **Gebruikers-id** -kenmerk. De waarde van dit kenmerk is het veld in Azure AD die deze identificeert op elke gebruiker in de toepassing. Bijvoorbeeld als de app is geïmplementeerd het "e-mailadres' als de gebruikersnaam en het unieke id gebruiken, zou klikt u vervolgens de waarde worden ingesteld naar het veld 'user.mail' in Azure AD.

Bewerken van extra kenmerken worden ondersteund in een latere voorbeeld.

###<a name="saml-signing-certificate"></a>Op SAML-certificaat voor ondertekening
Hier vindt u de details van het certificaat waarmee Azure AD worden ondertekend de SAML-tokens die worden verzonden naar de toepassing telkens wanneer de gebruiker wordt geverifieerd. Kan de eigenschappen van het huidige certificaat moet worden gecontroleerd, inclusief de vervaldatum.

De mogelijkheid om te rollover het certificaat en beheren van extra certificaat opties worden ondersteund in een latere preview-versie. Houd er rekening mee dat volledig beheer van certificaten nog steeds in de [portal van Azure klassieke](active-directory-sso-certs.md)kan worden uitgevoerd.

###<a name="application-configuration"></a>Configuratie van toepassingen
Het laatste gedeelte vindt u de documentatie en/of besturingselementen die zijn vereist voor het configureren van de toepassing zelf op Azure Active Directory gebruiken als een identiteitsprovider.

Het menu van de fly-out **Toepassing configureren** bevat nieuwe beknopte, ingesloten instructies voor het configureren van de toepassing. Dit is een andere nieuwe functie uniek zijn voor de nieuwe portal van Azure.

> [AZURE.NOTE] Zie de toepassing Salesforce.com voor een volledig voorbeeld van ingesloten documentatie. Documentatie voor Aanvullende apps wordt continu toegevoegd in de voorbeeldweergave.

![Ingesloten documenten][3]

##<a name="password-based-sign-on"></a>Aanmeldingsgegevens op basis van wachtwoorden
Als dit wordt ondersteund voor de toepassing, selecteren de modus SSO op basis van wachtwoorden en **Opslaan** direct geconfigureerd om uit te voeren op basis van wachtwoorden eenmalige aanmelding. Zie voor meer informatie over het implementeren van eenmalige aanmelding op basis van wachtwoorden, [hoe eenmalige aanmelding met Azure Active Directory werk](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Aanmeldingsgegevens op basis van wachtwoorden][4]


##<a name="linked-sign-on"></a>Gekoppelde aanmeldingsgegevens
Als dit wordt ondersteund voor de toepassing, kunt de gekoppelde SSO-modus u voert u de URL die u wilt dat het deelvenster Azure AD toegang of Office 365 om te leiden als gebruikers deze app op. Zie voor meer informatie over gekoppelde eenmalige aanmelding (voorheen bekend als "bestaande SSO"), [hoe eenmalige aanmelding met Azure Active Directory werk](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Gekoppelde aanmelding][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
