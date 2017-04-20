<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met 23 Video | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en 23 Video."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Zelfstudie: Azure Active Directory-integratie met 23 Video

Het doel van deze zelfstudie is om aan te geven hoe u 23 Video integreren met Azure Active Directory (Azure AD).  
Integratie van 23 biedt Video met Azure AD de volgende voordelen: 

- U kunt bepalen in Azure AD wie toegang tot 23 Video heeft 
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot 23 Video (eenmalige aanmelding) met hun Azure AD-accounts

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor 

Als u wilt configureren Azure AD-integratie met 23 Video, moet u de volgende items:

- Een Azure AD-abonnement
- Een 23 Video eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 

 
## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 23 Video toe te voegen vanuit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-23-video-from-the-gallery"></a>23 Video toe te voegen vanuit de galerie
Als u wilt de integratie van 23 Video in Azure AD configureren, moet u 23 Video vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**23 als Video wilt toevoegen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **23 Video**.

    ![Toepassingen][5]

7. Selecteer **23 Video**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Toepassingen][25]

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met 23 Video op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in 23 Video aan een gebruiker in Azure AD is. Met andere woorden, een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in 23 Video moet tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in 23 Video.
 
Als u wilt configureren en te testen Azure AD eenmalige aanmelding met 23 Video, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een 23 videobestand testen](#creating-a-23-video-test-user)** - hebben een tegenhanger van Britta Simon in 23 Video die is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw 23 Video-toepassing.

**Als u wilt configureren eenmalige aanmelding Azure AD met 23 Video, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **23 Video** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden bij 23 Video** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][7] 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Azure AD voor eenmalige aanmelding][8] 
 
     een. Typ in het tekstvak **Beantwoorden URL** de URL die wordt gebruikt door uw gebruikers voor eenmalige aanmelding bij uw 23 Video-site (bijvoorbeeld: *https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] Active Directory-integratie met SAML 2.0 is beschikbaar voor alle 23 Video-gebruikers. Neem contact op met de ondersteuning bij [support@23company.com](mailto:support@23company.com) als u de gerelateerde metagegevens nodig hebt.

     b. Klik op **volgende**.
 
4. Klik op de pagina **configureren eenmalige aanmelding bij 23 Video** kunt u de volgende stappen uitvoeren:

    ![Azure AD voor eenmalige aanmelding][9] 

    een. Klik op certificaat downloaden en sla het bestand op uw computer.

    b. Neem contact op met uw ondersteuningsteam van 23 Video via [support@23company.com](mailto:support@23company.com), met de gedownloade certificaat, de **Uitgever URL**, de **Eenmalige aanmelding Service URL**, de **URL van één Sign-Out**, geef hen dan en vervolgens vraag of ze kunnen SSO instellen voor uw 23 Video-app. 

    c. Klik op **volgende**.


6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**. 

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
  
    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
 
4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**. 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **vertellen over deze gebruiker** , moet u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-23-video-test-user"></a>Een 23 Video testgebruiker maken

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in 23 Video genoemd.

**Als u wilt maken van een gebruiker Britta Simon in 23 Video genoemd, moet u de volgende stappen uitvoeren:**

1. Meld u bij uw 23 Video-bedrijfssite aan als beheerder.

1. Ga naar **Instellingen**.


2. In de sectie **gebruikers** , klikt u op **configureren**.

    ![Gebruiker toewijzen][400]

3. Klik op **een nieuwe gebruiker toevoegen**. 

    ![Gebruiker toewijzen][401]

4. In de sectie **iemand uitnodigen om op te nemen aan deze site** , moet u de volgende stappen uitvoeren:

    ![Gebruiker toewijzen][402]

    een. Typ in het tekstvak **e-mailadressen** van Britta Simon e-mailadres in Azure AD.

    b. Klik op **de gebruiker toevoegen**.   


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan 23 Video.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar 23 Video, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **23 Video**.

    ![Gebruiker toewijzen][202] 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de 23 Video-tegel in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw 23 Video-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png




