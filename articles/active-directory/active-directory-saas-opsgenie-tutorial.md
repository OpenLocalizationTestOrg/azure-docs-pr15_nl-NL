<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met OpsGenie | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en OpsGenie configureren."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Zelfstudie: Azure Active Directory-integratie met OpsGenie

Het doel van deze zelfstudie is om aan te geven hoe u OpsGenie integreren met Azure Active Directory (Azure AD).

OpsGenie integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot OpsGenie heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot OpsGenie (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met OpsGenie, moet u de volgende items:

- Een Azure AD-abonnement
- Een OpsGenie eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. OpsGenie toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie toevoegen vanuit de galerie
Als u wilt de integratie van OpsGenie in Azure AD configureren, moet u OpsGenie uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen OpsGenie vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **OpsGenie**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. Selecteer **OpsGenie**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met OpsGenie op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD de gebruiker die beschikbaar zijn in OpsGenie weten aan een gebruiker in Azure AD. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in OpsGenie moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in OpsGenie.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met OpsGenie, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een OpsGenie testen](#creating-a-opsgenie-test-user)** - hebben een tegenhanger van Britta Simon in OpsGenie dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de klassieke Azure portal inschakelen en configureren eenmalige aanmelding in uw toepassing OpsGenie.



**Als u wilt configureren eenmalige aanmelding Azure AD met OpsGenie, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **OpsGenie** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij OpsGenie** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png) 


    een. Typ in het tekstvak Aanmeldingsadres op URL de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw OpsGenie-toepassing met het volgende patroon: **"https://app.opsgenie.com/auth/login"**.

    > [AZURE.NOTE] Neem contact op met uw [ondersteuningsteam OpsGenie](mailto:support@opsgenie.com) als u uw Aanmeldingsadres op URL nodig hebt.

    b. Klik op **volgende**.


4. Klik op de pagina **configureren eenmalige aanmelding bij OpsGenie** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png) 

    een. Klik op **Certificaat downloaden**en sla het bestand op uw computer. We nodig dit certificaat en metagegevens URL's (entiteit-ID, Eenmalige aanmelding In URL en aanmelding af URL) voor het instellen van eenmalige aanmelding op OpsGenie kant.

    b. Klik op **volgende**.


5. Open een ander browserexemplaar en vervolgens aanmelden bij OpsGenie als beheerder.

6. Klik op **Instellingen**en klik vervolgens op het tabblad **Eenmalige aanmelding** .
 
    ![OpsGenie voor eenmalige aanmelding](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 

7. Als u eenmalige aanmelding, selecteert u **ingeschakeld**.

    ![OpsGenie-instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
   
8. Klik op het tabblad **Azure Active Directory** in de sectie **Provider** .

    ![OpsGenie-instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
    
9. Klik op de pagina Azure Active Directory dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![OpsGenie-instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png) 

    een. Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij OpsGenie** de waarde voor **Eenmalige aanmelding op Service URL** kopiëren en plak deze in het tekstvak **SAML 2.0-eindpunt** .

    b. Een basis-64 gecodeerd-bestand uit uw gedownloade certificaat maken.      
    
    > [AZURE.NOTE] Zie [hoe u converteert een binair certificaat naar een tekstbestand](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be)voor meer informatie.

    c. Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plak deze in het tekstvak **X.500 certificaat** .

    d. Klik op **wijzigingen opslaan**.


6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.



![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-opsgenie-test-user"></a>Maken van een gebruiker OpsGenie testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in OpsGenie genoemd. 

1.  In een webbrowservenster, meld u aan bij uw tenant OpsGenie als beheerder.

2.  Navigeer naar de lijst inbelgebruikers door te klikken op de **gebruiker** in het linkerdeelvenster.
   
    ![OpsGenie-instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3.  Klik op '**gebruiker toevoegen**'.

3.  Klik in het dialoogvenster **Gebruiker toevoegen** de volgende stappen uitvoeren:

    ![OpsGenie-instellingen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 

    een. Typ in het tekstvak **e** Britta van e-mailadres in Azure Active Directory.

    b. Typ in het tekstvak **De naam van de volledige** **Britta Simon**.

    c. Klik op **Opslaan**. 

Britta krijgt een e-mail met instructies voor het instellen van haar profiel.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan OpsGenie.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan OpsGenie, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
 
    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **OpsGenie**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel OpsGenie in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw OpsGenie-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png
