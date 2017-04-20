<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met geavanceerde Suite | Microsoft Azure"
    description="Meer informatie over het gebruik van geavanceerde Suite met Azure Active Directory om te kunnen eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Zelfstudie: Azure Active Directory-integratie met geavanceerde Suite

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en geavanceerde Suite.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een geavanceerde Suite-tenant

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan geavanceerde Suite één Meld u aan bij de toepassing op uw bedrijfssite geavanceerde Suite (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor geavanceerde Suite inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scenario")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>De toepassingsintegratie van de voor geavanceerde Suite inschakelen

Het doel van dit gedeelte is om een overzicht van het inschakelen van de toepassingsintegratie van de voor geavanceerde Suite te.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor geavanceerde Suite inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **Geavanceerde Suite**.

    ![Galerie van toepassing] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Galerie van toepassing")

7.  Selecteer **Geavanceerde Suite**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Geavanceerde Suite] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Geavanceerde Suite")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers om te verifiëren geavanceerde suite met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor geavanceerde Suite configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Geavanceerde Suite** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op geavanceerde Suite** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-instellingen configureren** in het tekstvak **Beantwoorden URL** de URL voor het gebruik van de volgende patroon "*https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE =*', en klik op **volgende**.

    >[AZURE.NOTE] U kunt deze waarde krijgen van de geavanceerde Suite **SAML SSO** instellingenpagina.

    ![App-instellingen configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "App-instellingen configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij geavanceerde Suite** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite geavanceerde Suite als beheerder.

6.  Ga naar **beheerder**.

    ![Beheerder] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Beheerder")

7.  Klik in de sectie **gebruikers en rollen** op **SAML SSO-instellingen beheren**.

    ![Op SAML SSO-instellingen beheren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Op SAML SSO-instellingen beheren")

8.  Klik op de pagina **Instellingen voor eenmalige aanmelding SAML** kunt u de volgende stappen uitvoeren:

    ![Op SAML SSO-instellingen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "Op SAML SSO-instellingen")

    1.  Typ een naam voor uw configuratie in het tekstvak **naam van de provider identiteit** .
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij geavanceerde Suite** de **Entiteit-ID** -waarde kopiëren en plak deze in het tekstvak **identiteitsprovider entiteit-ID** .
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij geavanceerde Suite** de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **identiteitsprovider SSO-URL** .
    4.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij geavanceerde Suite** de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **aangepaste afmelden URL** .
    5.  Klik op **bestand kiezen**om uw gedownloade certificaat.
    6.  Selecteer als **SAML gebruikers-id** **van de gebruiker geavanceerde inzichten gebruikersnaam in te voeren**.
    7.  Als **SAML-gebruikerslocatie-id**, selecteert u **gebruikers-id in NameID van onderwerp**.
    8.  **Op SAML NameID opmaken**, selecteert u **e-mailadres**.
    9.  Als **SAML inschakelen**, selecteert u **toestaan SAML SSO en directe geavanceerde inzichten login**.
    10. Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij geavanceerde Suite, moeten ze ingericht in geavanceerde Suite.  
Geavanceerde-Suite is inrichting een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw bedrijfssite **Geavanceerde Suite** als beheerder.

2.  Ga naar **beheerder**.

    ![Beheerder] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Beheerder")

3.  Klik op **Gebruiker toevoegen**in de sectie **gebruikers en rollen** .

    ![Gebruiker toevoegen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Gebruiker toevoegen")

4.  Klik in de sectie **Nieuwe gebruiker** moet u de volgende stappen uitvoeren:

    ![Indienen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Indienen")

    1.  Typ de **naam**, **Login**, **e-mailadres**en **wachtwoord** van een geldige Azure Active Directory-gebruiker die u inrichten in de gerelateerde tekstvakken wilt.
    2.  Selecteer een **rol**.
    3.  Klik op **verzenden**.

>[AZURE.NOTE] U kunt een andere geavanceerde Suite gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door geavanceerde Suite aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers geavanceerde Suite, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Geavanceerde Suite **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
