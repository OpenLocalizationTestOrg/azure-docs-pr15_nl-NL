<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Optimizely | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Optimizely configureren."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Zelfstudie: Azure Active Directory-integratie met Optimizely

In deze zelfstudie leert u hoe u Optimizely integreren met Azure Active Directory (Azure AD).

Optimizely integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Optimizely heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Optimizely (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Optimizely, moet u de volgende items:

- Een Azure AD-abonnement
- Een **Optimizely** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Optimizely toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-optimizely-from-the-gallery"></a>Optimizely toevoegen vanuit de galerie
Als u wilt de integratie van Optimizely in Azure AD configureren, moet u Optimizely uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Optimizely vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Optimizely**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. Selecteer **Optimizely**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Optimizely op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Optimizely in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Optimizely moet met andere woorden, tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Optimizely.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Optimizely, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Optimizely testen](#creating-an-optimizely-test-user)** - hebben een tegenhanger van Britta Simon in Optimizely dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Optimizely.

Optimizely toepassing verwacht de bevestigingen SAML naar een kenmerk met de naam "e" bevatten. De waarde van "e" moet een e-mailbericht voor een Optimizely herkend die kunt ophalen geverifieerd door Azure AD. Configureer de claim "e" van deze toepassing. U kunt de waarden van de volgende kenmerken beheren op het tabblad **"Atrributes"** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld hiervoor. 


![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Als u wilt configureren eenmalige aanmelding Azure AD met Optimizely, moet u de volgende stappen uitvoeren:**

1. Klik in het Azure op klassieke-portal op de pagina **Optimizely** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]

2. Klik in het dialoogvenster SAML token kenmerken toevoegen het kenmerk "e".

    een. Klik op **gebruikerskenmerk toevoegen** om het dialoogvenster **Gebruikerskenmerk toevoegen** . 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. Typ in het tekstvak **Kenmerk** het kenmerk naam "e".

    c. Selecteer in de lijst **Kenmerkwaarde** het kenmerk waarde 'userprincipalname' of elke waarde die een e-mailbericht herkend door Azure AD bevat en Optimizely.

    d. Klik op **Voltooien**.
3. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren][6]
4. Klik in de klassieke-portal, klik op de pagina **Optimizely** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][7] 

5. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Optimizely** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    een. Typ in het tekstvak **Aanmeldingsadres op URL** :`https://app.optimizely.net/contoso`

    b. Typ in het tekstvak **id** :`urn:auth0:optimizely:contoso`

    c. Klik op **volgende**. 


    > [AZURE.NOTE] De waarden voor de **Aanmelding op URL** en **id** zijn alleen tijdelijke aanduidingen voor de werkelijke waarden. U vindt instructies voor het aquiring de werkelijke waarden uit de Optimizely verderop in deze zelfstudie.

7. Klik op de pagina **configureren eenmalige aanmelding bij Optimizely** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Kopieer de **URL van de Service voor eenmalige aanmelding**.

8. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw Account Optimizely Manager en de onderstaande informatie opgeven:

    - Uw gedownloade certificaat 
    - De URL van de Service voor eenmalige aanmelding
 
    In antwoord op uw e-mail biedt Optimizely u met de aanmelding op URL (SP-gestart SSO) en de waarden id (Service Provider entiteit-ID).

9. Ga terug naar **De App-instellingen configureren** dialoogvenster weer en voer de volgende stappen uit:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    een. In het tekstvak **Aanmeldingsadres op URL** typt u de **SP-gestart SSO URL** die is verstrekt door Optimizely.

    b. Typ in het tekstvak **id** de **Service Provider entiteit-ID** die is verstrekt door Optimizely.

    c. Klik op **volgende**.

10. Klik op de pagina **configureren eenmalige aanmelding bij Optimizely** de volgende stappen uitvoeren:
    
    ![Azure AD voor eenmalige aanmelding][10]

    een. Selecteer de voor eenmalige aanmelding Configuratiebevestiging.

    b. Klik op **volgende**.

11. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]

12. In een ander browservenster, eenmalige aanmelding in uw toepassing Optimizely.
13. Klik op de accountnaam in de rechterbovenhoek en klik vervolgens op **Accountinstellingen**.

    ![Azure AD voor eenmalige aanmelding](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. Schakel het selectievakje **SSO inschakelen** onder eenmalige aanmelding in de sectie **Overzicht** in het tabblad Account.

    ![Azure AD voor eenmalige aanmelding](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.
Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-optimizely-test-user"></a>Maken van een gebruiker Optimizely testen

In dit gedeelte maakt u een gebruiker Britta Simon in Optimizely genoemd.

1. Selecteer op de startpagina van Lotus **uw collega's** -tabblad
2. Klik op **Nieuwe collega** als u wilt een nieuwe collega toevoegen aan het project.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Vul het e-mailadres en een rol toewijzen. Klik op **uitnodigen**.


    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Een uitnodiging voor een e-mailbericht ontvangt. Gebruik het e-mailadres. ze moeten Meld u aan bij Optimizely.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Optimizely inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Optimizely, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Optimizely**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Optimizely in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Optimizely-toepassing.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
