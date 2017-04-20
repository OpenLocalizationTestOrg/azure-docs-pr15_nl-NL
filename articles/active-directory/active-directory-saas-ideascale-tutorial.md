<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met IdeaScale | Microsoft Azure" 
    description="Meer informatie over het gebruiken van IdeaScale met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Zelfstudie: Azure Active Directory-integratie met IdeaScale
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en IdeaScale.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een IdeaScale eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan IdeaScale één Meld u aan bij de toepassing de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruikt.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor IdeaScale inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Scenario")
##<a name="enabling-the-application-integration-for-ideascale"></a>De toepassingsintegratie van de voor IdeaScale inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor IdeaScale.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor IdeaScale door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **IdeaScale**.

    ![Galerie van toepassing] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Galerie van toepassing")

7.  Selecteer **IdeaScale**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij IdeaScale met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor IdeaScale configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **IdeaScale** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij IdeaScale** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** , in het tekstvak **IdeaScale aanmelding op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw IdeaScale-toepassing (bijvoorbeeld: "*https://company.IdeaScale.com*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij IdeaScale** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite IdeaScale als beheerder.

6.  Ga naar de **Community-instellingen**.

    ![Community-instellingen] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community-instellingen")

7.  Ga naar **beveiliging \> aanmelden instellingen één**.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Instellingen voor eenmalige aanmelding")

8.  Als het **Type eenmaal-aanmelden**, selecteer **SAML 2.0**.

    ![Type voor eenmalige aanmelding] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Type voor eenmalige aanmelding")

9.  Klik in het dialoogvenster **Instellingen voor eenmalige aanmelding** , moet u de volgende stappen uitvoeren:

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Instellingen voor eenmalige aanmelding")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij IdeaScale** de **Entiteit-ID** -waarde kopiëren en plak deze in het tekstvak **SAML IdP entiteit-ID** .
    2.  De inhoud van uw metagegevensbestand gedownloade Kopieer en plak deze in het tekstvak **SAML IdP metagegevens** .
    3.  Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **Afmelden Success URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij IdeaScale** .
    4.  Klik op **wijzigingen opslaan**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij IdeaScale, moeten ze worden ingericht in IdeaScale.  
Bij IdeaScale, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **IdeaScale** .

2.  Ga naar de **Community-instellingen**.

    ![Community-instellingen] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community-instellingen")

3.  Ga naar **basisinstellingen \> Ledenbeheer**.

4.  Klik op **lid toevoegen**.

    ![Ledenbeheer] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Ledenbeheer")

5.  Klik in de sectie Nieuw lid toevoegen de volgende stappen uitvoeren:

    ![Nieuw lid toevoegen] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Nieuw lid toevoegen")

    1.  Typ het e-mailadres van een geldige AAD-account inrichten gewenste in het tekstvak **E-mailadressen** .
    2.  Klik op **wijzigingen opslaan**.

    >[AZURE.NOTE] De eigenaar van een account Azure Active Directory krijgt een e-mailbericht met een koppeling naar het account bevestigen voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere IdeaScale gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door IdeaScale aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan IdeaScale, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **IdeaScale **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.