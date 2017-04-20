<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Boomi | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Boomi met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Zelfstudie: Azure Active Directory-integratie met Boomi

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Boomi.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Boomi eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Boomi één Meld u aan bij de toepassing op uw bedrijfssite Boomi (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Boomi inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Scenario")
##<a name="enabling-the-application-integration-for-boomi"></a>De toepassingsintegratie van de voor Boomi inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Boomi.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Boomi door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-boomi-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Boomi**.

    ![Galerie van toepassing] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Galerie van toepassing")

7.  Selecteer **Boomi**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Boomi met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Boomi** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Boomi** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** in het tekstvak **Boomi antwoord URL** , typt u uw **Boomi AtomSphere aanmeldings-URL** (bijvoorbeeld: "*https://platform.boomi.com/sso/AccountName/saml*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-boomi-tutorial/IC790826.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Boomi** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Boomi als beheerder.

6.  Op de werkbalk op de voorgrond, klikt u op uw bedrijfsnaam en klik vervolgens op **instellen**.

    ![Setup] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Setup")

7.  Klik op **Opties voor eenmalige aanmelding**.

    ![Opties voor eenmalige aanmelding] (./media/active-directory-saas-boomi-tutorial/IC790829.png "Opties voor eenmalige aanmelding")

8.  In de sectie **Opties voor eenmalige aanmelding** , moet u de volgende stappen uitvoeren:

    ![Opties voor eenmalige aanmelding] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Opties voor eenmalige aanmelding")

    1.  Selecteer **eenmalige aanmelding SAML inschakelen**.
    2.  Klik op **importeren**als u wilt het gedownloade certificaat uploaden.
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Boomi** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Identiteit Provider aanmeldings-URL** .
    4.  Als **Federatie Id locatie**, selecteer **Federatie-Id wordt NameID element van het onderwerp**.
    5.  Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Boomi, moeten ze worden ingericht in Boomi.  
Bij Boomi, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Boomi** .

2.  Ga naar **Gebruikersbeheer \> gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Gebruikers")

3.  Klik op **gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Gebruiker toevoegen")

4.  Klik op de pagina van het dialoogvenster **Gebruikersrollen toevoegen** de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Gebruiker toevoegen")

    1.  Typ de **Voornaam**, **Achternaam** en **e-mailbericht** van een geldige AAD-account dat u wilt inrichten in de gerelateerde tekstvakken.
    2.  Klik op OK.

>[AZURE.NOTE] U kunt een andere Boomi gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Boomi aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Boomi, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Boomi **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
