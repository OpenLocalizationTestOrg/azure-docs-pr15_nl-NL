<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Asana | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Asana configureren."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Zelfstudie: Azure Active Directory-integratie met Asana

In deze zelfstudie leert u hoe u Asana integreren met Azure Active Directory (Azure AD).

Asana integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Asana heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Asana (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Asana, moet u de volgende items:

- Een Azure AD-abonnement
- Een **Asana** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Asana toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-asana-from-the-gallery"></a>Asana toevoegen vanuit de galerie
Als u wilt de integratie van Asana in Azure AD configureren, moet u Asana uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Asana vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Asana**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. Selecteer **Asana**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Asana op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Asana in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Asana moet met andere woorden, tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Asana.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Asana, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Asana testen](#creating-an-Asana-test-user)** - hebben een tegenhanger van Britta Simon in Asana dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuratie van Azure AD eenmalige aanmelding

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Asana.

**Als u wilt configureren eenmalige aanmelding Azure AD met Asana, moet u de volgende stappen uitvoeren:**

1. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren][6]
2. Klik in de klassieke-portal, klik op de pagina **Asana** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][7] 

3. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Asana** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    een. Typ een URL waarbij het volgende patroon in het tekstvak Aanmeldingsadres op URL:`https://app.asana.com`

    c. Klik op **volgende**.

5. Klik op de pagina **configureren eenmalige aanmelding bij Asana** op **certificaat downloaden**, en sla het bestand op uw computer. Bovendien de waarde voor SAML SSO URL kopiëren.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. Klik met de rechtermuisknop op het certificaat en opent u het certificaatbestand in Kladblok of u voorkeur teksteditor. Kopieer de inhoud tussen de begin- en de titel van de eind-certificaat. Dit is de X.509-certificaat dat u wilt gebruiken in Asana voor eenmalige aanmelding configureren.

6. In een ander browservenster, eenmalige aanmelding in uw toepassing Asana. Als u wilt configureren SSO in Asana, toegang krijgen tot de werkruimte-instellingen door te klikken op de naam van de werkruimte in de rechterbovenhoek van het scherm. Klik vervolgens op ** \<de naam van de werkruimte\> instellingen**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. Klik in het venster **instellingen van de organisatie** op **beheer**. Klik vervolgens op **leden moeten aanmelden via SAML** kunnen worden geconfigureerd SSO. Het Voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    een. Plak het SAML-teken in de URL van Azure AD in de texbox **aanmeldingsproblemen URL voor de aanmeldingspagina** .

    b. Plak de X.509-certificaat dat u hebt gekopieerd van Azure AD in het tekstvak **X.509-certificaat** .

9. Klik op **Opslaan**. Ga naar het [Asana gids voor het instellen van eenmalige aanmelding](https://asana.com/guide/help/premium/authentication#gl-saml) als u meer hulp nodig hebt.

7. Ga naar **configureren eenmalige aanmelding bij Asana** de pagina in Azure AD, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

8. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-asana-test-user"></a>Maken van een gebruiker Asana testen

In dit gedeelte maakt u een gebruiker Britta Simon in Asana genoemd.

1. Ga naar de sectie **Teams** in het deelvenster links op **Asana**. Klik op de knop plusteken (+). 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Typ het e-mailbericht britta.simon@contoso.com in het tekstvak en selecteer vervolgens **uitnodigen**.
3. Klik op **Uitnodiging verzenden**. De nieuwe gebruiker ontvangt een e-mailbericht in haar e-mailaccount. Zij moet maken en valideren van het account.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Asana inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Asana, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Asana**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van dit gedeelte is Test uw Azure AD eenmalige aanmelding.

Ga naar de aanmeldingspagina Asana. Klik in het e-mailbericht adres tekstvak het e-mailadres invoegen britta.simon@contoso.com. Laat het tekstvak wachtwoord in leeg en klik vervolgens op **Log In**. U wordt omgeleid naar de aanmeldingspagina van Azure AD. Voer uw referenties Azure AD. Nu bent u aangemeld Asana.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
