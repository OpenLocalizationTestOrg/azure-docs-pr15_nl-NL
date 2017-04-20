<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cisco Webex | Microsoft Azure" 
    description="Leer hoe u Cisco Webex gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Zelfstudie: Azure Active Directory-integratie met Cisco Webex

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Cisco Webex.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Cisco Webex

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Cisco Webex één Meld u aan bij de toepassing op uw bedrijfssite Cisco Webex (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Cisco Webex inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Scenario")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>De toepassingsintegratie van de voor Cisco Webex inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Cisco Webex.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Cisco Webex inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **Zoals Cisco Webex**.

    ![Galerie van toepassing] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Galerie van toepassing")

7.  Selecteer **Cisco Webex**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Cisco Webex met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een base 64 gecodeerd certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Cisco Webex** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Cisco Webex** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** de volgende stappen uitvoeren en klik vervolgens op **volgende**.

    ![URL van app configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "URL van app configureren")

    1.  Typ in het tekstvak **Eenmalige op URL** de URL van de tenant Cisco Webex (bijvoorbeeld: *http://contoso.webex.com*).
    2.  Typ in het tekstvak **Cisco Webex antwoord URL** uw **Cisco Webex AssertionConsumerService URL** (bijvoorbeeld: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  Klik op de pagina **configureren eenmalige aanmelding bij Cisco Webex** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Cisco Webex als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Beheer van de Site**.

    ![Sitebeheer] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Sitebeheer")

7.  Klik op **SSO configuratie**in de sectie **Site beheren** .

    ![Configuratie van eenmalige aanmelding] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "Configuratie van eenmalige aanmelding")

8.  In de sectie federatieve Web SSO configuratie, kunt u de volgende stappen uitvoeren:

    ![Federatieve SSO configuratie] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Federatieve SSO configuratie")

    1.  Selecteer in de lijst **Federatie Protocol** **SAML 2.0**.
    2.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    3.  Open uw base 64 gecodeerde certificaat in Kladblok en kopieer de inhoud ervan.
    4.  Op **SAML-metagegevens importeren**en plak uw base 64 gecodeerde certificaat.
    5.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Cisco Webex** de **Uitgever URL** -waarde Kopieer en plak deze in het tekstvak **uitgever voor SAML (IdP ID)** .
    6.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Cisco Webex** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Klant SSO Service aanmeldings-URL** .
    7.  Selecteer in de lijst **NameID notatie** **e-mailadres**.
    8.  Typ in het tekstvak **AuthnContextClassRef** **Urn: oasis: namen: tc: SAML:2.0:ac:classes:Password**.
    9.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Cisco Webex** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL afmelden klant SSO-Service** .
    10. Klik op **bijwerken**.

9.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Cisco Webex** selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Cisco Webex, moeten ze worden ingericht in Cisco Webex.  
Bij Cisco Webex, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Cisco Webex** .

2.  Ga naar **gebruikers beheren \> gebruiker toevoegen**.

    ![Gebruikers toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Gebruikers toevoegen")

3.  In de sectie die gebruiker toevoegen, moet u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Gebruiker toevoegen")

    1.  Als het **Type Account**, selecteert u **Host**.
    2.  Typ de gegevens van een bestaande Azure AD-gebruiker in de volgende tekstvakken: **Voornaam, achternaam**, **gebruikersnaam in te voeren**, **E-mail**, **wachtwoord**, **Bevestig het wachtwoord**.
    3.  Klik op **toevoegen**.

>[AZURE.NOTE] U kunt een andere Cisco Webex gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Cisco Webex aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Cisco Webex, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Cisco Webex **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
