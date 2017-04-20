<properties
   pageTitle="Integreren met Azure Active Directory | Microsoft Azure"
   description="Een hulplijn naar de voordelen van en bronnen voor integratie met Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integreren met Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory biedt organisaties met bedrijfstoepassingen Identiteitsbeheer voor cloud-toepassingen.  Azure AD-integratie geeft uw gebruikers een gestroomlijnde aanmeldervaring en helpt uw toepassing die voldoen aan de IT-beleid.

## <a name="how-to-integrate"></a>Hoe u kunt integreren

Er zijn verschillende manieren voor uw toepassing integreren met Azure AD.  Maak gebruik van zoveel of als weinig van deze scenario's zoals geschikt is voor uw toepassing.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Azure AD als een manier om Meld u aan bij uw toepassing te ondersteunen

**Aanmelden wrijving verkleinen en ondersteuning verlagen.** Uw gebruikers geen met behulp van Azure AD te melden bij uw toepassing, één meer naam en wachtwoord te onthouden.  Een ontwikkelaar hebt u één minder wachtwoord om te slaan en te beveiligen.  Dat u niet hoeft te verwerken vergeten wachtwoord opnieuw mogelijk een aanzienlijk te sparen alleen.  Azure AD bevoegdheden aanmelden voor een deel van's werelds populairste cloud-toepassingen, zoals Office 365 en Microsoft Azure.  Met honderden miljoenen gebruikers uit miljoenen organisaties, de kans groot dat uw gebruikers al is aangemeld bij Azure AD.  Meer informatie over [toe te voegen ondersteuning voor Azure AD aanmelden](../active-directory-authentication-scenarios.md).

