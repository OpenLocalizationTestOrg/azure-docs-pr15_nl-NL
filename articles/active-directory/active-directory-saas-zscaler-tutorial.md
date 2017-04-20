<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zscaler | Microsoft Azure" 
    description="Informatie over het gebruiken van Zscaler met Azure Active Directory om te schakelen eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Zscaler. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant in Zscaler
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Zscaler inschakelen
2.  Eenmalige aanmelding configureren
3.  Proxy-instellingen configureren
4.  Configuratie van de inrichting van gebruiker
5.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scenario")

##<a name="enabling-the-application-integration-for-zscaler"></a>De toepassingsintegratie van de voor Zscaler inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Zscaler.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Zscaler door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Zscaler**.

    ![Galerie van toepassing] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Galerie van toepassing")

7.  Selecteer **Zscaler**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Zscaler met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
U moet een certificaat uploaden naar Zscaler als onderdeel van deze procedure.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Zscaler** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Eenmalige aanmelding inschakelen")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Zscaler** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Meld u aan op één configureren] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Meld u aan op één configureren")

3.  Typ uw Aanmeldingsadres in URL die u hebt ontvangen van Zscaler op de pagina **App-URL configureren** in het tekstvak **Zscaler Sign In URL** en klik vervolgens op **volgende**: 

    >[AZURE.NOTE] Neem contact op met het ondersteuningsteam Zscaler als u niet weet wat uw aanmelden URL is.

    ![URL van app configureren] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "URL van app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Zscaler** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Eenmalige aanmelding configureren")

    1.  Klik op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\Zscaler.cer**.
    2.  Kopieer de **verificatie aanvraag-URL** naar het Klembord.

5.  Meld u aan bij uw tenant Zscaler.

6.  Klik in het menu aan de bovenkant, op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Beheer")

7.  Klik onder **beheerders beheren en rollen**, klikt u op **beheren gebruikers en verificatie**.

    ![Beheerders en rollen beheren] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Beheerders en rollen beheren")

8.  In de sectie **Verificatie-optie kiezen voor uw organisatie** , moet u de volgende stappen uitvoeren:

    ![Kies verificatieopties voor] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Kies verificatieopties voor")

    1.  Selecteer **verifiëren met SAML eenmalige aanmelding**.
    2.  Klik op **één SAML aanmelding Parameters configureren**.

9.  Klik op de pagina **Configureren SAML eenmalige aanmelding Parameters** dialoogvenster de volgende stappen uitvoeren en klik vervolgens op **Gereed**:

    ![Certificaat uploaden] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Certificaat uploaden")

    1.  Plak de waarde van het veld **verificatie aanvraag-URL** van de Azure klassieke portal in het tekstvak **URL van de SAML-Portal waaraan gebruikers worden verzonden voor verificatie** .
    2.  Typ in het tekstvak **kenmerk die bevat aanmeldingsnaam** **NameID**.
    3.  Upload het certificaat dat u hebt gedownload van de Azure klassieke portal in het veld **Uploaden openbare SSL-certificaat** .
    4.  Selecteer **automatisch inrichting van SAML inschakelen**.

10. Klik op de pagina **Gebruikersverificatie configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Gebruikersverificatie configureren] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Gebruikersverificatie configureren")

    1.  Klik op **Opslaan**.
    2.  Klik op **nu activeren**.

11. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Eenmalige aanmelding configureren")

##<a name="configuring-proxy-settings"></a>Proxy-instellingen configureren

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen configureren in Internet Explorer

1.  Start **Internet Explorer**.

2.  Selecteer **Internet-opties** in het menu **Extra** in het dialoogvenster **Internetopties** openen.

    ![Internet-opties] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Internet-opties")

3.  Klik op het tabblad **verbindingen** .

    ![Verbindingen] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Verbindingen")

4.  Klik op **LAN-instellingen** om het dialoogvenster **LAN-instellingen** .

5.  In de sectie Proxy server, moet u de volgende stappen uitvoeren:

    ![Proxyserver] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxyserver")

    1.  Selecteer een proxyserver gebruiken voor uw LAN.
    2.  Typ in het tekstvak adres **gateway.zscalertwo.net**.
    3.  Typ in het tekstvak poort **80**.
    4.  Selecteer **de proxyserver niet gebruiken voor lokale adressen**.
    5.  Klik op **OK** om het dialoogvenster **LAN LAN-instellingen** te sluiten.

6.  Klik op **OK** om het dialoogvenster **Internetopties** te sluiten.

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Zscaler, moeten ze worden ingericht in Zscaler.  
Bij Zscaler, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Zscaler** .

2.  Klik op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Beheer")

3.  Klik op **Gebruikersbeheer**.

    ![Gebruikersbeheer] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Gebruikersbeheer")

4.  Klik in het tabblad **gebruikers** op **toevoegen**.

    ![Toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Toevoegen")

5.  Klik in de sectie gebruiker toevoegen de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Gebruiker toevoegen")

    1.  Typ de **gebruikersnaam**, **Weergavenaam gebruiker**, **wachtwoord**, **Bevestig het wachtwoord**en selecteer **groepen** en de **afdeling** van een geldige AAD-account inrichten gewenste.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere Zscaler gebruiker account hulpmiddel voor het maken of API's die is verstrekt door Zscaler aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Zscaler, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Zscaler** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
