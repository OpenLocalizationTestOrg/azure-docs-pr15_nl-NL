<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SCC levenscyclus | Microsoft Azure" 
    description="Leer hoe u SCC levenscyclus gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Zelfstudie: Azure Active Directory-integratie met SCC LifeCycle
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en SCC levenscyclus.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een SCC levenscyclus eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan de levenscyclus van de SCC één Meld u aan bij de toepassing op uw site in de levenscyclus van de SCC bedrijf (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor de levenscyclus van de SCC inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>De toepassingsintegratie van de voor de levenscyclus van de SCC inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor de levenscyclus van de SCC.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor de levenscyclus van de SCC inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **SCC levenscyclus**.

    ![Galerie van toepassing] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Galerie van toepassing")

7.  In het deelvenster met resultaten **SCC levenscyclus**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![SCC LifeCycle] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij SCC levenscyclus met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal, klik op de pagina **SCC levenscyclus** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de levenscyclus van de SCC** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door uw gebruikers aanmelden bij uw SCC levenscyclus-toepassing met het volgende patroon "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*" op de pagina **App-URL configureren** in het tekstvak **Aanmeldingsadres op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij de levenscyclus van de SCC** op **metagegevens downloaden**en sla metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Eenmalige aanmelding configureren")

5.  Doorsturen naar dat bestand metagegevens naar SCC levenscyclus-ondersteuningsteam.

    >[AZURE.NOTE]Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam SCC levenscyclus.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij de levenscyclus van de SCC, moeten ze worden ingericht in de levenscyclus van de SCC.
  
Er is geen actie-item voor u voor het configureren van de inrichting van de gebruiker aan SCC levenscyclus.  
Wanneer een toegewezen gebruiker probeert aan te melden bij de levenscyclus van de SCC, wordt automatisch een SCC levenscyclus-account gemaakt, indien nodig.

>[AZURE.NOTE]U kunt een andere SCC levenscyclus gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door SCC levenscyclus aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Als u wilt gebruikers aan de levenscyclus van de SCC toewijst, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **SCC levenscyclus **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.