<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met instructies voor Microsoft | Microsoft Azure" 
    description="Leer hoe u de aanwijzingen op Microsoft met Azure Active Directory gebruiken om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Zelfstudie: Azure Active Directory-integratie met instructies voor Microsoft

Het doel van deze zelfstudie is handig om de integratie van Azure Active Directory en routebeschrijvingen Microsoft.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een routebeschrijving van Microsoft-abonnement

Als u een federatieve aanwijzingen voor abonnement op Microsoft nog geen, e-een verzoek om "*service@DirectionsOnMicrosoft.com*".

Na het voltooien van deze zelfstudie, kunnen de Azure Active Directory-gebruikers die u hebt toegewezen aan de aanwijzingen op Microsoft één Meld u aan bij de toepassing via eenmalige aanmelding.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor instructies over het Microsoft inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Scenario")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>De toepassingsintegratie van de voor instructies over het Microsoft inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor instructies over het Microsoft.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie voor instructies over het Microsoft inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **de aanwijzingen op Microsoft**.

    ![Galerie van toepassing] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Galerie van toepassing")

7.  Selecteert u **de aanwijzingen op Microsoft**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Scenario] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Scenario")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij routebeschrijvingen op Microsoft met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **routebeschrijving op Microsoft** -toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Eenmalige aanmelding inschakelen")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de aanwijzingen op Microsoft** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Microsoft Azure AD Singel eenmalige aanmelding] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel eenmalige aanmelding")

3.  Typ **https://www.directionsonmicrosoft.com/user/login**op de pagina **App-URL configureren** in het tekstvak Aanmeldingsadres op URL en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij de aanwijzingen op Microsoft** op **metagegevens downloaden**en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Eenmalige aanmelding configureren")

5.  Het metagegevensbestand versturen naar de aanwijzingen op de Microsoft-ondersteuningsteam (*service@DirectionsOnMicrosoft.com*). Als u wilt inschakelen ondersteuningsteam de aanwijzingen op Microsoft om te zoeken van uw sitelidmaatschap federatieve, kunt u uw bedrijfsgegevens in uw e-mail.

    >[AZURE.NOTE] Eenmalige aanmelding voor instructies over het Microsoft moet worden ingeschakeld door de aanwijzingen op de Microsoft-ondersteuningsteam.
U ontvangt een melding wanneer eenmalige aanmelding is ingeschakeld.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Er is geen actie-item voor u van gebruikers aan de aanwijzingen op Microsoft configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij de aanwijzingen op Microsoft via het Configuratiescherm van access, informatie over het Microsoft Hiermee wordt gecontroleerd of de gebruiker bestaat. Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door de aanwijzingen op Microsoft.
##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan de aanwijzingen op Microsoft, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **routebeschrijving op Microsoft **-toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Ja")
