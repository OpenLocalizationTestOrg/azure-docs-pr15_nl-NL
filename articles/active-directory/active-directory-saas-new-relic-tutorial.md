<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met nieuwe Relic | Microsoft Azure" 
    description="Meer informatie over het gebruiken van nieuwe Relic met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Zelfstudie: Azure Active Directory-integratie met nieuwe Relic
  
Het doel van deze zelfstudie is om weer te geven hoe u eenmalige aanmelding tussen Azure Active Directory en nieuwe Relic instelt.
  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een nieuwe Relic eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure Active Directory-gebruikers die u hebt toegewezen aan nieuwe Relic eenmalige aanmelding via het Configuratiescherm AAD-toegang.

1.  De toepassingsintegratie van de voor de nieuwe Relic inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Scenario")
##<a name="enabling-the-application-integration-for-new-relic"></a>De toepassingsintegratie van de voor de nieuwe Relic inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor de nieuwe Relic.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor de nieuwe Relic inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **Nieuwe Relic**.

    ![Galerie van toepassing] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Galerie van toepassing")

7.  Selecteer **Nieuwe Relic**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Nieuwe Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Nieuwe Relic")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
In dit gedeelte vindt u een overzicht van het inschakelen van gebruikers worden geverifieerd bij nieuwe Relic met hun account in Azure Active Directory, met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Nieuwe Relic** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij nieuwe Relic** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Eenmalige aanmelding configureren")

3.  Typ de URL die wordt gebruikt door uw gebruikers aanmelden bij uw nieuwe Relic-toepassing op de pagina **App-URL configureren** in het tekstvak **Nieuwe Relic aanmelding op URL** en klik vervolgens op **volgende**. 

    De app-URL is de URL van uw nieuwe Relic tenant (bijvoorbeeld: *https://rpm.newrelic.com*):

    ![URL van App configureren] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij nieuwe Relic** op **Download-certificaat**te downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Eenmalige aanmelding configureren")

5.  In een andere webbrowservenster, meldt u zich op bij uw **Nieuwe Relic** bedrijfssite als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Accountinstellingen")

7.  Klik op het tabblad **beveiliging en verificatie** en klik vervolgens op het tabblad **eenmalige aanmelding** .

    ![Eenmalige aanmelding] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Eenmalige aanmelding")

8.  Klik op de pagina SAML dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Op SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "Op SAML")

    1.  Klik op **Bestand kiezen** als u wilt uw gedownloade Azure Active Directory-certificaat uploaden.
    2.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij nieuwe Relic** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **externe aanmeldings-URL** .
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij nieuwe Relic** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL lossen afmelden** .
    4.  Klik op **wijzigingen opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers van de Azure Active Directory aan te melden bij nieuwe Relic, moeten ze worden ingericht in nieuwe Relic.  
Bij nieuwe Relic, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Als u wilt een gebruikersaccount naar nieuwe Relic inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw **Nieuwe Relic** bedrijfssite als beheerder.

2.  In het menu aan de bovenkant, klikt u op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Accountinstellingen")

3.  Klik in het deelvenster **Account** aan de linkerkant op **Overzicht**en klik vervolgens op **gebruiker toevoegen**.

    ![Accountinstellingen] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Accountinstellingen")

4.  Klik in het dialoogvenster **actieve gebruikers** de volgende stappen uitvoeren:

    ![Actieve gebruikers] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Actieve gebruikers")

    1.  Typ in het tekstvak **e-mailadres** het e-mailadres van een geldige Azure Active Directory-gebruiker die u inrichten wilt.
    2.  Selecteer de **gebruiker**als **rol** .
    3.  Klik op **deze gebruiker toevoegen**.

>[AZURE.NOTE]U kunt een andere nieuwe Relic gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door nieuwe Relic aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan nieuwe Relic, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Nieuwe Relic** toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.




