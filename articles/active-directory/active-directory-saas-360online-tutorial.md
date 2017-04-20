<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met 360° Online | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en 360° Online configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-360-online"></a>Zelfstudie: Azure Active Directory-integratie met 360° Online


Het doel van deze zelfstudie is om aan te geven hoe u Online 360° integreren met Azure Active Directory (Azure AD).

Integratie van 360° biedt Online met Azure AD u met de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot 360° Online-Server heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot 360° Online (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met 360° Online, moet u de volgende items:

- Een Azure AD-abonnement
- Een onlinetenant 360 ° rechtsom draaien


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 360° Online toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding



## <a name="adding-360-online-from-the-gallery"></a>360° Online toevoegen vanuit de galerie
Als u wilt de integratie van 360° Online in Azure AD configureren, moet u Online 360° uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.


**Als u wilt toevoegen 360° Online vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **360 ° Online**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/tutorial_360online_01.png)

7. Selecteer **360 ° Online**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.
 
    ![Toepassingen](./media/active-directory-saas-360online-tutorial/tutorial_360online_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test Azure AD eenmalige aanmelding met 360° Online op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD weten welke in de gebruiker die beschikbaar zijn in 360° Online aan een gebruiker in Azure AD is. Met andere woorden, een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in 360° Online moet tot stand worden gebracht.


Als u wilt configureren en te testen Azure AD eenmalige aanmelding met 360° Online, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een 360 ° Online testen](#creating-a-360-online-test-user)** - heeft een tegenhanger van Britta Simon in 360 ° Online dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw 360° Online-toepassing.

**Als u wilt configureren eenmalige aanmelding Azure AD met 360° Online, kunt u de volgende stappen uitvoeren:**


1. Klik in het Azure op klassieke-portal op de pagina van de integratie in de **Online 360 °** toepassing, **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** te openen.

    ![Eenmalige aanmelding configureren][13] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden bij 360 ° Online** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_03.png) 

3. Klik op de pagina **App-URL configureren** dialoogvenster de volgende stappen uitvoeren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_04.png)

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding en uw 360 ° Online-toepassing via het volgende patroon:`https://<company name>.public360online.com`

    b. Klik op **volgende**

4. Klik op de pagina **App-URL configureren** dialoogvenster de volgende stappen uitvoeren en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_05.png) 

    een. Klik op **metagegevens downloaden**en sla deze op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw ondersteuningsteam Online 360° via [360online@software-innovation.com](mailto:360online@software-innovation.com) en de gedownloade metagegevensbestand toevoegen aan uw e-mail.

6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 


4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/create_aaduser_05.png) 

    een. Als het **Type van gebruiker**, selecteert u **nieuwe gebruiker in uw organisatie**.

    b. Typ in het tekstvak **Gebruikersnaam in te voeren** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/create_aaduser_07.png) 


8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-360online-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   




### <a name="creating-a-360-online-test-user"></a>Maken van een gebruiker van 360° Online testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in 360° Online genoemd. 

Als u een gebruiker in 360° Online hebt gemaakt, moet u contact op met uw team 360° onlineondersteuning via [360online@software-innovation.com](mailto:360online@software-innovation.com).


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot 360° Online.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan 360° Online, kunt u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
 
    ![Gebruiker toewijzen][201] 


2. Selecteer in de lijst met toepassingen, **360 ° Online**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-360online-tutorial/tutorial_360online_50.png) 


1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de 360° Online-tegel in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw 360° Online-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)







<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[10]: ./media/active-directory-saas-360online-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-360online-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-360online-tutorial/tutorial_general_08.png
[13]: ./media/active-directory-saas-360online-tutorial/tutorial_general_09.png
[20]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-360online-tutorial/tutorial_general_205.png
