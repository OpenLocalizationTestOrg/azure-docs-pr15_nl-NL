<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Intralinks | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Intralinks configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Zelfstudie: Azure Active Directory-integratie met Intralinks

In deze zelfstudie leert u hoe u Intralinks integreren met Azure Active Directory (Azure AD).

Intralinks integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Intralinks heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Intralinks (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Intralinks, moet u de volgende items:

- Een Azure AD-abonnement
- Een Intralinks eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Intralinks toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-intralinks-from-the-gallery"></a>Intralinks toevoegen vanuit de galerie
Als u wilt de integratie van Intralinks in Azure AD configureren, moet u Intralinks uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Intralinks vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Intralinks**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_01.png)

7. Selecteer **Intralinks**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding

In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Intralinks op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Intralinks in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Intralinks moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Intralinks.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Intralinks, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Intralinks testen](#creating-an-intralinks-test-user)** - hebben een tegenhanger van Britta Simon in Intralinks dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing Intralinks.


**Als u wilt configureren eenmalige aanmelding Azure AD met Intralinks, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **Intralinks** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Intralinks** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster de volgende stappen uitvoeren:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Intralinks-toepassing met het volgende patroon: **https://\<bedrijfsnaam\>.Intralinks.com/?PartnerIdpId= https://sts.windows.net/\<Azure AD-Tenant-ID\>**.

    b. Klik op **volgende**.
    
4. Klik op de pagina **configureren eenmalige aanmelding bij Intralinks** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, contact opnemen met het ondersteuningsteam Intralinks en e-mail in de gedownloade metagegevens-bestand bijvoegen.


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

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster de volgende stappen uitvoeren: ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-intralinks-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-intralinks-test-user"></a>Maken van een gebruiker Intralinks testen

In dit gedeelte maakt u een gebruiker Britta Simon in Intralinks genoemd. Neem werken met het ondersteuningsteam Intralinks om toe te voegen van de gebruikers in het platform Intralinks.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Intralinks inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Intralinks, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Intralinks**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]

### <a name="adding-intralinks-via-or-elite-application"></a>Intralinks VIA of Elite toepassing toe te voegen
Intralinks hetzelfde eenmalige aanmelding op identiteit platform voor alle andere Intralinks toepassingen met uitzondering van veel Nexus toepassing wordt gebruikt. Dus als u wilt gaan gebruiken een andere Intralinks-toepassing hebt eerst u voor het configureren van de eenmalige aanmelding voor één primaire Intralinks-toepassing met de bovenstaande procedure.

Daarna volgt u de onderstaande procedure om toe te voegen van een andere toepassing voor Intralinks in uw tenant waarin van deze primaire toepassing voor eenmalige aanmelding gebruikmaken kunt. 

> [AZURE.NOTE] Let erop dat deze functie alleen beschikbaar voor klanten van Azure AD Premium SKU en is niet beschikbaar gratis is of eenvoudige SKU klanten informatie.

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]
2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Uit de links uitlijnende tab Klik op het tabblad **aangepast**

    ![Intralinks VIA of Elite toepassing toe te voegen](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_51.png)

7. De juiste naam geven aan de toepassing bijvoorbeeld **Intralinks Elite** en klik op de knop Voltooien.

8. Klik op de knop **Eenmalige configureren**

9. Selecteer de optie **Bestaande eenmalige aanmelding**

    ![Intralinks VIA of Elite toepassing toe te voegen](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_52.png)

10. Krijgen de om de URL SP gestart eenmalige aanmelding van Intralinks teamsites voor de andere Intralinks-toepassing en voert u het zoals hieronder wordt weergegeven. 

    ![Intralinks VIA of Elite toepassing toe te voegen](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_53.png)

    een. Typ in het tekstvak Aanmeldingsadres op URL de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Intralinks-toepassing met het volgende patroon: **https://\<bedrijfsnaam\>.Intralinks.com/?PartnerIdpId= https://sts.windows.net/\<AzureADTenantID\> **


11. Klik op **volgende**.

12. De toepassing toewijst aan gebruikers of groepen zoals wordt weergegeven in de sectie ** [het toewijzen van de gebruiker Azure AD-test](#assigning-the-azure-ad-test-user)**


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de tegel Intralinks in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Intralinks-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_205.png
