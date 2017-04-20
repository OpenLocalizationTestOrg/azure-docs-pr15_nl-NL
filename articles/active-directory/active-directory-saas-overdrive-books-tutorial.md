<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Overdrive boeken | Microsoft Azure" 
    description="Leer hoe u Overdrive boeken gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Zelfstudie: Azure Active Directory-integratie met Overdrive boeken
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en OverDrive.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een OverDrive eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan OverDrive één Meld u aan bij de toepassing op uw bedrijfssite OverDrive (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor OverDrive inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Scenario")
##<a name="enabling-the-application-integration-for-overdrive"></a>De toepassingsintegratie van de voor OverDrive inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor OverDrive.

###<a name="to-enable-the-application-integration-for-overdrive-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor OverDrive inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **OverDrive**.

    ![Galerie van toepassing] (./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Galerie van toepassing")

7.  Selecteer **OverDrive**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![OverDrive] (./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij OverDrive met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **OverDrive** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Eenmalige aanmelding inschakelen")

2.  Klik op de pagina **Hoe wilt u gebruikers aanmelden bij OverDrive** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://mslibrarytest.libraryreserve.com*" op de pagina **App-URL configureren** in het tekstvak **OverDrive Sign In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "URL van App configureren")

4.  Op de **configureren eenmalige aanmelding bij OverDrive** pagina downloaden van het metagegevensbestand en stuur deze naar het ondersteuningsteam OverDrive.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Eenmalige aanmelding configureren")

    >[AZURE.NOTE]Het ondersteuningsteam OverDrive eenmalige aanmelding voor u configureert en stuurt u een melding wanneer de configuratie is voltooid.

5.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Er is geen actie-item voor u voor het configureren van de inrichting van de gebruiker aan OverDrive.  
Wanneer een toegewezen gebruiker probeert aan te melden bij OverDrive bevindt, wordt automatisch een OverDrive-account gemaakt, indien nodig.

>[AZURE.NOTE]U kunt een andere OverDrive gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door OverDrive aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-overdrive-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan OverDrive, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **OverDrive **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.