<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met vice | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en vice configureren."
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Zelfstudie: Azure Active Directory-integratie met vice

Het doel van deze zelfstudie is om aan te geven hoe u vice integreren met Azure Active Directory (Azure AD).

Vice integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot vice heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot vice (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met vice, moet u de volgende items:

- Een Azure AD-abonnement
- Een vice eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Vice toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-deputy-from-the-gallery"></a>Vice toevoegen vanuit de galerie
Als u wilt de integratie van vice in Azure AD configureren, moet u vice uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen vice vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .
    
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **vice**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. In het deelvenster resultaten **vice**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met vice op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in vice aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in vice moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in vice.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met vice, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een vice testen](#creating-a-deputy-test-user)** - hebben een tegenhanger van Britta Simon in vice dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing vice.

**Als u wilt configureren eenmalige aanmelding Azure AD met vice, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **vice** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij vice** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. Klik op de pagina van het dialoogvenster **App-instellingen configureren** als u configureren van de toepassing in **IDP modus gestart wilt**, de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **id** : `https://<your-subdomain>.<region>.deputy.com`.

    b. Typ in het tekstvak **Antwoord URL** een URL waarbij het volgende patroon: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.

    c. Klik op **volgende**.

4. Als u configureren van de toepassing in **SP modus gestart** op de pagina van het dialoogvenster **App-instellingen configureren wilt** , klik op de **"Weergeven geavanceerde instellingen (optioneel)"** en voer vervolgens de **Aanmelding op URL** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    een. Typ in het tekstvak **Aanmeldingsadres op URL** een URL waarbij het volgende patroon: `https://<your-subdomain>.<region>.deputy.com`.

    b. Klik op **volgende**.

    > [AZURE.NOTE] Vice regio achtervoegsel is opitional of deze moet een van de volgende gebruiken: au | NB | Europese Unie | als | la | af | een | ent-au | ent JavaScript | ent eu | ent-als | ent-la | ent-af | ent een

5. Klik op de pagina **configureren eenmalige aanmelding bij vice** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    een. Klik op **downloaden**certificaat en sla het bestand op uw computer.

    
6. Navigeer naar de volgende URL: https://(your-subdomain).deputy.com/exec/config/system_config. Ga naar de **Beveiligingsinstellingen** en klik op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. Kopieer de URL van de SSO SAML in de klassieke Azure portal, klik op de configureren eenmalige aanmelding bij vice pagina. 

8. Klik op deze pagina **Beveiligingsinstellingen** uitvoeren u onderstaande stappen te volgen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    een. **Sociale aanmelding**inschakelen.

    b. Een certificaat Base64-gecodeerde opent in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **OpenSSL certificaat** .

    c. Typ in het tekstvak SAM SSO URL`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. Vervang in het tekstvak SAM SSO URL `<your subdomain>` met een subdomein.

    e. Vervang in het tekstvak SAM SSO URL `<saml sso url>` met de SAML SSO-URL die u hebt gekopieerd van de Azure klassieke portal.

    f. Klik op **Instellingen opslaan**.

9. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

10. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-deputy-test-user"></a>Maken van een gebruiker vice testen

Als u wilt dat gebruikers Azure AD Meld u aan bij vice, moeten ze worden ingericht in vice. Bij vice, met de inrichting van is een handmatige taak.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Als u wilt een gebruikersaccount inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw bedrijfssite vice als beheerder.

2.  Klik op het deelvenster bovenste navigatiebalk op **personen**.

    ![Personen] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Personen")

3.  Klik op de knop **Personen toevoegen** en klikt u op **één persoon toevoegen**.

    ![Personen toevoegen] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Personen toevoegen")

4.  De volgende stappen uitvoeren en klik op **Opslaan en uitnodigen**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Nieuwe gebruiker")

    een. Typ in **het tekstvak** **Britta** en **Simon**.  

    b. Typ in het tekstvak **e-mailadres** het e-mailadres van een Azure AD-account dat u inrichten wilt.

    c. Typ in het tekstvak **werk tegen** de bussniess-naam.

    d. Klik op de knop **Opslaan en uitnodigen** .

    >[AZURE.NOTE]De eigenaar van een account AAD wordt ontvangen een e-mailbericht en volgt u een koppeling om te bevestigen hun-account voordat deze geactiveerd wordt. U kunt een andere vice gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door vice aan inrichten AAD-gebruikersaccounts.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan vice.
    
![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon aan vice, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **vice**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.
    
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.
 
Wanneer u op de tegel vice in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw vice-toepassing.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
