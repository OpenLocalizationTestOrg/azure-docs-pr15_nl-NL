<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Facebook in de praktijk | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Facebook op werk."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Zelfstudie: Azure Active Directory-integratie met Facebook op het werk

Het doel van deze zelfstudie is om aan te geven hoe u Facebook op het werk integreren met Azure Active Directory (Azure AD).

Facebook op het werk integreren met Azure AD biedt de volgende voordelen: 

- U kunt bepalen in Azure AD wie toegang tot Facebook op het werk heeft 
- U kunt automatisch inrichten van account voor gebruikers die toegang hebben gekregen met Facebook op het werk
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld bij Facebook op het werk (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie beheren 

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Vereisten voor 

Als u wilt configureren Azure AD-integratie met CS sterren, moet u de volgende items:

- Een Azure AD-abonnement
- Facebook op het werk met eenmalige aanmelding is ingeschakeld

Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Facebook toevoegen aan het werk in de galerie
Als u wilt configureren de Facebook-integratie in de praktijk in Azure AD, moet u Facebook op het werk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Facebook op het werk in de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Facebook op het werk**.

7. Selecteer **Facebook op het werk**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In dit gedeelte vindt u een overzicht van het inschakelen van gebruikers om te verifiëren met Facebook op het werk met hun account in Azure Active Directory, met Federatie op basis van het SAML-protocol.

**Als u wilt configureren eenmalige aanmelding Azure AD met Facebook op het werk, moet u de volgende stappen uitvoeren:**

1.  Na het toevoegen van Facebook op het werk in de portal van Azure klassieke, klikt u op **Configureren eenmalige aanmelding**.

2.  Voer in het scherm **Configureren App-URL** de URL waar gebruikers wordt Meld u aan bij uw Facebook op werk-toepassing. Dit is uw Facebook op werk tenant-URL (voorbeeld: https://example.facebook.com/). Klik op **volgende**wanneer u klaar bent.

3.  In een browservenster verschillende web en meld u aan bij uw Facebook op werk bedrijfssite als beheerder.

4. Volg de instructies op de volgende URL voor het configureren van Facebook op uw werk Azure AD als een identiteitsprovider: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Als voltooid, terug te keren naar de browservensters met de portal van Azure klassieke, klik op het selectievakje om te bevestigen dat u de procedure hebt voltooid en klik vervolgens op **volgende** en **voltooid**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Automatisch inrichting gebruikers met Facebook op het werk

Azure AD ondersteunt de mogelijkheid om te automatisch synchroniseren de accountgegevens met toegewezen gebruikers met Facebook op het werk. Deze automatische synchronisatie kunt Facebook op het werk om de vereiste gegevens voor access, aangekondigd vóór ze moeten aanmelden voor het eerst probeert gebruikers toe te staan. Deze ook bepalingen opgeheven gebruikers uit Facebook in de praktijk wanneer access in Azure AD is ingetrokken.

Automatische inrichting kan worden ingesteld door te klikken op de **inrichting van de account configureren** in het Azure klassieke portal-venster.

Zie voor meer informatie over het configureren van de inrichting van automatische [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Het toewijzen van gebruikers aan Facebook op het werk

Voor ingerichte AAD-gebruikers moeten kunnen Facebook op het werk op hun Configuratiescherm Access, moeten ze toegang binnen de Azure klassieke portal worden toegewezen.

**Gebruikers toewijzen aan Facebook op het werk:**

1.  Klik op de startpagina voor Facebook op het werk in de klassieke Azure-portal op **toewijzen rekeningen**.

2.  Selecteer in het menu **weergeven** , of u wilt toewijzen aan een gebruiker of een groep Facebook op het werk en klik op de knop vinkje.

3.  Selecteer de gebruikers of de groep aan wie wilt u Facebook op het werk toewijzen in de lijst met resultaten.

4.  Klik in de voettekst van de pagina, op de knop **toewijzen** .


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




