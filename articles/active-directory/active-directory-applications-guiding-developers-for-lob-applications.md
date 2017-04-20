<properties
    pageTitle="Azure AD en toepassingen: leidt ontwikkelaars | Microsoft Azure"
    description="In dit artikel bevat richtlijnen voor het integreren van Azure-toepassingen met Active Directory geschreven voor IT-professionals."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD en toepassingen: ontwikkel LOB-apps

Deze handleiding bevat een overzicht van het ontwikkelen van line-of-business (LoB) toepassingen voor Azure AD (Active Directory). De doelgroep is globale beheerders van Active Directory/Office 365.

## <a name="overview"></a>Overzicht

Bouwen geïntegreerd met Azure AD-toepassingen, geeft gebruikers in uw organisatie eenmalige aanmelding met Office 365. De toepassing niet in Azure AD-hebt die u meer controle over het verificatiebeleid voor de toepassing. Zie [toegangsregels configureren](active-directory-conditional-access-azuread-connected-apps.md)voor meer informatie over voorwaardelijke toegang en hoe u het beveiligen van apps gebruiken met meervoudige verificatie (MFA).

Uw toepassing gebruik van de Azure Active Directory registreren. Registreren van de toepassing, betekent dat uw ontwikkelaars Azure AD gebruiken kunnen bij het verifiëren van gebruikers en toegang tot user-bronnen, zoals e-mail, agenda en documenten aanvragen.

Een lid van de map (niet gasten) kunt een toepassing, ook bekend als het *maken van een toepassingsobject*registreren.

Registreren van een toepassing kan een gebruiker als volgt te werk:

- Een identiteit voor de toepassing die Azure AD herkent ophalen
- Een of meer geheimen/toetsen de toepassing gebruiken kunt om zichzelf te verifiëren aan AD ophalen
- Uw huisstijl toepassen op de toepassing in de Azure-portal met een aangepaste naam, het logo, enzovoort.
- Azure AD autorisatie voor functies van toepassing op hun Apps, waaronder:
  - Rolgebaseerd toegangsbeheer RBAC)
  - Azure Active Directory als oAuth autorisatie server (secure een API die worden aangeboden door de toepassing)

- Vereiste machtigingen voor de toepassing aan functie nodig declareren zoals verwacht, met inbegrip van:-App-machtigingen (alleen globale beheerders). Bijvoorbeeld: rollidmaatschap van een andere Azure AD-toepassing of rol lidmaatschap ten opzichte van een Azure Resource, resourcegroep, of een abonnement - gedelegeerde machtigingen (elke gebruiker). Bijvoorbeeld: Azure AD, aanmelden en gelezen profiel


> [AZURE.NOTE]Standaard kan alle leden van een toepassing registreren. Als u wilt weten hoe u beperkingen instellen voor het registreren van toepassingen aan specifieke leden, Zie [hoe toepassingen worden toegevoegd aan Azure AD](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Hier volgt wat u, de globale beheerder moet doen om te ontwikkelaars bereid de toepassing voor productie:

- Toegangsregels (access beleid/MFA) configureren
- De app om te vereisen Gebruikerstoewijzing en toewijzen aan gebruikers configureren
- De voorziening toestemming onderdrukken

## <a name="configure-access-rules"></a>Toegangsregels configureren

Toegangsregels per toepassing tot uw apps SaaS configureren. U kunt bijvoorbeeld MFA vereisen of alleen toegang geeft tot gebruikers op vertrouwde netwerken. De details hiervoor zijn beschikbaar in het document [toegangsregels configureren](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>De app om te vereisen Gebruikerstoewijzing en toewijzen aan gebruikers configureren

Standaard gebruikers hebben toegang tot toepassingen zonder te worden toegewezen. Als de toepassing ook rollen of als u wilt dat de toepassing in het deelvenster van de toegang van een gebruiker moet worden weergeven, moet u echter Gebruikerstoewijzing vereisen.

[Het vereisen van de gebruiker is toegewezen](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Als u een abonnee Azure AD Premium of Enterprise mobiliteit Suite (EMS), wordt aangeraden met groepen. Groepen toewijzen aan de toepassing, kunt u delegeren lopende toegangsbeheer naar de eigenaar van de groep. U kunt de groep hebt gemaakt of vraag degene die verantwoordelijk is in uw organisatie aan de groep met behulp van de functie van uw groep management hebt gemaakt.

[Het toewijzen van gebruikers aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md)  
[Groepen toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Onderdrukken toestemming van de gebruiker

Standaard doorloopt elke gebruiker een ervaring toestemming aan te melden. De toestemming-ervaring, gebruikers te machtigen om een toepassing, vragen kan worden toegevoegd voor gebruikers die niet bekend bent met deze beslissingen.

Voor toepassingen die u vertrouwt, kunt u de gebruikerservaring vereenvoudigen doordat u ermee akkoord dat de toepassing namens uw organisatie.

Zie voor meer informatie over toestemming van de gebruiker en de toestemming-ervaring in Azure [Toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Verwante artikelen

- [Veilige externe toegang tot on-premises implementatie-toepassingen met Azure AD-toepassingsproxy inschakelen](active-directory-application-proxy-get-started.md)
- [Voorbeeld van de Azure voorwaardelijke toegang voor SaaS-Apps](active-directory-conditional-access-azuread-connected-apps.md)
- [Toegang tot de apps gebruiken met Azure AD beheren](active-directory-managing-access-to-apps.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
