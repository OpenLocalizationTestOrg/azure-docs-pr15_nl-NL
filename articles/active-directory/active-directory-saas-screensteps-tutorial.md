<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ScreenSteps | Microsoft Azure" 
    description="Meer informatie over het gebruiken van ScreenSteps met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Zelfstudie: Azure Active Directory-integratie met ScreenSteps
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en ScreenSteps.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant ScreenSteps
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan ScreenSteps één Meld u aan bij de toepassing op uw bedrijfssite ScreenSteps (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor ScreenSteps inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Scenario")
##<a name="enabling-the-application-integration-for-screensteps"></a>De toepassingsintegratie van de voor ScreenSteps inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor ScreenSteps.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor ScreenSteps door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **ScreenSteps**.

    ![Galerie van toepassing] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Galerie van toepassing")

7.  Selecteer **ScreenSteps**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij ScreenSteps met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **ScreenSteps** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ScreenSteps** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **ScreenSteps Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. ScreenSteps.com*', en klik op **volgende**.

    ![URL van app configureren] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "URL van app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij ScreenSteps** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite ScreenSteps als beheerder.

6.  Klik op **Account beheren**.

    ![Accountbeheer] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Accountbeheer")

7.  Klik op **externe verificatie**.

    ![Externe verificatie] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Externe verificatie")

8.  Klik op **verificatie-eindpunt maken**.

    ![Externe verificatie] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Externe verificatie")

9.  Klik in de sectie **een verificatie-eindpunt maken** de volgende stappen uitvoeren:

    ![Een eindpunt verificatie maken] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Een eindpunt verificatie maken")

    1.  Typ in het tekstvak **titel** een titel.
    2.  Selecteer in de lijst **modus** **SAML**.
    3.  Klik op **maken**.

10. Klik in de sectie **Externe verificatie-eindpunt** de volgende stappen uitvoeren:

    ![Externe verificatie eindpunt] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Externe verificatie eindpunt")

    1.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij ScreenSteps** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Remote aanmeldings-URL** .
    2.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij ScreenSteps** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL afmelden** .
    3.  Klik op **een bestand kiezen**en upload het gedownloade certificaat.
    4.  Klik op **bijwerken**.

11. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij **ScreenSteps**, moeten ze worden ingericht in **ScreenSteps**.  
Bij **ScreenSteps**, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Als u wilt een gebruikersaccount ScreenSteps inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **ScreenSteps** .

2.  Klik op **Account beheren**.

    ![Accountbeheer] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Accountbeheer")

3.  Klik op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Gebruikers")

4.  Klik op **maken een gebruiker**.

    ![Alle gebruikers] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Alle gebruikers")

5.  Selecteer in de lijst **Gebruikersrol** een rol voor uw gebruikers.

6.  Typ in de sectie gebruikersrol de**'Voornaam**, **Achternaam**, **E-mail**, **Login**, **wachtwoord** en **Wachtwoord te bevestigen'**van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.

    ![Nieuwe gebruiker] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Nieuwe gebruiker")

7.  In de sectie groepen, selecteer **Verificatie groep (SAML)**en klik op **Gebruiker maken**.

    ![Groepen] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Groepen")

>[AZURE.NOTE]U kunt een andere ScreenSteps gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door ScreenSteps aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan ScreenSteps, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **ScreenSteps **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Toewijzen aan gebruikers")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.