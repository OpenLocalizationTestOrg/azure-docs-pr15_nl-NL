<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Skydesk E-mail | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Skydesk E-mail configureren."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Zelfstudie: Azure Active Directory-integratie met Skydesk E-mail

Het doel van deze zelfstudie is om aan te geven hoe u Skydesk E-mail integreren met Azure Active Directory (Azure AD).

Skydesk E-mail integreren met Azure AD biedt de volgende voordelen:

- U kunt op Azure AD wie toegang tot Skydesk E-mail heeft beheren
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot E-mail Skydesk (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de klassieke Azure Active Directory-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Skydesk e-mailadres, moet u de volgende items:

- Een Azure AD-abonnement
- Een Skydesk e eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen Skydesk E-mail vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-skydesk-email-from-the-gallery"></a>Toe te voegen Skydesk E-mail vanuit de galerie
Als u wilt de integratie van Skydesk E-mail in Azure AD configureren, moet u Skydesk E-mail vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Skydesk E-mail vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Skydesk E-mail**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. Selecteer **Skydesk e-mailbericht**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test Azure AD eenmalige aanmelding met Skydesk E-mail op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Skydesk e-mailbericht aan een gebruiker in Azure AD is. Met andere woorden, moet een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Skydesk e tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Skydesk e-mailbericht.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Skydesk e-mailadres, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een E-mail Skydesk testen](#creating-a-Skydesk-Email-test-user)** - hebben een tegenhanger van Britta Simon in e-mailbericht voor het Skydesk dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw Skydesk e-mailtoepassing.



**Als u wilt configureren eenmalige aanmelding Azure AD met Skydesk e-mailadres, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **Skydesk e** -toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Skydesk e** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    een. Typ in het tekstvak Aanmeldingsadres op URL de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Skydesk e-toepassing met het volgende patroon: **"https://mail.skydesk.jp/portal/\<bedrijfsnaam\>"**.

    b. Klik op **volgende**.


4. Klik op de pagina **configureren eenmalige aanmelding bij Skydesk E-mail** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    een. Klik op **downloaden**certificaat en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Schakel eenmalige aanmelding in **Skydesk e-mailbericht**door de volgende stappen uitvoeren:
 
    een. Aanmelding bij uw Skydesk e-mailaccount als beheerder.

    b. In het menu aan de bovenkant, klik op instellingen en selecteer Org. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c. Klik op domeinen uit het deelvenster aan de linkerkant.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Klik op domein toevoegen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Voer de naam van uw domein en controleer vervolgens of het domein.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Klik op **SAML-verificatie** in het linker deelvenster

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Klik op de pagina **Verificatie SAML** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] Als u wilt gebruiken op SAML gebaseerde verificatie, u moet over de **domein geverifieerd** of **portal URL** instellen. U kunt instellen dat de portal URL met de unieke naam.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    een. Klassieke Azure AD-Portal en de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .

    b. Kopieer de **URL van de Service één Sign-Out** -waarde in klassieke Azure AD-portal en plak deze in het tekstvak van de URL **Meld u af** .

    c. **URL van de wachtwoord wijzigen** is optioneel zo laat het veld leeg.

    d. Klik op de **Toets ophalen uit bestand** om het gedownloade Skydesk e-certificaat te selecteren en klik vervolgens op **openen** als u wilt uploaden van het certificaat.

    e. Als **algoritme**, selecteer **RSA**.

    f. Klik op **Ok** als de wijzigingen wilt opslaan.


7. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

8. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
  
    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-skydesk-email-test-user"></a>Maken van een gebruiker Skydesk e-test

In dit gedeelte maakt u een gebruiker met de naam van Britta Simon in Skydesk e-mailbericht.

een. Klik op **De toegang van gebruikers** in het linker deelvenster in Skydesk e-mailbericht en voer uw gebruikersnaam. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Als u nodig hebt om bulksgewijs gebruikers te maken, moet u contact opnemen met het ondersteuningsteam Skydesk E-mail.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen aan Skydesk E-mail.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Skydesk e-mail, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
 
    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **Skydesk E-mail**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Skydesk E-mail in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Skydesk e-mailtoepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
