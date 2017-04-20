<properties
    pageTitle="Veelgestelde vragen over Azure Active Directory | Microsoft Azure"
    description="Azure Active Directory Veelgestelde vragen over vindt u antwoorden op vragen in combinatie met de toegang tot Azure en Azure Active Directory, wachtwoord management en toepassing toegang."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Veelgestelde vragen over Azure Active Directory

Azure Active Directory is een volledig identiteit als een (IDaaS)-serviceoplossing die betrekking hebben op alle aspecten van identiteit, toegangsbeheer en beveiliging.


Zie voor meer informatie, [Wat Azure Active Directory is?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Toegang tot Azure en Azure Active Directory


**V: Waarom krijg ik 'geen abonnement gevonden' als ik me probeer voor toegang tot Azure AD in de portal van Azure klassieke (https://manage.windowsazure.com)?**

**A:** Toegang krijgen tot de portal van Azure klassieke moet elke gebruiker machtigingen hebt voor een Azure-abonnement. Als u een betaald Office 365 of Azure AD Navigeer naar [http://aka.ms/accessAAD](http://aka.ms/accessAAD) voor een eenmalige activeringsstap hebt, anders moet u een volledige [Azure proefabonnement](https://azure.microsoft.com/pricing/free-trial/) of een betaald abonnement te activeren. 

Zie voor meer informatie:

- [Hoe Azure abonnementen gekoppeld aan Azure Active Directory zijn](active-directory-how-subscriptions-associated-directory.md)

- [De map voor uw Office 365-abonnement in Azure beheren](active-directory-manage-o365-subscription.md)

---

**V: Wat is de relatie tussen Azure AD, Office 365 en Azure?**

**A:** Azure Active Directory biedt u algemene mogelijkheden voor de identiteit en toegang tot alle Microsoft online services. Of u met Office 365, Microsoft Azure, Intune of anderen, gebruikt u al een Azure AD voor eenmalige aanmelding inschakelen en openen van management voor alle van deze services. 

Alle gebruikers die u hebt ingeschakeld voor Microsoft Online services worden in feite gedefinieerd als gebruikersaccounts in een of meer Azure AD-processen. U kunt deze mogelijkheden accounts gratis Azure AD zoals toepassing cloudtoegang inschakelen.
 
Daarnaast kunnen Azure AD services betaald (bijvoorbeeld: Azure AD basic Premium, EMS, enzovoort) aanvullen andere Online services zoals Office 365 en Microsoft Azure met uitgebreide schaal management en beveiliging bedrijfsoplossingen.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Aan de slag met hybride Azure AD


**V: hoe kan ik mijn on-premises adreslijst verbinden met Azure AD?**

**A:** U kunt uw on-premises adreslijst verbinden met Azure AD met **Azure AD Connect**. 

Zie de [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md)voor meer informatie.


---

**V: hoe stel ik SSO in tussen mijn on-premises adreslijst en Mijn cloud-toepassingen?**

**A:** U moet alleen SSO tussen uw on-premises adreslijst en Azure AD instellen. Zo lang maken als u rechtstreeks toegang uw cloud-toepassingen via Azure AD tot, wordt uw gebruikers correct verificatie met hun referenties on-premises implementatie automatisch stations door de service.

Eenmalige aanmelding vanuit on-premises implementatie kunt worden gemakkelijk met Federatie solutions zoals ADFS of door te hash Wachtwoordsynchronisatie configureren. U kunt eenvoudig beide opties met de wizard van de configuratie Azure AD Connect implementeren.
  

Zie de [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md)voor meer informatie.
  

---

**V: Azure Active Directory biedt een portal selfservice voor gebruikers in mijn organisatie?**

**A:** Ja, Azure Active Directory biedt u het [deelvenster Azure AD toegang](http://myapps.microsoft.com) voor selfservice-gebruiker en toegang tot toepassingen. Als u een Office 365-klant bent, kunt u veel van dezelfde mogelijkheden vinden in de Office 365-beheerportal. 

Zie de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie. 



---

**V: Azure AD helpt ik mijn on-premises implementatie-infrastructuur beheren?**

**A:** Ja, de optie doet. De Azure AD Premium edition beschikt u over de **Status verbinding maken**. Azure AD verbinding systeemstatus kunt u controleren en inzicht in de infrastructuur van uw on-premises implementatie-identiteit en de Synchronisatieservices.  

Zie voor meer informatie [Monitor uw on-premises identiteit infrastructuur en synchronisatie services in de cloud](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Wachtwoordbeheer

**V: kan ik Azure AD wachtwoord terugschrijven gebruiken zonder Wachtwoordsynchronisatie? (Of, ik wil graag Azure AD SSPR gebruiken met wachtwoord terugschrijven maar ik wil niet dat mijn wachtwoorden die zijn opgeslagen in de cloud?)**

**A:** U hoeft niet te synchroniseren van uw wachtwoorden AD Azure AD om te kunnen inschakelen terugschrijven. In een omgeving met federatieve afhankelijk Azure AD SSO van de on-premises adreslijst om de gebruiker te verifiëren. Dit scenario hoeft niet het on-premises implementatie-wachtwoord voor het in Azure AD worden bijgehouden.

---

**V: hoe lang duurt het om een wachtwoord terug naar het lokale AD worden geschreven?**

**A:** Wachtwoord terugschrijven werkt in realtime. 

Zie voor meer informatie [aan de slag met wachtwoordbeheer](active-directory-passwords-getting-started.md) 


---

**V: kan ik wachtwoord terugschrijven gebruiken met wachtwoorden die worden beheerd door een beheerder?**

**A:** Ja, hebt u een wachtwoord terugschrijven ingeschakeld, het wachtwoord bewerkingen wordt uitgevoerd door een beheerder worden opgeslagen in uw on-premises omgeving.  

Vragen over, Zie voor meer antwoorden met een wachtwoord [Wachtwoord Management Veelgestelde vragen](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Toegang tot toepassingen


**V: waar vind ik een lijst met toepassingen die vooraf geïntegreerd met Azure AD en hun mogelijkheden?**

**A:** Azure AD heeft meer dan 2600 vooraf geïntegreerde toepassingen van Microsoft, toepassingsserviceproviders of partners. Alle vooraf geïntegreerde toepassingen ondersteuning voor eenmalige aanmelding. Eenmalige aanmelding kunt u uw organisatie-referenties gebruiken voor toegang tot uw apps. Enkele van de toepassingen ondersteunen ook geautomatiseerde inrichting en verwijderen van gegevens

Zie voor een volledige lijst van de vooraf geïntegreerde toepassingen, de [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**V: Wat gebeurt er als de toepassing die ik nodig is niet in de Azure AD-marktplaats?**

**A:** U kunt met Azure AD Premium, toevoegen en configureren van een toepassing die u wilt. Afhankelijk van de mogelijkheden van uw toepassing en uw voorkeuren, kunt u eenmalige aanmelding en inrichten van geautomatiseerde configureren.  

Zie voor meer informatie:

- [Eenmalige aanmelding naar toepassingen die niet zijn opgenomen in de galerie met Azure Active Directory-toepassing configureren](active-directory-saas-custom-apps.md)
- [Gebruikmaken van SCIM waarmee automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen](active-directory-scim-provisioning.md) 


---

**V: hoe kunnen gebruikers zich in met Azure Active Directory-toepassingen?**
 
**A:** Azure Active directory bevat verschillende manieren zodat gebruikers deze kunnen bekijken en toegang tot hun toepassingen, zoals:

- Het deelvenster Azure AD-toegang

- Startpictogram voor het Office 365-toepassing

- Rechtstreekse aanmelding tot federatieve apps

- Uitgebreide koppelingen naar federatieve, op basis van wachtwoorden of bestaande apps

Zie [implementeren Azure AD geïntegreerd toepassingen aan gebruikers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)voor meer informatie.


---

**V: wat zijn de verschillende manieren Azure Active Directory, kunt u verificatie-eenmalige aanmelding naar toepassingen?**
 
**A:** Azure Active Directory ondersteunt veel gestandaardiseerde protocollen voor verificatie en machtiging zoals SAML 2.0-, OpenID verbinden, OAuth 2.0 en WS-Federatie. Azure AD ondersteunt ook wachtwoord vaulting en geautomatiseerd aanmeldingsproblemen mogelijkheden voor apps die op formulieren gebaseerde verificatie alleen ondersteunen.  

Zie voor meer informatie:

- [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md)

- [Active Directory-verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Hoe eenmalige aanmelding met Azure Active Directory werk?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**V: kan ik mijn lokale computer wordt uitgevoerd toepassingen toevoegen?**

**A:** Azure AD-toepassingsproxy biedt u gemakkelijk en veilig toegang tot on-premises implementatie-webtoepassingen die u kiest. U kunt deze toepassingen openen op dezelfde manier als die u toegang krijgt uw apps SaaS in Azure Active Directory tot. Er is niet nodig voor een VPN-verbinding of wijzigen van de infrastructuur van uw netwerk.  

Lees [hoe u het beveiligde externe toegang tot on-premises implementatie-toepassingen](active-directory-application-proxy-get-started.md)voor meer informatie.


--- 

**V: hoe ik MFA vereisen voor gebruikers met toegang tot een bepaalde toepassing?**

**A:** U kunt een unieke-beleid voor elke toepassing toewijzen met voorwaardelijke Azure AD-toegang. U kunt in uw beleid, MFA vereisen allen tijde, of wanneer gebruikers geen verbinding is met het lokale netwerk.  

Zie voor meer informatie [beveiligen toegang tot Office 365 en andere apps die zijn verbonden met Azure Active Directory](active-directory-conditional-access.md).


---

**V: Wat is de inrichting van gebruiker wordt automatisch voor SaaS-Apps?**

**A:** Azure Active Directory kunt u de maken, onderhoud en verwijderen van de identiteit van de gebruikers in veel populaire cloud (SaaS)-toepassingen te automatiseren. 

Voor meer informatie raadpleegt u de [inrichting van gebruiker automatiseren en Deprovisioning naar SaaS toepassingen met Azure Active Directory](active-directory-saas-app-provisioning.md)

---



