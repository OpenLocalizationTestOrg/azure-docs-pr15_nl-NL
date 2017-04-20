<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en IBM Kenexa enquête Enterprise."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met IBM Kenexa enquête Enterprise

In deze zelfstudie leert u hoe u IBM Kenexa enquête Enterprise integreren met Azure Active Directory (Azure AD).

IBM Kenexa enquête Enterprise integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot IBM Kenexa enquête Enterprise heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot IBM Kenexa enquête Enterprise (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met IBM Kenexa enquête Enterprise, moet u de volgende items:

- Een Azure AD-abonnement
- Een IBM Kenexa enquête Enterprise eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. IBM Kenexa enquête Enterprise toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-ibm-kenexa-survey-enterprise-from-the-gallery"></a>IBM Kenexa enquête Enterprise toevoegen vanuit de galerie
Als u wilt de integratie van IBM Kenexa enquête Enterprise in Azure AD configureren, moet u IBM Kenexa enquête onderneming vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen IBM Kenexa enquête onderneming vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **IBM Kenexa enquête Enterprise**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_01.png)

7. Selecteer **IBM Kenexa enquête Enterprise**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met IBM Kenexa enquête Enterprise op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in IBM Kenexa enquête Enterprise in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in IBM Kenexa enquête Enterprise moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in IBM Kenexa enquête onderneming.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met IBM Kenexa enquête Enterprise, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een IBM Kenexa enquête onderneming testen](#creating-an-kenexasurvey-test-user)** - hebben een tegenhanger van Britta Simon in IBM Kenexa enquête Enterprise dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
6. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte u Azure AD eenmalige aanmelding in de klassieke portal in inschakelen en configureren eenmalige aanmelding uw bedrijfstoepassing IBM Kenexa enquête.


**Als u wilt configureren eenmalige aanmelding Azure AD met IBM Kenexa enquête Enterprise, moet u de volgende stappen uitvoeren:**

1. In de klassieke-portal op de pagina van de integratie in de **IBM Kenexa enquête Enterprise** -toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij IBM Kenexa enquête Enterprise** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
 
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_03.png)

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_04.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **id** :`https://surveys.kenexa.com/<company code>`

    b. Typ een URL waarbij het volgende patroon in het tekstvak **URL beantwoorden** :`https://surveys.kenexa.com/<company code>/tools/sso.asp`

    c. Klik op **volgende**.

    > [AZURE.NOTE] Houd er rekening mee dat deze niet de reële waarden zijn. U moet deze waarden bijwerken met de werkelijke id en beantwoorden URL. Neem contact op met een IBM Kenexa enquête Enterprise-ondersteuningsteam om deze waarden.

4. Klik op de pagina **configureren eenmalige aanmelding bij IBM Kenexa enquête Enterprise** op **certificaat downloaden** en sla het bestand op uw computer:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_05.png) 

5. Als u eenmalige aanmelding configureren voor uw toepassing, contact opnemen met het ondersteuningsteam IBM Kenexa en geef met de volgende items:

    • Het gedownloade certificaatbestand

    • De **uitgever URL**

    • De **SAML SSO-URL**

    • De **URL van de Service voor eenmalige afmelden**

    > [AZURE.NOTE] Neem heeft notitie dat NameID waarde in het antwoord claimen om aan te passen met eenmalige aanmelding ID geconfigureerd in Kenexa-systeem. Dus u moet werken met Kenexa ondersteuningsteam om toe te wijzen de juiste gebruikers-id in uw organisatie zoals SSO-ID. Azure AD wordt standaard de NameIdentifier als UPN-waarde. U kunt dit kenmerk tabblad zoals wordt weergegeven in de onderstaande schermafbeelding wijzigen. De integratie werkt alleen na het voltooien van de juiste toewijzing. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_51.png)

6. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
  
    ![Azure AD voor eenmalige aanmelding][11]

8. Klik in het Azure op klassieke-portal op de pagina **IBM Kenexa enquête Enterprise** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_06.png)

9. Klik in het dialoogvenster **SAML token kenmerken** , moet u de volgende stappen uitvoeren:

    een. Selecteer het kenmerk van **NameIdentifier** en klikt u op pictogram **bewerken** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_07.png)
    
    b. Typ in de lijst **Kenmerkwaarde** de kenmerkwaarde van SSO-ID die is geconfigureerd in Kenexa-systeem.
    
    c. Klik op **Voltooien**

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-ibm-kenexa-survey-enterprise-test-user"></a>Maken van een gebruiker van IBM Kenexa enquête Enterprise testen

In dit gedeelte maakt u een gebruiker Britta Simon in IBM Kenexa enquête Enterprise genoemd. Neem werken met het ondersteuningsteam IBM Kenexa de SSO-ID voor alle gebruikers toewijzen. Deze waarde ID voor eenmalige aanmelding moet ook worden toegewezen aan de waarde NameIdentifier van Azure AD. U kunt deze standaardinstellingen in het tabblad kenmerk wijzigen.


> [AZURE.NOTE] Als u een gebruiker handmatig maken moet, moet u contact opnemen met het ondersteuningsteam van IBM Kenexa enquête Enterprise.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan IBM Kenexa enquête Enterprise inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar IBM Kenexa enquête Enterprise, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **IBM Kenexa enquête Enterprise**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.
    
    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de IBM Kenexa enquête Enterprise-tegel in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw bedrijfstoepassing IBM Kenexa enquête.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_205.png
