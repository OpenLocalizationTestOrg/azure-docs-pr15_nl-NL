<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met PolicyStat | Microsoft Azure" 
    description="Meer informatie over het gebruiken van PolicyStat met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Zelfstudie: Azure Active Directory-integratie met PolicyStat
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en PolicyStat.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant PolicyStat
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan PolicyStat één Meld u aan bij de toepassing op uw bedrijfssite PolicyStat (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor PolicyStat inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenario")
##<a name="enabling-the-application-integration-for-policystat"></a>De toepassingsintegratie van de voor PolicyStat inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor PolicyStat.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor PolicyStat door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-policystat-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **PolicyStat**.

    ![Galerie van toepassing] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Galerie van toepassing")

7.  Selecteer **PolicyStat**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij PolicyStat met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Uw toepassing PolicyStat verwacht de bevestigingen SAML een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Kenmerken] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **PolicyStat** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij PolicyStat** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-instellingen configureren** in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw toepassing URL PolicyStat (bijvoorbeeld: *"https://demo-azure.policystat.com"*), en klik op **volgende**.

    ![App-instellingen configureren] (./media/active-directory-saas-policystat-tutorial/IC808631.png "App-instellingen configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij PolicyStat** op **metagegevens downloaden**en sla het metagegevensbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite PolicyStat als beheerder.

6.  Klik op de tab **beheerder** en klik op **Configuratie voor eenmalige aanmelding** in het linkernavigatiedeelvenster.

    ![Menu beheerder] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Menu beheerder")

7.  Selecteer in de sectie **Setup** **één inschakelen integratie voor eenmalige aanmelding**.

    ![Configuratie voor eenmalige aanmelding] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Configuratie voor eenmalige aanmelding")

8.  **Kenmerken configureren**op en klik vervolgens in de sectie **Kenmerken configureren** de volgende stappen uitvoeren:

    ![Configuratie voor eenmalige aanmelding] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Configuratie voor eenmalige aanmelding")

    1.  Typ in het tekstvak **Gebruikersnaam kenmerk** **uid**.
    2.  Typ in het tekstvak **Voornaam kenmerk** **Voornaam**.
    3.  Typ in het tekstvak **Laatste naamkenmerk** **Achternaam**.
    4.  Typ in het tekstvak **E-kenmerk** **emailaddress**.
    5.  Klik op **wijzigingen opslaan**.

9.  **Uw IDP metagegevens**op en klik vervolgens in de sectie **Uw IDP metagegevens** de volgende stappen uitvoeren:

    ![Configuratie voor eenmalige aanmelding] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Configuratie voor eenmalige aanmelding")

    1.  Opent u het gedownloade metagegevensbestand, de inhoud kopiëren en plak deze in het tekstvak **Uw identiteit Provider metagegevens**
    2.  Klik op **wijzigingen opslaan**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Eenmalige aanmelding configureren")

11. 12. In het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Kenmerken")

13. Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

    ![Kenmerken] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Kenmerken")

    1.  Klik op **gebruikerskenmerk toevoegen**.
    2.  Typ in het tekstvak **Kenmerk** **uid**.
    3.  Selecteer in het tekstvak **Kenmerkwaarde** **ExtractMailPrefix()**.
    4.  Selecteer in de lijst **E-mail** **User.mail**.
    5.  Klik op **Voltooien**.
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij PolicyStat, moeten ze worden ingericht in PolicyStat.  
PolicyStat ondersteunt alleen in de inrichting van tijd-gebruiker. Dit betekent u hoeft niet de gebruikers handmatig toevoegen aan PolicyStat.  
De gebruikers wordt automatisch ophalen toegevoegd aan hun eerste aanmelding via eenmalige op.

>[AZURE.NOTE]U kunt een andere PolicyStat gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door PolicyStat aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan PolicyStat, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **PolicyStat **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.