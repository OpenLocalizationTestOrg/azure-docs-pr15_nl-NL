<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Alcumus Info Exchange | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Alcumus Info Exchange configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Zelfstudie: Azure Active Directory-integratie met Exchange voor Alcumus Info

Het doel van deze zelfstudie is om aan te geven hoe u Alcumus Info Exchange integreren met Azure Active Directory (Azure AD).  
Alcumus Info Exchange integreren met Azure AD biedt de volgende voordelen: 

- U kunt bepalen in Azure AD wie toegang tot Alcumus Info Exchange heeft 
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Alcumus Info Exchange (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor 

Als u wilt configureren Azure AD-integratie met Alcumus Info Exchange, moet u de volgende items:

- Een [Azure AD](https://azure.microsoft.com/) -abonnement
- Een [Alcumus Info Exchange](http://www.alcumusgroup.com/) eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 

 
## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Alcumus Info Exchange toevoegen vanuit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Alcumus Info Exchange toevoegen vanuit de galerie
Als u wilt de integratie van Alcumus Info Exchange in Azure AD configureren, moet u Alcumus Info Exchange uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Alcumus Info Exchange vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Alcumus Info Exchange**.

    ![Toepassingen][5]

7. Klik in het resultatendeelvenster **Alcumus Info Exchange**, en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Toepassingen][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Alcumus Info Exchange op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Alcumus Info Exchange aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Alcumus Info Exchange moet met andere woorden, tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Alcumus Info Exchange.
 
Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Alcumus Info Exchange, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Alcumus Info Exchange testen](#creating-a-alcumus-info-exchange-test-user)** - hebben een tegenhanger van Britta Simon in Alcumus Info Exchange dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Alcumus Info Exchange.

**Als u wilt configureren eenmalige aanmelding Azure AD met Alcumus Info Exchange, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **Alcumus Info Exchange** -toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6]

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Alcumus Info Exchange** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][7]

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD voor eenmalige aanmelding][8]
 
    een. Typ in het tekstvak **Beantwoorden URL** de URL voor consumenten die setup u door het ondersteuningsteam van uw Alcumus Info Exchange.

    > [AZURE.NOTE] Als u niet wat de juiste waarde is weet, neem dan contact op met het ondersteuningsteam Alcumus Info Exchange via [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Klik op **volgende**.
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Alcumus Info Exchange** op **metagegevens downloaden**en sla het metagegevensbestand lokaal op uw computer.

    ![Wat is Azure AD Connect][9]

5. Neem contact op met het ondersteuningsteam Alcumus Info Exchange via [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), geef het metagegevensbestand en ze laten weten dat ze SSO voor u inschakelen mag.


6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**. 

    ![Wat is Azure AD Connect][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Wat is Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**. 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.
  
    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.
  
    c. Klik op volgende.



6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    een. Typ in het tekstvak **Voornaam** **Britta**.  
  
    b. In de **Achternaam** txtbox, type, **Simon**.
  
    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.
  
    d. Selecteer de **gebruiker**in de lijst **functie** .
  
    e. Klik op **volgende**.


7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.
  
    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Maken van een gebruiker Alcumus Info Exchange testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in Alcumus Info Exchange genoemd.

**Als u wilt maken van een gebruiker Britta Simon in Alcumus Info Exchange genoemd, moet u de volgende stappen uitvoeren:**

1. Neem contact op met het ondersteuningsteam Alcumus Info Exchange via [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Alcumus Info Exchange.

![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon met Alcumus Info Exchange, moet u de volgende stappen uitvoeren:**

1. Klik in de portal Azure, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Alcumus Info Exchange**.

    ![Gebruiker toewijzen][202]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de Alcumus Info Exchange-tegel in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Alcumus Info Exchange-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png