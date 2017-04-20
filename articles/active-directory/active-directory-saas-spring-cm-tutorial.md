<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Lenteachtergrond CM | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Lenteachtergrond CM met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-spring-cm"></a>Zelfstudie: Azure Active Directory-integratie met Lenteachtergrond CM
  
Het doel van deze zelfstudie is voor het instellen van eenmalige aanmelding tussen Azure Active Directory en SpringCM.
  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een SpringCM eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure Active Directory-gebruikers die u hebt toegewezen aan SpringCM eenmalige aanmelding via het Configuratiescherm AAD-toegang.

1.  De toepassingsintegratie van de voor SpringCM inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Scenario")

##<a name="enabling-the-application-integration-for-springcm"></a>De toepassingsintegratie van de voor SpringCM inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor SpringCM.

###<a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor SpringCM door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-spring-cm-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **SpringCM**.

    ![Galerie van toepassing] (./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Galerie van toepassing")

7.  Selecteer **SpringCM**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![SpringCM] (./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
In dit gedeelte vindt u een overzicht van het inschakelen van gebruikers worden geverifieerd bij SpringCM met hun account in Azure Active Directory, met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SpringCM** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SpringCM** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door uw gebruikers aanmelden bij uw SpringCM-toepassing op de pagina **App-URL configureren** in het tekstvak **SpringCM aanmelding op URL** en klik vervolgens op **volgende**. 

    De app-URL is de URL van uw SpringCM tenant (bijvoorbeeld: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):

    ![URL van App configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797049.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij SpringCM** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Eenmalige aanmelding configureren")

5.  In een andere webbrowservenster, meldt u zich op bij uw bedrijfssite **SpringCM** als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Ga naar**, klik op **Voorkeuren**en klik vervolgens in de sectie **Accountvoorkeuren** op **SAML SSO**.

    ![Op SAML eenmalige aanmelding] (./media/active-directory-saas-spring-cm-tutorial/IC797051.png "Op SAML eenmalige aanmelding")

7.  In de sectie identiteit Provider configureren, moet u de volgende stappen uitvoeren:

    ![Identiteit Provider configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Identiteit Provider configureren")

    1.  Klik op **Selecteer uitgever** of **Wijziging uitgever certificaat**om uw gedownloade Azure Active Directory-certificaat.
    2.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij SpringCM** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **uitgever** .
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij SpringCM** de waarde **Singel aanmelding de URL van de Service** kopiëren en plak deze in het tekstvak **Service Provider (SP) gestart eindpunt** .
    4.  Als het **SAML ingeschakeld**, selecteer **inschakelen**.
    5.  Klik op **Opslaan**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers van de Azure Active Directory aan te melden bij SpringCM, moeten ze worden ingericht in SpringCM.  
Bij SpringCM, met de inrichting van is een handmatige taak.

>[AZURE.NOTE] Zie voor meer informatie, [maken en bewerken van een gebruiker SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user)

###<a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Als u wilt een gebruikersaccount SpringCM inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **SpringCM** .

2.  Klik op **Ga naar**, en klik op **Adresboek**.

    ![Gebruiker maken] (./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Gebruiker maken")

3.  Klik op **gebruiker maken**.

4.  Selecteer een **gebruikersrol**.

5.  Selecteer **activering e-mailbericht verzenden**.

6.  Typ de voornaam, achternaam en e-mailadres van een geldige Azure Active Directory-gebruikersaccount inrichten in de gerelateerde tekstvakken gewenste.

7.  De gebruiker toevoegen aan een **beveiligingsgroep**.

8.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere SpringCM gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door SpringCM aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan SpringCM, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **SpringCM** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.