**Vereenvoudig teken omhoog voor uw toepassing.**  Tijdens Meld u aan voor uw toepassing, kunt Azure AD alle noodzakelijke informatie over een gebruiker verzenden, zodat u kunt vooraf uw registreren formulier invullen of volledig te verhelpen.  Gebruikers kunnen registreren voor uw toepassing via hun Azure AD-account via een vertrouwde toestemming ervaring die overeenkomen met die zijn gevonden in sociale media en mobiele toepassingen.  Elke gebruiker kan registreren en meld u aan bij een toepassing die is geïntegreerd met Azure AD zonder IT betrokkenheid.  Meer informatie over [uw toepassing voor Azure AD-accountaanmelding ondertekening-up](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Ga voor gebruikers, beheren inrichting van de gebruiker en toegang tot uw toepassing

**Blader voor gebruikers in de adreslijst.**  De grafiek-API gebruiken om gebruikers zoeken en bladert u naar andere personen in hun organisatie wanneer u anderen uitnodigt te helpen of verlenen van toegang, zodat ze niet te typen e-adressen.  Gebruikers kunnen bladeren met een vertrouwde adres adresboek stijlinterface, inclusief de details van de organisatie-hiërarchie weergeven.  Meer informatie over de [Grafiek-API](../active-directory-graph-api.md).

**Active Directory-groepen en distributielijsten die al van uw klant beheren opnieuw gebruiken.**  Azure AD bevat de groepen die uw klant al is gebruikt voor e-verdeling en toegang beheren.  Met de API Graph deze groepen in plaats van het vereisen van de klant maken en beheren van een afzonderlijke set groepen in uw toepassing opnieuw gebruiken.  Groepsinformatie kan ook worden verzonden in uw toepassing in aanmelden tokens.  Meer informatie over de [Grafiek-API](../active-directory-graph-api.md).

**Gebruik Azure AD om te bepalen wie toegang tot uw toepassing heeft.**  Beheerders en eigenaren van de toepassing in Azure AD toewijzen access aan toepassingen voor specifieke gebruikers en groepen.  De grafiek-API gebruiken, kunt u deze lijst lezen en deze gebruiken om te bepalen inrichting en verwijderen van gegevens van resources en openen in uw toepassing.

**Gebruik Azure AD voor rollen gebaseerd toegangsbeheer.**  Beheerders en eigenaren van de toepassing kunnen gebruikers en groepen toewijzen aan de rollen die u definieert wanneer u uw toepassing in Azure AD registreert.  Rolgegevens wordt verzonden naar uw toepassing in aanmelden tokens en kan ook worden gelezen met de API Graph.  Meer informatie over het [gebruik van Azure AD voor autorisatie](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Toegang krijgen tot gebruikersprofiel, agenda, E-mail, contactpersonen, bestanden en meer

**Azure AD is de server autorisatie voor Office 365 en andere services van Microsoft business.**  Als u Azure AD voor aanmelden bij uw toepassing of de ondersteuning van uw huidige gebruikersaccounts koppelen aan Azure AD-gebruikersaccounts met OAuth 2.0 ondersteunt, kunt u lees- en schrijftoegang voor een gebruikersprofiel, agenda, e-mail, contactpersonen, bestanden en andere gegevens aanvragen.  U kunt naadloos gebeurtenissen naar de agenda van de gebruiker, schrijven en lezen of schrijven van bestanden naar hun OneDrive.  Meer informatie over [toegang tot de Office 365-API](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Uw toepassing in de Azure en Office 365 marktplaatsen verhogen

**Niveau verhogen uw toepassing voor de miljoenen organisaties die al Azure AD gebruiken.**  Gebruikers die zoeken en bladert u deze marktplaatsen zijn al met een of meer cloudservices, zodat ze gekwalificeerd cloud service-klanten.  Meer informatie over het wijzigen van uw toepassing in [Het Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Wanneer gebruikers zich registreren voor uw toepassing, wordt deze wordt weergegeven in hun deelvenster Azure AD-toegang en het startprogramma voor Office 365.**  Gebruikers kunnen snel en gemakkelijk terugkeren naar de toepassing later, het verbeteren van de betrokkenheid van de gebruiker.  Meer informatie over de [Azure AD toegang hebt tot Configuratiescherm](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Beveiligde communicatie met apparaat-naar-Service en Service-naar-Service

**Azure AD gebruiken voor identiteitsbeheer van services en apparaten Hiermee reduceert u de code die u nodig hebt om te schrijven en kunnen IT-beheerders kunnen toegang.**  Services en apparaten kunnen krijgen van tokens van Azure AD OAuth gebruiken en deze tokens gebruiken voor toegang tot web API's.  Gebruik van Azure AD kunt u voorkomen dat complexe verificatiecode schrijven.  Aangezien de identiteit van de services en apparaten zijn opgeslagen in Azure AD IT toetsen en intrekkingsreferenties op één plaats in plaats van dat u dit wilt doen afzonderlijk in uw toepassing kan beheren.

## <a name="benefits-of-integration"></a>Voordelen van de integratie

Integratie met Azure AD wordt geleverd met voordelen, hoeft u extra code te schrijven.

### <a name="integration-with-enterprise-identity-management"></a>Integratie met identiteitsbeheer Enterprise

**Help uw toepassing akkoord gaat met de IT-beleid.**  Organisaties hun systemen voor enterprise identiteitsbeheer integreren met Azure AD zodat wanneer een persoon een organisatie verlaat, ze worden automatisch toegang tot uw toepassing zonder IT hoeft te extra stappen uitvoeren.  IT kunt beheren wie kan toegang tot uw toepassing en bepalen welke beleidsregels access zijn vereist - voor voorbeeld meervoudige verificatie - hoeft te code schrijven om te voldoen aan de complexe bedrijfsbeleid verkleinen.  Azure AD biedt beheerders met een gedetailleerde controlelogboek van wie aangemeld bij uw toepassing dus IT gebruik kunt bijhouden.

**Azure AD breidt Active Directory in de cloud zodat uw toepassing met AD integreren kunt.**  Veel organisaties overal ter wereld Active Directory gebruiken als hun principal aanmelden en een identiteitsbeheersysteem, en hun toepassingen voor gebruik met AD vereisen.  Uw app integreren met Azure AD worden geïntegreerd met Active Directory.

### <a name="advanced-security-features"></a>Geavanceerde beveiligingsfuncties

**Meervoudige verificatie.**  Azure AD biedt systeemeigen meervoudige verificatie.  IT-beheerders kunnen meervoudige verificatie voor toegang tot uw toepassing, nodig, zodat er geen code van deze ondersteuning uzelf.  Meer informatie over [Meervoudige verificatie](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Afwijkende aanmelden detectie.**  Azure AD verwerkt meer dan een miljard aanmeldingen per dag, bij het gebruik van machine learning algoritmen op te sporen verdachte activiteiten en IT-beheerders over mogelijke problemen een melding.  Door de ondersteuning van Azure AD-aanmelden, wordt uw toepassing het voordeel van deze bescherming. Meer informatie over het [weergeven van Azure Active Directory access-rapport](../active-directory-view-access-usage-reports.md).

**Voorwaardelijke toegang.**  Naast het meervoudige verificatie, kunnen beheerders opgeven dat specifieke voorwaarden worden voldaan om gebruikers zich kunnen aanmelden in uw toepassing.  Voorwaarden die kunnen worden ingesteld omvatten het bereik van de IP-adres van clientapparaten, lidmaatschap van de opgegeven groepen en de status van het apparaat wordt gebruikt om toegang te krijgen.  Meer informatie over [voorwaardelijke Azure Active Directory-toegang](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Eenvoudig ontwikkeling

**Standaard industriestandaardprotocollen.**  Microsoft is erop gebrand naar ondersteunende industriële standaarden.  Azure AD ondersteunt de SAML 2.0, OpenID verbinding 1.0 OAuth 2.0 en WS-Federation 1.2 protocollen voor verificatie.  De grafiek-API is OData 4.0 voldoen.  Als al uw toepassing de SAML 2.0 of OpenID verbinding 1.0 protocollen voor federatieve aanmelden ondersteunt, kan deze toe te voegen ondersteuning voor Azure AD ingewikkelde zijn.  Meer informatie over [protocollen voor verificatie van Azure AD worden ondersteund](../active-directory-authentication-protocols.md).

**Open de bron-bibliotheken.**  Microsoft biedt volledig ondersteunde bron openen bibliotheken voor populaire talen en platforms ontwikkelen versnellen.  De broncode wordt in licentie gegeven onder Apache 2.0 en werkt u gratis zich splitsen en bijdragen terug naar de projecten.  Meer informatie over [bibliotheken van Azure AD-verificatie](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Overal ter wereld aanwezigheid en betere beschikbaarheid

**Azure AD wordt is geïmplementeerd in datacenters overal ter wereld en beheerd en servicepakket gecontroleerd.**  Azure AD is de identiteitsbeheersysteem voor Microsoft Azure en Office 365 en wordt geïmplementeerd in 28 datacenters overal ter wereld.  Directory-gegevens is gegarandeerd worden gerepliceerd naar ten minste drie datacenters.  Globale netwerktaakverdelers Zorg ervoor dat gebruikers toegang tot de dichtstbijzijnde kopie van Azure AD die hun gegevens bevat en aanvragen voor andere datacenters automatisch opnieuw doorsturen als er een probleem is gevonden.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag code schrijven](../active-directory-developers-guide.md#getting-started).

[Teken-gebruikers In het gebruik van Azure AD](../active-directory-authentication-scenarios.md)
