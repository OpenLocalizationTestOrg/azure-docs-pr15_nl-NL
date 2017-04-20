<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Adobe EchoSign | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Adobe EchoSign met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Zelfstudie: Azure Active Directory-integratie met Adobe EchoSign

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Adobe EchoSign.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Adobe EchoSign één zich aanmeldt op ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Adobe EchoSign één Meld u aan bij de toepassing op uw bedrijfssite Adobe EchoSign (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Adobe EchoSign inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Scenario")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>De toepassingsintegratie van de voor Adobe EchoSign inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Adobe EchoSign.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Adobe EchoSign inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **Adobe EchoSign**.

    ![Galerie van toepassing] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Galerie van toepassing")

7.  Selecteer **Adobe EchoSign**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Adobe EchoSign met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **Adobe EchoSign** toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Adobe EchoSign** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.echosign.com/*" op de pagina **App-URL configureren** in het tekstvak **Adobe EchoSign aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Adobe EchoSign als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Account**en klikt u in het navigatiedeelvenster op de linker chip op **SAML-instellingen** onder **Accountinstellingen**.

    ![Account] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Account")

7.  In de sectie SAML-instellingen kunt u de volgende stappen uitvoeren:

    ![Op SAML-instellingen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "Op SAML-instellingen")

    1.  **Op SAML-modus**, selecteer **SAML verplicht**.
    2.  Selecteer **Toestaan EchoSign Account beheerders aan te melden met hun referenties EchoSign**.
    3.  Als de **Gebruiker maken**, selecteer **automatisch toevoegen gebruikers geverifieerd via SAML**.

8.  Verplaatsen op de volgende stappen uit te voeren:

    ![Op SAML-instellingen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "Op SAML-instellingen")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** de **Entiteit-ID** -waarde kopiëren en plak deze in het tekstvak **IdP entiteit-ID** .
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **IdP aanmeldings-URL** .
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Adobe EchoSign** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **IdP afmelden URL** .
    4.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **IdP certificaat** 6.  Klik op **wijzigingen opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Adobe EchoSign, moeten ze worden ingericht in Adobe EchoSign.  
Bij Adobe EchoSign, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Adobe EchoSign** .

2.  In het menu aan de bovenkant, klik op **Account**en klik vervolgens in het navigatiedeelvenster op de linker chip op **gebruikers en groepen**, en klik vervolgens op **een nieuwe gebruiker**.

    ![Account] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Account")

3.  Klik in de sectie **Nieuwe gebruiker maken** de volgende stappen uitvoeren:

    ![Gebruiker maken] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Gebruiker maken")

    1.  Typ het **E-mailadres**, **Voornaam** en **Achternaam** van een geldige AAD-account dat u wilt inrichten in de gerelateerde tekstvakken.
    2.  Klik op **gebruiker maken**.

        >[AZURE.NOTE] De eigenaar van een account Azure Active Directory, ontvangt een e-mailbericht met een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Adobe EchoSign gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Adobe EchoSign aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Adobe EchoSign, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina van de integratie in de **Adobe EchoSign **-toepassing, klikt u op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
