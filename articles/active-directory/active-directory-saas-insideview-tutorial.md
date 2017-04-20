<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met InsideView | Microsoft Azure" 
    description="Meer informatie over het gebruiken van InsideView met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-insideview"></a>Zelfstudie: Azure Active Directory-integratie met InsideView
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en InsideView.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant InsideView
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan InsideView één Meld u aan bij de toepassing op uw bedrijfssite InsideView (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor InsideView inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-insideview-tutorial/IC794128.png "Scenario")
##<a name="enabling-the-application-integration-for-insideview"></a>De toepassingsintegratie van de voor InsideView inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor InsideView.

###<a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor InsideView door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-insideview-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-insideview-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-insideview-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **InsideView**.

    ![Galerie van toepassing] (./media/active-directory-saas-insideview-tutorial/IC794129.png "Galerie van toepassing")

7.  Selecteer **InsideView**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![InsideView] (./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij InsideView met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **InsideView** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794131.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij InsideView** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794132.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** , in het tekstvak **InsideView beantwoorden URL** de URL van uw InsideView eenmalige aanmelding (bijvoorbeeld: `https://my.insideview.com/iv/<STS Name>/login.iv`), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-insideview-tutorial/IC794133.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij InsideView** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794134.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite InsideView als beheerder.

6.  In de werkbalk op de bovenkant, klik op **beheerder**, **SingleSignOn instellingen**en klik op **SAML toevoegen**.

    ![Op SAML eenmalige op instellingen] (./media/active-directory-saas-insideview-tutorial/IC794135.png "Op SAML eenmalige op instellingen")

7.  Klik in de sectie **een nieuwe SAML toevoegen** de volgende stappen uitvoeren:

    ![Een nieuwe SAML toevoegen] (./media/active-directory-saas-insideview-tutorial/IC794136.png "Een nieuwe SAML toevoegen")

    1.  Typ een naam voor uw configuratie in het tekstvak **STS** .
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij InsideView** de waarde **Service Provider (SP) gestart eindpunt** kopiëren en plak deze in het tekstvak **SamlP/WS-Fed Unsolicated eindpunt** .
    3.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.
        
        >[AZURE.TIP]Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **STS-certificaat**
    5.  Typ in het tekstvak **Crm User Id Mapping** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    6.  Typ in het tekstvak **Crm e-toewijzing** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Typ in het tekstvak **Crm voornaam toewijzing** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    8.  Typ in het tekstvak **Crm achternaam toewijzing** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    9.  Klik op **Opslaan**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-insideview-tutorial/IC794137.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij InsideView, moeten ze worden ingericht in InsideView.  
Bij InsideView, met de inrichting van is een handmatige taak.
  
Als u gebruikers of contactpersonen die zijn gemaakt in InsideView, neem contact op met uw klant success manager of e-mail verzenden**support@insideview.com**

>[AZURE.NOTE] U kunt een andere InsideView gebruiker account hulpmiddelen voor het maken of API's verstrekt door InsideView voor het inrichten van Azure AD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-insideview-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan InsideView, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **InsideView **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-insideview-tutorial/IC794138.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-insideview-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.