<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en SAP bedrijven ByDesign configureren."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Zelfstudie: Azure Active Directory-integratie met SAP Business ByDesign

In deze zelfstudie leert u hoe u SAP bedrijven ByDesign integreren met Azure Active Directory (Azure AD).

SAP bedrijven ByDesign integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot SAP Business ByDesign heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot SAP bedrijven ByDesign (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met SAP Business ByDesign, moet u de volgende items:

- Een Azure AD-abonnement
- Een SAP bedrijven ByDesign eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SAP bedrijven ByDesign toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP bedrijven ByDesign toevoegen vanuit de galerie
Als u wilt de integratie van SAP bedrijven ByDesign in Azure AD configureren, moet u SAP bedrijven ByDesign uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SAP bedrijven ByDesign vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .


3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **SAP bedrijven ByDesign**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. In het deelvenster met resultaten **SAP bedrijven ByDesign**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met SAP Business ByDesign op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in SAP bedrijven ByDesign in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SAP Business ByDesign moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in SAP bedrijven ByDesign.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met SAP Business ByDesign, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een SAP-Business-ByDesign testen](#creating-an-sap-business-bydesign-test-user)** - hebben een tegenhanger van Britta Simon in SAP bedrijven ByDesign dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw SAP bedrijven ByDesign-toepassing.

SAP bedrijven ByDesign toepassing verwacht de bevestigingen SAML een specifieke notatie. Configureer de volgende vorderingen die voortvloeien uit deze toepassing. U kunt de waarden van de volgende kenmerken beheren op het tabblad **"Atrribute"** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld hiervoor. 


**Als u wilt configureren eenmalige aanmelding Azure AD met SAP Business ByDesign, moet u de volgende stappen uitvoeren:**


1. Klik in het Azure op klassieke-portal op de pagina **SAP bedrijven ByDesign** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. In de lijst kenmerken SAML token kenmerken, selecteert u het kenmerk nameidentifier en klik vervolgens op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. Klik in het dialoogvenster gebruikerskenmerk bewerken, moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    een. Selecteer in de lijst kenmerkwaarde de TODAY **ExtractMailPrefix()**

    b. Selecteer de gebruikerskenmerk dat u wilt gebruiken voor uw implementatie in de lijst afdruk. 
    Bijvoorbeeld als u wilt de werknemer-id gebruiken als unieke gebruikers-id en u de kenmerkwaarde in de ExtensionAttribute2 hebt opgeslagen, selecteert u **user.extensionattribute2**. 

    c. Klik op **Voltooien**. 
    

4. Klik in de klassieke-portal, klik op de pagina **SAP bedrijven ByDesign** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

5. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SAP bedrijven ByDesign** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw SAP bedrijven ByDesign-toepassing met het volgende patroon:`https://<servername>.sapbydesign.com`
    
    b. Klik op **volgende**
 
7. Klik op de pagina **configureren eenmalige aanmelding bij SAP bedrijven ByDesign** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


8. Als u eenmalige aanmelding configureren voor uw toepassing, moet u de volgende stappen uitvoeren:

    een. Aanmelden bij de portal van uw SAP bedrijven ByDesign met beheerdersrechten.

    b. Ga naar **toepassingen en gebruiker Management algemene taak** en klik op het tabblad **Identiteitsprovider** .

    c. Klik op **Nieuwe identiteitsprovider** en selecteer de metagegevens XML-bestand dat u hebt gedownload van de Azure klassieke portal. Door deze te importeren de metagegevens, uploadt het systeem automatisch de vereiste handtekeningcertificaat en versleutelingscertificaat.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Als de **URL van de Service bevestiging consumenten** in het verzoek SAML, schakelt u **Bevestiging consumenten Service URL opnemen**.

    e. Klik op **activeren eenmalige aanmelding**.

    f. Sla uw wijzigingen op.

    g. Klik op het tabblad **Mijn systeem** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. Kopieer de **URL voor eenmalige aanmelding** en plak deze in het tekstvak **Azure AD (+) op URL** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    ik. Opgeven of de werknemer handmatig kiezen kunt tussen aanmelden met de gebruikersnaam en wachtwoord of eenmalige aanmelding door in te schakelen **Handmatige identiteit Provider selectie**.

    j. Geef in de sectie **SSO-URL** de URL die moet worden gebruikt door de werknemer zich aanmelden bij het systeem. 
    Klik in het verzonden naar de vervolgkeuzelijst werknemer URL, kunt u kiezen tussen de volgende opties:
    
    **Niet-SSO-URL**
 
    Het systeem worden alleen de URL van de normale systeem naar de werknemer. De werknemer niet kunt aanmelden met eenmalige aanmelding, en moet wachtwoord gebruiken of in plaats daarvan-certificaat.

    **SSO-URL** 

    Het systeem Hiermee worden alleen de URL voor eenmalige aanmelding bij de werknemer laten bezorgen. Er kan de werknemer aanmelden met eenmalige aanmelding. Verificatieverzoek wordt tot en met de IdP omgeleid.

    **Automatische selectie**
 
    Als eenmalige aanmelding niet actief is, wordt in het systeem de URL van de normale systeem verzendt naar de werknemer. Als eenmalige aanmelding actief is, wordt in het systeem gecontroleerd of de werknemer een wachtwoord heeft. Als een wachtwoord beschikbaar is, worden zowel eenmalige aanmelding van URL's en niet-SSO-URL verzonden naar de werknemer. Als de werknemer geen wachtwoord heeft, wordt alleen de URL voor eenmalige aanmelding echter verzonden naar de werknemer.

    k. Sla uw wijzigingen op.

9. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

10. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
 
    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Maken van een gebruiker SAP bedrijven ByDesign testen

In dit gedeelte maakt u een gebruiker Britta Simon in SAP bedrijven ByDesign genoemd. Neem werken met het ondersteuningsteam SAP bedrijven ByDesign om toe te voegen van de gebruikers in het SAP bedrijven ByDesign-platform. 

> [AZURE.NOTE] Zorg dat NameID waarde met het veld gebruikersnaam in het SAP bedrijven ByDesign-platform overeenkomen moet.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan SAP bedrijven ByDesign inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan SAP bedrijven ByDesign, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SAP bedrijven ByDesign**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de tegel SAP bedrijven ByDesign in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw SAP bedrijven ByDesign-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
