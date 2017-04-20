<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met MCM | Microsoft Azure" 
    description="Meer informatie over het gebruiken van MCM met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Zelfstudie: Azure Active Directory-integratie met MCM
  
Het doel van deze zelfstudie is om aan te geven hoe u MCM integreren met Azure Active Directory (Azure AD).

MCM integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot MCM heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot MCM (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met MCM, moet u de volgende items:

- Een geldig abonnement op Azure
- Een MCM eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.

## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. MCM toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-mcm-from-the-gallery"></a>MCM toevoegen vanuit de galerie
Als u wilt de integratie van MCM in Azure AD configureren, moet u MCM uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen MCM vanuit de galerie, moet u de volgende stappen uitvoeren:**

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **MCM**.

    ![Galerie van toepassing] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Galerie van toepassing")

7.  Selecteer **MCM**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met MCM op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in MCM aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in MCM moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in MCM.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met MCM, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een MCM testen](#creating-a-mcm-test-user)** - hebben een tegenhanger van Britta Simon in MCM dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren
  
In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing MCM.

**Als u wilt configureren eenmalige aanmelding Azure AD met MCM, moet u de volgende stappen uitvoeren:**

1.  Klik in de klassieke Azure portal, klik op de pagina **MCM** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij MCM** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Microsoft Azure AD voor eenmalige aanmelding] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD voor eenmalige aanmelding")

3.  Klik op de pagina App-instellingen configureren dialoogvenster kunt u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "URL van App configureren")

    een. Typ in het tekstvak **Aanmeldingsadres op URL** : `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij MCM** op **metagegevens downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Eenmalige aanmelding configureren")

5. Als u eenmalige aanmelding configureren voor uw toepassing, contact op met uw ondersteuningsteam MCM. De gedownloade metagegevensbestand bijvoegen en delen met MCM team voor het instellen van eenmalige aanmelding op hun kant.

6.  In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Eenmalige aanmelding configureren")

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Eenmalige aanmelding configureren")


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test

Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   

###<a name="creating-a-mcm-test-user"></a>Maken van een gebruiker MCM testen
  
In dit gedeelte maakt u een gebruiker Britta Simon in MCM genoemd. Neem werken met het ondersteuningsteam MCM om toe te voegen van de gebruikers in het platform MCM.

>[AZURE.NOTE]U kunt een andere MCM gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door MCM aan inrichten AAD-gebruikersaccounts.


###<a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test
  
Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan MCM.
    
![Toewijzen aan gebruikers] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Toewijzen aan gebruikers")

**Als u wilt toewijzen Britta Simon aan MCM, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
    
    ![Toewijzen aan gebruikers] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Toewijzen aan gebruikers")

2. Selecteer in de lijst met toepassingen **MCM**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Toewijzen aan gebruikers] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Toewijzen aan gebruikers")

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.
    
    ![Toewijzen aan gebruikers] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Toewijzen aan gebruikers")


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.
 
Wanneer u op de tegel MCM in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw MCM-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)