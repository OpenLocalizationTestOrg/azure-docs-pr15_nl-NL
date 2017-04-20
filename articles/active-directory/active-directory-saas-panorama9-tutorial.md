<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Panorama9 | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Panorama9 met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Zelfstudie: Azure Active Directory-integratie met Panorama9
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Panorama9.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Panorama9 eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Panorama9 één Meld u aan bij de toepassing op uw bedrijfssite Panorama9 (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Panorama9 inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Scenario")
##<a name="enabling-the-application-integration-for-panorama9"></a>De toepassingsintegratie van de voor Panorama9 inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Panorama9.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Panorama9 door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Panorama9**.

    ![Galerie van toepassing] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Galerie van toepassing")

7.  Selecteer **Panorama9**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Panorama9 met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Panorama9 configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Panorama9** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Panorama9** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Panorama9 aanmelding op URL** de URL die wordt gebruikt door uw gebruikers te melden bij een Panorama9 (bijvoorbeeld: "*https://dashboard.panorama9.com/saml/access/3262*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "URL van App configureren")

4.  Op de pagina **configureren eenmalige aanmelding bij Panorama9** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla deze lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Panorama9 als beheerder.

6.  In de werkbalk op de bovenkant, klik op **beheren**en klik op **extensies**.

    ![Uitbreidingen] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Uitbreidingen")

7.  Klik in het dialoogvenster **extensies** op **Eenmalige aanmelding**.

    ![Eenmalige aanmelding] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Eenmalige aanmelding")

8.  In de sectie **instellingen van** de volgende stappen uitvoeren:

    ![Instellingen] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Instellingen")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Panorama9** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **identiteit provider URL** .
    2.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **certificaat vingerafdruk** .  

        >[AZURE.TIP]Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    3.  Klik op **Opslaan**.

9.  In de klassieke Azure AD-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Panorama9, moeten ze worden ingericht in Panorama9.  
Bij Panorama9, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Panorama9** .

2.  In het menu aan de bovenkant, klik op **beheren**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Gebruikers")

3.  Klik op **+**.

4.  Klik in het gegevensgedeelte de gebruiker de volgende stappen uitvoeren:

    ![Gebruikers] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Gebruikers")

    1.  Typ in het tekstvak **e-mailadres** het e-mailadres van een geldige Azure Active Directory-gebruiker die u inrichten wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE]U kunt een andere Panorama9 gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Panorama9 aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Panorama9, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Panorama9** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.