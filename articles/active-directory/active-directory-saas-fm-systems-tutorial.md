<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met FM: systemen | Microsoft Azure" 
    description="Informatie over het gebruik van FM:-systemen met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Zelfstudie: Azure Active Directory-integratie met FM: systemen
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en FM:Systems.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een FM:Systems eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan FM:Systems één Meld u aan bij de toepassing op uw bedrijfssite FM:Systems (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor FM:Systems inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Scenario")
##<a name="enabling-the-application-integration-for-fmsystems"></a>De toepassingsintegratie van de voor FM:Systems inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor FM:Systems.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor FM:Systems door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **FM:Systems**.

    ![Galerie van toepassing] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Galerie van toepassing")

7.  Selecteer **FM:Systems**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![FM: systemen] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "FM: systemen")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij FM:Systems met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **FM:Systems** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij FM:Systems** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "URL van App configureren")

    1.  Typ in het tekstvak **FM:Systems aanmelding op URL** uw FM:Systems **Antwoord URL** (bijvoorbeeld: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] U kunt deze waarde openen op uw FM: systemen ondersteuningsteam.

    2.  Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij FM:Systems** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en slaat u de metagegevens op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Eenmalige aanmelding configureren")

5.  Het gedownloade metagegevensbestand verzenden naar uw FM: systemen ondersteuningsteam.

    >[AZURE.NOTE] Uw FM: Systemen ondersteuningsteam heeft de werkelijke SSO-configuratie uitvoeren.
U krijgt een melding wanneer eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij FM:Systems, moeten ze worden ingericht in FM:Systems.  
Bij FM:Systems, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  In een webbrowservenster, meld u aan bij uw bedrijfssite FM:Systems als beheerder.

2.  Ga naar **Systeembeheer \> beveiliging beheren \> gebruikers \> gebruikerslijst**.

    ![Systeembeheer van de] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Systeembeheer van de")

3.  Klik op **nieuwe gebruiker maken**.

    ![Nieuwe gebruiker maken] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Nieuwe gebruiker maken")

4.  Klik in de sectie **Gebruiker maken** de volgende stappen uitvoeren:

    ![Gebruiker maken] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Gebruiker maken")

    1.  Typ de gebruikersnaam, het wachtwoord en de bevestiging, het e-mailadres en de werknemer-ID van een geldige Azure Active Directory-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **volgende**.

>[AZURE.NOTE] U kunt een andere FM:Systems gebruiker account hulpmiddelen voor het maken of API's verstrekt door FM:Systems aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan FM:Systems, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **FM:Systems **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.