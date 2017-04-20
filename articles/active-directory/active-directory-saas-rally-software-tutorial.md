<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met bijeenkomst Software | Microsoft Azure" 
    description="Leer hoe u Rally Software gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Zelfstudie: Azure Active Directory-integratie met bijeenkomst Software
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Rally Software.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Rally Software
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Software Rally inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Scenario")
##<a name="enabling-the-application-integration-for-rally-software"></a>De toepassingsintegratie van de voor Software Rally inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Rally Software.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Software Rally inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **vak Zoeken in** **bijeenkomst**.

    ![Galerie van toepassing] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Galerie van toepassing")

7.  Selecteer **Rally Software**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Rally Software] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally Software")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Rally Software met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure moet u een certificaat uploaden naar Rally Software.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **Software Rally **-toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u gebruikers aanmelden bij bijeenkomst** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Microsoft Azure AD voor eenmalige aanmelding] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD voor eenmalige aanmelding")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Rally Software Tenant URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. rally.com*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij bijeenkomst** op metagegevens downloaden en sla deze op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Eenmalige aanmelding configureren")

5.  Meld u aan bij uw **Software Rally** -tenant.

6.  In de werkbalk op de bovenkant, klik op **Instellingen**en selecteer **abonnement**.

    ![Abonnement] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Abonnement")

7.  Klik op **de actieknop In de werkbalk op boven aan de rechterkant** en selecteer vervolgens **Bewerken abonnement**.

8.  De volgende stappen uitvoeren op de pagina van het dialoogvenster **abonnement** en klik vervolgens op **Opslaan en sluiten**:

    ![Verificatie] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Verificatie")

    1.  Selecteer **bijeenkomst of SSO verificatie** in de vervolgkeuzelijst verificatie
    2.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij de Software Rally** de **Identiteit Provider-ID** -waarde kopiëren en plak deze in het tekstvak **Identiteit Provider URL**
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij de Software Rally** door de waarde van de **URL van externe Meld u af** te kopiëren.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Voor AAD-gebruikers moeten kunnen aanmelden, moeten ze zijn geconfigureerd met de Software Rally-toepassing met hun naam Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw Software Rally-tenant.

2.  Ga naar **Setup \> gebruikers**, en klik vervolgens op **+ Add New**.

    ![Gebruikers] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Gebruikers")

3.  Typ de naam in het tekstvak nieuwe gebruiker en klik vervolgens op **toevoegen met Details**.

4.  Klik in de sectie **Gebruiker maken** de volgende stappen uitvoeren:

    ![Gebruiker maken] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Gebruiker maken")

    1.  Typ de naam van de gewenste inrichten Azure AD-gebruiker in het tekstvak **Gebruikersnaam in te voeren** .
    2.  Typ in het tekstvak **E-mailadres** het e-mailadres van de Azure AD-gebruiker die u inrichten wilt.
    3.  Klik op **Opslaan en sluiten**.

>[AZURE.NOTE]U kunt een andere Software Rally gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Software Rally aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers naar Rally Software, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina van de integratie in de **Software Rally** -toepassing, op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.




