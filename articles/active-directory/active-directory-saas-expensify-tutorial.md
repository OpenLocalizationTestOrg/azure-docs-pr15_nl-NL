<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Expensify | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Expensify configureren."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-expensify"></a>Zelfstudie: Azure Active Directory-integratie met Expensify

In deze zelfstudie leert u hoe u Expensify integreren met Azure Active Directory (Azure AD).

Expensify integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Expensify heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Expensify (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Expensify, moet u de volgende items:

- Een Azure AD-abonnement
- Een Expensify eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Expensify toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-expensify-from-the-gallery"></a>Expensify toevoegen vanuit de galerie
Als u wilt de integratie van Expensify in Azure AD configureren, moet u Expensify uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Expensify vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Expensify**.
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_01.png)

7. Selecteer **Expensify**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Expensify op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Expensify in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Expensify moet met andere woorden, tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Expensify.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Expensify, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Expensify testen](#creating-an-expensify-test-user)** - hebben een tegenhanger van Britta Simon in Expensify dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing Expensify.


**Als u wilt configureren eenmalige aanmelding Azure AD met Expensify, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **Expensify** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Expensify** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_04.png) 


    een. Typ in het tekstvak Aanmeldingsadres op URL de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Expensify-toepassing met het volgende patroon: **"https://www.expensify.com/authentication/saml/login"**.


4. Klik op de pagina **configureren eenmalige aanmelding bij Expensify** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Om te schakelen SSO in Expensify moet u eerst **Domein besturingselement** inschakelen in de toepassing. Klik in de toepassing kunt u domein besturingselement inschakelen. De stappen, vindt u [hier](http://help.expensify.com/domain-control). Voor extra ondersteuning, u kunt contact opnemen met de ondersteuning Expensify via [help@expensify.com](mailto:help@expensify.com). Nadat u domein beheer is ingeschakeld hebt, als volgt te werk:
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png) 

    een. Meld u aan uw toepassing Expensify.
   
    b. Klik op **beheerder**in de werkbalk op de voorgrond.

    c. Klik op **Domein besturing**in het linkerdeelvenster.

    d. Klik op de naam van uw domein geverifieerd.

    e. In het deelvenster links op **SAML**en selecteer vervolgens **ingeschakeld**.

    f. Open de gedownloade Federatie metagegevens van Azure AD, Kopieer de inhoud en plak deze in het tekstvak **Identiteit Provider metagegevens** .

  
6. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
  
    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-expensify-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-expensify-test-user"></a>Maken van een gebruiker Expensify testen

In dit gedeelte maakt u een gebruiker Britta Simon in Expensify genoemd. Neem werken met het ondersteuningsteam Expensify om toe te voegen van de gebruikers in het platform Expensify.

> [AZURE.NOTE] Als u een gebruiker handmatig maken moet, moet u contact opnemen met het ondersteuningsteam Expensify.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Expensify inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Expensify, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Expensify**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.  
Wanneer u op de tegel Expensify in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Expensify-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_205.png
