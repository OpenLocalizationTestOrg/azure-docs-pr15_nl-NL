<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Sprinklr | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Sprinklr met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Zelfstudie: Azure Active Directory-integratie met Sprinklr
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Sprinklr.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Sprinklr
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Sprinklr één Meld u aan bij de toepassing op uw bedrijfssite Sprinklr (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Sprinklr inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Scenario")

##<a name="enabling-the-application-integration-for-sprinklr"></a>De toepassingsintegratie van de voor Sprinklr inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Sprinklr.

###<a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Sprinklr door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-sprinklr-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Sprinklr**.

    ![Galerie van toepassing] (./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Galerie van toepassing")

7.  Selecteer **Sprinklr**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Sprinklr] (./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Sprinklr met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Sprinklr** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Sprinklr** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Sprinklr Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. sprinklr.com*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782905.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Sprinklr** op **Download-certificaat**te downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Sprinklr als beheerder.

6.  Ga naar **beheer \> instellingen**.

    ![Beheer] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Beheer")

7.  Ga naar **beheren Partner \> eenmalige** op in het linkerdeelvenster.

    ![Partner beheren] (./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Partner beheren")

8.  Klik op **+ eenmalige invoegtoepassingen**.

    ![Eén teken-onderdelen] (./media/active-directory-saas-sprinklr-tutorial/IC782909.png "Eén teken-onderdelen")

9.  Klik op de pagina **Eenmalige aanmelding** , moet u de volgende stappen uitvoeren:

    ![Eén teken-onderdelen] (./media/active-directory-saas-sprinklr-tutorial/IC782910.png "Eén teken-onderdelen")

    1.  Typ in het tekstvak **naam** een naam voor uw configuratie (bijvoorbeeld: *WAADSSOTest*).
    2.  Selecteer **ingeschakeld**.
    3.  Selecteer **De nieuwe SSO-certificaat gebruiken**.
    4.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    5.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **Identiteit Provider certificaat**
    6.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Sprinklr** de **Identiteit Provider-ID** -waarde kopiëren en plak deze in het tekstvak **Entiteit-Id** .
    7.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Sprinklr** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Identiteit Provider aanmeldings-URL** .
    8.  Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **Identiteit Provider afmelden URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Sprinklr** .
    9.  Als het **Type van SAML gebruikers-ID**, selecteert u **bevestiging gebruiker bevat ' s sprinklr.com gebruikersnaam**.
    10. Selecteer de **gebruikers-ID in het element naam aan van de onderwerp-instructie is**als **SAML ID gebruikerslocatie**.
    11. Sluit **Opslaan**.

        ![Op SAML] (./media/active-directory-saas-sprinklr-tutorial/IC782911.png "Op SAML")

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Voor AAD-gebruikers moeten kunnen aanmelden, moeten ze zijn geconfigureerd voor toegang binnen de toepassing Sprinklr.  
In deze sectie wordt beschreven hoe u de gebruikersaccounts AAD binnen Sprinklr maakt.

###<a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Als u wilt een gebruikersaccount in Sprinklr inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw bedrijfssite Sprinklr als beheerder.

2.  Ga naar **beheer \> instellingen**.

    ![Beheer] (./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Beheer")

3.  Ga naar **Client beheren \> gebruikers** in het linkerdeelvenster.

    ![Instellingen] (./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Instellingen")

4.  Klik op **gebruiker toevoegen**.

    ![Instellingen] (./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Instellingen")

5.  Klik in het dialoogvenster **gebruiker bewerken** , moet u de volgende stappen uitvoeren:

    ![Gebruiker bewerken] (./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Gebruiker bewerken")

    1.  In de **E-mail**, **Voornaam** en **Achternaam** tekstvakken, typ de gegevens van een Azure AD-gebruikersaccount gewenste inrichten.
    2.  Selecteer **wachtwoord uitgeschakeld**.
    3.  Selecteer een **taal**.
    4.  Selecteer een **gebruikerstype**.
    5.  Klik op **bijwerken**.

    >[AZURE.IMPORTANT] **Wachtwoord uitgeschakelde** moet zijn ingeschakeld zodat een gebruiker zich aanmelden via een identiteitsprovider.

6.  Ga naar de **rol**en voer de volgende stappen uit:

    ![Partner rollen] (./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partner rollen")

    1.  Selecteer in de lijst **globale** **alle\_machtigingen**.
    2.  Klik op **bijwerken**.

>[AZURE.NOTE] U kunt een andere Sprinklr gebruiker account hulpmiddelen voor het maken of API's verstrekt door Sprinklr voor het inrichten van Azure AD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Sprinklr, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Sprinklr **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.