<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met FreshService | Microsoft Azure" 
    description="Meer informatie over het gebruiken van FreshService met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Zelfstudie: Azure Active Directory-integratie met FreshService
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en FreshService.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een FreshService eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan FreshService één Meld u aan bij de toepassing de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruikt.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor FreshService inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-freshservice-tutorial/IC790807.png "Scenario")
##<a name="enabling-the-application-integration-for-freshservice"></a>De toepassingsintegratie van de voor FreshService inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor FreshService.

###<a name="to-enable-the-application-integration-for-freshservice-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor FreshService door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshservice-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-freshservice-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-freshservice-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-freshservice-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **FreshService**.

    ![Galerie van toepassing] (./media/active-directory-saas-freshservice-tutorial/IC790808.png "Galerie van toepassing")

7.  Selecteer **FreshService**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Freshservice] (./media/active-directory-saas-freshservice-tutorial/IC790809.png "Freshservice")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij FreshService met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor FreshService configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **FreshService** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790810.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij FreshService** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790811.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **FreshService aanmelding op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Freshdesk-toepassing (bijvoorbeeld: "*http://democompany.freshservice.com/*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-freshservice-tutorial/IC790812.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij FreshService** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790813.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite FreshService als beheerder.

6.  In het menu aan de bovenkant, klikt u op **beheerder**.

    ![Beheerder] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Beheerder")

7.  Klik in de **Portal voor klanten**op **beveiliging**.

    ![Beveiliging] (./media/active-directory-saas-freshservice-tutorial/IC790815.png "Beveiliging")

8.  In de sectie **beveiliging van** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-freshservice-tutorial/IC790816.png "Eenmalige aanmelding")

    1.  **Eenmalige OnON**schakelen.
    2.  Selecteer **SAML SSO**.
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij FreshService** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **SAML aanmeldings-URL** .
    4.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij FreshService** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL Meld u af** .
    5.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Beveiliging certificaat vingerafdruk** .
    
        >[AZURE.TIP]Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshservice-tutorial/IC790817.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij FreshService, moeten ze worden ingericht in FreshService.  
Bij FreshService, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **FreshService** .

2.  In het menu aan de bovenkant, klikt u op **beheerder**.

    ![Beheerder] (./media/active-directory-saas-freshservice-tutorial/IC790814.png "Beheerder")

3.  Klik in de sectie **Beheer van de gebruiker** op **aanvragers**.

    ![Aanvragers] (./media/active-directory-saas-freshservice-tutorial/IC790818.png "Aanvragers")

4.  Klik op **nieuwe aanvrager**.

    ![Nieuwe aanvragers] (./media/active-directory-saas-freshservice-tutorial/IC790819.png "Nieuwe aanvragers")

5.  Klik in de sectie **Nieuwe aanvrager** moet u de volgende stappen uitvoeren:

    ![Nieuwe aanvrager] (./media/active-directory-saas-freshservice-tutorial/IC790820.png "Nieuwe aanvrager")

    1.  Voer de **eerste naam** en **e** -kenmerken van een geldige Azure Active Directory-account dat u wilt inrichten in de gerelateerde tekstvakken.
    2.  Klik op **Opslaan**.

    >[AZURE.NOTE] De eigenaar van een account Azure Active Directory, krijgen een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt

>[AZURE.NOTE] U kunt een andere FreshService gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door FreshService aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-freshservice-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan FreshService, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **FreshService **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-freshservice-tutorial/IC790821.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-freshservice-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.