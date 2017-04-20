<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TalentLMS | Microsoft Azure" 
    description="Meer informatie over het gebruiken van TalentLMS met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Zelfstudie: Azure Active Directory-integratie met TalentLMS
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en TalentLMS.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant TalentLMS
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan TalentLMS één Meld u aan bij de toepassing op uw bedrijfssite TalentLMS (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor TalentLMS inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Scenario")

##<a name="enabling-the-application-integration-for-talentlms"></a>De toepassingsintegratie van de voor TalentLMS inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor TalentLMS.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor TalentLMS door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **TalentLMS**.

    ![Galerie van toepassing] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Galerie van toepassing")

7.  Selecteer **TalentLMS**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij TalentLMS met hun account in Azure AD met Federatie op basis van het SAML-protocol. .  
Eenmalige aanmelding voor TalentLMS configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **TalentLMS** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij TalentLMS** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **TalentLMS Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. TalentLMSapp.com*', en klik op **volgende**.

    ![Meld u aan op URL] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "Meld u aan op URL")

4.  Klik op de pagina **configureren eenmalige aanmelding bij TalentLMS** als u wilt downloaden van het certificaat, klikt u op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\TalentLMS.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite TalentLMS als beheerder.

6.  Klik op het tabblad **gebruikers** in de sectie **& Accountinstellingen** .

    ![& Accountinstellingen] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "& Accountinstellingen")

7.  Klik op **Eenmalige aanmelding (SSO)**,

8.  In de sectie eenmalige aanmelding, moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Eenmalige aanmelding")

    1.  Selecteer in de lijst **type voor eenmalige aanmelding integratie** **SAML 2.0**.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij TalentLMS** de **Identiteit Provider-ID** -waarde kopiëren en plak deze in het tekstvak **identiteitsprovider (IdP)** .
    3.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Certificaat vingerafdruk** .

        >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    4.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij TalentLMS** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **URL van externe aanmelden** .
    5.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij TalentLMS** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL van externe afmelden** .
    6.  Typ in het tekstvak **TargetedID** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  Typ in het tekstvak **Voornaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  Typ in het tekstvak **Achternaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  Typ in het tekstvak **e** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij TalentLMS, moeten ze worden ingericht in TalentLMS.  
Bij TalentLMS, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **TalentLMS** .

2.  Klik op **gebruikers**en klik vervolgens op **Gebruiker toevoegen**.

3.  Klik op de pagina van het dialoogvenster **gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Gebruiker toevoegen")

    1.  Typ de waarden gerelateerde kenmerk van de Azure AD-gebruikersaccount in de volgende tekstvakken: **Voornaam**, **Achternaam**, **e-mailadres**.
    2.  Klik op **gebruiker toevoegen**.

>[AZURE.NOTE] U kunt een andere TalentLMS gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door TalentLMS aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan TalentLMS, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **TalentLMS **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.