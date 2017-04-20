<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Trello | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Trello configureren."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-trello"></a>Zelfstudie: Azure Active Directory-integratie met Trello

In deze zelfstudie leert u hoe u Trello integreren met Azure Active Directory (Azure AD).

Trello integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Trello heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Trello (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Trello, moet u de volgende items:

- Een Azure AD-abonnement
- Een **Trello** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Trello toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-trello-from-the-gallery"></a>Trello toevoegen vanuit de galerie
Als u wilt de integratie van Trello in Azure AD configureren, moet u Trello uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Trello vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Trello**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/tutorial_trello_01.png)

7. Selecteer **Trello**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/tutorial_trello_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Trello op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Trello in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Trello moet met andere woorden, tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Trello. Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Trello, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Trello testen](#creating-a-the-funding-portal-test-user)** - hebben een tegenhanger van Britta Simon in Trello dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Trello.

Trello toepassing verwacht de bevestigingen SAML naar specifieke kenmerken bevatten. Configureer de volgende kenmerken van deze toepassing. U kunt de waarden van de volgende kenmerken beheren op het tabblad **"Atrributes"** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_03.png) 

**Als u wilt configureren eenmalige aanmelding Azure AD met Trello, moet u de volgende stappen uitvoeren:**

1. Klik in het Azure op klassieke-portal op de pagina **Trello** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]


2. Klik in het dialoogvenster **SAML token kenmerken** voor elke rij wordt weergegeven in de onderstaande tabel, de volgende stappen uitvoeren:
    

  	| Kenmerknaam | Kenmerkwaarde |
  	| --- | --- |    
  	| User.Email | User.mail |
  	| User.FirstName | User.givenName |
  	| User.LastName | User.surname |

    een. Klik op **gebruikerskenmerk toevoegen** om het dialoogvenster **Gebruiker Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_05.png)
    
    b. Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .
    
    c. Selecteer de waarde voor die rij in de lijst **Kenmerkwaarde** .
    
    d. Klik op **Voltooien**. Vervolgens **Apply Changes** onder aan de pagina.

3. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren][6]

4. Klik in de klassieke-portal, klik op de pagina **Trello** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][7] 

5. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Trello** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_06.png)

6. Klik op de pagina van het dialoogvenster **App-instellingen configureren** als u configureren van de toepassing in **IDP modus gestart wilt**, voert u de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_07.png)


    een. Typ in het tekstvak antwoord URL een URL waarbij het volgende patroon: `https://trello.com/auth/saml/consume/<enterprise>`.

    b. Klik op **volgende**.

> [AZURE.NOTE] U moet de ** \<enterprise\> ** witruimte uit Trello. Als u niet de waarde witruimte hebt, neem contact op met het ondersteuningsteam Trello bij <support@trello.com> om de witruimte voor u enterprise.

6. Als u configureren van de toepassing in **SP modus gestart** op de pagina van het dialoogvenster **App-instellingen configureren wilt** , klikt u op de **"Weergeven geavanceerde instellingen (optioneel)"** en voert u de **Aanmelding op URL**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_08.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **Aanmeldingsadres op URL** :`https://trello.com/auth/saml/consume/<enterprise>`

    b. Klik op **volgende**

7. Klik op de pagina **configureren eenmalige aanmelding bij Trello** op **certificaat downloaden**, en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_09.png)

6. Als u eenmalige aanmelding configureren voor uw toepassing, gaat u naar de pagina [Trello enterprise SSO configuratie](https://trello.com/sso-configuration) verzenden Trello team de aanmelding op URL en gedownloade certificaat koppelen.

7. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

8. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-trello-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-trello-test-user"></a>Maken van een gebruiker Trello testen

In dit gedeelte maakt u een gebruiker Britta Simon in Trello genoemd. In dit gedeelte maakt u een gebruiker Britta Simon in Trello genoemd. Trello ondersteunt just-in-time inrichting en de eerste keer dat u zich aanmeldt van Azure AD wordt gemaakt door een nieuw account.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Trello inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Trello, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Trello**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-trello-tutorial/tutorial_trello_10.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Trello in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Trello-toepassing.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trello-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trello-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trello-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trello-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-trello-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-trello-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-trello-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-trello-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-trello-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-trello-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trello-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trello-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-trello-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-trello-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-trello-tutorial/tutorial_general_205.png
