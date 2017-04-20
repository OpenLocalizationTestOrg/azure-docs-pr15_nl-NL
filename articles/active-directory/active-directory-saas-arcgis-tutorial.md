<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ArcGIS | Microsoft Azure" 
    description="Meer informatie over het gebruiken van ArcGIS met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Zelfstudie: Azure Active Directory-integratie met ArcGIS

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en ArcGIS. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een ArcGIS eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan ArcGIS één Meld u aan bij de toepassing op uw bedrijfssite ArcGIS (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor ArcGIS inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Scenario")
##<a name="enabling-the-application-integration-for-arcgis"></a>De toepassingsintegratie van de voor ArcGIS inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor ArcGIS.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor ArcGIS door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **ArcGIS**.

    ![Galerie met Applcation] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Galerie met Applcation")

7.  Selecteer **ArcGIS**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij ArcGIS met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **ArcGIS** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ArcGIS** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Eenmalige aanmelding configureren")

3.  Typ de URL wordt gebruikt door uw gebruikers voor Meld u aan met het volgende patroon "*https://company.maps.arcgis.com*" en klik vervolgens op **volgende**op de pagina **App-URL configureren** in het tekstvak **ArcGIS Sign In URL** .

    ![URL van App configureren] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij ArcGIS** op **metagegevens downloaden**en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite ArcGIS als beheerder.

6.  Klik op **Instellingen bewerken**.

    ![Instellingen bewerken] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Instellingen bewerken")

7.  Klik op **beveiliging**.

    ![Beveiliging] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Beveiliging")

8.  Klik in het vak **Enterprise aanmeldingen** **Identiteitsprovider instellen**.

    ![Enterprise aanmeldingen] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Enterprise aanmeldingen")

9.  Op de pagina **Identiteitsprovider instellen** , moet u de volgende stappen uitvoeren:

    ![Identiteitsprovider instellen] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Identiteitsprovider instellen")

    1.  Typ in het tekstvak naam van uw organisatie.
    2.  Voor **dat metagegevens voor de Enterprise-identiteitsprovider wordt geleverd met**, selecteert u **Een bestand**.
    3.  Klik op **bestand kiezen**om het gedownloade metagegevensbestand.
    4.  Klik op **identiteitsprovider instellen**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij ArcGIS, moeten ze worden ingericht in ArcGIS.  
Bij ArcGIS, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **ArcGIS** .

2.  Klik op **uitnodiging van leden**.

    ![Leden uitnodigen] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Leden uitnodigen")

3.  Selecteer **leden toevoegen zonder automatisch verzenden van een e-mailbericht**en klik vervolgens op **volgende**.

    ![Leden automatisch toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Leden automatisch toevoegen")

4.  Klik op de pagina **leden** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Toevoegen en beoordelen] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Toevoegen en beoordelen")

    1.  Voer de **Voornaam**, **Achternaam** en **e-mailbericht** van een geldige AAD-account dat u wilt inrichten.
    2.  Klik op **toevoegen en bekijken**.

5.  Controleer de gegevens die u hebt ingevoerd, en klik op **Leden toevoegen**.

    ![Lid toevoegen] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Lid toevoegen")

>[AZURE.NOTE] U kunt een andere ArcGIS gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door ArcGIS aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan ArcGIS, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **ArcGIS **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
