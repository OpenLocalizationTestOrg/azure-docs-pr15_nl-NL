<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Freshdesk | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Freshdesk met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Zelfstudie: Azure Active Directory-integratie met Freshdesk
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Freshdesk.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Freshdesk
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Freshdesk één Meld u aan bij de toepassing op uw bedrijfssite Freshdesk (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Freshdesk inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Scenario")
##<a name="enabling-the-application-integration-for-freshdesk"></a>De toepassingsintegratie van de voor Freshdesk inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Freshdesk.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Freshdesk door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Freshdesk**.

    ![Galerie van toepassing] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Galerie van toepassing")

7.  Selecteer **Freshdesk**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Freshdesk met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Freshdesk configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Freshdesk** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Freshdesk** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Freshdesk Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. Freshdesk.com*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Freshdesk** als u wilt downloaden van het certificaat, klikt u op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\Freshdesk.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Freshdesk als beheerder.

6.  In het menu aan de bovenkant, klikt u op **beheerder**.

    ![Beheerder] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Beheerder")

7.  Klik op het tabblad **Algemene instellingen** op **beveiliging**.

    ![Beveiliging] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Beveiliging")

8.  In de sectie **beveiliging van** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Eenmalige aanmelding")

    1.  Voor **Eenmalige aanmelding aanmelding (SSO)**, selecteert u **op**.
    2.  Selecteer **SAML SSO**.
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Freshdesk** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **SAML aanmeldings-URL** .
    4.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Freshdesk** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL Meld u af** .
    5.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Beveiliging certificaat vingerafdruk** .  

        >[AZURE.TIP]Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    6.  Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Freshdesk, moeten ze worden ingericht in Freshdesk.  
Bij Freshdesk, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Freshdesk** .

2.  In het menu aan de bovenkant, klikt u op **beheerder**.

    ![Beheerder] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Beheerder")

3.  Klik op het tabblad **Algemene instellingen** op **agenten**.

    ![Agenten] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agenten")

4.  Klik op **nieuwe Agent**.

    ![Nieuwe Agent] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Nieuwe Agent")

5.  Klik in het dialoogvenster informatie over de volgende stappen uitvoeren:

    ![Informatie over] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Informatie over")

    1.  Typ in het tekstvak **Volledige naam** de naam van de Azure AD-account dat u inrichten wilt.
    2.  Typ in het tekstvak **e** het Azure AD-e-mailadres van de Azure AD-account dat u inrichten wilt.
    3.  Typ in het tekstvak **titel** de titel van de Azure AD-account dat u inrichten wilt.
    4.  Selecteer **agenten rol**en klik vervolgens op **toewijzen**.
    5.  Klik op **Opslaan**.
    
        >[AZURE.NOTE] De eigenaar van een account Azure AD krijgt een e-mailbericht met een koppeling om te bevestigen van het account voordat deze wordt geactiveerd.

>[AZURE.NOTE] U kunt een andere Freshdesk gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Freshdesk aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Freshdesk, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Freshdesk **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.