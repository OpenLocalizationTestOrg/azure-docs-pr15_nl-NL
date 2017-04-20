<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Tangoe opdracht Premium Mobile | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Tangoe opdracht Premium Mobile configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Zelfstudie: Azure Active Directory-integratie met Tangoe opdracht Premium Mobile

In deze zelfstudie leert u hoe u Tangoe opdracht Premium Mobile integreren met Azure Active Directory (Azure AD).

Tangoe opdracht Premium Mobile integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Tangoe opdracht Premium Mobile heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Tangoe opdracht Premium Mobile (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Tangoe opdracht Premium Mobile, moet u de volgende items:

- Een Azure-abonnement
- Een Tangoe opdracht Premium Mobile eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. 

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Tangoe opdracht Premium Mobile toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Tangoe opdracht Premium Mobile toevoegen vanuit de galerie
Als u wilt de integratie van Tangoe opdracht Premium Mobile in Azure AD configureren, moet u Tangoe opdracht Premium Mobile uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Tangoe opdracht Premium Mobile vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Tangoe opdracht Premium Mobile**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)

7. Selecteer **Tangoe opdracht Premium Mobile**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Tangoe opdracht Premium Mobile op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Tangoe opdracht Premium Mobile is aan een gebruiker in Azure AD. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tangoe opdracht Premium Mobile moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Tangoe opdracht Premium Mobile.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Tangoe opdracht Premium Mobile, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Maken van een Tangoe opdracht Premium mobiele gebruiker testen](#creating-an-tangoe-test-user)** - moet een tegenhanger van Britta Simon in Tangoe opdracht Premium Mobile dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing Tangoe opdracht Premium Mobile.


**Als u wilt configureren eenmalige aanmelding Azure AD met Tangoe opdracht Premium Mobile, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **Tangoe opdracht Premium Mobile** -toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6]


2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden bij Tangoe opdracht Premium Mobile** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 


3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 


    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Tangoe opdracht Premium Mobile-toepassing met het volgende patroon: **"https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<tenant uitgever\>& Target =\<doel-URL voor de aanmeldingspagina\>"**.

    b. Typ in het tekstvak **Beantwoorden URL** de URL in het volgende patroon: **"https://sso.tangoe.com/sp/ACS.saml2"**

    > [AZURE.NOTE]  Als u de juiste waarden voor de URL's niet weet, kunt u de bovenstaande waarden als tijdelijke aanduidingen en verzoek om het koppelen van de juiste waarden uit de ondersteuning van uw Tangoe-klant.


4. Klik op de pagina **configureren eenmalige aanmelding bij Tangoe opdracht Premium Mobile** kunt u de volgende stappen uitvoeren:
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5.  Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw Tangoe klantondersteuning koppelen en geef de volgende:


    - Het metagegevensbestand gedownloade
    - De **URL van de uitgever**
    - De **URL SAML eenmalige aanmelding**
    - De **URL van de Service voor eenmalige afmelden**


  
6. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
  
    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)


5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Maken van een gebruiker Tangoe opdracht Premium Mobile testen

In dit gedeelte maakt u een gebruiker Britta Simon in Tangoe opdracht Premium Mobile genoemd. Tangoe opdracht Premium Mobile-toepassing moet u alle gebruikers in de toepassing voordat u eenmalige aanmelding in te richten. Dus u moet werken met het koppelen van de ondersteuning Tangoe klant voor het inrichten van alle deze gebruikers in de toepassing. 


> [AZURE.NOTE] Als u moet een gebruiker handmatig maakt of batch gebruikers, moet u contact opnemen met het ondersteuningsteam Tangoe opdracht Premium Mobile.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Tangoe opdracht Premium Mobile inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon bij Tangoe opdracht Premium Mobile, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Tangoe opdracht Premium Mobile**.

![Eenmalige aanmelding configureren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de tegel Tangoe opdracht Premium Mobile in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Tangoe opdracht Premium Mobile-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png
