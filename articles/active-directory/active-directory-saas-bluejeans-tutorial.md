<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met BlueJeans | Microsoft Azure" 
    description="Meer informatie over het gebruiken van BlueJeans met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Zelfstudie: Azure AD-integratie met BlueJeans

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en BlueJeans.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een BlueJeans eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan BlueJeans één Meld u aan bij de toepassing op uw bedrijfssite BlueJeans (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor BlueJeans inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Scenario")
##<a name="enabling-the-application-integration-for-bluejeans"></a>De toepassingsintegratie van de voor BlueJeans inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor BlueJeans.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor BlueJeans door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **BlueJeans**.

    ![Galerie van toepassing] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Galerie van toepassing")

7.  Selecteer **BlueJeans**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij BlueJeans met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **BlueJeans** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij BlueJeans** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.BlueJeans.com*" op de pagina **App-URL configureren** in het tekstvak **BlueJeans aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij BlueJeans** op **Download-certificaat**te downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite **BlueJeans** als beheerder.

6.  Ga naar **beheerder \> groepsinstellingen \> beveiliging**.

    ![Beheerder] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Beheerder")

7.  In de sectie **beveiliging van** de volgende stappen uitvoeren:

    ![Op SAML eenmalige aanmelding] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "Op SAML eenmalige aanmelding")

    1.  Selecteer **SAML eenmalige aanmelding**.
    2.  Selecteer **automatische inrichting inschakelen**.

8.  Verplaatsen op de volgende stappen uit:

    ![Certificaatpad] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Certificaatpad")

    1.  Klik op **Bestand kiezen**en upload het gedownloade certificaat.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij BlueJeans** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    3.  Kopieer de waarde van de **URL van de wachtwoord wijzigen** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij BlueJeans** en plak deze in het tekstvak **URL van wachtwoord wijzigen** .
    4.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij BlueJeans** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL Meld u af** .

9.  Verplaatsen op de volgende stappen uit:

    ![Wijzigingen opslaan] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Wijzigingen opslaan")

    1.  Typ in het tekstvak **gebruikersnaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    2.  Typ in het tekstvak **e** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Klik op **wijzigingen opslaan**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij BlueJeans, moeten ze worden ingericht in BlueJeans.  
Bij BlueJeans, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **BlueJeans** .

2.  Ga naar **beheerder \> gebruikers beheren \> gebruiker toevoegen**.

    ![Beheerder] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Beheerder")

    >[AZURE.IMPORTANT] Het tabblad **Gebruiker toevoegen** is alleen beschikbaar als het **tabblad Beveiliging** **inschakelen automatische inrichting** uitgeschakeld is.

3.  Klik in de sectie **Gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Gebruiker toevoegen")

    1.  Typ een **BlueJeans gebruikersnaam**, een **e-mailadres**, een **BlueJeans vergadering-ID**, een **Moderator wachtwoordcode**, een **Volledige naam**, het **bedrijf** van een geldige AAD-account dat u wilt inrichten in de gerelateerde tekstvakken.
    2.  Klik op **gebruiker toevoegen**.

>[AZURE.NOTE] U kunt een andere BlueJeans gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door BlueJeans aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan BlueJeans, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **BlueJeans **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
