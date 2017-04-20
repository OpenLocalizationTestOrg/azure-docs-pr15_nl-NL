<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Lucidchart | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Lucidchart met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Zelfstudie: Azure Active Directory-integratie met Lucidchart
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Lucidchart.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Lucidchart eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Lucidchart één Meld u aan bij de toepassing op uw bedrijfssite Lucidchart (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Lucidchart inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Scenario")
##<a name="enabling-the-application-integration-for-lucidchart"></a>De toepassingsintegratie van de voor Lucidchart inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Lucidchart.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Lucidchart door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Lucidchart**.

    ![Galerie van toepassing] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Galerie van toepassing")

7.  Selecteer **Lucidchart**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Lucidchart met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Lucidchart** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Lucidchart** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** , in het tekstvak **Lucidchart aanmelding op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Lucidchart-toepassing (bijvoorbeeld: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Lucidchart** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en slaat u het gegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Lucidchart als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Team**.

    ![Team] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Team")

7.  Klik op **toepassing \> SAML beheren**.

    ![Op SAML beheren] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "Op SAML beheren")

8.  Klik op de pagina van het dialoogvenster **SAML verificatie-instellingen** kunt u de volgende stappen uitvoeren:

    1.  Selecteer **SAML-verificatie inschakelen**en klik vervolgens op **optioneel**.
        ![Op SAML verificatie-instellingen] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "Op SAML verificatie-instellingen")
    2.  Typ uw domein in het tekstvak **domein** en klik vervolgens op **Certificaat wijzigen**.
        ![Certificaat voor wijzigen] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Certificaat voor wijzigen")
    3.  Opent u het gedownloade metagegevensbestand, de inhoud kopiëren en plak deze in het tekstvak **Metagegevens uploaden** .
        ![Metagegevens uploaden] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Metagegevens uploaden")
    4.  Selecteer **nieuwe gebruiker automatisch toevoegen aan het team**en klik vervolgens op **wijzigingen opslaan**.
        ![Wijzigingen opslaan] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Wijzigingen opslaan")

9.  Selecteer de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Er is geen actie-item voor u van gebruikers aan Lucidchart configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Lucidchart via het Configuratiescherm van access, Lucidchart Hiermee wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Lucidchart.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Lucidchart, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Lucidchart **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.