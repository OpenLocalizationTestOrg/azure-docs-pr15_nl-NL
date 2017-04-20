<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TeamSeer | Microsoft Azure" 
    description="Meer informatie over het gebruiken van TeamSeer met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Zelfstudie: Azure Active Directory-integratie met TeamSeer
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en TeamSeer.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant TeamSeer
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan TeamSeer één Meld u aan bij de toepassing op uw bedrijfssite TeamSeer (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor TeamSeer inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-teamseer-tutorial/IC789618.png "Scenario")

##<a name="enabling-the-application-integration-for-teamseer"></a>De toepassingsintegratie van de voor TeamSeer inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor TeamSeer.

###<a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor TeamSeer door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-teamseer-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-teamseer-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-teamseer-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-teamseer-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **TeamSeer**.

    ![Galerie van toepassing] (./media/active-directory-saas-teamseer-tutorial/IC789619.png "Galerie van toepassing")

7.  Selecteer **TeamSeer**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![TeamSeer] (./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij TeamSeer met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **TeamSeer** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789621.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij TeamSeer** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789628.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://www.teamseer.com/companyid*" op de pagina **App-URL configureren** in het tekstvak **TeamSeer Sign In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-teamseer-tutorial/IC789629.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij TeamSeer** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789630.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite TeamSeer als beheerder.

6.  Ga naar **HR beheerder**.

    ![HR-beheerder] (./media/active-directory-saas-teamseer-tutorial/IC789634.png "HR-beheerder")

7.  Klik op **Instellingen**.

    ![Setup] (./media/active-directory-saas-teamseer-tutorial/IC789635.png "Setup")

8.  Klik op **SAML-provider details instellen**.

    ![Op SAML-instellingen] (./media/active-directory-saas-teamseer-tutorial/IC789636.png "Op SAML-instellingen")

9.  In de sectie van de details in de SAML-provider, moet u de volgende stappen uitvoeren:

    ![Op SAML-instellingen] (./media/active-directory-saas-teamseer-tutorial/IC789637.png "Op SAML-instellingen")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij TeamSeer** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **URL** .
    2.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    3.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **IdP openbare certificaat** .

10. U voltooit de configuratie van SAML-provider, moet u de volgende stappen uitvoeren:

    ![Op SAML-instellingen] (./media/active-directory-saas-teamseer-tutorial/IC789638.png "Op SAML-instellingen")

    1.  Typ in het **E-mailadressen testen**, e-mailadres van de testgebruiker.
    2.  Typ de URL van de uitgever van de serviceprovider in het tekstvak **uitgever** .
    3.  Klik op **Opslaan**.

11. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-teamseer-tutorial/IC789639.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij TeamSeer, moeten ze worden ingericht in ShiftPlanning.  
Bij TeamSeer, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **TeamSeer** .

2.  De volgende stappen uitvoeren:

    ![HR-beheerder] (./media/active-directory-saas-teamseer-tutorial/IC789640.png "HR-beheerder")

    1.  Ga naar **HR beheerder \> gebruikers**.
    2.  Klik op **uitvoeren van de wizard nieuwe gebruiker**.

3.  Klik in de sectie **Gebruikersgegevens** de volgende stappen uitvoeren:

    ![Gebruikersgegevens] (./media/active-directory-saas-teamseer-tutorial/IC789641.png "Gebruikersgegevens")

    1.  Typ de **Voornaam**, **Achternaam**, **gebruikersnaam in te voeren (e-mailadres)** van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **volgende**.

4.  Volg de instructies op het scherm voor het toevoegen van een nieuwe gebruiker en klik op **Voltooien**.

>[AZURE.NOTE] U kunt een andere TeamSeer gebruiker account hulpmiddelen voor het maken of API's verstrekt door TeamSeer voor het inrichten van Azure AD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan TeamSeer, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **TeamSeer **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-teamseer-tutorial/IC789642.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-teamseer-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.