<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met LogicMonitor | Microsoft Azure" 
    description="Meer informatie over het gebruiken van LogicMonitor met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Zelfstudie: Azure Active Directory-integratie met LogicMonitor
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en LogicMonitor.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant LogicMonitor
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor LogicMonitor inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scenario")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>De toepassingsintegratie van de voor LogicMonitor inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor LogicMonitor.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor LogicMonitor door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **logicmonitor**.

    ![Galerie van toepassing] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Galerie van toepassing")

7.  Selecteer **LogicMonitor**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij LogicMonitor met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **LogicMonitor **toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij LogicMonitor** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **URL App configureren** in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij LogicMonitor \(e, g,: "*http://company.logicmonitor.com*"\), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij LogicMonitor** op **metagegevens downloaden**en sla deze op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Eenmalige aanmelding configureren")

5.  Meld u als beheerder aan bij uw bedrijfssite **LogicMonitor** .

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Instellingen")

7.  In de navigatie-bat aan de linkerkant, klikt u op **Eenmalige aanmelding**

    ![Eenmalige aanmelding] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Eenmalige aanmelding")

8.  In de sectie **Instellingen voor eenmalige aanmelding (SSO)** de volgende stappen uitvoeren:

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Instellingen voor eenmalige aanmelding")

    1.  Selecteer **inschakelen eenmalige aanmelding**.
    2.  Als **Standaard roltoewijzing**, selecteert u **alleen-lezen**.
    3.  Open het metagegevensbestand gedownloade in Kladblok en inhoud van het bestand vervolgens in het tekstvak **Identiteit Provider metagegevens** plakken.
    4.  Klik op **wijzigingen opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Voor AAD-gebruikers moeten kunnen aanmelden, moeten ze zijn geconfigureerd met de LogicMonitor-toepassing met hun naam Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite LogicMonitor.

2.  In het menu aan de bovenkant, klikt u op **Instellingen**en klik vervolgens op **rollen en gebruikers**.

    ![Rollen en gebruikers] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Rollen en gebruikers")

3.  Klik op **toevoegen**.

4.  Klik in de sectie **een account toevoegen** de volgende stappen uitvoeren:

    ![Een account toevoegen] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Een account toevoegen")

    1.  Typ de **gebruikersnaam**, **e-mailadres**, **wachtwoord** en **Typ wachtwoord opnieuw** waarden van de Azure Active Directory-gebruiker die u inrichten in de gerelateerde tekstvakken wilt.
    2.  Selecteer **rollen**, **machtigingen weergeven** en de **Status**.
    3.  Klik op **verzenden**.

>[AZURE.NOTE]U kunt een andere LogicMonitor gebruiker account hulpmiddelen voor het maken of API's verstrekt door LogicMonitor inrichten Azure Active Directory gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan LogicMonitor, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **LogicMonitor** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.




