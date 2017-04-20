<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met tekenpapier LMS | Microsoft Azure" 
    description="Leer hoe u tekenpapier LMS gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Zelfstudie: Azure Active Directory-integratie met tekenpapier LMS

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Canvas.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant tekenpapier

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan tekenpapier één Meld u aan bij de toepassing op uw bedrijfssite tekenpapier (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor tekenpapier inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Scenario")
##<a name="enabling-the-application-integration-for-canvas"></a>De toepassingsintegratie van de voor tekenpapier inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Canvas.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor tekenpapier inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **tekenpapier**.

    ![Galerie van toepassing] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Galerie van toepassing")

7.  In het deelvenster met resultaten selecteert **tekenpapier**en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Tekenpapier] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Tekenpapier")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij tekenpapier met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor tekenpapier configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **tekenpapier** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij tekenpapier** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Tekenpapier Sign In URL** de URL voor het gebruik van de volgende patroon `https://<tenant-name>.instructure.com`, en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij tekenpapier** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite tekenpapier als beheerder.

6.  Ga naar **cursussen \> beheerde Accounts \> Microsoft**.

    ![Tekenpapier] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Tekenpapier")

7.  In het navigatiedeelvenster aan de linkerkant, selecteer **verificatie**en klik vervolgens op **Nieuwe SAML Config toevoegen**.

    ![Verificatie] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Verificatie")

8.  Klik op de integratie van de huidige pagina, kunt u de volgende stappen uitvoeren:

    ![Huidige-integratie] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Huidige-integratie")

    1.  Kopieer de waarde **Entiteit-ID** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij tekenpapier** en plak deze in het tekstvak **IdP entiteit-ID** .
    2.  De waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Log op URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Canvas** .
    3.  De waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Log Out URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Canvas** .
    4.  Kopieer de waarde van de **URL van de wachtwoord wijzigen** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij tekenpapier** en plak deze in het tekstvak **De koppeling wachtwoord wijzigen** .
    5.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Certificaat vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    6.  Selecteer in de lijst **Login kenmerk** **NameID**.
    7.  Selecteer in de lijst **Id notatie** **emailAddress**.
    8.  Klik op **Opslaan verificatie-instellingen**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij tekenpapier, moeten ze worden ingericht in Canvas.  
Tekenpapier is inrichting een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Canvas** .

2.  Ga naar **cursussen \> beheerde Accounts \> Microsoft**.

    ![Tekenpapier] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Tekenpapier")

3.  Klik op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Gebruikers")

4.  Klik op **nieuwe gebruiker toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Gebruikers")

5.  Klik op het toevoegen van een pagina van het dialoogvenster Nieuwe gebruiker, moet u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Gebruiker toevoegen")

    1.  Typ in het tekstvak **De naam van de volledige** naam van de gebruiker.
    2.  Typ het e-mailadres van de gebruiker in het tekstvak **e-mailbericht** .
    3.  Typ in het tekstvak **Login** Azure AD e-mailadres van de gebruiker.
    4.  Selecteer **E-mail de gebruiker over het maken van dit account**.
    5.  Klik op **gebruiker toevoegen**.

>[AZURE.NOTE] U kunt een andere tekenpapier gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Canvas naar inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers tekenpapier, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **tekenpapier **toepassing integratie op **toewijzen aan gebruikers**.

    ![Gebruikers toewijzen] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Gebruikers toewijzen")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
