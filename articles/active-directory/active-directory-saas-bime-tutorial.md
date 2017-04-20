<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Bime | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Bime met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Zelfstudie: Azure Active Directory-integratie met Bime

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Bime.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Bime

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Bime één Meld u aan bij de toepassing op uw bedrijfssite Bime (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Bime inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-bime-tutorial/IC775552.png "Scenario")
##<a name="enabling-the-application-integration-for-bime"></a>De toepassingsintegratie van de voor Bime inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Bime.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Bime door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-bime-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-bime-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-bime-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Bime**.

    ![Galerie van toepassing] (./media/active-directory-saas-bime-tutorial/IC775553.png "Galerie van toepassing")

7.  Selecteer **Bime**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Bime met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Bime configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Bime** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC771709.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Bime** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775555.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Bime Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. Bimeapp.com*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-bime-tutorial/IC775556.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Bime** als u wilt downloaden van het certificaat, klikt u op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\Bime.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775557.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Bime als beheerder.

6.  Klik in de werkbalk op **beheer**en klik op **Account**.

    ![Beheerder] (./media/active-directory-saas-bime-tutorial/IC775558.png "Beheerder")

7.  Klik op de accountpagina configuratie, kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775559.png "Eenmalige aanmelding configureren")

    1.  Selecteer **inschakelen op SAML-verificatie**.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Bime** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Externe aanmeldings-URL** .
    3.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Certificaat vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    4.  Klik op **Opslaan**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-bime-tutorial/IC775560.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Bime, moeten ze worden ingericht in Bime.  
Bij Bime, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Bime** .

2.  Klik in de werkbalk op **beheer**en klik op **gebruikers**.

    ![Beheerder] (./media/active-directory-saas-bime-tutorial/IC775561.png "Beheerder")

3.  Klik op **Nieuwe gebruiker toevoegen** ('en') in de **Lijst inbelgebruikers**.

    ![Gebruikers] (./media/active-directory-saas-bime-tutorial/IC775562.png "Gebruikers")

4.  Klik op de pagina **Details van de gebruiker** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Gebruikersgegevens] (./media/active-directory-saas-bime-tutorial/IC775563.png "Gebruikersgegevens")

    1.  Voer de voornaam, achternaam, Login, e-mailbericht met een geldig AAD-account inrichten gewenste.
    2.  Klik op opslaan.

>[AZURE.NOTE] U kunt een andere Bime gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Bime aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Bime, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Bime **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-bime-tutorial/IC775564.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-bime-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
