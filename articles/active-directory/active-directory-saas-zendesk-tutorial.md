<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zendesk | Microsoft Azure" 
    description="Informatie over het gebruiken van Zendesk met Azure Active Directory om te schakelen eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Zelfstudie: Azure Active Directory-integratie met Zendesk
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Zendesk.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Zendesk
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Zendesk één Meld u aan bij de toepassing op uw bedrijfssite Zendesk (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Zendesk inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Scenario")

##<a name="enabling-the-application-integration-for-zendesk"></a>De toepassingsintegratie van de voor Zendesk inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Zendesk.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Zendesk door de volgende stappen uitvoeren:

1.  Klik in de beheerportal Azure, klik op het linker navigatiedeelvenster, op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Zendesk**.

    ![Galerie van toepassing] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Galerie van toepassing")

7.  Selecteer **Zendesk**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Zendesk met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Zendesk configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de portal Azure AD op de pagina van de integratie in de **Zendesk** toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Eenmalige aanmelding")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Zendesk** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van app configureren] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "URL van app configureren")
  
    een. Typ in het tekstvak **Zendesk Sign In URL** de URL voor het gebruik van de volgende patroon:`https://<tenant-name>.zendesk.com`

    b. Klik op **volgende**.



4.  Klik op de pagina **configureren eenmalige aanmelding bij Zendesk** op **certificaat downloaden**en sla het certificaatbestand lokaal op uw compiter.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Zendesk als beheerder.

6.  Klik op **beheerder**.

7.  In het linkernavigatiedeelvenster, klikt u op **Instellingen**en klik vervolgens op **beveiliging**.

    ![Beveiliging] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Beveiliging")

8.  Klik op het tabblad **beheerder en agenten** op de pagina **beveiliging** .

9.  Selecteer **eenmalige aanmelding (SSO) en SAML**, en selecteer vervolgens **SAML**.

10. Klik in de portal Azure AD op de pagina **configureren eenmalige aanmelding bij Zendesk** de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **SAML SSO-URL** .

11. Klik in de portal Azure AD op de pagina **configureren eenmalige aanmelding bij Zendesk** de waarde van de **URL van externe afmelden** kopiëren en plak deze in het tekstvak **URL van externe Meld u af** .

    ![Eenmalige aanmelding] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Eenmalige aanmelding")

12. De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Certificaat vingerafdruk** .

    >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

13. Klik op **Opslaan**.

14. In de portal Azure AD, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij **Zendesk**, moeten ze worden ingericht in **Zendesk**.  
Bij **Zendesk**, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Als u wilt een gebruikersaccount Zendesk inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Zendesk** .

2.  Selecteer het tabblad **Lijst met klanten** .

3.  Selecteer het tabblad van de **gebruiker** en klik op **toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Gebruiker toevoegen")

4.  Typ het e-mailadres van een bestaand Azure AD-account dat u wilt inrichten, en klik vervolgens op **Opslaan**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Nieuwe gebruiker")

>[AZURE.NOTE] U kunt een andere Zendesk gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Zendesk aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Zendesk, moet u de volgende stappen uitvoeren:

1.  Klik in de portal Azure AD een testaccount te maken.

2.  Klik op de pagina **Zendesk **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
