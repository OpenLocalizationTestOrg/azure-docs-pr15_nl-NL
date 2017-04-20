<properties
    pageTitle="Meldingen voor accountinrichting | Microsoft Azure"
    description="Leer hoe u ervoor zorgen dat u wordt geïnformeerd van problemen met betrekking tot de inrichting van de gebruiker die uw aandacht nodig doordat accountinrichting meldingen."
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
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>Account inrichting van meldingen

Met de gebruiker is geïnstalleerd, kunt u het proces van het beheren van gebruikers in toepassingen van derden SaaS automatiseren. <br>
Dit is een geautomatiseerde proces, is uw interactie met dit proces te bezoeken vereist. <br>
Dit is, bijvoorbeeld het geval is, wanneer het wachtwoord van het account dat u hebt geconfigureerd voor het exchange-gegevens met een derde partij SaaS toepassing is verlopen. 

Doordat accountinrichting meldingen, kunt u ervoor zorgen dat u wordt geïnformeerd van problemen met betrekking tot de inrichting van de gebruiker die uw aandacht nodig.

U activeren of deactiveren accountinrichting meldingen als onderdeel van uw gebruikers-configuratie voor een derde partij SaaS toepassing inrichten.

![De inrichting van gebruiker][1] 



Als u wilt activeren accountinrichting meldingen, schakel het selectievakje gerelateerde op de pagina **Confirmation** dialoogvenster en typ het e-mailalias van de geadresseerde.

![Account inrichting van meldingen][2]
 


U kunt een distributielijst invoeren als de geadresseerde; het is echter Houd er rekening mee dat het e-mailmelding bevat koppelingen naar rapporten die alleen toegankelijk zijn voor de Azure AD-beheerders zijn.

Als er accountinrichting meldingen ingeschakeld, ontvangt u e-mailberichten over kritieke problemen die zijn gerelateerd aan de inrichting van de gebruiker. Echter om te voorkomen dat een e-overbelasting, ontvangt alleen u een e-mailmelding per dag voor elke toepassing SaaS die het e-mailmelding is geconfigureerd voor gebruik.


##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Automatiseren gebruiker inrichting/Deprovisioning aan SaaS-Apps](active-directory-saas-app-provisioning.md)
- [Kenmerktoewijzingen voor het inrichten van de gebruiker aan te passen](active-directory-saas-customizing-attribute-mappings.md)
- [Expressies voor het toewijzen van kenmerken schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Een bereik van Filters voor het inrichten van de gebruiker instellen](active-directory-saas-scoping-filters.md)
- [Gebruikmaken van SCIM waarmee automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen](active-directory-scim-provisioning.md)
- [Lijst met zelfstudies over het integreren van SaaS Apps](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png