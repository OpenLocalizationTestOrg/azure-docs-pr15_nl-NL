<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Tableau Online | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Tableau Online configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Online

In deze zelfstudie leert u hoe u Tableau Online integreren met Azure Active Directory (Azure AD).

Online Tableau integreren met Azure AD biedt de volgende voordelen:

- U kunt op Azure AD wie toegang tot Tableau Online heeft beheren
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Online Tableau (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Tableau Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een **Online Tableau** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Tableau Online toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-tableau-online-from-the-gallery"></a>Tableau Online toevoegen vanuit de galerie
Als u wilt de integratie van Tableau Online in Azure AD configureren, moet u Tableau Online toevoegen vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen vanuit de galerie Tableau Online, kunt u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. In het zoekvak, typ **Tableau Online**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. Selecteer **Tableau Online**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Tableau Online op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Tableau Online in Azure AD aan een gebruiker is. Met andere woorden, moet een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tableau Online tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Tableau Online.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Tableau Online, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Tableau Online testen](#creating-a-Tableau-Online-test-user)** - hebben een tegenhanger van Britta Simon in Tableau Online die is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw Tableau Online-toepassing.

**Als u wilt configureren eenmalige aanmelding Azure AD met Tableau Online, kunt u de volgende stappen uitvoeren:**

1. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren][6]
2. Klik in de klassieke-portal op de pagina **Tableau Online** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][7] 

3. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden bij Tableau Online** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    een. Typ een URL waarbij het volgende patroon in het tekstvak Aanmeldingsadres op URL:`https://sso.online.tableau.com`

    c. Klik op **volgende**.

5. Klik op de pagina **configureren eenmalige aanmelding bij Tableau Online** op **downloaden van metagegevens**, en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Selecteer de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]
8. In een ander browservenster, eenmalige aanmelding in uw Tableau Online toepassing. Ga naar **Instellingen** en klik vervolgens op **verificatie**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. Klik onder sectie **Verificatietypen** . Schakel het selectievakje **eenmalige aanmelding met SAML** om in te schakelen SAML.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Schuif omlaag totdat sectie **importeert metagegevensbestand in Tableau Online** .  Klik op Bladeren en de metagegevensbestand dat u hebt gedownload van Azure AD importeren. Klik vervolgens op **toepassen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. Klik in het gedeelte **van de zoekwaarde bevestigingen** invoegen de bijbehorende naam van de bevestiging identiteitsprovider voor e-mailadres, voornaam en achternaam. Deze informatie ophalen van Azure AD:

    een. Ga terug naar Azure AD. Klik in het Azure op klassieke-portal op de **Tableau Online** pagina in het menu aan de bovenkant, de toepassing-integratie **kenmerken**. De naam voor de waarden kopiëren: userprincipalname, givenname en achternaam.
     
    ![Azure AD voor eenmalige aanmelding](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    b. Ga naar de Tableau Online-toepassing en de sectie **Tableau Online kenmerken** instellen als volgt:
    
    -  E-mail: **e-mail** of **userprincipalname**
    -  Voornaam: **givenname**
    -  Achternaam: **Achternaam**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-tableau-online-test-user"></a>Een Online Tableau test-gebruiker maken

In dit gedeelte maakt u een gebruiker Britta Simon in Tableau Online genoemd.

1. Klik op **Online Tableau**, op **Instellingen** en klik vervolgens op sectie **verificatie** . Schuif omlaag naar de sectie **Gebruikers selecteren** . Klik op **gebruikers toevoegen** en **Voer e-mailadressen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Selecteer **de gebruikers toevoegen voor eenmalige aanmelding (SSO) verificatie**. In het tekstvak **Voer e-mailadressen** toevoegenbritta.simon@contoso.com

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Klik op **maken**.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen aan Tableau Online inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon bij Tableau Online, kunt u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

3. Klik in de lijst toepassingen Selecteer **Tableau Online**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

5. Selecteer in de lijst alle gebruikers **Britta Simon**.

6. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Tableau Online in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Online Tableau-toepassing.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
