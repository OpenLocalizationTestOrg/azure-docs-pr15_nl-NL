<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer | Microsoft Azure" 
    description="Leer hoe u Thoughtworks Singleplayer gebruiken met Azure Active Directory om te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Zelfstudie: Azure Active Directory-integratie met Thoughtworks Singleplayer
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Thoughtworks Singleplayer.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Thoughtworks Singleplayer
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Thoughtworks Singleplayer inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Scenario")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>De toepassingsintegratie van de voor Thoughtworks Singleplayer inschakelen
  
Het doel van dit gedeelte is om een overzicht van het inschakelen van de toepassingsintegratie van de voor Thoughtworks Singleplayer te.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Thoughtworks Singleplayer inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ **thoughtworks Singleplayer**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Galerie van toepassing")

7.  Selecteer **Thoughtworks Singleplayer**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Thoughtworks Singleplayer] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Singleplayer")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Thoughtworks Singleplayer met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
U moet een certificaat uploaden naar het Thoughtworks Singleplayer als onderdeel van deze procedure.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina toepassing integratie **Thoughtworks Singleplayer **op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Thoughtworks Singleplayer** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://company.mingle.thoughtworks.com*" op de pagina **App-URL configureren** in het tekstvak **Thoughtworks Singleplayer Tenant URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Thoughtworks Singleplayer** op metagegevens downloaden en sla deze op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Eenmalige aanmelding configureren")

5.  Meld u aan bij uw bedrijf **Thoughtworks Singleplayer** site als beheerder.

6.  Klik op de tab **beheerder** en klik vervolgens op **SSO Config**.

    ![Eenmalige aanmelding zoekconfiguratie] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "Eenmalige aanmelding zoekconfiguratie")

7.  Klik in de sectie **Config voor eenmalige aanmelding van** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding zoekconfiguratie] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "Eenmalige aanmelding zoekconfiguratie")

    1.  Klik op **bestand kiezen**om het metagegevensbestand.
    2.  Klik op **wijzigingen opslaan**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Voor AAD-gebruikers moeten kunnen aanmelden, moeten ze zijn geconfigureerd met de toepassing Thoughtworks Singleplayer met hun naam Azure Active Directory.  
Bij Thoughtworks Singleplayer, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw bedrijf Thoughtworks Singleplayer site als beheerder.

2.  Klik op **profiel**.

    ![Uw eerste Project] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Uw eerste Project")

3.  Klik op de tab **beheerder** en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Gebruikers")

4.  Klik op **nieuwe gebruiker**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Nieuwe gebruiker")

5.  Klik op de pagina van het dialoogvenster **Nieuwe gebruiker** , moet u de volgende stappen uitvoeren:

    ![Nieuwe gebruiker] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Nieuwe gebruiker")

    1.  Typ de **aanmeldingsnaam**, **weergavenaam**, **Kies wachtwoord**, **Bevestig het wachtwoord** van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Als het **type van de gebruiker**, selecteert u **volledig gebruiker**.
    3.  Klik op **Dit profiel maken**.

>[AZURE.NOTE] U kunt een andere Thoughtworks Singleplayer gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Thoughtworks Singleplayer aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Thoughtworks Singleplayer, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina toepassing integratie **Thoughtworks Singleplayer** op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
