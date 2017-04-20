<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zscaler ZSCloud | Microsoft Azure"
    description="Informatie over het gebruiken van Zscaler ZSCloud met Azure Active Directory om te schakelen eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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


#<a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Zelfstudie: Azure Active Directory-integratie met Zscaler ZSCloud
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en ZScaler ZSCloud.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een ZScaler ZSCloud eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan ZScaler ZSCloud één Meld u aan bij de toepassing op uw bedrijfssite ZScaler ZSCloud (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md) gebruiken
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor ZScaler ZSCloud inschakelen
2.  Eenmalige aanmelding configureren
3.  Proxy-instellingen configureren
4.  Configuratie van de inrichting van gebruiker
5.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800275.png "Scenario")

##<a name="enabling-the-application-integration-for-zscaler-zscloud"></a>De toepassingsintegratie van de voor ZScaler ZSCloud inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor ZScaler ZSCloud.

###<a name="to-enable-the-application-integration-for-zscaler-zscloud-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor ZScaler ZSCloud inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **ZScaler ZSCloud**.

    ![Galerie van toepassing] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800276.png "Galerie van toepassing")

7.  Selecteer **ZScaler ZSCloud**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![ZScaler ZSCloud] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800277.png "ZScaler ZSCloud")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij ZScaler ZSCloud met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u vereist een base 64 gecodeerde certificaat uploaden naar uw tenant ZScaler ZSCloud.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **ZScaler ZSCloud** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800278.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ZScaler ZSCloud** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800279.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw toepassing ZScaler ZSCloud op de pagina **App-URL configureren** in het tekstvak **ZScaler ZSCloud aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800280.png "URL van App configureren")

    >[AZURE.NOTE] U kunt de werkelijke waarde krijgen voor uw omgeving van uw ondersteuningsteam ZScaler ZSCloud als u deze nodig hebt.

4.  Klik op de pagina **configureren eenmalige aanmelding bij ZScaler ZSCloud** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800281.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite ZScaler ZSCloud als beheerder.

6.  Klik in het menu aan de bovenkant, op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800206.png "Beheer")

7.  Klik onder **beheerders beheren en rollen**, klikt u op **gebruikers beheren en verificatie**.

    ![Gebruikers en verificatie beheren] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800207.png "Gebruikers en verificatie beheren")

8.  In de sectie **Opties voor verificatie kiezen voor uw organisatie** , moet u de volgende stappen uitvoeren:

    ![Verificatie] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800208.png "Verificatie")

    1.  Selecteer **verifiëren met eenmalige aanmelding SAML**.
    2.  Klik op **één SAML aanmelding Parameters configureren**.

9.  Klik op de pagina **Configureren SAML eenmalige aanmelding Parameters** dialoogvenster de volgende stappen uitvoeren en klik vervolgens op **Gereed**:

    ![Eenmalige aanmelding] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800209.png "Eenmalige aanmelding")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij ZScaler ZSCloud** de waarde **Verificatie aanvragen URL** kopiëren en plak deze in het tekstvak **URL van de SAML-Portal waaraan gebruikers worden verzonden voor verificatie** .
    2.  Typ in het tekstvak **kenmerk die bevat aanmeldingsnaam** **NameID**.
    3.  Als u wilt uw gedownloade certificaat uploaden, klikt u op **Zscaler pem**.
    4.  Selecteer **automatisch inrichting van SAML inschakelen**.

10. Klik op de pagina **Gebruikersverificatie configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Beheer] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800210.png "Beheer")

    1.  Klik op **Opslaan**.
    2.  Klik op **nu activeren**.

11. In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij ZScaler ZSCloud** selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800282.png "Eenmalige aanmelding configureren")

##<a name="configuring-proxy-settings"></a>Proxy-instellingen configureren

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>De proxy-instellingen configureren in Internet Explorer

1.  Start **Internet Explorer**.

2.  Selecteer **Internet-opties** in het menu **Extra** in het dialoogvenster **Internetopties** openen.

    ![Internet-opties] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC769492.png "Internet-opties")

3.  Klik op het tabblad **verbindingen** .

    ![Verbindingen] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC769493.png "Verbindingen")

4.  Klik op **LAN-instellingen** om het dialoogvenster **LAN-instellingen** .

5.  In de sectie Proxy server, moet u de volgende stappen uitvoeren:

    ![Proxyserver] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC769494.png "Proxyserver")

    1.  Selecteer een proxyserver gebruiken voor uw LAN.
    2.  Typ in het tekstvak adres **gateway.zscalerone.net**.
    3.  Typ in het tekstvak poort **80**.
    4.  Selecteer **de proxyserver niet gebruiken voor lokale adressen**.
    5.  Klik op **OK** om het dialoogvenster **LAN LAN-instellingen** te sluiten.

6.  Klik op **OK** om het dialoogvenster **Internetopties** te sluiten.

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij ZScaler ZSCloud, moeten ze worden ingericht naar ZScaler ZSCloud.  
Bij ZScaler ZSCloud, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Zscaler** .

2.  Klik op **beheer**.

    ![Beheer] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781035.png "Beheer")

3.  Klik op **Gebruikersbeheer**.

    ![Toevoegen] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781037.png "Toevoegen")

4.  Klik in het tabblad **gebruikers** op **toevoegen**.

    ![Toevoegen] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781037.png "Toevoegen")

5.  Klik in de sectie gebruiker toevoegen de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC781038.png "Gebruiker toevoegen")

    1.  Typ de **gebruikersnaam**, **Weergavenaam gebruiker**, **wachtwoord**, **Bevestig het wachtwoord**en selecteer **groepen** en de **afdeling** van een geldige AAD-account inrichten gewenste.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere ZScaler ZSCloud gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door ZScaler ZSCloud aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-zscaler-zscloud-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan ZScaler ZSCloud, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **ZScaler ZSCloud** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC800283.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zscaler-zscloud-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.