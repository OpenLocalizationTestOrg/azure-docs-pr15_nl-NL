<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Bonus.ly | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Bonus.ly met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Zelfstudie: Azure Active Directory-integratie met Bonus.ly

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Bonus.ly. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een test-tenant in Bonus.ly

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Bonus.ly inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Scenario")
##<a name="enabling-the-application-integration-for-bonusly"></a>De toepassingsintegratie van de voor Bonus.ly inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Bonus.ly.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Bonus.ly door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Eenmalige aanmelding inschakelen")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-bonus-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Bonus.ly**.

    ![Galerie van toepassing] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Galerie van toepassing")

7.  Selecteer **Bonus.ly**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Bonus.ly met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Bonus.ly configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Bonus.ly** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Bonus.ly** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Bonus.ly Tenant URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. Bonus.LY*', en klik op **volgende**: 

    ![URL van app configureren] (./media/active-directory-saas-bonus-tutorial/IC773684.png "URL van app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Bonus.ly** op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\Bonusly.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Eenmalige aanmelding configureren")

5.  In een ander browservenster, meld u aan bij uw tenant **Bonus.ly** .

6.  Op de werkbalk op de voorgrond, klikt u op **Instellingen**en selecteer vervolgens **integraties en apps**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  Selecteer onder **Eenmalige aanmelding** **SAML**.

8.  Klik op de pagina **SAML** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Bonus.ly** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **IdP SSO doel-URL** .
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Bonus.ly** de **Uitgever-ID** -waarde kopiëren en plak deze in het tekstvak **IdP uitgever** .
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Bonus.ly** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **IdP aanmeldings-URL** .
    4.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Certificaat vingerafdruk** .

        >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

9.  Klik op **Opslaan**.

10. In de klassieke portal van Microsoft Azure, selecteert u de Configuratiebevestiging en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Bonus.ly, moeten ze worden ingericht in Bonus.ly.  
Bij Bonus.ly, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  In een webbrowservenster, meld u aan bij uw tenant Bonus.ly.

2.  Klik op **Instellingen**

    ![Instellingen] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Instellingen")

3.  Klik op het tabblad **gebruikers en bonus** .

    ![Gebruikers en bonus] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Gebruikers en bonus")

4.  Klik op **gebruikers beheren**.

    ![Gebruikers beheren] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Gebruikers beheren")

5.  Klik op **gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Gebruiker toevoegen")

6.  Klik in het dialoogvenster **Gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Gebruiker toevoegen")

    1.  Typ de "**e**, **Voornaam**, **Achternaam**' van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **Opslaan**.

    >[AZURE.NOTE] De eigenaar van een account AAD ontvangt een e-mailbericht met een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Bonus.ly gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Bonus.ly aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Bonus.ly, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina Bonus.ly toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
