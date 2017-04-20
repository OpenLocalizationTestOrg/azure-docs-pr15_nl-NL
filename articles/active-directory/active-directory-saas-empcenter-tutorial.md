<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met EmpCenter | Microsoft Azure" 
    description="Meer informatie over het gebruiken van EmpCenter met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Zelfstudie: Azure Active Directory-integratie met EmpCenter
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en EmpCenter.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een EmpCenter eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan EmpCenter één Meld u aan bij de toepassing op uw bedrijfssite EmpCenter (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor EmpCenter inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")
##<a name="enabling-the-application-integration-for-empcenter"></a>De toepassingsintegratie van de voor EmpCenter inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor EmpCenter.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor EmpCenter door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **EmpCenter**.

    ![Galerie van toepassing] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Galerie van toepassing")

7.  Selecteer **EmpCenter**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij EmpCenter met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **EmpCenter** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij EmpCenter** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-instellingen configureren** de volgende stappen uitvoeren:

    ![App-instellingen configureren] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "App-instellingen configureren")

    1.  Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw toepassing EmpCenter (bijvoorbeeld: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij EmpCenter** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en sla het metagegevensbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Eenmalige aanmelding configureren")

5.  De gedownloade metagegevensbestand verzenden naar uw ondersteuningsteam EmpCenter.

    >[AZURE.NOTE] Uw ondersteuningsteam EmpCenter heeft de werkelijke SSO-configuratie uitvoeren.
U krijgt een melding wanneer eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Om te kunnen Azure AD-gebruikers aan te melden bij EmpCenter inschakelt, moeten ze worden ingericht in EmpCenter.  
EmpCenter moeten de gebruikersaccounts worden gemaakt door het ondersteuningsteam van uw EmpCenter.

>[AZURE.NOTE] U kunt een andere EmpCenter gebruiker account hulpmiddelen voor het maken of API's verstrekt door EmpCenter inrichten Azure Active Directory gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Als u wilt gebruikers aan EmpCenter toewijst, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **EmpCenter **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.