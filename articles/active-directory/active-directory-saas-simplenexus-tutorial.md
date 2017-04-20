<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SimpleNexus | Microsoft Azure" 
    description="Meer informatie over het gebruiken van SimpleNexus met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Zelfstudie: Azure Active Directory-integratie met SimpleNexus
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en SimpleNexus.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een SimpleNexus eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan SimpleNexus één Meld u aan bij de toepassing op uw bedrijfssite SimpleNexus (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor SimpleNexus inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Scenario")
##<a name="enabling-the-application-integration-for-simplenexus"></a>De toepassingsintegratie van de voor SimpleNexus inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor SimpleNexus.

###<a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor SimpleNexus door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-simplenexus-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **eenvoudige nexus**.

    ![Galerie van toepassing] (./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Galerie van toepassing")

7.  Selecteer **SimpleNexus**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Eenvoudige Nexus] (./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Eenvoudige Nexus")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij SimpleNexus met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SimpleNexus** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SimpleNexus** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **SimpleNexus Sign In URL** de URL voor het gebruik van de volgende patroon "*https://simplenexus.com/CompanyName\_login*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-simplenexus-tutorial/IC786904.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij SimpleNexus** op **metagegevens downloaden**en stuurt vervolgens de metagegevensbestand naar het ondersteuningsteam SimpleNexus.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Eenmalige aanmelding configureren")

    >[AZURE.NOTE] Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam SimpleNexus.

5.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Om te kunnen Azure AD-gebruikers aan te melden bij SimpleNexus inschakelt, moeten ze worden ingericht in SimpleNexus.  
Bij SimpleNexus, met de inrichting van is een handmatige taak uitgevoerd door de tenantbeheerder.

>[AZURE.NOTE] U kunt een andere SimpleNexus gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door SimpleNexus aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan SimpleNexus, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **SimpleNexus **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.