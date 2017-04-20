<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Veracode | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Veracode met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Zelfstudie: Azure Active Directory-integratie met Veracode
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Veracode. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Veracode eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Veracode één Meld u aan bij de toepassing de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruikt.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Veracode inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Scenario")

##<a name="enabling-the-application-integration-for-veracode"></a>De toepassingsintegratie van de voor Veracode inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Veracode.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Veracode door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-veracode-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Veracode**.

    ![Galerie van toepassing] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Galerie van toepassing")

7.  Selecteer **Veracode**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Veracode met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Uw toepassing Veracode verwacht de bevestigingen SAML een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Kenmerken] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Veracode** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Veracode** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-instellingen configureren** op **volgende**.

    ![App-instellingen configureren] (./media/active-directory-saas-veracode-tutorial/IC802909.png "App-instellingen configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Veracode** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Veracode als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**en klik vervolgens op **beheerder**.

    ![Beheer] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Beheer")

7.  Klik op het tabblad **SAML** .

8.  In de sectie **Organisatie SAML-instellingen** kunt u de volgende stappen uitvoeren:

    ![Beheer] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Beheer")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Veracode** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **uitgever**
    2.  Klik op **Bestand kiezen**om uw gedownloade certificaat.
    3.  Selecteer **Self registratie inschakelen**.

9.  De volgende stappen uitvoeren en klik vervolgens op **Opslaan**in de sectie **Self registratie-instellingen** :

    ![Beheer] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Beheer")

    1.  Selecteer **Geen activering vereist**als **Nieuwe gebruikersactivering**.
    2.  Als **Gebruiker Gegevensupdates**, selecteert u de **Voorkeur Veracode User Data**.
    3.  Voor **SAML Kenmerkdetails**, selecteer de volgende opties:
        -   **Gebruikersrollen**
        -   **De beheerder het beleid**
        -   **Revisor**
        -   **Beveiliging potentiële klant**
        -   **Executive**
        -   **Inzender**
        -   **Creator**
        -   **Alle scannen typen**
        -   **Team lidmaatschappen**
        -   **Standaardteam**

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Eenmalige aanmelding configureren")

11. In het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Kenmerken")

12. Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

    ![Kenmerken] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Kenmerken")

  	| Kenmerknaam | Kenmerkwaarde |
  	|:---------------|:----------------|
  	| Voornaam      | User.givenName  |
  	| Achternaam       | User.surname    |
  	| E-mail          | User.mail       |

    1.  Klik op **gebruikerskenmerk toevoegen**voor elke gegevensrij met in de tabel hierboven.
    
    2.  Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .

    3.  Selecteer in het tekstvak **Kenmerkwaarde** de kenmerkwaarde weergegeven voor die rij.

    4.  Klik op **Voltooien**.

13. Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Veracode, moeten ze worden ingericht in Veracode.  
Bij Veracode, met de inrichting van is een geautomatiseerde taak.  
Er is geen actie-item voor u...
  
Gebruikers worden automatisch zo nodig gemaakt tijdens de eerste eenmalige aanmelding poging.

>[AZURE.NOTE] U kunt een andere Veracode gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Veracode aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Veracode, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Veracode **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.