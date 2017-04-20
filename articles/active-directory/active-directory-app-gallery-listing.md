<properties
   pageTitle="Vermelding van de toepassing in de galerie met Azure Active Directory-toepassing"
   description="Hoe u een toepassing die ondersteuning biedt voor eenmalige aanmelding in de galerie met Azure Active Directory | Microsoft Azure"
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


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Vermelding van de toepassing in de galerie met Azure Active Directory-toepassing

Als u een toepassing die ondersteuning biedt voor eenmalige aanmelding met Azure Active Directory in de [galerie met Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), de toepassing eerst moet implementeren een van de volgende integratie modi:

* **OpenID verbinding** - directe integratie met Azure AD OpenID verbinding maken met voor verificatie en de API Azure AD toestemming voor configuratie. Als u alleen een integratie begint en SAML biedt geen ondersteuning voor uw toepassing, is dit de aanbevolen-modus.

* **Op SAML** -al uw toepassing heeft de mogelijkheid om te configureren identiteitsprovider van derden met het SAML-protocol.

Vermelding vereisten voor iedere modus zijn onder.

##<a name="openid-connect-integration"></a>OpenID verbinding integratie

Uw toepassing integreren met Azure AD, de [ontwikkelaars instructies](active-directory-authentication-scenarios.md)te volgen. Vervolgens de onderstaande vragen voltooien en verzenden naar waadpartners@microsoft.com.

* Referenties voor een account of test-tenant met de toepassing die kan worden gebruikt door het team van Azure AD om te testen van de integratie opgeven.  

* Bieden instructies voor hoe het Azure AD-team kunt aanmelden en een exemplaar van Azure AD verbinden met uw-toepassing via de [Azure AD toestemming framework](active-directory-integrating-applications.md#overview-of-the-consent-framework). 

* Geef eventuele verdere instructies voor het team Azure AD test eenmalige aanmelding met uw toepassing vereist. 

* De onderstaande informatie opgeven:

> Bedrijfsnaam:
> 
> Bedrijf Website:
> 
> De naam van de toepassing:
> 
> Beschrijving van de toepassing (maximaal 256 tekens):
> 
> De Website van de toepassing is (informatieve):
> 
> Toepassing technische ondersteuning Website of contactgegevens:
> 
> Client-ID van de toepassing, zoals wordt weergegeven in de Toepassingdetails op https://manage.windowsazure.com:
> 
> Waar klanten gaan voor het aanmelden bij de URL van de toepassing aanmelden bij en/of kopen de toepassing:
> 
> Kies maximaal drie categorieën voor uw toepassing voor de lijst (voor beschikbare categorieën Zie de Azure Active Directory-marktplaats):
> 
> Bijvoegen kleine toepassingspictogram (PNG-bestand, 45px door 45px, effen achtergrondkleur):
> 
> Bijvoegen grote toepassingspictogram (PNG-bestand, 215px door 215px, effen achtergrondkleur):
> 
> Bijvoegen toepassing Logo (PNG-bestand, 150px door 122px, transparante achtergrondkleur):

##<a name="saml-integration"></a>Op SAML-integratie

Een app die ondersteuning biedt voor SAML 2.0 kan worden geïntegreerd rechtstreeks met een Azure AD-tenant met [deze instructies voor het toevoegen van een aangepaste toepassing](active-directory-saas-custom-apps.md). Zodra u hebt getest dat toepassingsintegratie met de met Azure AD werkt, verzendt u de volgende informatie <waadpartners@microsoft.com>.

* Referenties voor een account of test-tenant met de toepassing die kan worden gebruikt door het team van Azure AD om te testen van de integratie opgeven.  

* Bevatten de SAML aanmelding URL, uitgever URL (entiteit-ID) en beantwoorden URL (bevestiging consumenten service)-waarden voor uw toepassing, als beschreven [hier](active-directory-saas-custom-apps.md). Als u meestal deze waarden als onderdeel van een bestand met SAML metagegevens opgeven, klikt u vervolgens Stuur die ook.

* Geef een korte beschrijving van het configureren van Azure AD als een identiteitsprovider in uw toepassing met SAML 2.0. Als uw toepassing configureren Azure AD als een identiteitsprovider via een administratieve selfservice-portal ondersteunt, klikt u vervolgens Controleer of de referenties die hierboven omvatten de mogelijkheid om in te stellen dit.

* De onderstaande informatie opgeven:

> Bedrijfsnaam:
> 
> Bedrijf Website:
> 
> De naam van de toepassing:
> 
> Beschrijving van de toepassing (maximaal 256 tekens):
> 
> De Website van de toepassing is (informatieve):
> 
> Toepassing technische ondersteuning Website of contactgegevens:
> 
> Waar klanten gaan voor het aanmelden bij de URL van de toepassing aanmelden bij en/of kopen de toepassing:
> 
> Maximaal drie categorieën voor uw toepassing voor de lijst (voor beschikbare categorieën Zie de [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/)) Kies):
> 
> Bijvoegen kleine toepassingspictogram (PNG-bestand, 45px door 45px, effen achtergrondkleur):
> 
> Bijvoegen grote toepassingspictogram (PNG-bestand, 215px door 215px, effen achtergrondkleur):
> 
> Bijvoegen toepassing Logo (PNG-bestand, 150px door 122px, transparante achtergrondkleur):
