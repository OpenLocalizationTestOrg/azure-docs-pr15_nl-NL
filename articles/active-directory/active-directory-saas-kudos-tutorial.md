<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met kudo | Microsoft Azure" 
    description="Meer informatie over het gebruiken van kudo met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-kudos"></a>Zelfstudie: Azure Active Directory-integratie met kudo
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en kudo.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant kudo
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan kudo één Meld u aan bij de toepassing op uw bedrijfssite kudo (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor kudo inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-kudos-tutorial/IC787799.png "Scenario")
##<a name="enabling-the-application-integration-for-kudos"></a>De toepassingsintegratie van de voor kudo inschakelen
  
Het doel van dit gedeelte is om een overzicht van het inschakelen van de toepassingsintegratie van de voor kudo te.

###<a name="to-enable-the-application-integration-for-kudos-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor kudo inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kudos-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-kudos-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-kudos-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-kudos-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **kudo**.

    ![Galerie van toepassing] (./media/active-directory-saas-kudos-tutorial/IC787800.png "Galerie van toepassing")

7.  Selecteer **kudo**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Kudo] (./media/active-directory-saas-kudos-tutorial/IC787801.png "Kudo")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij kudo met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **kudo** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787802.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij kudo** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787803.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.kudosnow.com*" op de pagina **App-URL configureren** in het tekstvak **Kudo Aanmeldingsadres op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-kudos-tutorial/IC787804.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij kudo** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787805.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite kudo als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Instellingen")

7.  Klik op **integraties \> SSO**.

8.  Klik in de sectie **eenmalige aanmelding van** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-kudos-tutorial/IC787807.png "Eenmalige aanmelding")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij kudo** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **URL aanmelden** .
    2.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP]
        Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    3.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **X.509-certificaat**
    4.  Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij kudo** en plak deze in het tekstvak **Afmelden naar URL** .
    5.  Typ de naam van uw bedrijf in het tekstvak **Uw kudo-URL** .
    6.  Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kudos-tutorial/IC787808.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij kudo, moeten ze worden ingericht in kudo.  
Bij kudo, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **kudo** .

2.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-kudos-tutorial/IC787806.png "Instellingen")

3.  Klik op **Gebruikerbeheerder**.

4.  Klik op het tabblad **gebruikers** en klik vervolgens op **een gebruiker toevoegen**.

    ![Gebruikersbeheerder] (./media/active-directory-saas-kudos-tutorial/IC787809.png "Gebruikersbeheerder")

5.  Klik in de sectie **een gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Een gebruiker toevoegen] (./media/active-directory-saas-kudos-tutorial/IC787810.png "Een gebruiker toevoegen")

    1.  Typ de **Voornaam**, **Achternaam**, **E-mail** en andere details van een geldige Azure Active Directory-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **gebruiker maken**.

>[AZURE.NOTE]U kunt een andere kudo gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door kudo aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-kudos-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan kudo, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **kudo **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-kudos-tutorial/IC787811.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-kudos-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.