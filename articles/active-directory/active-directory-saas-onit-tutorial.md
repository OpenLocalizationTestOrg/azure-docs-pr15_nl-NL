<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Onit | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Onit met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Zelfstudie: Azure Active Directory-integratie met Onit
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Onit.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Onit eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Onit één Meld u aan bij de toepassing op uw bedrijfssite Onit (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Onit inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-onit-tutorial/IC791166.png "Scenario")
##<a name="enabling-the-application-integration-for-onit"></a>De toepassingsintegratie van de voor Onit inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Onit.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Onit door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-onit-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-onit-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-onit-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Onit**.

    ![Galerie van toepassing] (./media/active-directory-saas-onit-tutorial/IC791167.png "Galerie van toepassing")

7.  Selecteer **Onit**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Onit met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Onit configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).
  
Uw toepassing Onit verwacht de bevestigingen SAML een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Eenmalige aanmelding] (./media/active-directory-saas-onit-tutorial/IC791168.png "Eenmalige aanmelding")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina **Onit** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-onit-tutorial/IC791169.png "Kenmerken")

2.  Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

    
  	|Kenmerknaam|Kenmerkwaarde|
  	|---|---|
  	|naam|User.userPrincipalName|
  	|E-mail|User.mail|

    1.  Klik op **gebruikerskenmerk toevoegen**voor elke gegevensrij met in de tabel hierboven.
    2.  Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .
    3.  Selecteer de kenmerkwaarde weergegeven voor die rij in de lijst **Kenmerkwaarde** .
    4.  Klik op **Voltooien**.

3.  Klik op **wijzigingen toepassen**.

4.  Klik op **terug** om het dialoogvenster **Quick Start** opnieuw in uw browser.

5.  Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791170.png "Eenmalige aanmelding configureren")

6.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Onit** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791171.png "Eenmalige aanmelding configureren")

7.  Typ op de pagina **App-URL configureren** , in het tekstvak **Onit aanmelding op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Onit-toepassing (bijvoorbeeld: "*https://ms-sso-test.onit.com*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-onit-tutorial/IC791172.png "URL van App configureren")

8.  Klik op de pagina **configureren eenmalige aanmelding bij Onit** op **Download-certificaat**te downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791173.png "Eenmalige aanmelding configureren")

9.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Onit als beheerder.

10. Klik in het menu aan de bovenkant, op **beheer**.

    ![Beheer] (./media/active-directory-saas-onit-tutorial/IC791174.png "Beheer")

11. Klik op **Bewerken Corporation**.

    ![Corporation bewerken] (./media/active-directory-saas-onit-tutorial/IC791175.png "Corporation bewerken")

12. Klik op het tabblad **beveiliging** .

    ![Bedrijfsgegevens bewerken] (./media/active-directory-saas-onit-tutorial/IC791176.png "Bedrijfsgegevens bewerken")

13. Klik op het tabblad **beveiliging** , moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-onit-tutorial/IC791177.png "Eenmalige aanmelding")

    1.  Als **Verificatie strategie**, selecteert u **eenmalige aanmelding en wachtwoord**.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Onit** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Idp doel-URL** .
    3.  Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **Idp afmelden URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Onit** .
    4.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Idp certificaat vingerafdruk (SHA1)** .  

        >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    5.  Als het **Type voor eenmalige aanmelding**, selecteer **SAML**.
    6.  Typ in het tekstvak **Eenmalige aanmelding knoptekst** de tekst van een knop die u tevreden bent.
    7.  Selecteer **aanmelden met eenmalige aanmelding: vereist voor de volgende domeinen/gebruikers**, typt u het e-mailadres van een testgebruiker in de gerelateerde tekstvak en klik vervolgens op **bijwerken**.
        ![Corporation bewerken] (./media/active-directory-saas-onit-tutorial/IC791178.png "Corporation bewerken")

14. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-onit-tutorial/IC791179.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Onit, moeten ze worden ingericht in Onit.  
Bij Onit, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u bij uw bedrijfssite **Onit** aan als beheerder.

2.  Klik op **gebruiker toevoegen**.

    ![Beheer] (./media/active-directory-saas-onit-tutorial/IC791180.png "Beheer")

3.  Klik op de pagina van het dialoogvenster **Gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-onit-tutorial/IC791181.png "Gebruiker toevoegen")

    1.  Typ de **naam** en het **E-mailadres** van een geldige AAD-account dat u wilt inrichten in de gerelateerde tekstvakken.
    2.  Klik op **maken**.  

        >[AZURE.NOTE] De accounteigenaar krijgt een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Onit gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Onit aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Onit, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Onit **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-onit-tutorial/IC791182.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-onit-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.