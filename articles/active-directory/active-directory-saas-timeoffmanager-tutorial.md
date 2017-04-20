<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TimeOffManager | Microsoft Azure" 
    description="Meer informatie over het gebruiken van TimeOffManager met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Zelfstudie: Azure Active Directory-integratie met TimeOffManager
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en TimeOffManager.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een TimeOffManager eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan TimeOffManager één Meld u aan bij de toepassing op uw bedrijfssite TimeOffManager (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor TimeOffManager inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Scenario")

##<a name="enabling-the-application-integration-for-timeoffmanager"></a>De toepassingsintegratie van de voor TimeOffManager inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor TimeOffManager.

###<a name="to-enable-the-application-integration-for-timeoffmanager-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor TimeOffManager door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **TimeOffManager**.

    ![Galerie van toepassing] (./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Galerie van toepassing")

7.  Selecteer **TimeOffManager**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![TimeOffManager] (./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij TimeOffManager met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw tenant TimeOffManager.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **TimeOffManager** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij TimeOffManager** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **TimeOffManager beantwoorden URL** de URL van uw TimeOffManager AssertionConsumerService (bijvoorbeeld: "*Voorbeeld: https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_id = IC34216*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "URL van App configureren")

    U kunt de URL antwoord krijgen van de TimeOffManager eenmalige op pagina-instelling.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Instellingen voor eenmalige aanmelding")

4.  Klik op de pagina **configureren eenmalige aanmelding bij TimeOffManager** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite TimeOffManager als beheerder.

6.  Ga naar **Account \> Opties Account \> enkele instellingen voor eenmalige aanmelding**.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Instellingen voor eenmalige aanmelding")

7.  In de sectie **Instellingen voor eenmalige aanmelding** , moet u de volgende stappen uitvoeren:

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Instellingen voor eenmalige aanmelding")

    een.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    b.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plak het volledige certificaat in het tekstvak **X.509-certificaat** .
    
    c.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij TimeOffManager** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **Idp uitgever** .
    
    d.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij TimeOffManager** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **IdP eindpunt URL** .
    
    e.  Als **SAML afdwingen**, selecteert u **Nee**.
    

    f.  Als **Gebruikers automatisch maken**, selecteert u **Ja**.
    
    g.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij TimeOffManager** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL Meld u af** .
    
    h.  Klik op **wijzigingen opslaan**.

8.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij TimeOffManager** selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Eenmalige aanmelding configureren")

9.  In het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Kenmerken")

10. Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

    ![op SAML token kenmerken] (./media/active-directory-saas-timeoffmanager-tutorial/123.png "op SAML token kenmerken")

  	|Kenmerknaam|Kenmerkwaarde|
  	|---|---|
  	|E-mail|User.mail|
  	|Voornaam|User.givenName|
  	|Achternaam|User.surname|

    een.  Klik op **gebruikerskenmerk toevoegen**voor elke gegevensrij met in de tabel hierboven.

    b.  Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .

    c.  Selecteer in het tekstvak **Kenmerkwaarde** de kenmerkwaarde weergegeven voor die rij.

    d.  Klik op **Voltooien**.

11. Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij TimeOffManager, moeten ze worden ingericht naar TimeOffManager.  
TimeOffManager ondersteunt alleen in de inrichting van tijd-gebruiker. Er is geen actie-item voor u.  
De gebruikers worden automatisch toegevoegd tijdens de eerste Meld u aan met eenmalige op.

>[AZURE.NOTE] U kunt een andere TimeOffManager gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door TimeOffManager aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-timeoffmanager-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan TimeOffManager, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **TimeOffManager** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
