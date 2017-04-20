<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ImageRelay | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en ImageRelay configureren."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-imagerelay"></a>Zelfstudie: Azure Active Directory-integratie met ImageRelay

Het doel van deze zelfstudie is om aan te geven hoe u ImageRelay integreren met Azure Active Directory (Azure AD).  
ImageRelay integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot ImageRelay heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot ImageRelay (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met ImageRelay, moet u de volgende items:

- Een Azure AD-abonnement
- Een ImageRelay eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ImageRelay toevoegen vanuit de galerie

2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-imagerelay-from-the-gallery"></a>ImageRelay toevoegen vanuit de galerie
Als u wilt de integratie van ImageRelay in Azure AD configureren, moet u ImageRelay uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ImageRelay vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **ImageRelay**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)

7. Selecteer **ImageRelay**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met ImageRelay op basis van een testgebruiker 'Britta Simon' genoemd.

Azure AD vereisten voor eenmalige aanmelding om te werken, een gebruikersaccount met de gerelateerde gebruiker in ImageRelay.  Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ImageRelay moet met andere woorden, tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in ImageRelay.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met ImageRelay, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een ImageRelay testen](#creating-a-userecho-test-user)** - hebben een tegenhanger van Britta Simon in ImageRelay dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing ImageRelay.


**Als u wilt configureren eenmalige aanmelding Azure AD met ImageRelay, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **ImageRelay** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

     ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ImageRelay** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

     ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw ImageRelay-toepassing (bijvoorbeeld: *https://fabrikam.ImageRelay.com/*).

    b. Klik op **volgende**.

4. Klik op de pagina **configureren eenmalige aanmelding bij ImageRelay** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png) 

    een. Klik op **downloaden**certificaat en sla het bestand op uw computer.

    b. Klik op **volgende**.

5. In een ander browservenster, meld u aan bij uw bedrijfssite ImageRelay als beheerder.

1. Op de werkbalk op de voorgrond, klikt u op de werklast van de **gebruikers en machtigingen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Klik op **Nieuw machtigingsniveau maken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png) 

1. Schakel het selectievakje **deze groep kunnen alleen aanmelden via eenmalige aanmelding** in de werklast **Eenmalige aanmelding op instellingen** en klik vervolgens op **Opslaan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Ga naar **Accountinstellingen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Ga naar de werklast **Eenmalige aanmelding op instellingen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

1. Klik in het dialoogvenster **SAML-instellingen** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)

    een. Klik in de klassieke Azure portal de **Eenmalige aanmelding Service URL**kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .


    b. Kopieer de **URL van de Service één Sign-Out**in de portal voor Azure klassieke en plak deze in het tekstvak **URL Meld u af** .

    c. Als **Id naamindeling**, selecteert u **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**.

    
    d. Selecteer **Posten binden**als **Binden opties voor het aanvragen van de Service-Provider (afbeelding Relay)**.
   

    e. Klik onder **x.509-certificaat**, klikt u op **Updatecertificaat**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Open het gedownloade certificaat in Kladblok, Kopieer de inhoud en plak deze in het tekstvak x.509-certificaat.
  
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. Selecteer de **Inrichting van gebruiker met JIT inschakelen**in de sectie **JIT gebruiker inrichting** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Selecteer de machtigingsgroep (bijvoorbeeld **Eenvoudige eenmalige aanmelding**) die alleen via eenmalige aanmelding aanmelden is toegestaan.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    ik. Klik op **Opslaan**.

6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.

    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-imagerelay-test-user"></a>Maken van een gebruiker ImageRelay testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in ImageRelay genoemd.

**Als u wilt maken van een gebruiker Britta Simon in ImageRelay genoemd, moet u de volgende stappen uitvoeren:**

1. Eenmalige aanmelding bij de site van uw ImageRelay bedrijf als beheerder.

1. Ga naar **gebruikers en machtigingen** en selecteer **Maak gebruiker voor eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Voer de **e-mailbericht**, **Voornaam**, **Achternaam** en **bedrijf** van de gebruiker die u wilt inrichten en selecteert u de machtigingsgroep (bijvoorbeeld standaard SSO) is de groep die alleen via eenmalige aanmelding zich kan aanmelden.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Klik op **maken**.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan ImageRelay.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan ImageRelay, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal de toepassingen om weergave te openen, in de directoryweergave, klikt u op **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ImageRelay**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de tegel ImageRelay in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw ImageRelay-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png
