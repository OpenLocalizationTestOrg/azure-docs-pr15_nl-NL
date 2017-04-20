<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Learningpool | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Learningpool met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Zelfstudie: Azure Active Directory-integratie met Learningpool
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Learningpool.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Learningpool eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Learningpool één Meld u aan bij de toepassing op uw bedrijfssite Learningpool (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Learningpool inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Scenario")
##<a name="enabling-the-application-integration-for-learningpool"></a>De toepassingsintegratie van de voor Learningpool inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Learningpool.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Learningpool door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Learningpool**.

    ![Galerie van toepassing] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Galerie van toepassing")

7.  Selecteer **Learningpool**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Learningpool met hun account in Azure AD met Federatie op basis van het SAML-protocol.
  
Uw toepassing Learningpool verwacht de bevestigingen SAML een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Op SAML Token kenmerken] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "Op SAML Token kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina **Learningpool** toepassing integratie, in het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Kenmerken")

2.  Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

    ###  

  	|Kenmerknaam                |Kenmerkwaarde            |
  	|------------------------------|---------------------------|

     urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     urn: oid:2.5.4.42|User.givenName   
  	|urn: oid:0.9.2342.19200300.100.1.3|User.mail
  	|urn: oid:2.5.4.4|User.surname

    1.  Klik op **gebruikerskenmerk toevoegen**voor elke gegevensrij met in de tabel hierboven.
    2.  Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .
    3.  Selecteer de kenmerkwaarde weergegeven voor die rij in de lijst **Kenmerkwaarde** .
    4.  Klik op **Voltooien**.

3.  Klik op **wijzigingen toepassen**.

4.  Klik op **terug** om het dialoogvenster **Quick Start** opnieuw in uw browser.

5.  Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Singel eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Singel eenmalige aanmelding configureren")

6.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Learningpool** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Eenmalige aanmelding configureren")

7.  Typ op de pagina **App-URL configureren** , in het tekstvak **Learningpool aanmelding op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Learningpool-toepassing (bijvoorbeeld: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "URL van App configureren")

8.  Klik op de pagina **configureren eenmalige aanmelding bij Learningpool** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Eenmalige aanmelding configureren")

9.  Doorsturen naar dat bestand metagegevens naar uw ondersteuningsteam Learningpool.

    >[AZURE.NOTE]Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam Learningpool.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Learningpool, moeten ze worden ingericht in Learningpool.
  
Er is geen actie-item voor u van gebruikers aan Learningpool configureren.  
Gebruikers moeten worden gemaakt door het ondersteuningsteam van uw Learningpool.

>[AZURE.NOTE]U kunt een andere Learningpool gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Learningpool aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Learningpool, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Learningpool **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.