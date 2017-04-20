<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met vak | Microsoft Azure" 
    description="Meer informatie over het gebruik van vak met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Zelfstudie: Azure Active Directory-integratie met vak


  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en vak.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een test-tenant in vak
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan het vak één Meld u aan bij de toepassing op uw bedrijfssite vak (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor vak inschakelen
2.  Eenmalige aanmelding configureren
3.  Gebruiker en de inrichting van de groep configureren
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-box-tutorial/IC769537.png "Scenario")



##<a name="enabling-the-application-integration-for-box"></a>De toepassingsintegratie van de voor vak inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor vak.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor vak inschakelen, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-box-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-box-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-box-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  **Typen in het **zoekvak**.**

    ![Galerie van toepassing] (./media/active-directory-saas-box-tutorial/IC701023.png "Galerie van toepassing")

7.  Schakel **in**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Vak] (./media/active-directory-saas-box-tutorial/IC701024.png "Vak")



##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers om te verifiëren naar vak met hun account in Azure AD met Federatie op basis van het SAML-protocol. Als onderdeel van deze procedure, bent u verplicht om metagegevens te uploaden naar Box.com.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **vak** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC769538.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij het vak** Selecteer **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC769539.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Vak Tenant URL** de URL van de tenant vak (bijvoorbeeld: https://<mydomainname>. box.com), en klik op **volgende**.

    ![URL van app configureren] (./media/active-directory-saas-box-tutorial/IC669826.png "URL van app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding vak** wilt downloaden van de metagegevens van uw, **metagegevens downloaden**en klik vervolgens op het gegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC669824.png "Eenmalige aanmelding configureren")

5.  Doorsturen dat metagegevensbestand naar vak ondersteuningsteam. De behoeften van de team ondersteuning Hiermee configureert u eenmalige aanmelding voor u.

6.  Selecteer de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-box-tutorial/IC769540.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de inrichting van Active Directory-gebruikersaccounts naar vak.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1. Klik op van de **inrichting van de gebruiker configureren** om het dialoogvenster **Gebruiker inrichting configureren** in de klassieke Azure portal, klik op de pagina **vak** toepassing integratie. 

    ![Automatische gebruiker inrichting inschakelen] (./media/active-directory-saas-box-tutorial/IC769541.png "Automatische gebruiker inrichting inschakelen")

2. Klik op **inschakelen inrichting van de gebruiker**op de pagina dialoogvenster **gebruiker inrichting naar vak hebt ingeschakeld** . 

    ![Automatische gebruiker inrichting inschakelen] (./media/active-directory-saas-box-tutorial/IC769544.png "Automatische gebruiker inrichting inschakelen")

3. Klik op de pagina **aanmelden bij het verlenen van toegang tot in** de vereiste referenties opgeven en klik op **autoriseren**. 

    ![Automatische gebruiker inrichting inschakelen] (./media/active-directory-saas-box-tutorial/IC769546.png "Automatische gebruiker inrichting inschakelen")


4. Klik op **toegang verlenen aan het vak** deze bewerking toe te staan en om terug te keren naar de klassieke Azure-portal. 

    ![Automatische gebruiker inrichting inschakelen] (./media/active-directory-saas-box-tutorial/IC769549.png "Automatische gebruiker inrichting inschakelen")


5. Klik op de pagina **Configuratieopties** kunt de selectievakjes **Objecttypen inrichten** u aangeven of objecten groeperen aan het vak naast gebruikersobjecten is ingericht.  Zie "Toewijzen sectie gebruikers en groepen" hieronder voor meer informatie.


6. Klik op de knop Voltooien om de configuratie. 

    ![Automatische gebruiker inrichting inschakelen] (./media/active-directory-saas-box-tutorial/IC769551.png "Automatische gebruiker inrichting inschakelen")



##<a name="assigning-a-test-user"></a>Het toewijzen van een testgebruiker
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers naar vak, moet u de volgende stappen uitvoeren:

1. Klik in de klassieke Azure portal een testaccount te maken.

2. Klik op de pagina **vak **toepassing integratie op **toewijzen aan gebruikers**. 

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-box-tutorial/IC769552.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing. 

    ![Ja] (./media/active-directory-saas-box-tutorial/IC767830.png "Ja")
  
U moet nu 10 minuten wachten en controleer of het account dat is gesynchroniseerd naar vak.

Als eerste stap verificatie, kunt u de inrichten status controleren door te klikken op Dashboard op het tabblad D op de pagina vak toepassing-integratie in de klassieke Azure-portal.

![Dashboard] (./media/active-directory-saas-box-tutorial/IC769553.png "Dashboard")

De inrichting van cyclus van een met succes zijn gebruiker wordt aangegeven door een gerelateerde status:

![Status van de integratie] (./media/active-directory-saas-box-tutorial/IC769555.png "Status van de integratie")


Gesynchroniseerde gebruikers worden in uw tenant vak vermeld onder **Beheerde gebruikers** in de **Beheerconsole**.

![Status van de integratie] (./media/active-directory-saas-box-tutorial/IC769556.png "Status van de integratie")


##<a name="assigning-users-and-groups"></a>Het toewijzen van gebruikers en groepen

De **vak > gebruikers en groepen** tabblad in de portal van Azure klassieke kunt u opgeven welke gebruikers en groepen toegang moeten worden verleend naar vak. Toewijzing van een gebruiker of groep zorgt ervoor dat de volgende taken uitvoeren:

* Azure AD toestemming geeft dat de toegewezen gebruiker (op basis van directe toewijzing of groepslidmaatschap) om te verifiëren naar vak. Als een gebruiker niet is toegewezen, klikt u vervolgens Azure AD wordt niet toestaan dat ze te melden bij een vak en een fout zullen retourneren op de aanmeldingspagina van Azure AD.

* De tegel van een app voor vak wordt toegevoegd aan van de gebruiker- [toepassingen installeren](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

* Als automatische inrichting is ingeschakeld, worden klikt u vervolgens de toegewezen gebruikers en/of groepen toegevoegd aan de inrichten wachtrij kan automatisch worden opgenomen.

    * Als er slechts gebruikersobjecten zijn geconfigureerd om te worden ingericht, klikt u vervolgens alle direct toegewezen gebruikers in de inrichten wachtrij worden geplaatst en alle gebruikers die deel uitmaken van een toegewezen groepen in de wachtrij inrichten worden geplaatst. 
    
    * Als objecten groeperen zijn geconfigureerd om te worden ingericht, is klikt u vervolgens alle toegewezen groepsobjecten ingericht naar het vak, evenals alle gebruikers die lid zijn van deze groepen. De groep en gebruiker lidmaatschappen blijven behouden na het vak wordt geschreven.
    
U kunt de **kenmerken > eenmalige aanmelding** tabblad welke gebruikerskenmerken (of claims), worden gepresenteerd naar vak tijdens SAML-gebaseerde verificatie configureren en de **kenmerken > Provisioning** tab om te configureren hoe de kenmerken van gebruikers en groepen wordt stromen van Azure AD naar vak tijdens het inrichten van bewerkingen. Zie de onderstaande bronnen voor meer informatie.


## <a name="additional-resources"></a>Aanvullende informatie

* [Vorderingen die in het SAML-token uitgegeven aan te passen](active-directory-saml-claims-customization.md)
* [Aanbod: Kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)
