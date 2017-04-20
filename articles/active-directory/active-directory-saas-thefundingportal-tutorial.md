<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met de Portal middelen | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de middelen Portal."
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
    ms.date="09/02/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Zelfstudie: Azure Active Directory-integratie met de Portal middelen

In deze zelfstudie leert u hoe u de middelen Portal integreren met Azure Active Directory (Azure AD).

De middelen Portal integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot de Portal middelen heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld bij de Portal met middelen (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met de Portal middelen, moet u de volgende items:

- Een Azure AD-abonnement
- Een **De middelen Portal** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. De Portal middelen uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-the-funding-portal-from-the-gallery"></a>De Portal middelen uit de galerie toe te voegen
Als u wilt de integratie van de Portal middelen in Azure AD configureren, moet u de middelen Portal uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt de middelen Portal uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **De middelen Portal**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_01.png)

7. In het deelvenster met resultaten selecteert u **De Portal middelen**en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met de middelen-Portal op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in de Portal middelen in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Portal middelen moet met andere woorden, tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in de Portal middelen.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met de Portal middelen, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een de middelen Portal testen](#creating-a-the-funding-portal-test-user)** - hebben een tegenhanger van Britta Simon in de Portal middelen dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing de middelen Portal.

De toepassing middelen Portal verwacht de bevestigingen SAML naar een kenmerk met de naam 'externalId1' bevatten. De waarde van "externalId1" moet een herkende studentID. Configureer de claim "externalId1" van deze toepassing. U kunt de waarden van de volgende kenmerken beheren op het tabblad **"Atrributes"** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_03.png) 

**Als u wilt configureren eenmalige aanmelding Azure AD met de Portal middelen, moet u de volgende stappen uitvoeren:**

1. Klik in het Azure op klassieke-portal op de pagina **De middelen Portal** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]

2. Klik in het dialoogvenster SAML token kenmerken toevoegen het kenmerk "externalId1".

    een. Klik op **gebruikerskenmerk toevoegen** om het dialoogvenster **Gebruikerskenmerk toevoegen** . 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_05.png)

    b. Typ in het tekstvak **Kenmerk** het kenmerk naam 'externalId1'.

    c. Selecteer in de lijst **Kenmerkwaarde** het kenmerk dat u wilt gebruiken voor uw implementatie. Bijvoorbeeld als u de waarde StudentID in de ExtensionAttribute1 hebt opgeslagen, selecteert u user.extensionattribute1.

    d. Klik op **Voltooien**. Klik vervolgens op **Apply Changes**.

1. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren][6]

2. Klik in de klassieke-portal op de Integratiepagina van **De Portal middelen** toepassing op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][7] 

3. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de Portal middelen** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_06.png)

4. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_07.png)


    een. Typ in het vak Aanmeldingsadres op URL een URL waarbij het volgende patroon: `https://<subdomain>.regenteducation.net/`.

    b. Klik op **volgende**.

5. Klik op de pagina **configureren eenmalige aanmelding bij de Portal middelen** op **metagegevens downloaden**, en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_08.png)

6. Als u eenmalige aanmelding configureren voor uw toepassing, contact opnemen met de Portal middelen ondersteuning. Ze worden helpen met het juiste kanaal voor het configureren van eenmalige aanmelding. Neem notitie die u moet e-mailbericht verzenden en bijvoegen metagegevensbestand naar gedownload info@regenteducation.com.

7. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

8. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-the-funding-portal-test-user"></a>Maken van een gebruiker van de Portal middelen testen

In dit gedeelte maakt u een gebruiker Britta Simon met de naam in de Portal middelen. Als u niet hoe weet u Britta Simon toevoegt in de Portal middelen, raadpleegt u werken met de middelen Portal ondersteuningsteam de testgebruiker toevoegen en inschakelen van eenmalige aanmelding. Contact kunt opnemen bij info@regenteducation.com.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van haar toegang aan de Portal middelen inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon bij de Portal middelen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer **De middelen Portal**in de lijst met toepassingen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_09.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel van de Portal middelen in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw toepassing de middelen Portal.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_205.png
