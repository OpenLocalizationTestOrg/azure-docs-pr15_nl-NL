<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox | Microsoft Azure"
    description="Leer hoe u Salesforce Sandbox gebruiken met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Zelfstudie: Azure Active Directory-integratie met Salesforce Sandbox
>[AZURE.TIP]Voor feedback, klikt u op [hier](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Salesforce Sandbox.  
Sandboxen zodat u de kan meerdere kopieën van uw organisatie maken in verschillende omgevingen voor diverse doeleinden, zoals ontwikkeling, testen en training, zonder dat de gegevens en toepassingen in uw organisatie Salesforce-productie.  
Zie [Overzicht van de Sandbox](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US) voor meer informatie
  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een sandbox in Salesforce.com
  
Als u nog een geldige sandbox in Salesforce.com niet hebt, moet u contact opnemen met Salesforce.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Salesforce Sandbox inschakelen
2.  Eenmalige aanmelding configureren
3.  Uw domein inschakelen
4.  Configuratie van de inrichting van gebruiker
5.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scenario")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>De toepassingsintegratie van de voor Salesforce Sandbox inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Salesforce-sandbox.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Salesforce sandbox inschakelen, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Toepassingen")

4.  Als u wilt openen in de **Galerie van toepassing**, klik op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie om te gebruiken**.

    ![Wat wilt u doen?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Wat wilt u doen?")

5.  Typ in het **zoekvak** **Salesforce Sandbox**.

    ![Galerie van toepassing] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Galerie van toepassing")

6.  In het deelvenster met resultaten **Salesforce Sandbox**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![SalesForce-Sandbox] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "SalesForce-Sandbox")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Salesforce met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **Salesforce sandbox-** toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Salesforce Sandbox** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![SalesForce-Sandbox] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "SalesForce-Sandbox")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Aanmeldingsadres op URL** de URL voor het gebruik van de volgende patroon `http://company.my.salesforce.com`, en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "URL van App configureren")

4. Als u al eenmalige aanmelding voor een ander exemplaar van de Salesforce Sandbox in uw adreslijst hebt geconfigureerd, moet u ook de **id** om hetzelfde resultaat als het u **zich aanmeldt op URL**configureren. Het veld **id** kan worden gevonden door te schakelen van het selectievakje **weergeven geavanceerde instellingen** op de pagina **Configureren App-URL** van het dialoogvenster.

4.  Klik op de pagina **configureren eenmalige aanmelding bij Salesforce Sandbox** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw Salesforce-sandbox als beheerder.

6.  In het menu aan de bovenkant, klikt u op **Instellingen**.

    ![Setup] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Setup")

7.  Klik in het navigatiedeelvenster aan de linkerkant op **Besturingselementen voor beveiliging**en klik vervolgens op **Instellingen voor eenmalige aanmelding**.

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Instellingen voor eenmalige aanmelding")

8.  Klik op de sectie instellingen voor eenmalige aanmelding, moet u de volgende stappen uitvoeren:

    ![Instellingen voor eenmalige aanmelding] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Instellingen voor eenmalige aanmelding")

    een.  Selecteer **SAML ingeschakeld**.
    
    b.  Klik op **Nieuw**.

9.  Klik op het SAML eenmalige aanmelding Settings kunt u de volgende stappen uitvoeren:

    ![Op SAML eenmalige aanmelding-instellingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "Op SAML eenmalige aanmelding-instellingen")

    een.  Typ in het tekstvak Naam de naam van de configuratie (bijvoorbeeld: *SPSSOWAAD\_Test*).
    
    b.  Kopieer de **URL van de uitgever** -waarde in de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij Salesforce Sandbox** en plak deze in het tekstvak **uitgever** .
    
    c.  Typ in het tekstvak **Entiteit-Id** **https://test.salesforce.com** als dit het eerste exemplaar van Salesforce Sandbox die u aan uw adreslijst toevoegt. Als u al een exemplaar van Salesforce Sandbox en klik vervolgens voor het type **Entiteit-ID** in de **Aanmelding op URL**, die in deze indeling worden moeten hebt toegevoegd:`http://company.my.salesforce.com`
    
    d.  Klik op **Bladeren** om het gedownloade certificaat uploaden.
    
    e.  Als het **SAML identiteitstype**, selecteer **bevestiging bevat de ID met Federatie uit het gebruikersobject**.
    
    f.  Als **SAML identiteit locatie**, selecteer **identiteit wordt in het element NameIdentifier van de instructie onderwerp**.
    
    g.  In de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij Salesforce Sandbox** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Identiteit Provider aanmeldings-URL** .
    
    h.  SFDC biedt geen ondersteuning voor SAML afmelden.  Als een tijdelijke oplossing te plakken 'https://login.windows.net/common/wsfederation?wa=wsignout1.0' in het tekstvak **Identiteit Provider afmelden URL** .
    
    ik.  Als de **Service Provider gestart aanvragen binden**en selecteert u **HTTP-bericht**.
    
    j. Klik op **Opslaan**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Eenmalige aanmelding configureren")

