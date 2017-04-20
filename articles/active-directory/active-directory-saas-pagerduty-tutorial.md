<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Pagerduty | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Pagerduty met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Zelfstudie: Azure Active Directory-integratie met Pagerduty
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Pagerduty.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Pagerduty
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Pagerduty één Meld u aan bij de toepassing op uw bedrijfssite Pagerduty (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Pagerduty inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Scenario")
##<a name="enabling-the-application-integration-for-pagerduty"></a>De toepassingsintegratie van de voor Pagerduty inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Pagerduty.

###<a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Pagerduty door de volgende stappen uitvoeren:

1.  Klik in de beheerportal Azure, klik op het linker navigatiedeelvenster, op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-pagerduty-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Pagerduty**.

    ![Galerie van toepassing] (./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Galerie van toepassing")

7.  Selecteer **Pagerduty**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![PagerDuty] (./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Pagerduty met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Pagerduty** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Pagerduty** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Pagerduty Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. Pagerduty.com*', en klik op **volgende**.

    ![Url van de app configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Url van de app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Pagerduty** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Pagerduty als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Accountinstellingen")

7.  Klik op **eenmalige aanmelding**.

    ![Eenmalige aanmelding] (./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Eenmalige aanmelding")

8.  Klik op de pagina inschakelen eenmalige aanmelding (SSO) de volgende stappen uitvoeren:

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Eenmalige aanmelding inschakelen")

    1.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    2.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **X.509-certificaat**
    3.  In de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij Pagerduty** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    4.  Kopieer de **URL van externe afmelden** -waarde in de klassieke Azure portal, klik op de dialoog **configureren eenmalige aanmelding bij Pagerduty** pagina en plak deze in het tekstvak **URL Meld u af** .
    5.  Schakel **eenmalige aanmelding inschakelen**.
    6.  Klik op **wijzigingen opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Pagerduty, moeten ze worden ingericht in Pagerduty.  
Bij Pagerduty, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Pagerduty** .

2.  In het menu aan de bovenkant, klikt u op **gebruikers**.

3.  Klik op **gebruikers toevoegen**.

    ![Gebruikers toevoegen] (./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Gebruikers toevoegen")

4.  Typ in het dialoogvenster **uitnodigen van uw team** de **Voornaam en achternaam** en het **e** -mailadres van de Azure AD-gebruiker die u wilt inrichten, klikt u op **toevoegen**en klik vervolgens op **Verzenden om uit te nodigen**.

    ![Uw team uitnodigen] (./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Uw team uitnodigen")

    >[AZURE.NOTE] Alle toegevoegde gebruikers, ontvangt een uitnodiging om een PagerDuty-account te maken.

>[AZURE.NOTE] U kunt een andere Pagerduty gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Pagerduty aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Pagerduty, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Pagerduty **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.