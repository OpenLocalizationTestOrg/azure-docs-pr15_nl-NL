<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met AppDynamics | Microsoft Azure" 
    description="Meer informatie over het gebruiken van AppDynamics met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Zelfstudie: Azure Active Directory-integratie met AppDynamics

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en AppDynamics. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een AppDynamics eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan AppDynamics één Meld u aan bij de toepassing op uw bedrijfssite AppDynamics (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor AppDynamics inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Scenario")
##<a name="enabling-the-application-integration-for-appdynamics"></a>De toepassingsintegratie van de voor AppDynamics inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor AppDynamics.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor AppDynamics door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **AppDynamics**.

    ![Galerie van toepassing] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Galerie van toepassing")

7.  Selecteer **AppDynamics**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij AppDynamics met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **AppDynamics** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij AppDynamics** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door uw gebruikers kunnen aanmelding bij AppDynamics ("*https://companyname.saas.appdynamics.com*") op de pagina **App-URL configureren** in het tekstvak **AppDynamics aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij AppDynamics** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite AppDynamics als beheerder.

6.  Op de werkbalk op de voorgrond, klikt u op **Instellingen**en klik vervolgens op **beheer**.

    ![Beheer] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Beheer")

7.  Klik op het tabblad **Verificatie-Provider** .

    ![Verificatieprovider] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Verificatieprovider")

8.  In de sectie **Verificatie-Provider** , moet u de volgende stappen uitvoeren:

    ![Op SAML-configuratie] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "Op SAML-configuratie")

    1.  Als **Verificatie-Provider**, selecteer **SAML**.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij AppDynamics** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij AppDynamics** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL Meld u af** .
    4.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    5.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **certificaat**
    6.  Klik op **Opslaan**.
        ![Opslaan] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Opslaan")

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij AppDynamics, moeten ze worden ingericht in AppDynamics.  
Bij AppDynamics, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw bedrijfssite AppDynamics als beheerder.

2.  Ga naar **gebruikers**en klik vervolgens op **+** om het dialoogvenster **Gebruiker maken** te openen.

    ![Gebruikers] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Gebruikers")

3.  Klik in de sectie **Gebruiker maken** de volgende stappen uitvoeren:

    ![Gebruiker maken] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Gebruiker maken")

    1.  Typ de **gebruikersnaam**, **naam**, **e**, **Een nieuw wachtwoord**, **Nieuw wachtwoord herhalen** van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere AppDynamics gebruiker account hulpmiddelen voor het maken of API's verstrekt door AppDynamics voor het inrichten van Azure AD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan AppDynamics, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **AppDynamics **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
