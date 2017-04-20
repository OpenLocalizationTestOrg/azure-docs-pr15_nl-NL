<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Thirdlight | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Thirdlight met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Zelfstudie: Azure Active Directory-integratie met Thirdlight
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Thirdlight.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Thirdlight eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Thirdlight één Meld u aan bij de toepassing op uw bedrijfssite Thirdlight (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Thirdlight inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Scenario")

##<a name="enabling-the-application-integration-for-thirdlight"></a>De toepassingsintegratie van de voor Thirdlight inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Thirdlight.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Thirdlight door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Thirdlight**.

    ![Galerie van toepassing] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Galerie van toepassing")

7.  Selecteer **Thirdlight**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Thirdlight met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Thirdlight configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Thirdlight** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Thirdlight** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Thirdlight Sign In URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Thirdlight-toepassing (bijvoorbeeld: "*http://azuresso2.thirdlight.com/*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Thirdlight** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Thirdlight als beheerder.

6.  Ga naar **configuratie \> Systeembeheer**, en klik vervolgens op **SAML2**.

    ![Systeembeheer van de] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Systeembeheer van de")

7.  In de sectie SAML2 configuratie van de volgende stappen uitvoeren:

    ![Eenmalige aanmelding SAML] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "Eenmalige aanmelding SAML")

    1.  Selecteer **eenmalige aanmelding SAML2 inschakelen**.
    2.  Als **bron voor IdP metagegevens**, selecteer **Laden IdP metagegevens van XML**.
    3.  Open het metagegevensbestand gedownloade, Kopieer de inhoud en plak deze in het tekstvak **IdP metagegevens XML** .
    4.  Klik op **Opslaan SAML2-instellingen**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Thirdlight, moeten ze worden ingericht in Thirdlight.  
Bij Thirdlight, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Thirdlight** .

2.  Ga naar het tabblad **gebruikers** .

3.  Selecteer **gebruikers en groepen**.

4.  Klik op de knop **nieuwe gebruiker toevoegen** .

5.  Voer **de gebruikersnaam, naam of beschrijving, E-mail, kies een vooraf ingestelde of groep van nieuwe leden** van een geldige AAD-account dat u wilt inrichten.

6.  Klik op **maken**.

>[AZURE.NOTE] U kunt een andere Thirdlight gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Thirdlight aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Thirdlight, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Thirdlight **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.