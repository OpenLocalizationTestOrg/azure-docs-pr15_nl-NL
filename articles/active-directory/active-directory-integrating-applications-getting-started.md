<properties
   pageTitle="Handleiding Azure Active Directory integreren met toepassingen die aan de slag met |  Microsoft Azure"
   description="In dit artikel is een ophalen handleiding voor het integreren van Azure Active Directory (AD) met on-premises implementatie-toepassingen en cloud-toepassingen."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Handleiding Azure Active Directory integreren met toepassingen die aan de slag
## <a name="overview"></a>Overzicht
In dit onderwerp is bedoeld om aan te geven u een routekaart voor toepassingen integreren met Azure Active Directory (AD). Elk van de volgende secties bevatten een kort overzicht van een onderwerp voor meer gedetailleerde zodat u kunt aangeven welke onderdelen van deze ophalen handleiding die voor u relevant zijn.  Volg de koppelingen voor een grondigere kennismaking op elke onderwerp.

## <a name="before-you-begin-take-inventory"></a>Voordat u begint, inventarisatie
Voordat u naar toepassingen integreren met Azure AD in gaan, is het belangrijk te weten waar u ook bent en waar u naartoe wilt gaan.  De volgende vragen zijn bedoeld om u Denk na over uw project-integratie van Azure AD-toepassing te helpen.

### <a name="application-inventory"></a>Toepassing voorraad
- Waar zijn al uw toepassingen? Eigenaar van deze?
- Welk type verificatie hebben uw toepassingen nodig?
- Wie heeft toegang tot welke toepassingen nodig?
- Wilt u een nieuwe toepassing implementeren?
  - Wordt u interne bouwen en implementeren op een exemplaar van de Azure berekeningscluster?
  - U gebruikt een die beschikbaar is in de galerie met Azure-toepassing?

### <a name="user-and-group-inventory"></a>Gebruikers en groepen voorraad
- Waar zich uw gebruikersaccounts?
 - On-premises Active Directory
 - Azure AD
 - In een afzonderlijk databasetoepassing dat u eigenaar bent
 - In verzoek tot niet toegestande toepassingen
 - Alle bovenstaande antwoorden.
- Welke machtigingen en roltoewijzingen individuele gebruikers hebt? Moet u hun toegang controleren of wilt u dat de gebruiker toegang en rol toewijzingen geschikt nu zijn?
- Groepen reeds bestaan in uw lokale Active Directory?
 - Hoe is uw groepen ingedeeld?
 - Wie de groepsleden zijn?
 - Welke machtigingen/roltoewijzingen de groepen die momenteel heb?
- Hebt u nodig om op te schonen gebruiker/groep databases voordat u integreren?  (Dit is een heel belangrijk vraag. Ongewenste in ongewenste informatie.)

### <a name="access-management-inventory"></a>Access management voorraad
- Hoe u momenteel beheer gebruikerstoegang naar toepassingen? Die moet worden gewijzigd?  Hebt u andere manieren voor het beheren van access, zoals met [RBAC](role-based-access-control-configure.md) bijvoorbeeld beschouwd als?
- Wie heeft toegang tot een site nodig?

Wellicht zoekt u niet de antwoorden hebt op alle van deze vragen kunt vooraf maar dat is geen probleem.  Deze handleiding kunt u enkele van deze vragen beantwoorden en sommige beslissingen nemen.

## <a name="prerequisites"></a>Vereisten voor
- Een Azure-abonnement en een directory Azure Active Directory.  Als u niet al een Azure-abonnement hebt, kunt u Azure uitproberen gratis voor 30 dagen. [Probeer het zelf!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Toepassingsintegratie met Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Zoeken naar verzoek tot niet toegestande cloud-toepassingen met de Cloud-App Discovery
Bovengenoemde, kunnen er toepassingen die nog niet is beheerd door uw organisatie tot nu.  Als onderdeel van het voorraadproces is het mogelijk te verzoek tot niet toegestande cloud-toepassingen vinden. Zie [verzoek tot niet toegestande cloud-toepassingen met de Cloud App Discovery zoeken](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Verificatietypen
Elk van uw toepassingen mogelijk verschillende verificatievereisten. Met Azure AD, kan het ondertekenen van certificaten worden gebruikt met toepassingen die gebruikmaken van SAML 2.0, WS-Federatie, of OpenID verbinding protocollen, evenals wachtwoord Single Sign On. Voor meer informatie over de toepassing Zie verificatietypen voor gebruik met Azure AD [Certificaten voor federatieve eenmalige aanmelding in Azure Active Directory beheren](active-directory-sso-certs.md) en [wachtwoord op basis van eenmalige van](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Eenmalige aanmelding met Azure AD-App-Proxy inschakelen
Met Microsoft Azure AD-toepassingsproxy, kunt u toegang tot toepassingen bevinden binnen uw privé-netwerk veilig, vanaf elke locatie en met elk apparaat bieden. Nadat u een toepassing proxy-connector hebt geïnstalleerd in uw omgeving, kan deze eenvoudig worden geconfigureerd met Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Toepassingen integreren met Azure AD
De volgende artikelen Bespreek de verschillende manieren toepassingen integreren met Azure AD en geef enkele richtlijnen.

- [Vaststellen welke Active Directory gebruiken](active-directory-administer.md)
- [Toepassingen gebruiken in de galerie met Azure-toepassing](active-directory-appssoaccess-whatis.md)
- [Integratie van SaaS toepassingen zelfstudies lijst](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Toegang tot toepassingen beheren
De volgende artikelen wordt uitgelegd manieren kunt u toegang tot toepassingen beheren nadat ze zijn geïntegreerd met Azure AD van Azure AD-Connectors en Azure AD.

- [Toegang tot de apps met behulp van Azure AD beheren](active-directory-managing-access-to-apps.md)
- [Automatiseren met Azure AD-Connectors](active-directory-saas-app-provisioning.md)
- [Het toewijzen van gebruikers aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md)
- [Groepen toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-groups.md)
- [Delen van accounts](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integratie van aangepaste toepassingen
Als u een nieuwe toepassing schrijft en ontwikkelaars helpen bij het gebruikmaken van de kracht Azure AD wilt, raadpleegt u [Guiding ontwikkelaars](active-directory-applications-guiding-developers-for-lob-applications.md).

Als u toevoegen van uw aangepaste toepassing in de galerie met Azure-toepassing wilt, raadpleegt u ['doen om uw eigen app"met Azure AD Self-Service SAML configuratie](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Zie ook

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
