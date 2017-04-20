<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met houden bestanden | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en houden bestanden."
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


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Zelfstudie: Azure Active Directory-integratie met houden bestanden

Het doel van deze zelfstudie is om aan te geven hoe u kunt houden bestanden integreren met Azure Active Directory (Azure AD).  
Houden bestanden integreren met Azure AD biedt de volgende voordelen: 

- U kunt op Azure AD wie toegang tot houden bestanden heeft beheren 
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot houden bestanden (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de klassieke Azure Active Directory-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor 

Als u wilt configureren Azure AD-integratie met bestanden die houden, moet u de volgende items:

- Een Azure AD-abonnement
- Een houden bestanden eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 

 
## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Houden bestanden toe te voegen vanuit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-flatter-files-from-the-gallery"></a>Houden bestanden toe te voegen vanuit de galerie
Als u wilt de integratie van houden bestanden in Azure AD configureren, moet u houden bestanden uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt houden bestanden uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Houden bestanden**.


    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. Selecteer **Houden bestanden**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met houden bestanden op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in houden bestanden aan een gebruiker in Azure AD is. Met andere woorden, moet een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in houden bestanden tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in houden-bestanden.
 
Als u wilt configureren en te testen eenmalige aanmelding Azure AD met houden bestanden, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker een houden-bestanden maken testen](#creating-a-halogen-software-test-user)** - hebben een tegenhanger van Britta Simon in houden bestanden die is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de klassieke Azure AD-portal inschakelen en configureren eenmalige aanmelding in uw toepassing houden bestanden. Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat. Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

Als u wilt configureren eenmalige aanmelding voor houden bestanden, moet u eerst een geregistreerde domeinnaam. Als u niet over een geregistreerde domein nog, contact houden bestanden ondersteuningsteam via [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Als u wilt configureren eenmalige aanmelding Azure AD met bestanden die houden, kunt u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal van Azure AD, klik op de pagina **Houden bestanden** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij houden bestanden** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. Klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] De dezelfde SSO aanmeldings-URL voor alle klanten wordt gebruikt door houden bestanden: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. Klik op de pagina **configureren eenmalige aanmelding bij houden bestanden** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


1. Eenmalige aanmelding in uw toepassing houden bestanden als beheerder.

2. Klik op Dashboard. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. Klik op **Instellingen**en voer de volgende stappen uit op het tabblad **bedrijf** : 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    een. Selecteer **gebruiken SAML 2.0 voor verificatie**.

    b. Klik op **SAML configureren**.



2. Klik in het dialoogvenster **SAML-configuratie** , kunt u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    een. Typ in het tekstvak domein geregistreerde van uw domein.

    > [AZURE.NOTE] Als u niet over een geregistreerde domein nog, contact houden bestanden ondersteuningsteam via [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. In de Azure klassieke-portal op de configureren één aanmelding bij houden bestanden dialoogvenster copt eenmalige aanmelding de URL van de en plak deze in het tekstvak identiteit Provider URL.

    c.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

    >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    d.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **FlatterFiles identiteit Provider certificaat** .

    e. Klik op **bijwerken**.

6. In de klassieke Azure AD-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**. 

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Azure AD voor eenmalige aanmelding][11]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**. 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Maken van een gebruiker houden bestanden testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in houden bestanden genoemd.

**Als u wilt maken van een gebruiker Britta Simon in houden bestanden genoemd, moet u de volgende stappen uitvoeren:**

1. Meld u bij uw bedrijfssite **Houden bestanden** aan als beheerder.

2. In het navigatiedeelvenster aan de linkerkant, klikt u op **Instellingen**en klik vervolgens op het **tabblad**van gebruikers.

    ![Cfreate een houden bestanden-gebruiker](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Klik op **gebruiker toevoegen**. 

4. Klik in het dialoogvenster **Gebruiker toevoegen** de volgende stappen uitvoeren:

    ![Cfreate een houden bestanden-gebruiker](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.

    b. Typ in het tekstvak **Achternaam** **Simon**. 

    c. Typ in het tekstvak **E-mailadres** van de Britta e-mailadres in de portal van Azure klassieke.

    d. Klik op **verzenden**.   


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot houden bestanden.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon tot houden bestanden, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst toepassingen **Houden bestanden**.

    ![Gebruiker toewijzen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de tegel houden bestanden in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw toepassing houden bestanden.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






