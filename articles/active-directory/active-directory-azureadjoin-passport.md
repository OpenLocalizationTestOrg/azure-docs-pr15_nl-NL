<properties
    pageTitle="Verificatie identiteiten zonder wachtwoorden via Microsoft Passport | Microsoft Azure"
    description="Biedt een overzicht van Microsoft Passport en aanvullende informatie over het implementeren van Microsoft Passport."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren

De huidige methoden verificatie met alleen wachtwoorden zijn niet voldoende om gebruikers te beschermen. Gebruikers opnieuw gebruiken en wachtwoorden vergeet. Wachtwoorden zijn breachable, phishable, vatbaar voor scheuren en te raden. Ook worden verkregen moeilijk te onthouden en vatbaar voor aanvallen zoals '[de hash doorgeven](https://technet.microsoft.com/dn785092.aspx)'.

## <a name="about-microsoft-passport"></a>Info over Microsoft Passport
Microsoft Passport is een privé/openbare sleutel of certificaatverificatie methode voor organisaties en consumenten die voorrang wachtwoorden. Dit formulier verificatie is afhankelijk van een combinatie van sleutel referenties die wachtwoorden kunnen vervangen en inbreuk op, diefstal en phishing te beschermen tegen zijn.

 Microsoft Passport kan een gebruiker zijn of een Microsoft-account, een Windows Server Active Directory-account, een Microsoft Azure Active Directory (Azure AD)-account of een niet-Microsoft-service die ondersteuning biedt voor snel identiteit Online (FIDO) verificatie wordt geverifieerd. Microsoft Passport is ingesteld op van de gebruiker apparaat na een verificatie in eerste twee stappen tijdens de registratie van Microsoft Passport, en de gebruiker Hiermee stelt u een penbeweging Windows Hallo of een PINCODE. De gebruiker biedt de beweging om hun identiteit de verifiëren. Windows wordt Microsoft Passport voor de verificatie van de gebruiker en helpen voor toegang tot beveiligde bronnen en services.

De persoonlijke sleutel is verstrekt enkel via een 'gebruiker beweging"zoals een PINCODE, biometrie of een extern apparaat zoals een smartcard waarin de gebruiker voor het aanmelden bij het apparaat wordt gebruikt. Deze informatie wordt gekoppeld aan een certificaat of een combinatie van asymmetrische sleutel. De persoonlijke sleutel is hardware gestaafd als het apparaat een vertrouwde Platform Module (Trusted)-chip heeft. De persoonlijke sleutel is nooit verlaat van het apparaat.

De openbare sleutel is geregistreerd met Azure Active Directory en Windows Server Active Directory (voor on-premises implementatie). Identiteitsprovider (IDPs) valideren van de gebruiker door het toewijzen van de openbare sleutel van de gebruiker aan de persoonlijke sleutel en bevatten informatie aanmelden via één keer wachtwoord (OTP), PhoneFactor of een om verschillende melding.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Waarom ondernemingen moeten werken met Microsoft Passport

Doordat Microsoft Passport ondernemingen kunnen hun resources zelfs beter beveiligen door:

* Bij het instellen van Microsoft Passport met een optie hardware voorkeurstaal. Dit betekent dat sleutels gegenereerd op TPM 1.2 of TPM 2.0 indien beschikbaar. Wanneer TPM niet beschikbaar is, wordt de software de sleutel genereren.

* De complexiteit en de lengte van de PINCODE, definiëren en of Hallo gebruik in uw organisatie is ingeschakeld.

* Configuratie van Microsoft Passport ter ondersteuning van smartcard-achtige scenario's met behulp van vertrouwen op basis van certificaten.

## <a name="how-microsoft-passport-works"></a>De werking van Microsoft Passport
1. Toetsen worden aan de hardware gegenereerd door TPM of software. Veel apparaten hebben een ingebouwde TPM-chip die de hardware beveiligt door te integreren cryptografische sleutels in apparaten. TPM 1.2 of TPM 2.0 genereert toetsen of certificaten die zijn gemaakt op basis van de gegenereerde toetsen.

2. De TPM verklaart deze toetsen hardware gebonden.

3. Het apparaat dat u een beweging één ontgrendelen ontgrendelt. Deze beweging voor toegang tot meerdere resources als het apparaat een domein behoren of Azure is AD-die zijn gekoppeld.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>De werking van de levenscyclus van Microsoft Passport

![Levenscyclus van Microsoft Passport](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Het diagram ziet u een combinatie van de privé/openbare sleutel en de validatie door de identiteitsprovider. Elk van deze stappen wordt uitgelegd in detail hier:

1. De gebruiker blijkt hun identiteit tot en met meerdere ingebouwde taalprogramma methoden (bewegingen, fysieke smartcards, meervoudige verificatie) en verzendt deze informatie aan een identiteit Provider (IDP) zoals Azure Active Directory of lokale Active Directory.

2. Het apparaat vervolgens de sleutel gemaakt, verklaart de toets, duurt het openbare gedeelte van deze toets, worden deze gekoppeld met station-instructies, zich aanmeldt en verzonden naar de IDP registreren van de toets.

4. Zodra de IDP de openbare deel van de sleutel registreert, vraagt de IDP het apparaat aan te melden met het persoonlijke gedeelte van de sleutel.

5. De IDP vervolgens gevalideerd en de beveiligde bronnen voor de verificatietoken waarmee de gebruiker en de toegang tot de apparaten problemen. IDPs kan meerdere platforms apps schrijven of browserondersteuning (via JavaScript/Webcrypto API's) maken en gebruiken van Microsoft Passport-referenties voor hun gebruikers.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>De implementatievereisten voor Microsoft Passport
### <a name="at-the-enterprise-level"></a>Op het ondernemingsniveau van de

* De onderneming heeft een Azure-abonnement.

### <a name="at-the-user-level"></a>Op het gebruikersniveau van de

* Computer van de gebruiker wordt uitgevoerd voor Windows 10 Professional of Enterprise.

Zie voor gedetailleerde instructies, [Microsoft Passport inschakelen voor werk in de organisatie](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Aanvullende informatie

* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
