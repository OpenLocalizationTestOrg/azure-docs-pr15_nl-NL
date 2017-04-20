<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Intacct | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Intacct met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Zelfstudie: Azure Active Directory-integratie met Intacct
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Intacct.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Intacct
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Intacct één Meld u aan bij de toepassing op uw bedrijfssite Intacct (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Intacct inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Scenario")
##<a name="enabling-the-application-integration-for-intacct"></a>De toepassingsintegratie van de voor Intacct inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Intacct.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Intacct door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-intacct-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Intacct**.

    ![Galerie van toepassing] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Galerie van toepassing")

7.  Selecteer **Intacct**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Intacct met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Intacct** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Intacct** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://Intacct.com/company*" op de pagina **App-URL configureren** in het tekstvak **Intacct aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-intacct-tutorial/IC790035.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Intacct** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Intacct als beheerder.

6.  Klik op het tabblad van het **bedrijf** en klik vervolgens op **Info bedrijf**.

    ![Bedrijf] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Bedrijf")

7.  Klik op het tabblad **beveiliging** en klik vervolgens op **bewerken**.

    ![Beveiliging] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Beveiliging")

8.  Klik in de sectie **eenmalige aanmelding (SSO)** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Eenmalige aanmelding")

    1.  Selecteer **eenmalige op inschakelen**.
    2.  Als het **type van de provider identiteit**, selecteer **SAML 2.0**.
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Intacct** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **URL van de uitgever** .
    4.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Intacct** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    5.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.
        
        >[AZURE.TIP]Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **certificaat**
    7.  Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Intacct, moeten ze worden ingericht in Intacct.  
Bij Intacct, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Intacct** .

2.  Klik op het tabblad van het **bedrijf** en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Gebruikers")

3.  Klik op het tabblad **toevoegen**

    ![Toevoegen] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Toevoegen")

4.  In de sectie **Gebruikersgegevens** de volgende stappen uitvoeren:

    ![Gebruikersgegevens] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Gebruikersgegevens")

    1.  Typ de **Gebruikers-ID**, de **Achternaam**, **Voornaam**, het **e-mailadres**, de **titel** en de **Telefoon** van een Azure AD-account dat u inrichten in de gerelateerde tekstvakken wilt.
    2.  Selecteer de **bevoegdheden van groepsbeheerders** van een Azure AD-account dat u inrichten wilt.
    3.  Klik op **Opslaan**.
        
        >[AZURE.NOTE] De eigenaar van een account AAD wordt ontvangen een e-mailbericht en volgt u een koppeling om te bevestigen hun-account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Intacct gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Intacct aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Intacct, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Intacct **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.