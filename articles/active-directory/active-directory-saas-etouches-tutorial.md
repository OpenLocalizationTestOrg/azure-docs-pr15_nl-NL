<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met eTouches | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en eTouches configureren."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Zelfstudie: Azure Active Directory-integratie met eTouches

In deze zelfstudie leert u hoe u eTouches integreren met Azure Active Directory (Azure AD).

ETouches integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot eTouches heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot eTouches (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met eTouches, moet u de volgende items:

- Een Azure AD-abonnement
- Een eTouches eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ETouches toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-etouches-from-the-gallery"></a>ETouches toevoegen vanuit de galerie
Als u wilt de integratie van eTouches in Azure AD configureren, moet u eTouches uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen eTouches vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]
2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **eTouches**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_01.png)

7. Selecteer **eTouches**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met eTouches op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in eTouches in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in eTouches moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in eTouches.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met eTouches, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een eTouches testen](#creating-a-predictix-price-reporting-test-user)** - heeft een tegenhanger van Britta Simon in eTouches dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing eTouches.

de bevestigingen SAML verwacht eTouches toepassing een specifieke notatie. Configureer de volgende vorderingen die voortvloeien uit deze toepassing. U kunt de waarden van de volgende kenmerken beheren op het tabblad **"Atrribute"** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld hiervoor. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_07.png) 

**Als u wilt configureren eenmalige aanmelding Azure AD met eTocuhes, moet u de volgende stappen uitvoeren:**


1. Klik in het Azure op klassieke-portal op de pagina **eTouches** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_general_80.png) 


2. Klik in het dialoogvenster **SAML token kenmerken** voor elke rij wordt weergegeven in de onderstaande tabel, de volgende stappen uitvoeren:

  	| Kenmerknaam | Kenmerkwaarde |
  	| --- | --- |    
  	| E-mail | User.mail |

    een. Klik op **gebruikerskenmerk toevoegen** om het dialoogvenster **Gebruiker Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_general_81.png) 


    b. Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Attrubute** .

    c. In de lijst **Kenmerkwaarde** selsect de kenmerkwaarde weergegeven voor die rij.

    d. Klik op **Voltooien**.  
    

3. Klik in de klassieke-portal, klik op de pagina **eTouches** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

4. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij eTouches** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_03.png) 

5. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw eTouches-toepassing met het volgende patroon: **https://www.eiseverywhere.com/saml/accounts/?sso&accountid=\<accountid\>**.
    
    b. Klik op **volgende**
 
6. Klik op de pagina **configureren eenmalige aanmelding bij eTouches** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


7. Als u eenmalige aanmelding configureren voor uw toepassing, moet u de volgende stappen uitvoeren in de toepassing eTouches:

    een. Meld u aan met de beheerder rechten **eTouches** -toepassing.
    
    b. Ga naar de **SAML** -configuratie

    c. In de sectie **Algemene instellingen** de metagegevens van de Federatie Azure AD-inhoud in het tekstvak te plakken.

    d. Klik op de knop **Opslaan & blijven**

    e. Klik op de knop **Updatemetagegevens** in de sectie SAML-metagegevens. 

    f. Hiermee opent u de pagina en eenmalige aanmelding wordt uitgevoerd. Zodra de SSO werkt kunt klik u instellen dat de gebruikersnaam

    g. Selecteer in het veld **Username** de **emailaddress** zoals wordt weergegeven in de onderstaande afbeelding. 

    h. Kopie de **SSO URL / ACS** waarde en zet dit in het tekstvak Azure AD-toepassing configuratie wizard Aanmeldingsadres op URL.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png)

8. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

9. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
 
    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster de volgende stappen uitvoeren: ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-etouches-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-etouches-test-user"></a>Maken van een gebruiker eTouches testen

In dit gedeelte maakt u een gebruiker Britta Simon in eTouches genoemd. Neem werken met het ondersteuningsteam eTouches om toe te voegen van de gebruikers in het platform eTouches.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot eTouches inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan eTouches, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **eTouches**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de tegel eTouches in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw eTouches-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_205.png
