<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile | Microsoft Azure" 
    description="Leer hoe u Citrix ShareFile gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Zelfstudie: Azure Active Directory-integratie met Citrix ShareFile

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Citrix ShareFile.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Citrix ShareFile

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Citrix ShareFile één Meld u aan bij de toepassing op uw bedrijfssite Citrix ShareFile (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Citrix ShareFile inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Scenario")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>De toepassingsintegratie van de voor Citrix ShareFile inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Citrix ShareFile.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Citrix ShareFile inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **Citrix ShareFile**.

    ![Galerie van toepassing] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Galerie van toepassing")

7.  Selecteer **Citrix ShareFile**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Citrix ShareFile met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Citrix ShareFile** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Eenmalige aanmelding inschakelen")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Citrix ShareFile** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL App configureren** in het tekstvak **Citrix ShareFile aanmelding op URL** de URL voor het gebruik van de volgende patroon `https://<tenant-name>.shareFile.com`, en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Citrix ShareFile** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![ConfigureSingle eenmalige aanmelding] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle eenmalige aanmelding")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite **Citrix ShareFile** als beheerder.

6.  Klik op **beheerder**in de werkbalk op de voorgrond.

7.  Selecteer in het linkernavigatiedeelvenster **Configureren eenmalige aanmelding**.

    ![Accountbeheer] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Accountbeheer")

8.  Klik op de **eenmalige aanmelding / SAML 2.0 configuratie** dialoogvenster weer onder **Basisinstellingen**de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Eenmalige aanmelding")

    1.  Klik op **SAML inschakelen**.
    2.  Kopieer de waarde **Entiteit-ID** in de Azure klassieke portal, klik op de pagina **configureren eenmalige aanmelding bij Citrix ShareFile** en plak deze in de **Your IDP uitgever / entiteit-ID** tekstvak.
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Citrix ShareFile** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    4.  Kopieer de **URL van externe afmelden** -waarde in de klassieke Azure portal, klik op **de configureren eenmalige aanmelding bij Citrix ShareFile** dialoogvenster pagina en plak deze in het tekstvak **URL Meld u af** .
    5.  Klik op **wijzigen** naast het veld **X.509-certificaat** en upload het certificaat dat u hebt gedownload van de klassieke Azure AD-portal.
        ![Basisinstellingen] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Basisinstellingen")

9.  Klik op **Opslaan** in de beheerportal Citrix ShareFile.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Citrix ShareFile, moeten ze worden ingericht in Citrix ShareFile.  
Bij Citrix ShareFile, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Citrix ShareFile** .

2.  Klik op **gebruikers beheren \> beheren van gebruikers Start \> + werknemer maken**.

    ![Werknemer maken] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Werknemer maken")

3.  Voer de **e-mailbericht**, **Voornaam** en **Achternaam** van een geldige Azure AD-account u wilt inrichten.

    ![Basisinformatie] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Basisinformatie")

4.  Klik op **gebruiker toevoegen**.

    >[AZURE.NOTE] De eigenaar van een account AAD wordt ontvangen een e-mailbericht en volgt u een koppeling om te bevestigen hun-account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Citrix ShareFile gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Citrix ShareFile aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Citrix ShareFile, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Citrix ShareFile **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
