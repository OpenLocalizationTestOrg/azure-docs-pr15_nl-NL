<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Brightspace door Desire2Learn | Microsoft Azure" 
    description="Leer hoe u Brightspace door Desire2Learn gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerd inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Zelfstudie: Azure Active Directory-integratie met Brightspace door Desire2Learn

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Brightspace door Desire2Learn.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Brightspace door eenmalige aanmelding Desire2Learn ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Brightspace door Desire2Learn één Meld u aan bij de toepassing op uw Brightspace Desire2Learn bedrijfssite (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruikt.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Brightspace door Desire2Learn inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Scenario")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>De toepassingsintegratie van de voor Brightspace door Desire2Learn inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Brightspace door Desire2Learn.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Brightspace door Desire2Learn inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Brightspace door Desire2Learn**.

    ![Galerie met Apllication] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Galerie met Apllication")

7.  Selecteer **Brightspace door Desire2Learn**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Brightspace door Desire2Learn] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace door Desire2Learn")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Brightspace door Desire2Learn met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Brightspace door Desire2Learn** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Brightspace door Desire2Learn** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "URL van App configureren")

    1.  Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers u zich aanmeldt bij uw **Brightspace door Desire2Learn** (bijvoorbeeld: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij Brightspace door Desire2Learn** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en slaat u de metagegevens op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Eenmalige aanmelding configureren")

5.  De gedownloade metagegevensbestand verzenden naar uw Brightspace door het ondersteuningsteam Desire2Learn.

    >[AZURE.NOTE] Uw Brightspace door het ondersteuningsteam Desire2Learn heeft de werkelijke SSO-configuratie uitvoeren.
U krijgt een melding wanneer eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Brightspace door Desire2Learn, ze moeten worden deze is ingericht in Brightspace door Desire2Learn.  
Brightspace door Desire2Learn moeten de gebruikersaccounts worden gemaakt met uw Brightspace door het ondersteuningsteam Desire2Learn.

>[AZURE.NOTE] U kunt een andere Brightspace met hulpprogramma's voor Desire2Learn gebruiker account maken of API's verstrekt door Brightspace door Desire2Learn inrichten Azure Active Directory gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Brightspace door Desire2Learn, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Brightspace door Desire2Learn **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
