<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met kruipen dicht | Microsoft Azure" 
    description="Leer hoe u kruipen dicht gebruiken met Azure Active Directory om te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Zelfstudie: Azure Active Directory-integratie met kruipen dicht
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en kruipen dicht.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een kruipen dicht eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan kruipen dicht één Meld u aan bij de toepassing op uw bedrijfssite kruipen dicht (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor kruipen dicht inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Eenmalige aanmelding configureren")
##<a name="enabling-the-application-integration-for-huddle"></a>De toepassingsintegratie van de voor kruipen dicht inschakelen
  
Het doel van dit gedeelte is om een overzicht van het inschakelen van de toepassingsintegratie van de voor kruipen dicht te.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor kruipen dicht inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-huddle-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ **kruipen dicht**in het **zoekvak**.

    ![Galerie van toepassing] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Galerie van toepassing")

7.  Selecteer **kruipen dicht**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Kruipen dicht] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Kruipen dicht")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij kruipen dicht met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina toepassing integratie **kruipen dicht** op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u gebruikers aanmelden bij kruipen dicht** **Eenmalige aanmelding Microsoft Azure AD**selecteren en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Eenmalige aanmelding configureren")

3.  Typ de URL van uw kruipen dicht-tenant met behulp van de volgende patroon "*http://company.huddle.com*" op de pagina **App-URL configureren** in het tekstvak **Aanmeldingsadres kruipen dicht op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-huddle-tutorial/IC787835.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij kruipen dicht** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Eenmalige aanmelding configureren")

    1.  Klik op **certificaat downloaden**en sla het certificaatbestand op uw computer.
    2.  Kopieer de waarde van de **URL van de uitgever** , de **SAML SSO URL** -waarde en het gedownloade certificaat en stuur deze naar het ondersteuningsteam kruipen dicht.

    >[AZURE.NOTE] Eenmalige aanmelding moet worden ingeschakeld door het ondersteuningsteam kruipen dicht.
U krijgt een melding wanneer de configuratie is voltooid.

5.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij kruipen dicht, moeten ze worden ingericht in kruipen dicht.  
Bij kruipen dicht, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijf **kruipen dicht** site.

2.  Klik op de **werkruimte**.

3.  Klik op **personen \> personen uitnodigen**.

    ![Personen] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Personen")

4.  In de sectie **maken een uitnodiging voor een nieuwe** , moet u de volgende stappen uitvoeren:

    ![Uitnodiging voor de nieuwe] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Uitnodiging voor de nieuwe")

    1.  Selecteer in de lijst **Kies een team personen uitnodigen voor het deelnemen aan** **team**.
    2.  Typ het **E-mailadres** van een geldige AAD-account inrichten in de gerelateerde tekstvak gewenste.
    3.  Klik op **uitnodigen**.

    >[AZURE.NOTE] De eigenaar van een account Azure AD ontvangt een e-mailbericht met inbegrip van een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere kruipen dicht gebruiker account hulpmiddelen voor het maken of API's verstrekt door het kruipen dicht inrichten AAD gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan kruipen dicht, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina toepassing integratie **kruipen dicht **op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.