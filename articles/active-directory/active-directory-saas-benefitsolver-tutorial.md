<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Benefitsolver | Microsoft Azure"
    description="Meer informatie over het gebruiken van Benefitsolver met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Zelfstudie: Azure Active Directory-integratie met Benefitsolver

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Benefitsolver.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Benefitsolver eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Benefitsolver één Meld u aan bij de toepassing de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruikt.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Benefitsolver inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>De toepassingsintegratie van de voor Benefitsolver inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Benefitsolver.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Benefitsolver door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Benefitsolver**.

    ![Galerie van toepassing] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Galerie van toepassing")

7.  Selecteer **Benefitsolver**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Benefitsolver met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Uw toepassing Benefitsolver verwacht de bevestigingen SAML een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Kenmerken] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Benefitsolver** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Benefitsolver** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-instellingen configureren** de volgende stappen uitvoeren:

    ![App-instellingen configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "App-instellingen configureren")

    1.  Typ in het tekstvak **Aanmeldingsadres op URL** **http://azure.benefitsolver.com**.
    2.  Typ in het tekstvak **URL antwoord** **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  


    3.  Klik op **volgende**.

4.  Klik op de pagina **configureren eenmalige aanmelding bij Benefitsolver** op **metagegevens downloaden**als u wilt downloaden van de metagegevens van uw, en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Eenmalige aanmelding configureren")

5.  De gedownloade metagegevensbestand verzenden naar uw ondersteuningsteam Benefitsolver.

    >[AZURE.NOTE] Uw ondersteuningsteam Benefitsolver heeft de werkelijke SSO-configuratie uitvoeren.
U krijgt een melding wanneer eenmalige aanmelding is ingeschakeld voor uw abonnement.

6.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Eenmalige aanmelding configureren")

7.  In het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Kenmerken")

8.  Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

    ![Kenmerken] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Kenmerken")

  	|Kenmerknaam|Kenmerkwaarde|
  	|---|---|
  	|ClientID|Moet u deze waarde ophalen uit uw ondersteuningsteam Benefitsolver.|
  	|ClientKey|Moet u deze waarde ophalen uit uw ondersteuningsteam Benefitsolver.|
  	|LogoutURL|Moet u deze waarde ophalen uit uw ondersteuningsteam Benefitsolver.|
  	|Werknemer-id|Moet u deze waarde ophalen uit uw ondersteuningsteam Benefitsolver.|

    1.  Klik op **gebruikerskenmerk toevoegen**voor elke gegevensrij met in de tabel hierboven.
    2.  Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .
    3.  Selecteer in het tekstvak **Kenmerkwaarde** de kenmerkwaarde weergegeven voor die rij.
    4.  Klik op **Voltooien**.

9.  Klik op **wijzigingen toepassen**.

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Benefitsolver, moeten ze worden ingericht in Benefitsolver.  
Benefitsolver is werknemersgegevens in uw toepassing via een telling bestand vanaf uw HRIS-systeem (meestal elke nacht) zijn ingevuld.  

>[AZURE.NOTE] U kunt een andere Benefitsolver gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Benefitsolver aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Benefitsolver, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Benefitsolver **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
