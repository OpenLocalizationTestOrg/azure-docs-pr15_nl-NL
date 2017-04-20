<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met handel | Microsoft Azure" 
    description="Meer informatie over het gebruiken van handel met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Zelfstudie: Azure Active Directory-integratie met handel
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en handel.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een vergunning eenmalige aanmelding abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan de handel één Meld u aan bij de toepassing op uw bedrijfssite handel (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor handel inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Scenario")
##<a name="enabling-the-application-integration-for-greenhouse"></a>De toepassingsintegratie van de voor handel inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor handel.

###<a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor handel door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-greenhouse-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **handel**.

    ![Galerie van toepassing] (./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Galerie van toepassing")

7.  Selecteer **handel**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Handel] (./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Handel")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij handel met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **handel** toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij handel** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.greenhouse.io*" op de pagina **App-URL configureren** in het tekstvak **Aanmeldingsadres op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790788.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij handel** op **metagegevens downloaden**en sla metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Eenmalige aanmelding configureren")

5.  Doorsturen naar dat bestand metagegevens naar handel ondersteuningsteam.

    >[AZURE.NOTE] Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam handel.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij de handel, moeten ze worden ingericht in handel.  
Bij handel, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **handel** .

2.  In het menu aan de bovenkant, klikt u op **configureren**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Gebruikers")

3.  Klik op **nieuwe gebruikers**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-greenhouse-tutorial/IC790792.png "Nieuwe gebruiker")

4.  Klik in de sectie **Nieuwe gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Nieuwe gebruiker toevoegen] (./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Nieuwe gebruiker toevoegen")

    1.  Typ het e-mailadres van een geldige Azure Active Directory-account inrichten gewenste in het tekstvak **de e-mail van de Enter-gebruikers** .
    2.  Klik op **Opslaan**.
        
        >[AZURE.NOTE] De Azure Active Directory-account houders ontvangt een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere handel gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door handel aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>Als u wilt gebruikers aan handel toewijst, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina van de integratie in de **handel **-toepassing, klikt u op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.