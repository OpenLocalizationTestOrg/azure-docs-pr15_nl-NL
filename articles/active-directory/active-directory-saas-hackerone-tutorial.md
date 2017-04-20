<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met HackerOne | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en HackerOne configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Zelfstudie: Azure Active Directory-integratie met HackerOne

In deze zelfstudie kunt u HackerOne integreren met Azure Active Directory (Azure AD).

HackerOne integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot HackerOne heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot HackerOne (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met HackerOne, moet u de volgende items:

- Een Azure-abonnement
- Een HackerOne eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie Configureer en test eenmalige aanmelding Azure AD in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. HackerOne toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-hackerone-from-the-gallery"></a>HackerOne toevoegen vanuit de galerie
Als u wilt HackerOne integreren met Azure AD, moet u HackerOne uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen HackerOne vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

![Toepassingen][4]

6. Typ in het zoekvak **HackerOne**.

![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. Selecteer **HackerOne**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Vervolgens configureert en testen eenmalige aanmelding Azure AD met HackerOne op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in HackerOne aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in HackerOne moet met andere woorden, tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in HackerOne.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met HackerOne, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een HackerOne testen](#creating-a-hackerone-test-user)** - hebben een tegenhanger van Britta Simon in certificeren dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Vervolgens inschakelen u Azure AD eenmalige aanmelding in de klassieke portal en configureren van eenmalige aanmelding in uw toepassing HackerOne.

Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

**Als u wilt configureren eenmalige aanmelding Azure AD met HackerOne, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **HackerOne** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij HackerOne** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. De volgende stappen uitvoeren en klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw HackerOne-toepassing met het volgende patroon: **"https://hackerone.com/\<bedrijfsnaam\>/authentication"**. 

    b. Neem contact op met het ondersteuningsteam HackerOne via [support@hackerone.com](mailto:support@hackerone.com) voor uw tenant-URL als u deze niet weet.

    c. Typ de URL van de tenant in het tekstvak **id** . 

    d. Klik op **volgende**.


4. Klik op de pagina **configureren eenmalige aanmelding bij HackerOne** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    een. Klik op **downloaden**certificaat en sla het bestand op uw computer.

    b. Klik op **volgende**.


1. Eenmalige aanmelding aan bij uw tenant HackerOne als beheerder.

1. Klik op de **Instellingen**in het menu aan de bovenkant.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. Ga naar '**verificatie**' en klikt u op "**toevoegen SAML instellingen**".

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. Klik in het dialoogvenster **SAML-instellingen** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    een. Typ in het tekstvak **E-maildomein** een geregistreerde domein.

    b. Kopieer de **Eenmalige aanmelding Service URL**in de klassieke Azure portal, en plak deze in het tekstvak eenmalige aanmelding op URL.

    c. Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

       >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)
    
    d. Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar de **X509 certificaat** tekstvak.

    e. Klik op de **Opslaan**


1. Klik in het dialoogvenster verificatie-instellingen kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    een. Klik op **test uitvoeren**.

    b. Als de waarde van de **Status** van het veld is gelijk aan **laatst testen status: gemaakt**, neem contact op met uw ondersteuningsteam HackerOne via [support@hackerone.com](mailto:support@hackerone.com) aanvragen van een beoordeling van uw configuratie.


6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
 
    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test

U kunt vervolgens een testgebruiker maken in de klassieke portal Britta Simon genoemd.  

![Azure AD-gebruiker maken][20]

**U maakt een test SECURE geven gebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   


### <a name="creating-a-hackerone-test-user"></a>Maken van een gebruiker HackerOne testen

Vervolgens kunt u een gebruiker met de naam van Britta Simon in HackerOne maken. HackerOne ondersteunt just-in-time is geïnstalleerd, die is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Wanneer u HackerOne openen, wordt een nieuwe gebruiker wordt gemaakt als dit nog niet bestaat. [Azure AD voor eenmalige aanmelding configureren](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Als u een gebruiker handmatig maken moet, moet u contact opnemen met het ondersteuningsteam certificeren.




### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Vervolgens kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan HackerOne inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan HackerOne, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **HackerOne**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Ten slotte, test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.  
Wanneer u op de tegel HackerOne in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw HackerOne-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png