<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Aha! | Microsoft Azure" 
    description="Meer informatie over het gebruik van Aha! met Azure Active Directory zodat eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Zelfstudie: Azure Active Directory-integratie met Aha!

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Aha!  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Aha! eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, de Azure AD-gebruikers u hebt toegewezen aan Aha! kunnen één Meld u aan bij de toepassing op uw Aha! bedrijfssite (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Aha zodat.
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-aha-tutorial/IC798944.png "Scenario")
##<a name="enabling-the-application-integration-for-aha"></a>De toepassingsintegratie van de voor Aha zodat.

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Aha!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Om in te schakelen van de toepassingsintegratie van de voor Aha!, de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-aha-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-aha-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-aha-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **Aha!**.

    ![Galerie van toepassing] (./media/active-directory-saas-aha-tutorial/IC798945.png "Galerie van toepassing")

7.  Selecteer in het deelvenster met resultaten **Aha!**, en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Aha!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Aha!")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Aha! met hun rekening in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal op de **Aha!** integratie van toepassingen pagina, klikt u op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798946.png "Eenmalige aanmelding configureren")

2.  Klik op de **Hoe wilt u dat gebruikers aanmelden bij Aha!** pagina's, selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798947.png "Eenmalige aanmelding configureren")

3.  Op de pagina **App-URL configureren** in de **Aha! Aanmelden op URL** tekstvak, typ de URL wordt gebruikt door uw gebruikers voor aanmelding bij uw Aha! Toepassing (bijvoorbeeld: "*https://company.aha.io/session/new*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-aha-tutorial/IC798948.png "URL van App configureren")

4.  Klik op de **eenmalige aanmelding configureren op Aha!** pagina aan uw metagegevensbestand downloaden, klikt u op **metagegevens te downloaden**en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798949.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw Aha! bedrijfssite als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-aha-tutorial/IC798950.png "Instellingen")

7.  Klik op **Account**.

    ![Profiel] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profiel")

8.  Klik op **beveiliging en eenmalige aanmelding**.

    ![Beveiligings- en eenmalige aanmelding] (./media/active-directory-saas-aha-tutorial/IC798952.png "Beveiligings- en eenmalige aanmelding")

9.  Selecteer in de sectie **Eenmalige aanmelding** , als **Identiteitsprovider**, **SAML2.0**.

    ![Beveiligings- en eenmalige aanmelding] (./media/active-directory-saas-aha-tutorial/IC798953.png "Beveiligings- en eenmalige aanmelding")

10. Klik op de pagina **Eenmalige aanmelding** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-aha-tutorial/IC798954.png "Eenmalige aanmelding")

    1.  Typ een naam voor uw configuratie in **het tekstvak** .
    2.  Selecteer **Bestand met metagegevens**voor het **gebruik van configureren**.
    3.  Klik op **Bladeren**om het gedownloade metagegevensbestand.
    4.  Klik op **bijwerken**.

11. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-aha-tutorial/IC798955.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat Azure AD-gebruikers kunnen Meld u aan bij Aha!, ze moeten deze is ingericht in Aha!.  
In het geval van Aha!, inrichting is een geautomatiseerde taak.  
Er is geen actie-item voor u.
  
Gebruikers worden automatisch zo nodig gemaakt tijdens de eerste eenmalige aanmelding poging.

>[AZURE.NOTE] U kunt een andere Aha! hulpmiddelen voor het maken van account of verstrekt door Aha-API's! voor het inrichten van AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Gebruikers toewijzen aan Aha!, de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de **Aha!** integratie van toepassingen pagina, klikt u op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-aha-tutorial/IC798956.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-aha-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
