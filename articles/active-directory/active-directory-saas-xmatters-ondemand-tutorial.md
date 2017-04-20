<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand | Microsoft Azure"
    description="Meer informatie over het gebruiken van xMatters OnDemand met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Zelfstudie: Azure Active Directory-integratie met xMatters OnDemand
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en xMatters OnDemand. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een xMatters OnDemand tenant
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan xMatters OnDemand één Meld u aan bij de toepassing op uw xMatters OnDemand bedrijfssite (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor xMatters OnDemand inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scenario")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>De toepassingsintegratie van de voor xMatters OnDemand inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor xMatters OnDemand.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor xMatters OnDemand inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **xMatters OnDemand**.

    ![Galerie van toepassing] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Galerie van toepassing")

7.  Selecteer **XMatters OnDemand**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![xMatters OnDemand] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij XMatters OnDemand met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **XMatters OnDemand** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij XMatters OnDemand** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van app configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "URL van app configureren")

    een. Typ in het tekstvak **XMatters OnDemand Sign In URL** de URL voor het gebruik van de volgende patroon:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Klik op **volgende**.


4.  Klik op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** als wilt downloaden van het certificaat, klikt u op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Moet u het certificaat naar het ondersteuningsteam xMatters doorsturen. Het certificaat moet worden geüpload door het ondersteuningsteam xMatters voordat u de configuratie voor eenmalige aanmelding kunt voltooien.

    ![Meld u aan op één configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Meld u aan op één configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite XMatters OnDemand als beheerder.

6.  In de werkbalk op de bovenkant, klik op **beheerder**en klik vervolgens op **Bedrijfsdetails** in de navigatiebalk aan de linkerkant.

    ![Beheerder] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Beheerder")

7.  Klik op de pagina **Op SAML-configuratie** kunt u de volgende stappen uitvoeren:

    ![Op SAML-configuratie] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "Op SAML-configuratie")

    1.  Selecteer **SAML inschakelen**.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** de **Identiteit Provider-ID** -waarde kopiëren en plak deze in het tekstvak **Identiteit Provider-ID** .
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **Eenmalige aanmelding op URL** .
    4.  Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij XMatters OnDemand** en plak deze in het tekstvak **URL van één Meld u af** .
    5.  Klik op de pagina Details van bedrijf aan de bovenkant, klikt u op **Wijzigingen opslaan**.
        ![Bedrijfsgegevens] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Bedrijfsgegevens")

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Meld u aan op één configureren] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Meld u aan op één configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij XMatters OnDemand, moeten ze worden ingericht in XMatters OnDemand.  
Bij XMatters OnDemand, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **XMatters OnDemand** .

2.  Klik op het tabblad **gebruikers** .

3.  Klik op **gebruiker toevoegen**.

    ![Gebruikers] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Gebruikers")

4.  Selecteer **actieve**.

5.  Klik in de sectie **een gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Een gebruiker toevoegen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Een gebruiker toevoegen")

    1.  Voer de **gebruikersnaam**, **Voornaam**, **Achternaam**, **Site** van een geldige AAD-account inrichten gewenste.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere XMatters OnDemand gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door XMatters OnDemand aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan XMatters OnDemand, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **XMatters OnDemand **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.