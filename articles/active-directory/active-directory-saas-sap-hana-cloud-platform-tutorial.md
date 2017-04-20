<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SAP HANA Cloud Platform | Microsoft Azure" 
    description="Meer informatie over het gebruik van SAP HANA Cloud Platform met Azure Active Directory om te kunnen eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Zelfstudie: Azure Active Directory-integratie met SAP HANA Cloud Platform
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en SAP HANA Cloud Platform.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een SAP HANA Cloud Platform-account
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan SAP HANA Cloud Platform één Meld u aan bij de toepassing de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruikt.

>[AZURE.IMPORTANT]Moet u uw eigen toepassing implementeren of u abonneren op een toepassing op uw account SAP HANA Cloud Platform eenmalige testen op. In deze zelfstudie is een toepassing geïmplementeerd in het account.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor SAP HANA Cloud Platform inschakelen
2.  Eenmalige aanmelding configureren
3.  Het toewijzen van een rol aan een gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>De toepassingsintegratie van de voor SAP HANA Cloud Platform inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor SAP HANA Cloud Platform.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor SAP HANA Cloud Platform inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in de beheerportal Azure, klik op het linker navigatiedeelvenster, op **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **SAP HANA Cloud Platform**.

    ![Galerie van toepassing] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Galerie van toepassing")

7.  **SAP HANA Cloud Platform**selecteren in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![SAP Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij SAP HANA Cloud Platform met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw tenant SAP HANA Cloud Platform.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SAP HANA Cloud Platform** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SAP HANA Cloud Platform** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Eenmalige aanmelding configureren")

3.  In een andere webbrowservenster, moet u zich aanmelden bij de SAP HANA Cloud Platform Cockpit bij https://account. \<liggend host\>.ondemand.com/cockpit (bijvoorbeeld: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Klik op het tabblad **vertrouwen** .

    ![Vertrouwen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Vertrouwen")

5.  In de sectie vertrouwen beheer van de volgende stappen uitvoeren:

    ![Metagegevens ophalen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Metagegevens ophalen")

    1.  Klik op het tabblad **Lokale-Provider** .
    2.  Het bestand te downloaden SAP HANA Cloud Platform metagegevens, klikt u op **Metagegevens ophalen**.

6.  De volgende stappen uitvoeren in de Azure Active klassieke portal, klik op de pagina **URL App configureren** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "URL van App configureren")

    1.  Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers u zich aanmeldt bij uw **SAP HANA Cloud Platform** -toepassing. Dit is de account / regiospecifieke-URL van een beveiligde bron in uw SAP HANA Cloud Platform-toepassing. De URL is gebaseerd op de volgende patroon: https:// *\<applicationName\>\<accountnaam\>.\< Liggend host\>.ondemand.com/\<pad\_naar\_beveiligde\_resource\> * (bijvoorbeeld: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Dit is de URL in uw SAP HANA Cloud Platform-toepassing waarin de gebruiker om te verifiëren.

    2.  Open het gedownloade bestand van SAP HANA Cloud Platform metagegevens, en Ga naar de tag **ns3:AssertionConsumerService** .
    3.  De waarde van het kenmerk **locatie** kopiëren en plak deze in het tekstvak **SAP HANA Cloud Platform antwoord URL** .

7.  Klik op de pagina **configureren eenmalige aanmelding bij SAP HANA Cloud Platform** wilt downloaden van de metagegevens van uw, op **metagegevens downloaden**en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Eenmalige aanmelding configureren")

8.  Klik op de SAP HANA Cloud Platform Cockpit, in de sectie **Lokale-Provider** de volgende stappen uitvoeren:

    ![De tekenpagina vergroten] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "De tekenpagina vergroten")

    1.  Klik op **bewerken**.
    2.  Als het **Type van de configuratie**, selecteer **aangepast**.
    3.  Als **De naam van lokale Provider**, laat u de standaardwaarde.
    4.  Als u wilt een **Toets aanmelden** en een combinatie van een **Certificaat voor ondertekening** sleutel genereren, klikt u op **Paar sleutel genereren**.
    5.  Als **Het doorgeven van Principal**, selecteer **uitgeschakeld**.
    6.  Als de **Verificatie van kracht**, selecteer **uitgeschakeld**.
    7.  Klik op **Opslaan**.

9.  Klik op het tabblad **Vertrouwde id-Provider** en klik vervolgens op **Vertrouwde identiteitsprovider toevoegen**.

    ![De tekenpagina vergroten] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "De tekenpagina vergroten")

    >[AZURE.NOTE]Als u wilt beheren de lijst met vertrouwde identiteitsprovider, moet u het type aangepast configuratie in de sectie lokale-Provider voor hebt gekozen. Voor de configuratie van standaardtype hebt u een niet-bewerkbare en impliciet vertrouwen bij de SAP-ID-Service. Voor geen, als u geen vertrouwensinstellingen.

