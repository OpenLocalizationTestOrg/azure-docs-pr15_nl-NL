<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Clever | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Clever met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Zelfstudie: Azure Active Directory-integratie met Clever

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Clever. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een slimme tenant

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Clever één Meld u aan bij de toepassing op uw bedrijfssite slimme (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Clever inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-clever-tutorial/IC798977.png "Scenario")
##<a name="enabling-the-application-integration-for-clever"></a>De toepassingsintegratie van de voor Clever inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Clever.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Clever door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-clever-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-clever-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-clever-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Clever**.

    ![Galerie van toepassing] (./media/active-directory-saas-clever-tutorial/IC798978.png "Galerie van toepassing")

7.  Selecteer **Clever**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Slimme] (./media/active-directory-saas-clever-tutorial/IC798979.png "Slimme")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Clever met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Uw slimme toepassing verwacht de bevestigingen SAML een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Kenmerken] (./media/active-directory-saas-clever-tutorial/IC798980.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Clever** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC784682.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Clever** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC798981.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Slimme Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw slimme toepassing (bijvoorbeeld: *https://clever.com/in/azsandbox*), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-clever-tutorial/IC798982.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Clever** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC798983.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw slimme bedrijfssite als beheerder.

6.  Klik in de werkbalk op **Direct Login**.

    ![Meld u direct] (./media/active-directory-saas-clever-tutorial/IC798984.png "Meld u direct")

7.  Klik op de pagina **Login direct** moet u de volgende stappen uitvoeren:

    ![Meld u direct] (./media/active-directory-saas-clever-tutorial/IC798985.png "Meld u direct")

    1.  Typ de **aanmeldings-URL**.  

        >[AZURE.NOTE] De **Aanmeldings-URL** is een aangepaste waarde.
U kunt de werkelijke waarde krijgen van het slimme ondersteuningsteam.

    2.  Als **Systeem identiteit**, selecteer **ADFS**.
    3.  Klik op **Opslaan**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clever-tutorial/IC798986.png "Eenmalige aanmelding configureren")

9.  In het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-clever-tutorial/IC795920.png "Kenmerken")

10. Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

    ![op SAML token kenmerken] (./media/active-directory-saas-clever-tutorial/IC795921.png "op SAML token kenmerken")

  	|Kenmerknaam|Kenmerkwaarde|
  	|---|---|
  	|clever.student.credentials.district\_gebruikersnaam|User.userPrincipalName|

    1.  Klik op **gebruikerskenmerk toevoegen**voor elke gegevensrij met in de tabel hierboven.
    2.  Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .
    3.  Selecteer in het tekstvak **Kenmerkwaarde** de kenmerkwaarde weergegeven voor die rij.
    4.  Klik op **Voltooien**.

11. Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Clever, moeten ze worden ingericht in Clever.  
Bij Clever is het een handmatige taak dat moet worden uitgevoerd door het ondersteuningsteam van uw slimme inrichten.

>[AZURE.NOTE] U kunt een andere gebruiker slimme account hulpmiddelen voor het maken of API's die is verstrekt door Clever aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Clever, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Clever **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-clever-tutorial/IC798987.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-clever-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
