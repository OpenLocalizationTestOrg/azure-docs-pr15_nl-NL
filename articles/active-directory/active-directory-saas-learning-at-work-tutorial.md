<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met leren in de praktijk | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en studie op werk."
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


# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a>Zelfstudie: Azure Active Directory-integratie met Learning op het werk

In deze zelfstudie leert u hoe u de informatie in de praktijk integreren met Azure Active Directory (Azure AD).

Leren in de praktijk integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot leren aan het werk heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot informatie op mijn werk (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Learning op het werk, moet u de volgende items:

- Een Azure AD-abonnement
- Een zal het leren van op het werk (Saba Cloud) eenmalige aanmelding op abonnement ingeschakeld


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Learning toe te voegen aan het werk in de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-learning-at-work-from-the-gallery"></a>Learning toe te voegen aan het werk in de galerie
Als u wilt configureren de integratie van leren op het werk in Azure AD, moet u leren op het werk uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt leren op het werk uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]
2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ **zal het leren van op het werk**in het zoekvak.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_01.png)

7. In het deelvenster met resultaten **leren in de praktijk**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In dit gedeelte Configureer en test Azure AD eenmalige aanmelding met Learning op het werk op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in onderwijs aan het werk in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker weten op het werk moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** weten op het werk.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Learning op het werk, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een leren in de praktijk testen](#creating-a-predictix-price-reporting-test-user)** - moet een tegenhanger van Britta Simon weten op het werk dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte u Azure AD eenmalige aanmelding in de klassieke portal in inschakelen en configureren eenmalige aanmelding begrepen op werk-toepassing.


**Als u wilt configureren eenmalige aanmelding Azure AD met Learning op het werk, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **leren in de praktijk** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij leren in de praktijk** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers kunnen aanmelding voor toegang tot uw Learning op werk-toepassing via van de volgende patroon:`https://\<company name\>.sabacloud.com/Saba/Web/<company code>`
    
    b. Typ de URL met het volgende patroon in het tekstvak **id** : ' https://<company name>.sabacloud.com/Saba/SAML/sso/alias/<company name>``

    c. Klik op **volgende**
 
4. Klik op de pagina **configureren eenmalige aanmelding van Learning op het werk** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met studie bij ondersteuningsteam van werk (Saba Cloud) en geef met de volgende items:

    • De gedownloade metagegevens

    • De **uitgever Url**

    • De **SAML SSO-URL**

    • Het **één afmelden Service-URL**

6. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
 
    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster de volgende stappen uitvoeren: ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-learning-at-work-test-user"></a>Een leren maken bij werk testgebruiker

In dit gedeelte maakt u Britta Simon genoemd weten op het werk van een gebruiker. Neem werken met Learning op werk ondersteuningsteam om toe te voegen van de gebruikers in de Learning op werkplatform.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen om te leren in de praktijk inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon om te leren op het werk, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **leren op het werk**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op het leren op werk-tegel in het deelvenster Access, u moet krijgen automatisch ondertekend voor toegang tot uw Learning op werk-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_205.png
