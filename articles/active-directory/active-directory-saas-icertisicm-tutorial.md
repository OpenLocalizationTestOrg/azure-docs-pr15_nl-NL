<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Icertis Contract Management Platform | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Icertis Contract Management Platform."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a>Zelfstudie: Azure Active Directory-integratie met Icertis Contract Management Platform

Het doel van deze zelfstudie is om aan te geven hoe u Icertis Contract Management Platform integreren met Azure Active Directory (Azure AD).

Icertis Contract Management Platform integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Icertis Contract Management Platform heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Icertis Contract Management Platform (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Icertis Contract Management Platform, moet u de volgende items:

- Een Azure AD-abonnement
- Een Icertis Contract Management Platform eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Icertis Contract Management Platform uit de galerie toe te voegen
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-icertis-contract-management-platform-from-the-gallery"></a>Icertis Contract Management Platform uit de galerie toe te voegen
Als u wilt de integratie van Icertis Contract Management Platform in Azure AD configureren, moet u Icertis Contract Management Platform uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Icertis Contract Management Platform vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .
    
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Icertis Contract Management Platform**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_01.png)

7. Selecteer **Icertis Contract Management Platform**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Icertis Contract Management Platform op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Icertis Contract Management Platform aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Icertis Contract Management Platform moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Icertis Contract Management Platform.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Icertis Contract Management Platform, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een Icertis Contract Management Platform testen](#creating-a-icertis-contract-management-platform-test-user)** - hebben een tegenhanger van Britta Simon in Icertis Contract Management Platform dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing Icertis Contract Management Platform.

**Als u wilt configureren eenmalige aanmelding Azure AD met Icertis Contract Management Platform, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **Icertis Contract Management Platform** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Icertis Contract Management Platform** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_03.png) 

3. De volgende stappen uitvoeren en klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_04.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **Aanmeldingsadres op URL** :`https://<company name>.icertis.com`

    b. Klik op **volgende**


    > [AZURE.NOTE] Houd er rekening mee dat deze niet de reële waarden zijn. U moet deze waarden bijwerken met de werkelijke aanmelden op de URL. Als u deze waarden, neemt u contact Icertis Contract Management Platform.

4. Klik op de pagina **configureren eenmalige aanmelding bij Icertis Contract Management Platform** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw ondersteuningsteam Icertis Contract Management Platform en geef met de volgende items: 

    -Het bestand **gedownload van metagegevens** 
    
    -De **entiteit-ID** 
        
    -De **SAML SSO-URL** 
        
    -De **URL van de Service voor eenmalige afmelden**

6. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.
    
![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_03.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_05.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_07.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-icertis-contract-management-platform-test-user"></a>Maken van een gebruiker Icertis Contract Management Platform testen

In dit gedeelte maakt u een gebruiker Britta Simon in Icertis Contract Management Platform genoemd. Neem werken met het ondersteuningsteam Icertis Contract Management Platform om toe te voegen van de gebruikers in het Platform Icertis Contract Management.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Icertis Contract Management Platform.
    
![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon aan Icertis Contract Management Platform, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Icertis Contract Management Platform**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Icertis Contract Management Platform in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw toepassing Icertis Contract Management Platform.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_205.png
