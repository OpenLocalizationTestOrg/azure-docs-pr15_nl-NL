<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met hoeksteen OnDemand | Microsoft Azure" 
    description="Leer hoe u hoeksteen OnDemand gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Zelfstudie: Azure Active Directory-integratie met hoeksteen OnDemand

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en hoeksteen OnDemand.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant hoeksteen OnDemand

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan hoeksteen OnDemand één Meld u aan bij de toepassing op uw bedrijfssite hoeksteen OnDemand (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor hoeksteen OnDemand inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Scenario")
##<a name="enabling-the-application-integration-for-cornerstone-ondemand"></a>De toepassingsintegratie van de voor hoeksteen OnDemand inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor hoeksteen OnDemand.

###<a name="to-enable-the-application-integration-for-cornerstone-ondemand-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor hoeksteen OnDemand inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **hoeksteen ondemand**.

    ![Galerie van toepassing] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Galerie van toepassing")

7.  Selecteer **Hoeksteen OnDemand**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Hoeksteen OnDemand] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Hoeksteen OnDemand")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij hoeksteen OnDemand met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Hoeksteen OnDemand** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Eenmalige aanmelding inschakelen")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij hoeksteen OnDemand** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Microsoft Azure AD voor eenmalige aanmelding] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD voor eenmalige aanmelding")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://company.csod.com*" op de pagina **App-URL configureren** in het tekstvak **Hoeksteen OnDemand Sign In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij hoeksteen OnDemand** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Eenmalige aanmelding configureren")

5.  Verzenden met het ondersteuningsteam van de volgende items naar de OnDemand hoeksteen:

    1.  Het gedownloade certificaat
    2.  De waarde van de **Externe aanmeldings-URL**
    3.  De waarde van de **URL van externe Meld u af** .

    >[AZURE.NOTE] Eenmalige aanmelding moet worden geconfigureerd door het ondersteuningsteam hoeksteen OnDemand.
U krijgt een melding van het ondersteuningsteam wanneer de configuratie is voltooid.

6.  Selecteer de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij hoeksteen OnDemand, moeten ze worden ingericht in hoeksteen OnDemand.  
Bij hoeksteen OnDemand, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Informatie over de verzenden (bijvoorbeeld: naam, Emial) over de Azure AD-gebruiker die u verlenen aan de OnDemand hoeksteen wilt ondersteuningsteam.

>[AZURE.NOTE] U kunt een andere hoeksteen OnDemand gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door hoeksteen OnDemand aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-cornerstone-ondemand-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan hoeksteen OnDemand, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Hoeksteen OnDemand **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Toewijzen aan gebruikers")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
