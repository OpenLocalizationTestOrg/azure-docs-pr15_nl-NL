<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met 15Five | Microsoft Azure" 
    description="Meer informatie over het gebruiken van 15Five met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Zelfstudie: Azure Active Directory-integratie met 15Five

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en 15Five. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een 15Five eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan 15Five één Meld u aan bij de toepassing op uw bedrijfssite 15Five (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor 15Five inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-15five-tutorial/IC784667.png "Scenario")
##<a name="enabling-the-application-integration-for-15five"></a>De toepassingsintegratie van de voor 15Five inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor 15Five.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor 15Five door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-15five-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-15five-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-15five-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **15Five**.

    ![Galerie van toepassing] (./media/active-directory-saas-15five-tutorial/IC784668.png "Galerie van toepassing")

7.  Klik in het resultatendeelvenster **15Five**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij 15Five met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal, klik op de pagina **15Five** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784670.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij 15Five** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784671.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.15Five.com*" op de pagina **App-URL configureren** in het tekstvak **15Five Sign In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-15five-tutorial/IC784672.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij 15Five** op **metagegevens downloaden**en stuurt vervolgens de metagegevensbestand naar het ondersteuningsteam 15Five.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784673.png "Eenmalige aanmelding configureren")

    >[AZURE.NOTE] Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam 15Five.

5.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-15five-tutorial/IC784674.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Om te kunnen Azure AD-gebruikers aan te melden bij 15Five inschakelt, moeten ze worden ingericht in 15Five.  
Bij 15Five, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **15Five** .

2.  Ga naar het **bedrijf beheren**.

    ![Bedrijf beheren] (./media/active-directory-saas-15five-tutorial/IC784675.png "Bedrijf beheren")

3.  Ga naar **personen \> personen toevoegen**.

    ![Personen] (./media/active-directory-saas-15five-tutorial/IC784676.png "Personen")

4.  Klik in de sectie nieuwe persoon toevoegen de volgende stappen uitvoeren:

    ![Nieuwe persoon toevoegen] (./media/active-directory-saas-15five-tutorial/IC784677.png "Nieuwe persoon toevoegen")

    1.  Typ de **Voornaam**, **Achternaam**, **titel**, **e-mailadres** van een geldig Azure Active Directory-account dat u inrichten in de gerelateerde tekstvakken wilt.
    2.  Klik op **Gereed**.

    >[AZURE.NOTE] De eigenaar van een account Azure AD ontvangt een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere 15Five gebruiker account hulpmiddelen voor het maken of API's verstrekt door 15Five aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan 15Five, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **15Five **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-15five-tutorial/IC784678.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-15five-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
