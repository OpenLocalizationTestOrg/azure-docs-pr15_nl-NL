<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met TOPdesk - openbare | Microsoft Azure" 
    description="Meer informatie over het gebruiken van TOPdesk - openbare met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Zelfstudie: Azure-Directory-integratie met TOPdesk - openbare

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en TOPdesk - openbare.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Per TOPdesk - openbare eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, de Azure AD-gebruikers die u hebt toegewezen aan TOPdesk - openbare kunnen één Meld u aan bij de toepassing op uw TOPdesk - openbare bedrijfssite (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor TOPdesk - openbare inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Scenario")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>De toepassingsintegratie van de voor TOPdesk - openbare inschakelen
  
Het doel van dit gedeelte is om een overzicht van het inschakelen van de toepassingsintegratie van de voor TOPdesk - openbare te.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>Om in te schakelen van de toepassingsintegratie van de voor TOPdesk - openbare, de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **TOPdesk - openbare**.

    ![Galerie van toepassing] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Galerie van toepassing")

7.  Selecteer **TOPdesk - openbare**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![TOPdesk openbare] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk openbare")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij TOPdesk - openbare met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Configureren eenmalige aanmelding voor TOPdesk - openbare, moet u een logo pictogrambestand uploaden. Als u het pictogrambestand, contact op met het ondersteuningsteam TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Meld u bij uw bedrijfssite **TOPdesk - openbare** aan als beheerder.

2.  Klik in het menu **TOPdesk** op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Instellingen")

3.  Klik op **Instellingen voor aanmelding**.

    ![Instellingen voor aanmelding] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Instellingen voor aanmelding")

4.  Vouw het menu **Instellingen voor aanmelding** en klik vervolgens op **Algemeen**.

    ![Algemene] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Algemene")

5.  In het **openbare** gedeelte van de sectie **SAML login** configuratie, kunt u de volgende stappen uitvoeren:

    ![Technical Settings] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Technical Settings")

    1.  Klik op **downloaden** om het metagegevensbestand openbare te downloaden en sla deze lokaal op uw computer.
    2.  Open het metagegevensbestand en zoek het knooppunt **AssertionConsumerService** .
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Kopieer de waarde **AssertionConsumerService** .  

        >[AZURE.NOTE] Verderop in deze zelfstudie moet u de waarde in de sectie **URL van de App configureren** .

6.  In een browservenster verschillende web en meld u aan bij uw **Azure klassieke portal** als beheerder.

7.  Klik op de pagina **TOPdesk - openbare** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Eenmalige aanmelding configureren")

8.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij TOPdesk - openbare** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Eenmalige aanmelding configureren")

9.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "URL van App configureren")

    1.  Typ in het tekstvak **TOPdesk - openbare aanmelding op URL** de URL die wordt gebruikt door uw gebruikers u zich aanmeldt bij uw TOPdesk - openbare toepassing (bijvoorbeeld: "*https://qssolutions.topdesk.net*").
    2.  Plak in het tekstvak **TOPdesk – openbare beantwoorden URL** de **TOPdesk - openbare AssertionConsumerService-URL** (bijvoorbeeld: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klik op **volgende**.

10. Klik op de pagina **configureren eenmalige aanmelding bij TOPdesk - openbare** op **metagegevens downloaden**uw metagegevensbestand te downloaden en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Eenmalige aanmelding configureren")

11. Als u wilt een certificaatbestand hebt gemaakt, kunt u de volgende stappen uitvoeren:

    ![Certificaat] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Certificaat")

    1.  Open het metagegevensbestand gedownloade.
    2.  Vouw **RoleDescriptor** waarop een **xsi: type** van **vloot: ApplicationServiceType**.
    3.  Kopieer de waarde van het knooppunt **X509Certificate** .
    4.  De gekopieerde **X509Certificate** waarde lokaal opslaan in een bestand op uw computer.

12. Klik op uw TOPdesk - openbare bedrijfssite, in het menu **TOPdesk** op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Instellingen")

13. Klik op **Instellingen voor aanmelding**.

    ![Instellingen voor aanmelding] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Instellingen voor aanmelding")

14. Vouw het menu **Instellingen voor aanmelding** en klik vervolgens op **Algemeen**.

    ![Algemene] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Algemene")

15. In de **openbare** sectie, klikt u op **toevoegen**.

    ![Op SAML Login] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "Op SAML Login")

16. Klik op de pagina **SAML configuratie assistent** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Op SAML configuratie assistent] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "Op SAML configuratie assistent")

    1.  Klik op **Bladeren**om het metagegevensbestand gedownloade onder **Metagegevens Federatie**.
    2.  Klik op **Bladeren**om uw certificaatbestand, klikt u onder **Certificaat (RSA)**, te uploaden.
    3.  Klik op **Bladeren**om het logobestand dat u hebt ontvangen van het ondersteuningsteam TOPdesk onder **Logo pictogram**, te uploaden.
    4.  Typ in het tekstvak **kenmerk van gebruikersnaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Typ een naam voor uw configuratie in het tekstvak **naam weer te geven** .
    6.  Klik op **Opslaan**.

17. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat Azure AD-gebruikers kunnen Meld u aan bij TOPdesk - openbare, ze in TOPdesk - openbare moeten zijn geconfigureerd.  
In het geval van TOPdesk - openbare, de inrichting van een handmatige taak is.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u bij uw bedrijfssite **TOPdesk - openbare** aan als beheerder.

2.  Klik in het menu aan de bovenkant, op **TOPdesk \> nieuw \> ondersteuningsbestanden \> persoon**.

    ![Persoon] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Persoon")

3.  Klik in het dialoogvenster nieuwe persoon moet u de volgende stappen uitvoeren:

    ![Nieuwe persoon] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Nieuwe persoon")

    1.  Klik op het tabblad Algemeen.
    2.  Typ in het tekstvak Achternaam de achternaam van een geldige Azure Active Directory-account inrichten gewenste.
    3.  Selecteer een **Site** voor het account.
    4.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere TOPdesk - hulpmiddelen voor het maken van account openbare gebruiker of API's verstrekt door TOPdesk - publiek inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>Gebruikers toewijzen aan TOPdesk - openbare, de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **TOPdesk - openbare **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.