10. Klik op het tabblad **Algemeen** en klik vervolgens op **Bladeren** om het gedownloade metagegevensbestand te uploaden.

    ![De tekenpagina vergroten] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "De tekenpagina vergroten")

    >[AZURE.NOTE] Na het uploaden van het metagegevensbestand, worden de waarden voor **eenmalige aanmelding URL**, **URL van één Meld u af** en **Certificaat voor ondertekening** automatisch ingevuld.

11. Klik op het tabblad **kenmerken** .

12. Klik op het tabblad **kenmerken** kunt u de volgende stappen uitvoeren:

    ![Kenmerken] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Kenmerken")

    1.  Toevoegen door te klikken op **Add Assertion-Based kenmerk**, de volgende bevestiging gebaseerde kenmerken:

        |Bevestiging kenmerk| Hoofdsom kenmerk|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/givenName|   Voornaam|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/surname|        Achternaam|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/EmailAddress|E-mail|

    >[AZURE.NOTE]De configuratie van de kenmerken, is afhankelijk van hoe de toepassingen op rechtermuisknop worden ontwikkeld, dat wil zeggen welke kenmerken ze in het antwoord op SAML verwachten en onder welke naam (hoofdsom kenmerk) toegang tot dit kenmerk in de code.
    >  
    >een.  Het **Kenmerk standaard** in de schermopname is alleen betrekking heeft op illustratie. Het scenario werken is niet verplicht.  
    >
    >b.  De namen en waarden voor **Principal kenmerk** in de schermopname weergegeven afhankelijk van hoe de toepassing is ontwikkeld. Het is mogelijk dat voor uw toepassing verschillende toewijzingen is vereist.

13. In de klassieke Azure portal, klik op de dialoog **configureren eenmalige aanmelding bij SAP HANA Cloud Platform** pagina selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Eenmalige aanmelding configureren")
  
Als een optionele stap, kunt u groepen bevestiging gebaseerde configureren voor uw identiteitsprovider Azure Active Directory

>[AZURE.NOTE]Met groepen op SAP HANA Cloud Platform kunt u een of meer gebruikers dynamisch toewijzen aan een of meer rollen in uw SAP HANA Cloud Platform-toepassingen, bepaald door de waarden van kenmerken in de SAML 2.0-bevestiging. Als de bevestiging het kenmerk bevat bijvoorbeeld "*contract = tijdelijke*', u kunt alle getroffen gebruikers moet worden toegevoegd aan de groep"*tijdelijke*". De groep "*tijdelijke*" kan een of meer functies uit een of meer toepassingen die zijn geïmplementeerd in uw SAP HANA Cloud Platform-account bevatten.
>  
>Bevestiging gebaseerde groepen gebruiken als u wilt toewijzen massa veel gebruikers aan een of meer rollen van toepassingen in uw SAP HANA Cloud Platform-account. Als u alleen wilt toewijzen van een enkel of een klein aantal gebruikers aan (a) specifieke rollen wordt u aangeraden toe te wijzen rechtstreeks op het tabblad "**vergunningen**" van de cockpit SAP HANA Cloud Platform.

##<a name="assigning-a-role-to-a-user"></a>Het toewijzen van een rol aan een gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij SAP HANA Cloud Platform, moet u rollen in het SAP HANA Cloud-Platform toewijzen aan deze.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Als u wilt een rol aan een gebruiker toewijst, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw cockpit **SAP HANA Cloud Platform** .

2.  De volgende stappen uitvoeren:

    ![Vergunningen] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Vergunningen")

    1.  Klik op **autorisatie**.
    2.  Klik op het tabblad **gebruikers** .
    3.  Typ in het tekstvak **gebruiker** e-mailadres van de gebruiker.
    4.  Klik op **toewijzen** om de gebruiker toewijzen aan een rol.
    5.  Klik op **Opslaan**.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers naar SAP HANA Cloud-Platform, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **SAP HANA Cloud Platform** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.