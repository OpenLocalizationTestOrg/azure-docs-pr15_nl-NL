<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Igloo Software | Microsoft Azure" 
    description="Leer hoe u Igloo Software gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Zelfstudie: Azure Active Directory-integratie met Igloo-Software
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Igloo Software.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een [Igloo Software](http://www.igloosoftware.com/) -eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Igloo Software één Meld u aan bij de toepassing op uw bedrijfssite Igloo Software (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Igloo Software inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scenario")
##<a name="enabling-the-application-integration-for-igloo-software"></a>De toepassingsintegratie van de voor Igloo Software inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Igloo Software.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Igloo Software inschakelen, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Igloo Software**.

    ![Galerie van toepassing] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Galerie van toepassing")

7.  Selecteer **Igloo Software**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Igloo Software met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw tenant centraal bureaublad.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **Igloo Software** -toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de Software Igloo** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Microsoft Azure AD voor eenmalige aanmelding] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD voor eenmalige aanmelding")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.igloocommunities.com/?signin*" op de pagina **App-URL configureren** in het tekstvak **Igloo Software Sign In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Igloo Software** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Igloo Software als beheerder.

6.  Ga naar het **Configuratiescherm**.

    ![Het Configuratiescherm] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Het Configuratiescherm")

7.  Klik op **Sign In instellingen**op het tabblad **lidmaatschap** .

    ![Meld u aan bij instellingen] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Meld u aan bij instellingen")

8.  Klik op **SAML-verificatie configureren**in de sectie SAML configuratie.

    ![Op SAML-configuratie] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "Op SAML-configuratie")

9.  In de sectie **Algemene configuratie van** de volgende stappen uitvoeren:

    ![Configuratie van algemene] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Configuratie van algemene")

    1.  Typ in het tekstvak **Verbindingsnaam** een aangepaste naam voor de configuratie.
    2.  In de klassieke Azure portal, klik op de dialoog **configureren eenmalige aanmelding bij Igloo Software** pagina de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **IdP aanmeldings-URL** .
    3.  Kopieer de **URL van externe afmelden** -waarde in de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij Igloo Software** en plak deze in het tekstvak **IdP afmelden URL** .
    4.  **Meld u af antwoord en Type HTTP-aanvragen**, selecteert u **bericht**.
    5.  Een tekstbestand maken van het gedownloade certificaat.
        
        >[AZURE.TIP]Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  De eerste regel en de laatste regel verwijderen uit de tekstversie van het certificaat, de resterende certificaat tekst kopiëren en plak deze in het tekstvak **Openbare certificaat** .

10. Klik in de **reactie en verificatie-configuratie**, kunt u de volgende stappen uitvoeren:

    ![Antwoord en verificatie-configuratie] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Antwoord en verificatie-configuratie")

    1.  Selecteer **Microsoft ADFS**als **Identiteitsprovider**.
    2.  Als het **Type id**, selecteert u **E-mailadres**.
    3.  Typ in het tekstvak **E-kenmerk** **emailaddress**.
    4.  Typ in het tekstvak **Voornaam kenmerk** **givenname**.
    5.  Typ in het tekstvak **Laatste naamkenmerk** **Achternaam**.

11. De volgende stappen uit om de configuratie te voltooien uitvoeren:

    ![Het maken van de gebruiker op aanmelden] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Het maken van de gebruiker op aanmelden")

    1.  Als **het maken van de gebruiker op aanmelden**, selecteert u **een nieuwe gebruiker in uw site wanneer ze zich aanmeldt**.
    2.  **Als u **Instellingen aanmelden**, gebruik SAML knop**selecteren op scherm "Aanmelden".
    3.  Klik op **Opslaan**.

12. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Er is geen actie-item voor u voor het configureren van de inrichting van de gebruiker naar Igloo Software.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Igloo Software via het Configuratiescherm van access, Igloo Software Hiermee wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Igloo Software.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers naar Igloo Software, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina van de integratie in de **Igloo Software **-toepassing, op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.