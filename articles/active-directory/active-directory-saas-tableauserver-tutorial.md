<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Tableau Server | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Tableau Server configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Zelfstudie: Azure Active Directory-integratie met Tableau Server

Het doel van deze zelfstudie is om aan te geven hoe u Tableau Server integreren met Azure Active Directory (Azure AD).

Tableau Server integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Tableau Server heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Tableau-Server (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Tableau-Server, moet u de volgende items:

- Een Azure AD-abonnement
- Een Tableau Server eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Tableau Server toe te voegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-tableau-server-from-the-gallery"></a>Tableau Server toe te voegen vanuit de galerie
Als u wilt de integratie van Tableau Server in Azure AD configureren, moet u Tableau Server uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt Tableau Server uit de galerie toevoegen, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 
 
    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Tableau Server**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. Selecteer **Tableau Server**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Tableau Server op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Tableau Server aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Tableau Server moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Tableau-Server.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Tableau-Server, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Server Tableau testen](#creating-a-tableauserver-test-user)** - hebben een tegenhanger van Britta Simon in Tableau-Server die is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Tableau Server.

De bevestigingen SAML verwacht tableau servertoepassing een specifieke notatie. De volgende schermafbeelding ziet u een voorbeeld hiervoor. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Als u wilt configureren eenmalige aanmelding Azure AD met Tableau-Server, moet u de volgende stappen uitvoeren:**


1. Klik in het Azure op klassieke-portal op de pagina **Tableau Server** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. Klik in het dialoogvenster **SAML token kenmerken** , moet u de volgende stappen uitvoeren:

    

    een. Klik op **gebruikerskenmerk toevoegen** om het dialoogvenster **Gebruiker Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. Typ in het tekstvak **Attrubute** **gebruikersnaam**.

    c. In de lijst **Kenmerkwaarde** selsect **user.displayname**.

    d. Klik op **Voltooien**.  
    



1. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 



2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Tableau Server** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. De volgende stappen uitvoeren en klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    een. Typ in het tekstvak **Sign In URL** de URL van uw Tableau-server. 

    b. In de id vak kopie de 

    c. Klik op **volgende**


4. Klik op de pagina **configureren eenmalige aanmelding bij Tableau Server** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


6. Als u eenmalige aanmelding configureren voor uw toepassing, moet u eenmalige aanmelding aan bij uw tenant Tableau Server als beheerder.

    een. Klik op het tabblad **SAML** in de configuratie Tableau Server.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Schakel het selectievakje in van **De SAML gebruiken voor eenmalige aanmelding**.

    c. Zoek uw Federatie metagegevens-bestand dat is gedownload van Azure klassieke-portal en uploadt dit in het **bestand met SAML Idp metagegevens**.

    d. Tableau Server terug URL, de URL die Tableau Server gebruikers toegang, zoals http://tableau_server tot. Gebruik http://localhost wordt niet aanbevolen. Via een URL met een slash (bijvoorbeeld http://tableau_server/) wordt niet ondersteund. **Tableau Server terug URL** kopiëren en plakken naar Azure AD **Aanmeldingsadres op URL** tekstvak zoals wordt weergegeven in de stap 3

    e. Op SAML entiteit-ID: deze identificeert op de afdelings-ID uw Tableau Server-installatie voor de IdP. U kunt de URL van de Server Tableau opnieuw hier invoert, als u dat wilt, maar er geen moeten de URL van uw Tableau-Server. **Op SAML entiteit-ID** kopiëren en plakken naar Azure AD- **id** tekstvak zoals wordt weergegeven in de stap 3.

    f. Klik op het **Bestand met metagegevens exporteren** en deze in de tekst editor-toepassing te openen. Bevestiging consumenten Service URL met Http Post zoeken en indexeren 0 en kopieer de URL. Nu geplakt op Azure AD **Antwoord URL** tekstvak zoals wordt weergegeven in stap 3. 

    g. Klik op de knop **OK** op de pagina Tableau Server Configiuration.

    > [AZURE.NOTE] Als u hulp bij de configuratie van SAML op Tableau Server nodig vervolgens raadpleegt u in dit artikel [SAML configureren](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**. 
 
    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    een. Als het **Type van gebruiker**, selecteert u **nieuwe gebruiker in uw organisatie**.

    b. Typ in het tekstvak **Gebruikersnaam in te voeren** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-tableau-server-test-user"></a>Maken van een gebruiker Tableau Server testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in Tableau Server genoemd. Moet u alle gebruikers in de server Tableau inrichten. Bedenk ook dat gebruikersnaam van de gebruiker moet overeenkomen met de waarde die u hebt geconfigureerd in het aangepaste Azure AD-kenmerk van **gebruikersnaam**. De integratie moet de juiste toewijzing [Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)werken.

> [AZURE.NOTE] Als u een gebruiker handmatig maken moet, moet u contact op met de beheerder Tableau Server in uw organisatie.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Tableau Server.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Tableau Server, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
 
    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Tableau Server**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Tableau Server in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Tableau Server-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
