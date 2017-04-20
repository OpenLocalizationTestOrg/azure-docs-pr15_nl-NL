<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met centraal bureaublad | Microsoft Azure" 
    description="Leer hoe u centraal bureaublad gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerd inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Zelfstudie: Azure Active Directory-integratie met centraal bureaublad

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en centraal bureaublad. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een centrale bureaublad eenmalige aanmelding ingeschakeld abonnement / centraal bureaublad tenant

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor centraal bureaublad inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")
##<a name="enabling-the-application-integration-for-central-desktop"></a>De toepassingsintegratie van de voor centraal bureaublad inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor centraal bureaublad.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor centraal bureaublad door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Centraal bureaublad**.

    ![Galerie van toepassing] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Galerie van toepassing")

7.  Selecteer **Centraal bureaublad**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Centraal bureaublad] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Centraal bureaublad")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Centraal bureaublad met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw tenant centraal bureaublad.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Centraal bureaublad** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Centraal bureaublad** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** de volgende stappen uitvoeren en klik vervolgens op **volgende**: 

    -   Typ in het tekstvak **Centraal bureaublad Sign In URL** de URL van uw tenant centraal bureaublad (bijvoorbeeld: *http://contoso.centraldesktop.com*).
    -   Typ in het tekstvak centraal bureaublad antwoord URL uw centraal bureaublad AssertionConsumerService-URL (bijvoorbeeld: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] U kunt de waarde openen via het centrale bureaublad metagegevens (bijvoorbeeld: *http://contoso.centraldesktop.com*).

    ![URL van app configureren] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "URL van app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Centraal bureaublad** als u wilt downloaden van het certificaat, op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Eenmalige aanmelding configureren")

5.  Meld u aan bij uw tenant **Centraal bureaublad** .

6.  Ga naar **Instellingen**, klikt u op **Geavanceerd**en klik vervolgens op **Eenmalige aanmelding**.

    ![Setup - geavanceerde] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - geavanceerde")

7.  Klik op de pagina **Aanmelden op instellingen voor eenmalige** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding instellingen] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Eenmalige aanmelding instellingen")

    1.  Selecteer **inschakelen SAML v2 eenmalige aanmelding**.
    2.  Kopieer de **URL van de uitgever** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Centraal bureaublad** en plak deze in het tekstvak **SSO-URL** .
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Centraal bureaublad** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **SSO aanmeldings-URL** .
    4.  Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Centraal bureaublad** en plak deze in het tekstvak **URL voor eenmalige aanmelding meld u af** .

8.  Klik in de sectie **Bericht handtekening Verification Method** kunt u de volgende stappen uitvoeren:

    ![Bericht handtekening Verification Method] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Bericht handtekening Verification Method")

    1.  Selecteer het **certificaat**.
    2.  Selecteer in de lijst **SSO certificaat** **RSH SHA256**.
    3.  Een tekstbestand maken vanuit het gedownloade certificaat, Kopieer de inhoud van het tekstbestand en plak deze in het veld **SSO certificaat** .  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  Selecteer **een koppeling naar de aanmeldingspagina SAMLv2 weergeven**.

9.  Klik op **bijwerken**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Voor AAD-gebruikers moeten kunnen aanmelden, moeten ze zijn geconfigureerd met de toepassing centraal bureaublad. In deze sectie wordt beschreven hoe AAD-gebruikersaccounts maken in de bureaubladversie van centraal.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Voor het inrichten van gebruikersaccounts centraal bureaublad:

1.  Meld u aan bij uw tenant centraal bureaublad.

2.  Ga naar **personen \> interne leden**.

3.  Klik op **interne leden toevoegen**.

    ![Personen] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Personen")

4.  Typ in het tekstvak **E-mailadres van nieuwe leden** een AAD account dat u wilt inrichten en klik vervolgens op **volgende**.

    ![E-mailadressen van nieuwe leden] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-mailadressen van nieuwe leden")

5.  Klik op **toevoegen interne leden**.

    ![Interne lid toevoegen] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Interne lid toevoegen")

    >[AZURE.NOTE] De gebruikers die u hebt toegevoegd, ontvangt een e-mailbericht met een bevestigingskoppeling moet worden geklikt om het account te activeren.

>[AZURE.NOTE] U kunt een andere centraal bureaublad gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Centraal bureaublad naar inrichten AAD-gebruikersaccounts

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers centraal bureaublad, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Centraal bureaublad** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
