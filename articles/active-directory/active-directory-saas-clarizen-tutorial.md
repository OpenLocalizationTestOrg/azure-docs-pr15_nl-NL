<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Clarizen | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Clarizen met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Zelfstudie: Azure Active Directory-integratie met Clarizen

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Clarizen.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Clarizen eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Clarizen één Meld u aan bij de toepassing op uw bedrijfssite Clarizen (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Clarizen inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scenario")
##<a name="enabling-the-application-integration-for-clarizen"></a>De toepassingsintegratie van de voor Clarizen inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Clarizen.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Clarizen door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Clarizen**.

    ![Galerie van toepassing] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Galerie van toepassing")

7.  Selecteer **Clarizen**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Clarizen met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Clarizen** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Clarizen** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **configureren eenmalige aanmelding bij Clarizen** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Eenmalige aanmelding configureren")

4.  In een andere webbrowservenster, meld u aan bij uw site van het bedrijf **Clarizen** als een beheerder (bijvoorbeeld: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Klik op uw gebruikersnaam en klik vervolgens op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Instellingen")

6.  Klik op het tabblad **Globale instellingen** en klik vervolgens naast **Federatieve verificatie**, op **bewerken**.

    ![Globale instellingen] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Globale instellingen")

7.  Klik in het dialoogvenster **Federatieve verificatie** kunt u de volgende stappen uitvoeren:

    ![Federatieve verificatie] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Federatieve verificatie")

    1.  Klik op **uploaden** als u wilt uw gedownloade certificaat uploaden.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Clarizen** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **URL aanmelden** .
    3.  Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure portal, klik op de pagina **configureren één afmelden bij Clarizen** dialoogvenster en plak deze in het tekstvak **Sign-out-URL** .
    4.  Selecteer **gebruiken posten**.
    5.  Klik op **Opslaan**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Clarizen, moeten ze worden ingericht in Clarizen.  
Bij Clarizen, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Clarizen** .

2.  Klik op **personen**.

    ![Personen] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Personen")

3.  Klik op **uitnodiging van de gebruiker**.

    ![Gebruikers uitnodigen] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Gebruikers uitnodigen")

4.  Klik op de pagina van het dialoogvenster personen uitnodigen, moet u de volgende stappen uitvoeren:

    ![Mensen uitnodigen] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Mensen uitnodigen")

    1.  Typ in het tekstvak **e-mailadres** het e-mailadres van een geldige Azure Active Directory-account inrichten gewenste.
    2.  Klik op **uitnodigen**.

    >[AZURE.NOTE] De eigenaar van een account Azure Active Directory wordt ontvangen een e-mailbericht en volgt u een koppeling om te bevestigen hun-account voordat deze geactiveerd wordt.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Clarizen, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Clarizen **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
