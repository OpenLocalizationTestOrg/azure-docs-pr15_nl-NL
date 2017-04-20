<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SanSan | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en SanSan configureren."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Zelfstudie: Azure Active Directory-integratie met SanSan

In deze zelfstudie leert u hoe u SanSan integreren met Azure Active Directory (Azure AD).

SanSan integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot SanSan heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot SanSan (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met SanSan, moet u de volgende items:

- Een Azure AD-abonnement
- Een SanSan eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Microsoft Microsoft Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. SanSan toevoegen vanuit de galerie
2. Configureren en testen van Microsoft Azure AD eenmalige aanmelding


## <a name="adding-sansan-from-the-gallery"></a>SanSan toevoegen vanuit de galerie
Als u wilt de integratie van SanSan in Azure AD configureren, moet u SanSan uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen SanSan vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **SanSan**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_01.png)

7. Selecteer **SanSan**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_06.png)

##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Configureren en testen van Microsoft Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen van Microsoft Azure AD eenmalige aanmelding met SanSan op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in SanSan in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SanSan moet met andere woorden, tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in SanSan.

Als u wilt configureren en te testen Microsoft Azure AD eenmalige aanmelding met SanSan, die u moet uitvoeren van de volgende elementen:

1. **[Configureren Microsoft Azure AD eenmalige aanmelding](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Microsoft Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een SanSan testen](#creating-an-sansan-test-user)** - hebben een tegenhanger van Britta Simon in SanSan dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Microsoft Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Configuratie van Microsoft Azure AD voor eenmalige aanmelding

In dit gedeelte Microsoft Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing SanSan.


**Als u wilt configureren eenmalige aanmelding Microsoft Azure AD met SanSan, moet u de volgende stappen uitvoeren:**


1. Klik in de klassieke Azure portal op de pagina van de integratie in de **SanSan** toepassing, klik op configureren eenmalige aanmelding om het dialoogvenster configureren eenmalige aanmelding te openen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png) 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SanSan** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL in het volgende patroon:

  	| Omgeving             | URL |
  	| :--                     | :-- |
  	| PC web                  | `https://ap.sansan.com/v/saml2/<company name>/acs`|
  	| Systeemeigen mobiele app       | `https://internal.api.sansan.com/saml2/<company name>/acs` |
  	| Instellingen voor mobiele browser | `https://ap.sansan.com/s/saml2/<company name>/acs` |


    b. Typ de URL in het volgende patroon in het tekstvak **id** : 

  	| Omgeving             | URL |
  	| :--                     | :-- |
  	| PC web                  | `https://ap.sansan.com/v/saml2/<company name>`|
  	| Systeemeigen mobiele app       | `https://internal.api.sansan.com/saml2/<company name>` |
  	| Instellingen voor mobiele browser | `https://ap.sansan.com/s/saml2/<company name>` |


    c. Klik op **volgende**.

4. Klik op de pagina **configureren eenmalige aanmelding bij SanSan** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5.  Als u eenmalige aanmelding configureren voor uw toepassing, worden contactpersonen SanSan ondersteuningsteam en ze helpen eenmalige aanmelding configureren. Geef ze met de volgende informatie:

    • Het gedownloade **certificaat**

    • De **identiteit Provider-ID**

    • De **SAML SSO-URL**

    • Het **één afmelden Service-URL**

> [AZURE.NOTE] PC browserinstelling werkt ook voor de mobiele app en mobiele browser samen met de PC web. 


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
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-sansan-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-sansan-test-user"></a>Maken van een gebruiker SanSan testen

In dit gedeelte maakt u een gebruiker Britta Simon in SanSan genoemd. SanSan-toepassing moet de gebruiker kan worden opgenomen in de toepassing voordat u eenmalige aanmelding uitvoert. 


> [AZURE.NOTE] Als u moet een gebruiker handmatig maakt of batch gebruikers, moet u contact opnemen met het ondersteuningsteam SanSan.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan SanSan inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan SanSan, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **SanSan**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Microsoft Azure AD eenmalige aanmelding configureren via het Configuratiescherm van Access.
Wanneer u op de tegel SanSan in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw SanSan-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_205.png
