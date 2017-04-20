<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Zoho E-mail | Microsoft Azure" 
    description="Leer hoe u Zoho Mail met Azure Active Directory gebruiken om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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
    ms.date="09/09/2016" 
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Zelfstudie: Azure Active Directory-integratie met Zoho E-mail
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Zoho Mail.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Zoho Mail
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Zoho E-mail één Meld u aan bij de toepassing op uw bedrijfssite Zoho Mail (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Zoho E-mail inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenario")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>De toepassingsintegratie van de voor Zoho E-mail inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Zoho Mail.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Zoho E-mail door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Klik in het **zoekvak**de tekst **Zoho E-mail**te typen.

    ![Galerie van toepassing] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Galerie van toepassing")

7.  Selecteer **Zoho E-mail**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Zoho Mail] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Zoho E-mail met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Zoho Mail** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Zoho Mail** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "URL van App configureren")

    een. Typ in het tekstvak **Zoho Mail aanmelden op URL** de URL voor het gebruik van de volgende patroon:`http://<company name>.ZohoMail.com`

    b. Klik op **volgende**.


4.  Klik op de pagina **configureren eenmalige aanmelding bij Zoho E-mail** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Eenmalige aanmelding configureren")

5.  In een andere webbrowservenster, meld u aan bij uw bedrijfssite Zoho E-mail als een beheerder.

6.  Ga naar het **Configuratiescherm**.

    ![Het Configuratiescherm] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Het Configuratiescherm")

7.  Klik op het tabblad **SAML-verificatie** .

    ![Op SAML-verificatie] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "Op SAML-verificatie")

8.  In de sectie **Details van SAML-verificatie** , moet u de volgende stappen uitvoeren:

    ![Op SAML verificatie Details] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "Op SAML verificatie Details")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Zoho E-mail** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    2.  Kopieer de **URL van externe afmelden** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Zoho E-mail** en plak deze in het tekstvak **URL Meld u af** .
    3.  Kopieer de waarde van de **URL van de wachtwoord wijzigen** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Zoho E-mail** en plak deze in het tekstvak **URL van de wachtwoord wijzigen** .
    4.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    5.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **PublicKey** .
    6.  Als **algoritme**, selecteer **RSA**.
    7.  Klik op **OK**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Zoho Mail, moeten ze ingericht in Zoho Mail.  
Bij Zoho Mail, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Zoho Mail** .

2.  Ga naar **Configuratiescherm \> Mail- en -documenten**.

3.  Ga naar **Gebruikersgegevens \> gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Gebruiker toevoegen")

4.  Klik in het dialoogvenster **gebruikers toevoegen** , moet u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Gebruiker toevoegen")

    1.  Typ de **Voornaam**, **Achternaam**, **E-ID**en **wachtwoord** van een geldige Azure Active Directory-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **OK**.  

        >[AZURE.NOTE] De eigenaar van een account Azure Active Directory, ontvangt een e-mail met een koppeling naar het account bevestigen voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Zoho E-mail gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Zoho E-mail naar inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers naar Zoho Mail, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Zoho Mail **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.