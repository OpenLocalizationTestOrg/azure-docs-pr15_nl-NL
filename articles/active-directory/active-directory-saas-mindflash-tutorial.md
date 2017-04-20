<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Mindflash | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Mindflash met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mindflash"></a>Zelfstudie: Azure Active Directory-integratie met Mindflash
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Mindflash.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Mindflash eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Mindflash één Meld u aan bij de toepassing op uw bedrijfssite Mindflash (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Mindflash inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-mindflash-tutorial/IC787132.png "Scenario")
##<a name="enabling-the-application-integration-for-mindflash"></a>De toepassingsintegratie van de voor Mindflash inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Mindflash.

###<a name="to-enable-the-application-integration-for-mindflash-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Mindflash door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mindflash-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-mindflash-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-mindflash-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-mindflash-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Mindflash**.

    ![Galerie van toepassing] (./media/active-directory-saas-mindflash-tutorial/IC787133.png "Galerie van toepassing")

7.  Selecteer **Mindflash**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Mindflash] (./media/active-directory-saas-mindflash-tutorial/IC787134.png "Mindflash")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Mindflash met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Mindflash** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787135.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Mindflash** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787136.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://company.mindflash.com*" op de pagina **App-URL configureren** in het tekstvak **Aanmeldingsadres op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-mindflash-tutorial/IC787137.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Mindflash** op **metagegevens downloaden**en sla het metagegevensbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787138.png "Eenmalige aanmelding configureren")

5.  De MetadataFile toe aan het ondersteuningsteam Mindflash sturen

    >[AZURE.NOTE] De configuratie voor eenmalige aanmelding heeft moet worden uitgevoerd door het ondersteuningsteam Mindflash. U krijgt een melding zodra de configuratie is voltooid.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mindflash-tutorial/IC787139.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Om te kunnen Azure AD-gebruikers aan te melden bij Mindflash inschakelt, moeten ze worden ingericht in Mindflash.  
Bij Mindflash, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Mindflash** .

2.  Ga naar **gebruikers beheren**.

    ![Gebruikers beheren] (./media/active-directory-saas-mindflash-tutorial/IC787140.png "Gebruikers beheren")

3.  Klik op de **Gebruikers toevoegen**en klik vervolgens op **Nieuw**.

4.  In de sectie **Nieuwe gebruikers toevoegen** , moet u de volgende stappen uitvoeren:

    ![Nieuwe gebruikers toevoegen] (./media/active-directory-saas-mindflash-tutorial/IC787141.png "Nieuwe gebruikers toevoegen")

    1.  Typ de **Voornaam**, **Achternaam** en **e-mailbericht** van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **toevoegen**.

>[AZURE.NOTE]U kunt een andere Mindflash gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Mindflash aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-mindflash-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Mindflash, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Mindflash **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-mindflash-tutorial/IC787142.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-mindflash-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.