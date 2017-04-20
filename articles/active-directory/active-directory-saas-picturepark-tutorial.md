<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Picturepark | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Picturepark met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Zelfstudie: Azure Active Directory-integratie met Picturepark
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Picturepark.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Picturepark
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Picturepark één Meld u aan bij de toepassing op uw bedrijfssite Picturepark (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Picturepark inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenario")

##<a name="enabling-the-application-integration-for-picturepark"></a>De toepassingsintegratie van de voor Picturepark inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Picturepark.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Picturepark door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Picturepark**.

    ![Galerie van toepassing] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Galerie van toepassing")

7.  Selecteer **Picturepark**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Picturepark met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Picturepark configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)...

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Picturepark** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Picturepark** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://company.picturepark.com*" op de pagina **App-URL configureren** in het tekstvak **Picturepark aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Picturepark** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Picturepark als beheerder.

6.  In de werkbalk op de bovenkant, **beheerprogramma's**op en klik vervolgens op **Management Console**.

    ![Beheerconsole] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Beheerconsole")

7.  Klik op **verificatie via**en klik vervolgens op **identiteitsprovider**.

    ![Verificatie] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Verificatie")

8.  In de sectie **identiteit provider configureren** , moet u de volgende stappen uitvoeren:

    ![Identiteit provider configureren] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identiteit provider configureren")

    1.  Klik op **toevoegen**.
    2.  Typ een naam voor uw configuratie.
    3.  Selecteer **als standaard instellen**.
    4.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Picturepark** de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **Uitgever URI** .
    5.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Vertrouwde uitgever miniatuur afdrukken** .  

        >[AZURE.TIP]Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    6.  Klik op **JoinDefaultUsersGroup**.
    7.  Typ een **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**om het kenmerk **Emailaddress** in het tekstvak **claimen** .
        ![Configuratie] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Configuratie")
    8.  Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Picturepark, moeten ze worden ingericht in Picturepark.  
Bij Picturepark, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Picturepark** .

2.  Op de werkbalk op de voorgrond, klikt u op **Systeembeheer**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Gebruikers")

3.  Op het tabblad **overzicht van de gebruikers** , klikt u op **Nieuw**.

    ![Gebruikersbeheer] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Gebruikersbeheer")

4.  Klik in het dialoogvenster **Gebruiker maken** de volgende stappen uitvoeren:

    ![Gebruiker maken] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Gebruiker maken")

    1.  Type de: **E-mailadres**, **wachtwoord**, **Bevestig het wachtwoord**, **Voornaam**, **Achternaam**, **bedrijf**, **land**, **ZIP**, **plaats** van een geldige Azure Active Directory-gebruiker de gewenste gegevens leveren aan in de gerelateerde tekstvakken.
    2.  Selecteer een **taal**.
    3.  Klik op **maken**.

>[AZURE.NOTE]U kunt een andere Picturepark gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Picturepark aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Picturepark, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Picturepark **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.