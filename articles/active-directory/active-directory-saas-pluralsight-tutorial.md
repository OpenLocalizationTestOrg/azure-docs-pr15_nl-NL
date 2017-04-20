<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Pluralsight | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Pluralsight configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Zelfstudie: Azure Active Directory-integratie met Pluralsight

Het doel van deze zelfstudie is om aan te geven hoe u Pluralsight integreren met Azure Active Directory (Azure AD).

Pluralsight integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Pluralsight heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Pluralsight (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren


Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Pluralsight, moet u de volgende items:

- Een Azure-abonnement
- Een Pluralsight eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Pluralsight toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-pluralsight-from-the-gallery"></a>Pluralsight toevoegen vanuit de galerie
Als u wilt de integratie van Pluralsight in Azure AD configureren, moet u Pluralsight uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Pluralsight vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
 
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Pluralsight**.
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_01.png)

7. Selecteer **Pluralsight**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Pluralsight op basis van een testgebruiker 'Britta Simon' genoemd.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Pluralsight, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Pluralsight testen](#creating-a-pluralsight-test-user)** - hebben een tegenhanger van Britta Simon in Pluralsight dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Pluralsight.

Uw toepassing Pluralsight verwacht de bevestigingen SAML een specifieke notatie, waarin u aangepaste kenmerktoewijzingen toevoegen aan uw SAML token kenmerken configuratie is vereist. De volgende schermafbeelding ziet u een voorbeeld hiervoor. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_02.png) 

U kunt ook het kenmerk **"unieke ID"** met de juiste waarde, zoals werknemer-id of iets anders die past bij toevoegen voor uw organisatie. Bedenk ook dit is niet het vereist kenmerk. u kunt echter om te bepalen van de unieke gebruiker toevoegen. 

**Als u wilt configureren eenmalige aanmelding Azure AD met Pluralsight, moet u de volgende stappen uitvoeren:**

1. Klik in het Azure op klassieke-portal op de pagina **Pluralsight** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_81.png) 



1. Als u wilt verwijderen de overtollige **SAML token kenmerken**, moet u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/2829.png) 


    een. Plaats de muisaanwijzer op het kenmerk voor het gebruikerskenmerk van elke in het rood vak van de bovenstaande tabel, en klik vervolgens op verwijderen. 




1. Als u wilt toevoegen van de vereiste **SAML token kenmerken**, voor elke rij wordt weergegeven in de onderstaande tabel, kunt u de volgende stappen uitvoeren:

  	| Kenmerknaam | Kenmerkwaarde |
  	| --- | --- |    
  	| Voornaam| User.givenName |
  	| Achternaam  | User.surname |
  	| E-mail | User.mail |

    een. Klik op **gebruikerskenmerk toevoegen** om het dialoogvenster **Gebruiker Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_82.png) 


    b. Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Attrubute** .

    c. In de lijst **Kenmerkwaarde** selsect de kenmerkwaarde weergegeven voor die rij.

    d. Klik op **Voltooien**.  
    


1. Klik op **wijzigingen toepassen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/3232.png)  



1. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_83.png)  









1. Klik in de klassieke Azure portal, klik op de pagina **Pluralsight** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Pluralsight** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster de volgende stappen uitvoeren:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_04.png) 


    een. Typ in het tekstvak Aanmeldingsadres op URL de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Pluralsight-toepassing met het volgende patroon:`https://<instance name>.pluralsight.com/sso/<comapny name>`

    b. Klik op **volgende**.


4. Klik op de pagina **configureren eenmalige aanmelding bij Pluralsight** de volgende stappen uitvoeren:  ![configureren eenmalige aanmelding](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, contact opnemen met Pluralsight [Professional Services](mailTo:professionalservices@pluralsight.com) team en geef het metagegevensbestand gedownloade.


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

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-pluralsight-test-user"></a>Maken van een gebruiker Pluralsight testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in Pluralsight genoemd. Neem werken met het ondersteuningsteam Pluralsight om toe te voegen van de gebruikers in het Pluralsight-account. 


> [AZURE.NOTE]Als u een gebruiker handmatig maken moet, moet u contact opnemen met het ondersteuningsteam Pluralsight.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Pluralsight.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Pluralsight, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Pluralsight**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Pluralsight in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Pluralsight-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_205.png
