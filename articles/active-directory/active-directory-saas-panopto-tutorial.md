<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Panopto | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Panopto met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Zelfstudie: Azure Active Directory-integratie met Panopto
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Panopto.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Panopto
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Panopto één Meld u aan bij de toepassing op uw bedrijfssite Panopto (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Panopto inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Scenario")
##<a name="enabling-the-application-integration-for-panopto"></a>De toepassingsintegratie van de voor Panopto inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Panopto.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Panopto door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-panopto-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Panopto**.

    ![Galerie met Appkication] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Galerie met Appkication")

7.  Selecteer **Panopto**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Panopto met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Panopto** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Panopto** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Panopto Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. Panopto.com*', en klik op **volgende**.

    ![URL van app configureren] (./media/active-directory-saas-panopto-tutorial/IC777528.png "URL van app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Panopto** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Panopto als beheerder.

6.  Op de werkbalk aan de linkerkant, klikt u op **systeem**en klik vervolgens op **Identiteitsprovider**.

    ![Systeem] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Systeem")

7.  Klik op **Provider toevoegen**.

    ![Identiteitsproviders] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Identiteitsproviders")

8.  In de sectie SAML-provider, moet u de volgende stappen uitvoeren:

    ![SaaS configuratie] (./media/active-directory-saas-panopto-tutorial/IC777672.png "SaaS configuratie")

    1.  Selecteer in de lijst **Type Provider** **SAML20**
    2.  Typ een naam voor het exemplaar in het tekstvak **Exemplaar** .
    3.  Typ in het tekstvak **Beschrijving** een beschrijving.
    4.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Panopto** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **uitgever** .
    5.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Panopto** de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **Url voor de aanmeldingspagina Stuiteren** .
    6.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    7.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **Openbare sleutel**
    8.  Klik op **Opslaan**.
        ![Opslaan] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Opslaan")

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Er is geen actie-item voor u van gebruikers aan Panopto configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Panopto via het Configuratiescherm van access, Panopto Hiermee wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Panopto.

>[AZURE.NOTE]U kunt een andere Panopto gebruiker account hulpmiddelen voor het maken of API's verstrekt door Panopto voor het inrichten van Azure AD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Panopto, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Panopto **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.