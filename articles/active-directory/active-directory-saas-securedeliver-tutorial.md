<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Novatus | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de SECURE geven."
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


# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a>Zelfstudie: Azure Active Directory-integratie met SECURE geven

Het doel van deze zelfstudie is om aan te geven hoe u kunt SECURE geven integreren met Azure Active Directory (Azure AD).  
SECURE geven integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot SECURE geven heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot SECURE geven (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met SECURE geven, moet u de volgende items:

- Een Azure-abonnement
- Een SECURE geven eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toe te voegen vanuit de galerie met SECURE geven
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-secure-deliver-from-the-gallery"></a>Toe te voegen vanuit de galerie met SECURE geven
Als u wilt de integratie van SECURE geven in Azure AD configureren, moet u SECURE geven uit de galerie aan uw lijst met beheerde SaaS apps toevoegen.

**Als u wilt beveiligen geven uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ **SECURE geven**in het zoekvak.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_01.png)

7. Selecteer **SECURE geven**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.
    
    ![App-logo en de naam in de galerie](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met SECURE leveren op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in SECURE geven aan een gebruiker in Azure AD is. Met andere woorden, moet een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in SECURE geven tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in SECURE geven.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met SECURE geven, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een SECURE geven testen](#creating-a-secure-deliver-test-user)** - moet een tegenhanger van Britta Simon in personen die is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing SECURE geven.



**Als u wilt configureren eenmalige aanmelding Azure AD met SECURE geven, kunt u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina toepassing integratie **SECURE geven** op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden voor het leveren van SECURE** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_03.png) 

3. De volgende stappen uitvoeren en klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** :
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw toepassing SECURE geven met het volgende patroon: **"https://i-securedeliver.jp/sd/\<bedrijfsnaam\>/jsf/login/sso"**.

    b. Contact opnemen met de SECURE geven ondersteuningsteam via [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) voor uw tenant-URL als u de waarde niet weet.

    c. Typ de URL van de tenant in het tekstvak **id** . 

    d. Klik op **volgende**


4. Klik op de pagina **configureren eenmalige aanmelding bij SECURE geven** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_05.png) 

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw ondersteuningsteam SECURE geven via [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) en geeft u de volgende handelingen uit:
    
    • Het gedownloade certificaatbestand

    • De **entiteit-ID**

    • De **URL van de Service voor eenmalige aanmelding**

    • De **URL van de Service voor eenmalige afmelden**



6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
  
    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**U maakt een test SECURE geven gebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
  
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-secure-deliver-test-user"></a>Maken van een gebruiker met SECURE geven testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in SECURE geven genoemd. Neem werken met ondersteuningsteam SECURE geven om toe te voegen van de gebruikers in het account SECURE geven.

> [AZURE.NOTE] Als u een gebruiker handmatig maken moet, moet u contact opnemen met het ondersteuningsteam SECURE geven.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen voor het leveren van SECURE.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon voor het leveren van SECURE, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer **SECURE geven**in de lijst met toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de tegel SECURE geven in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw toepassing SECURE geven.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_205.png
