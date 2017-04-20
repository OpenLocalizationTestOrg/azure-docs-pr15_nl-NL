<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Ceridian Dayforce HCM | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Ceridian Dayforce HCM configureren."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Zelfstudie: Azure Active Directory-integratie met Ceridian Dayforce HCM

Het doel van deze zelfstudie is om aan te geven hoe u Ceridian Dayforce HCM integreren met Azure Active Directory (Azure AD).  
Ceridian Dayforce HCM integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Ceridian Dayforce HCM heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Ceridian Dayforce HCM (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren


Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Ceridian Dayforce HCM, moet u de volgende items:

- Een Azure AD-abonnement
- Een Ceridian Dayforce HCM eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Ceridian Dayforce HCM toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Ceridian Dayforce HCM toevoegen vanuit de galerie
Als u wilt de integratie van Ceridian Dayforce HCM in Azure AD configureren, moet u Ceridian Dayforce HCM uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Ceridian Dayforce HCM vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Ceridian Dayforce HCM**.

    ![Toepassingen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. Selecteer **Ceridian Dayforce HCM**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Toepassingen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Ceridian Dayforce HCM op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Ceridian Dayforce HCM aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Ceridian Dayforce HCM moet met andere woorden, tot stand worden gebracht.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Ceridian Dayforce HCM, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Ceridian Dayforce HCM testen](#creating-a-ceridian-dayforce-hcm-test-user)** - hebben een tegenhanger van Britta Simon in Ceridian Dayforce HCM dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Ceridian Dayforce HCM.

Uw toepassing Ceridian Dayforce HCM verwacht de bevestigingen SAML een specifieke notatie. Neem werken met Dayforce HCM team eerst naar de juiste gebruikers-id identificeren. Microsoft raadt het kenmerk **"naam"** als gebruikers-id. U kunt de waarde van dit kenmerk in het dialoogvenster **"Atrribute"** beheren. De volgende schermafbeelding ziet u een voorbeeld hiervoor. 

![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Als u wilt configureren eenmalige aanmelding Azure AD met Ceridian Dayforce HCM, moet u de volgende stappen uitvoeren:**




1. Klik in het Azure op klassieke-portal op de pagina **Ceridian Dayforce HCM** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. In de lijst kenmerken **saml token kenmerken** , selecteert u het naamkenmerk en klik vervolgens op **bewerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. Klik in het dialoogvenster **Gebruikerskenmerk bewerken** , moet u de volgende stappen uitvoeren:
 
    een. Selecteer de gebruikerskenmerk dat u wilt gebruiken voor uw implementatie in de lijst **Kenmerkwaarde** .  
    Bijvoorbeeld als u wilt de werknemer-id gebruiken als unieke gebruikers-id en u de kenmerkwaarde in de ExtensionAttribute2 hebt opgeslagen, selecteert u **user.extensionattribute2**. 

    b. Klik op **Voltooien**.  
    



1. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Klik in de klassieke Azure portal, klik op de pagina **Ceridian Dayforce HCM** toepassing integratie op **eenmalige aanmelding configureren**.

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Ceridian Dayforce HCM** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster de volgende stappen uitvoeren:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw toepassing Ceridian Dayforce HCM. Gebruik de volgende URL-indeling voor productieomgevingen:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Voor helderheid, vervangt u DayforcehcmNamespace door de naamruimte van uw omgeving of uw bedrijfs-ID; bijvoorbeeld:`https://sso.dayforcehcm.com/contoso`
    
    Gebruik de volgende URL-indeling voor testomgeving:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. Typ in het tekstvak **Beantwoorden URL** de URL die wordt gebruikt door Azure AD voor het antwoord posten.  
    Gebruik voor productieomgevingen:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Voor testomgevingen, gebruiken:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Klik op de pagina **configureren eenmalige aanmelding bij Ceridian Dayforce HCM** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    een. Klik op **downloaden**certificaat en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw ondersteuningsteam Ceridian Dayforce HCM via e-mail en geef ze met de volgende items:

    - Het gedownloade certificaatbestand
    - De **URL van de uitgever**
    - De **URL SAML eenmalige aanmelding** 
    - Het **één afmelden Service-URL** 


6. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Maken van een gebruiker Ceridian Dayforce HCM testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in Ceridian Dayforce HCM genoemd. 

Neem werken met het ondersteuningsteam Ceridian Dayforce HCM om te zorgen dat gebruikers die zijn toegevoegd aan uw in de toepassing Ceridian Dayforce HCM. 





### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Ceridian Dayforce HCM.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Ceridian Dayforce HCM, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Ceridian Dayforce HCM**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de tegel Ceridian Dayforce HCM in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Ceridian Dayforce HCM-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
