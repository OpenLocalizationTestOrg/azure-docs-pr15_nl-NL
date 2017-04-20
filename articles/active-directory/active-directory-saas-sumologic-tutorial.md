<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SumoLogic | Microsoft Azure" 
    description="Meer informatie over het gebruiken van SumoLogic met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Zelfstudie: Azure Active Directory-integratie met SumoLogic
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en SumoLogic.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant SumoLogic
  
Na het voltooien van deze zelfstudie, bedrijf de Azure AD-gebruikers die u hebt toegewezen aan SumoLogicwill kunnen eenmalige aanmelding worden in de toepassing op uw SumoLogic site (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor SumoLogic inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-sumologic-tutorial/IC778549.png "Scenario")

##<a name="enabling-the-application-integration-for-sumologic"></a>De toepassingsintegratie van de voor SumoLogic inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor SumoLogic.

###<a name="to-enable-the-application-integration-for-sumologic-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor SumoLogic door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sumologic-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-sumologic-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-sumologic-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-sumologic-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **sumologic**.

    ![Galerie van toepassing] (./media/active-directory-saas-sumologic-tutorial/IC778550.png "Galerie van toepassing")

7.  Selecteer **SumoLogic**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![SumoLogic] (./media/active-directory-saas-sumologic-tutorial/IC778551.png "SumoLogic")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij SumoLogic met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw SumoLogictenant.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SumoLogic** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778552.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SumoLogic** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778553.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL App configureren** in het tekstvak **SumoLogic Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. SumoLogic.com*', en klik op **volgende**.

    ![Configureren aoo URL] (./media/active-directory-saas-sumologic-tutorial/IC778554.png "Configureren aoo URL")

4.  Klik op de pagina **configureren eenmalige aanmelding bij SumoLogic** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778555.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite SumoLogic als beheerder.

6.  Ga naar **beheren \> beveiliging**.

    ![Beheren] (./media/active-directory-saas-sumologic-tutorial/IC778556.png "Beheren")

7.  Klik op **SAML**.

    ![Globale beveiligingsinstellingen] (./media/active-directory-saas-sumologic-tutorial/IC778557.png "Globale beveiligingsinstellingen")

8.  In de lijst **Selecteer een configuratie of maak een nieuwe** Selecteer **Azure AD**en klik vervolgens op **configureren**.

    ![Op SAML 2.0 configureren] (./media/active-directory-saas-sumologic-tutorial/IC778558.png "Op SAML 2.0 configureren")

9.  Klik in het dialoogvenster **SAML 2.0 configureren** , moet u de volgende stappen uitvoeren:

    ![Op SAML 2.0 configureren] (./media/active-directory-saas-sumologic-tutorial/IC778559.png "Op SAML 2.0 configureren")

    1.  Typ in het tekstvak **Configuratie** **Azure AD**.
    2.  **Fouten opsporen in modus**selecteren
    3.  Kopieer de **URL van de uitgever** -waarde in de klassieke Azure portal, klik op de dialoog **configureren eenmalige aanmelding bij SumoLogic** pagina en plak deze in het tekstvak **uitgever** .
    4.  In de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij SumoLogic** de waarde **Verificatie aanvragen URL** kopiëren en plak deze in het tekstvak **Authn verzoek-URL** .
    5.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plak het volledige certificaat in het tekstvak **X.509-certificaat** .
    7.  Als **E-kenmerk**, selecteert u **Gebruik SAML onderwerp**.
    8.  Selecteer **SP Login configuratie gestart**.
    9.  Typ in het tekstvak **Login pad** **Azure**.
    10. Klik op **Opslaan**.

10. In de klassieke Azure portal, klik op de dialoog **configureren eenmalige aanmelding bij SumoLogic** pagina selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sumologic-tutorial/IC778560.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij SumoLogic, moeten ze worden ingericht naar SumoLogic.  
Bij SumoLogic, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **SumoLogic** .

2.  Ga naar **beheren \> gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-sumologic-tutorial/IC778561.png "Gebruikers")

3.  Klik op **toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-sumologic-tutorial/IC778562.png "Gebruikers")

4.  Klik in het dialoogvenster **Nieuwe gebruiker** , moet u de volgende stappen uitvoeren:

    ![Nieuwe gebruiker] (./media/active-directory-saas-sumologic-tutorial/IC778563.png "Nieuwe gebruiker")

    1.  Typ de gerelateerde gegevens van de Azure AD-account dat u inrichten in de tekstvakken **Voornaam**, **Achternaam** en **e-mailbericht wilt** .
    2.  Selecteer een rol.
    3.  **Status**, selecteert u **actieve**.
    4.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere SumoLogic gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door SumoLogic aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-sumologic-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan SumoLogic, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **SumoLogic** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-sumologic-tutorial/IC778564.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-sumologic-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.