##<a name="enabling-your-domain"></a>Uw domein inschakelen
  
In deze sectie wordt ervan uitgegaan dat u al hebt gemaakt een domein.  
Zie [Uw domeinnaam definiëren](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US)voor meer informatie.

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Als u wilt dat uw domein, moet u de volgende stappen uitvoeren:

1.  In het linkernavigatiedeelvenster **Domain Management**op en klik vervolgens op **Mijn domein.**

    ![Mijn domein] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Mijn domein")

    >[AZURE.NOTE]Zorg ervoor dat uw domein correct is geconfigureerd.

2.  In de sectie **Instellingen van de pagina Login** klikt u op **bewerken**, als **Verificatieservice**, selecteer de naam van de SAML eenmalige aanmelding instelling uit de vorige sectie en ten slotte op **Opslaan**.

    ![Mijn domein] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Mijn domein")
  
Zodra u een domein geconfigureerd hebt, moeten uw gebruikers de URL van het domein voor Meld u aan bij de Salesforce-sandbox gebruiken.  
Als u de waarde van de URL, klikt u op het SSO-profiel dat u in de vorige sectie hebt gemaakt.
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de gebruiker de inrichting van Active Directory-gebruikersaccounts aan Salesforce Sandbox.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Klik in de portal Salesforce in de bovenste navigatiebalk, selecteert u uw naam om het gebruikersmenu te openen:

    ![Mijn instellingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Mijn instellingen")

2.  Selecteer in het gebruikersmenu **Instellingen voor mijn** om uw **Instellingen voor mijn** pagina te openen.

3.  In het linkerdeelvenster op **persoonlijke** om uit te vouwen van de persoonlijke sectie en klik vervolgens op **Opnieuw mijn beveiligingstoken**:

    ![Mijn instellingen] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Mijn instellingen")

4.  Klik op de pagina **Opnieuw mijn beveiligingstoken** op **Beveiligingstoken opnieuw instellen** voor het aanvragen van een e-mailbericht met uw beveiligingstoken Salesforce.com.

    ![Nieuwe Token] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Nieuwe Token")

5.  Controleer uw e-mailpostvak e-mailbericht van Salesforce.com met '**salesforce.com.com beveiligingsbevestiging**' als onderwerp.

6.  Bekijk deze e en kopieer de waarde voor token.

7.  Klik op van de **inrichting van de gebruiker configureren** om het dialoogvenster **Gebruiker inrichting configureren** in de klassieke Azure portal, klik op de pagina **salesforce Sandbox** -toepassing integratie.

    ![Inrichting van de gebruiker configureren] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Inrichting van de gebruiker configureren")

8.  Klik op de pagina **Voer uw referenties Salesforce Sandbox om in te schakelen automatische gebruiker inrichting** bevatten de volgende configuratieinstellingen:

    ![SalesForce-Sandbox] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "SalesForce-Sandbox")

    een.  Typ in het tekstvak **Gebruikersnaam voor Salesforce Sandbox-beheerder** de accountnaam van een Salesforce-sandbox met het profiel van de **Systeembeheerder** in Salesforce.com die zijn toegewezen.

    b.  Typ het wachtwoord voor dit account in het tekstvak **Salesforce Sandbox-beheerderswachtwoord** .

    c.  Plak de waarde voor token in het tekstvak **Gebruiker beveiligingstoken** .

    d.  Klik op **valideren** om te controleren of uw configuratie.

    e.  Klik op de knop **volgende** om de pagina **bevestiging** te openen.

9.  Klik op **de bevestigingspagina** op **voltooid** als u wilt opslaan van uw configuratie.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Salesforce Sandbox, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Salesforce sandbox- **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ja")
  
U moet nu 10 minuten wachten en controleer of dat het account is gesynchroniseerd met Salesforce Sandbox.
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](https://msdn.microsoft.com/library/dn308586)voor meer informatie over het Access-deelvenster.
