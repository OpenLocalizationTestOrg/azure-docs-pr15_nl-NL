<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Sciforma | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Sciforma met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-ad-integration-with-sciforma"></a>Zelfstudie: Azure AD-integratie met Sciforma
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Sciforma.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Sciforma
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Sciforma één Meld u aan bij de toepassing op uw bedrijfssite Sciforma (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Sciforma inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-sciforma-tutorial/IC777369.png "Scenario")
##<a name="enabling-the-application-integration-for-sciforma"></a>De toepassingsintegratie van de voor Sciforma inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Sciforma.

###<a name="to-enable-the-application-integration-for-sciforma-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Sciforma door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sciforma-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-sciforma-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-sciforma-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-sciforma-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Sciforma**.

    ![Galerie van toepassing] (./media/active-directory-saas-sciforma-tutorial/IC777370.png "Galerie van toepassing")

7.  Selecteer **Sciforma**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Sciforma] (./media/active-directory-saas-sciforma-tutorial/IC777371.png "Sciforma")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Sciforma met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Sciforma** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sciforma-tutorial/IC777372.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Sciforma** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sciforma-tutorial/IC777373.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Sciforma Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. Sciforma.com*', en klik op **volgende**.

    ![URL van app configureren] (./media/active-directory-saas-sciforma-tutorial/IC777374.png "URL van app configureren")

4.  Klik op de **configureren eenmalige aanmelding bij Sciforma** pagina wilt downloaden van de metagegevens van uw, klikt u op het **downloaden van metagegevens**en klikt u vervolgens de gegevens lokaal opslaan als **c:\\SciformaMetaData.xml**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sciforma-tutorial/IC777375.png "Eenmalige aanmelding configureren")

5.  Doorsturen dat metagegevensbestand naar Sciforma ondersteuningsteam. De behoeften van de team ondersteuning Hiermee configureert u eenmalige aanmelding voor u.

6.  Selecteer de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sciforma-tutorial/IC777376.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Er is geen actie-item voor u van gebruikers aan Sciforma configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Sciforma via het Configuratiescherm van access, Sciforma Hiermee wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Sciforma.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-sciforma-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Sciforma, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Sciforma **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-sciforma-tutorial/IC777377.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-sciforma-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.