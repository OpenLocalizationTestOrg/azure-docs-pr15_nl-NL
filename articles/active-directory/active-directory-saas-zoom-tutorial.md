<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met zoomen | Microsoft Azure" 
    description="Leer hoe u inzoomen met Azure Active Directory gebruiken om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Zelfstudie: Azure Active Directory-integratie met zoomen
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en zoomen.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant en uitzoomen
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan zoomen één Meld u aan bij de toepassing op uw bedrijfssite zoomen (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md) gebruiken
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor zoomen inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Scenario")

##<a name="enabling-the-application-integration-for-zoom"></a>De toepassingsintegratie van de voor zoomen inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor zoomen.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor zoomen door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-zoom-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ **in-en uitzoomen**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Galerie van toepassing")

7.  Selecteer **-en uitzoomen**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![In-en uitzoomen] (./media/active-directory-saas-zoom-tutorial/IC784695.png "In-en uitzoomen")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij en uitzoomen met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina toepassing-integratie **in-en uitzoomen** op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u gebruikers aanmelden bij en uitzoomen** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://company.zoom.us*" op de pagina **App-URL configureren** in het tekstvak **Sign in-en uitzoomen In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-zoom-tutorial/IC784698.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij zoomen** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite zoomen als beheerder.

6.  Klik op het tabblad **Eenmalige aanmelding** .

    ![Eenmalige aanmelding] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Eenmalige aanmelding")

7.  Klik op het tabblad **Beveiliging beheer** en ga vervolgens naar de instellingen voor **Eenmalige aanmelding** .

8.  In de sectie eenmalige aanmelding, moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Eenmalige aanmelding")

    1.  De waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **aanmeldingsproblemen pagina URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij zoomen** .
    2.  Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij zoomen** en plak deze in het tekstvak **URL voor de aanmeldingspagina afmelden** .
    3.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **identiteit provider certificaat**
    5.  Kopieer de **URL van de uitgever** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij zoomen** en plak deze in het tekstvak **uitgever** .
    6.  Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij zoomen, moeten ze ingericht in zoomen.  
Zoomen is inrichting een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **in-en uitzoomen** .

2.  Klik op het tabblad **Account beheren** en klik vervolgens op **Gebruikersbeheer**.

3.  Klik op **gebruikers toevoegen**in de sectie beheer van de gebruiker.

    ![Gebruikersbeheer] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Gebruikersbeheer")

4.  Klik op de pagina **gebruikers toevoegen** , moet u de volgende stappen uitvoeren:

    ![Gebruikers toevoegen] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Gebruikers toevoegen")

    1.  Als het **Type van de gebruiker**, selecteert u **eenvoudige**.
    2.  Typ in het tekstvak **e-mailberichten** , het e-mailadres van een geldige AAD-account inrichten gewenste.
    3.  Klik op **toevoegen**.

>[AZURE.NOTE] U kunt een andere zoomen gebruiker account hulpmiddelen voor het maken of API's verstrekt door zoomen inrichten AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers om te zoomen, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina toepassing-integratie **in-en uitzoomen **op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.