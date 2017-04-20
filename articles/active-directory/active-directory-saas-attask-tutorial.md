<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met @Task| Microsoft Azure"
    description="Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en @Task."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Zelfstudie: Azure Active Directory-integratie met@Task

Het doel van deze zelfstudie is om aan te geven hoe u kunt integreren @Task met Azure Active Directory (Azure AD).  
Integratie van @Task met Azure AD bevat de volgende voordelen: 

- U kunt bepalen in Azure AD die toegang heeft tot@Task
- U kunt uw gebruikers u automatisch aangemeld voor toegang tot @Task (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-Portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor 

Voor het configureren van Azure AD-integratie met @Task, moet u de volgende items:

- Een Azure AD-abonnement
- Een @Task eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 

 
## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Toe te voegen @Task vanuit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-task-from-the-gallery"></a>Toe te voegen @Task vanuit de galerie
Voor het configureren van de integratie van @Task in Azure AD, moet u toevoegen @Task vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Om toe te voegen @Task uit de galerie met de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1] 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2] 

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3] 

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4] 

6. Typ in het zoekvak **@Task**.

    ![Toepassingen][5] 

7. Selecteer in het deelvenster met resultaten **@Task**, en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Toepassingen][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

Het doel van dit gedeelte is om aan te geven hoe configureren en test Azure AD eenmalige aanmelding met @Task op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, Azure AD moet weten welke in de gebruiker die beschikbaar zijn in @Task aan een gebruiker in Azure AD is. Met andere woorden, een koppeling relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in @Task moet tot stand worden gebracht.   
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in @Task.
 
Configureren en testen Azure AD eenmalige aanmelding met @Task, u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maken van een @Tasktest gebruiker](#creating-a-halogen-software-test-user) ** - heeft een tegenhanger van Britta Simon in @Taskthat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van deze sectie wordt Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren voor eenmalige aanmelding in uw @Task toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met @Task, de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal op de **@Task** integratie van toepassingen pagina, klikt u op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de **Hoe wilt u dat gebruikers aan te melden bij @Task ** pagina, selecteert u **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][7] 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![App-instellingen configureren][8] 
 
     een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding naar uw @Task toepassing (bijvoorbeeld:*https://<Tenant name>.attask ondemand.com*).

     b. Klik op **volgende**.

4. Klik op de **configureren eenmalige aanmelding bij @Task ** pagina, klikt u op **metagegevens downloaden**, sla het metagegevensbestand lokaal op uw computer en klik vervolgens op **volgende**.

    ![Wat is Azure AD Connect][9] 



1. Aanmelding bij uw @Task bedrijfssite als beheerder.

2. Ga naar **eenmalige aanmelding configuratie**.


1. Klik in het dialoogvenster **Eenmalige aanmelding** de volgende stappen uitvoeren

    ![Eenmalige aanmelding configureren][23]

    een. Als het **Type**, selecteer **SAML 2.0**.

    b. Selecteer **serviceprovider-ID**.

    c. Kopieer de **Externe aanmeldings-URL**in de klassieke Azure portal, en plak deze in het tekstvak **Aanmeldings-Portal-URL** .

    d. Kopieer de **URL van de Service één Sign-Out**in de klassieke Azure portal, en plak deze in het tekstvak **Sign-Out-URL** .

    e. Kopieer de **URL van de wachtwoord wijzigen**in de klassieke Azure portal, en plak deze in het tekstvak **URL van de wachtwoord wijzigen** .

    f. Klik op **Opslaan**.

6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**. 

    ![Wat is Azure AD Connect][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Wat is Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**. 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-an-task-test-user"></a>Maken van een @Task testgebruiker

Het doel van dit gedeelte is het opzetten van een gebruiker met de naam van Britta Simon in @Task.


**Maken van een gebruiker met de naam van Britta Simon in @Task, de volgende stappen uitvoeren:**

1. Meld u aan bij uw @Task bedrijfssite als beheerder.

2. Klik in het menu aan de bovenkant, op **personen**.

3. Klik op **nieuwe persoon**. 

4. Klik in het dialoogvenster nieuwe persoon moet u de volgende stappen uitvoeren:

    ![Maak een @Task testgebruiker][21] 

    een. Typ in het tekstvak **Voornaam** 'Britta'.

    b. Typ in het tekstvak **Achternaam** "Simon".

    c. Typ in het tekstvak **E-mailadres** Britta Simon van e-mailadres in Azure Active Directory.

    d. Klik op de **persoon toevoegen**.




### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot @Task.

![Gebruiker toewijzen][200] 

**Britta Simon aan toewijzen @Task, de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **@Task**.

    ![Gebruiker toewijzen][202] 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u klikt op de @Task -tegel in het deelvenster Access, ontvangt u automatisch aangemeld voor toegang tot uw @Task toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






