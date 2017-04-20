<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cherwell | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Cherwell met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Zelfstudie: Azure Active Directory-integratie met Cherwell

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Cherwell. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Cherwell eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Cherwell één Meld u aan bij de toepassing op uw bedrijfssite Cherwell (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Cherwell inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Scenario")
##<a name="enabling-the-application-integration-for-cherwell"></a>De toepassingsintegratie van de voor Cherwell inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Cherwell.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Cherwell door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  Selecteer **Cherwell**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Cherwell met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Cherwell** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Cherwell** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "URL van App configureren")

    een.  Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers u zich aanmeldt bij uw **Cherwell** (bijvoorbeeld: *https://\<bedrijfsnaam\>.cherwellondemand.com/cherwellclient*).

    b.  Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij Cherwell** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Eenmalige aanmelding configureren")

    een.  Klik op **certificaat downloaden**en sla het certificaat lokaal op uw computer.

    b.  Kopieer de **URL van de Provider identiteit**.

    c.  Kopieer de **URL van de Service voor eenmalige aanmelding**.

    d.  Klik op **volgende**.

5.  Het gedownloade certificaat voor de **Identiteit Provider URL** en de **Eenmalige aanmelding Service URL** naar uw ondersteuningsteam Cherwell verzenden.

    >[AZURE.NOTE] Uw ondersteuningsteam Cherwell heeft de werkelijke SSO-configuratie uitvoeren.
U krijgt een melding wanneer eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Cherwell, moeten ze worden ingericht in Cherwell.  
Cherwell moeten de gebruikersaccounts worden gemaakt door het ondersteuningsteam van uw Cherwell.

>[AZURE.NOTE] U kunt een andere Cherwell gebruiker account hulpmiddelen voor het maken of API's verstrekt door Cherwell inrichten Azure Active Directory gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Als u wilt gebruikers aan Cherwell toewijst, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Cherwell** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
