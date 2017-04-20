<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met RunMyProcess | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en RunMyProcess configureren."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Zelfstudie: Azure Active Directory-integratie met RunMyProcess

Het doel van deze zelfstudie is om aan te geven hoe u RunMyProcess integreren met Azure Active Directory (Azure AD).

RunMyProcess integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot RunMyProcess heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot RunMyProcess (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met RunMyProcess, moet u de volgende items:

- Een Azure AD-abonnement
- Een RunMyProcess eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. RunMyProcess toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-runmyprocess-from-the-gallery"></a>RunMyProcess toevoegen vanuit de galerie
Als u wilt de integratie van RunMyProcess in Azure AD configureren, moet u RunMyProcess uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen RunMyProcess vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **RunMyProcess**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. Selecteer **RunMyProcess**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met RunMyProcess op basis van een testgebruiker 'Britta Simon' genoemd.

Voor eenmalige aanmelding om te werken, is de behoeften van Azure AD weten wat de gebruiker die beschikbaar zijn in RunMyProcess in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in RunMyProcess moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in RunMyProcess.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met RunMyProcess, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een RunMyProcess testen](#creating-a-runmyprocess-test-user)** - hebben een tegenhanger van Britta Simon in RunMyProcess dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.


### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing RunMyProcess.

**Als u wilt configureren eenmalige aanmelding Azure AD met RunMyProcess, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **RunMyProcess** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij RunMyProcess** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** een URL waarbij het volgende patroon: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Klik op **volgende**

    > [AZURE.NOTE] Houd er rekening mee dat u de waarde wordt bijgewerkt met de werkelijke aanmelden moet op de URL. Als u deze waarde, neem contact op met het ondersteuningsteam RunMyProcess via <mailto:support@runmyprocess.com>.
 
4. Klik op de pagina **configureren eenmalige aanmelding bij RunMyProcess** op **Certificaat downloaden** en sla het bestand op uw computer:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. In een andere webbrowservenster, eenmalige aanmelding aan bij uw tenant RunMyProcess als beheerder.

6. Klik in het linkernavigatievenster, klikt u op **Account** en selecteer **configuratie**.

    ![Eenmalige aanmelding configureren op App-kant](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Ga naar de sectie **verificatiemethode** en Voer onderstaande stappen te volgen:

    ![Eenmalige aanmelding configureren op App-kant](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    een. Selecteer als **methode**, **SSO met Samlv2**.

    b. In de **SSO omleiden** plaatst tekstvak de waarde van **SAML SSO-URL** van de configuratiewizard van Azure AD-toepassing.

    c. In het **Afmelden omleiden** plaatst tekstvak de waarde van **Één Sign-Out Service-URL** van de configuratiewizard van Azure AD-toepassing.

    d. In de **Notatie voor de naam Id** plaatst tekstvak de waarde van de **Indeling van de id** van de configuratiewizard van Azure AD-toepassing.

    e. De inhoud van het gedownloade certificaatbestand Kopieer en plak deze in het tekstvak **certificaat** . 

    f. Klik op pictogram **Opslaan** .
    
8. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

9. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
 
    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster de volgende stappen uitvoeren: ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   

### <a name="creating-a-runmyprocess-test-user"></a>Maken van een gebruiker RunMyProcess testen
  
Als u wilt dat gebruikers Azure AD Meld u aan bij RunMyProcess, moeten ze worden ingericht in RunMyProcess. Bij RunMyProcess, met de inrichting van is een handmatige taak.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite RunMyProcess.

2.  Klik op **Account** en selecteer **gebruikers** in het linkernavigatievenster en klik op **Nieuwe gebruiker**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Nieuwe gebruiker")

3.  In de sectie **Instellingen van de gebruiker** , moet u de volgende stappen uitvoeren:

    ![Profiel] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profiel")

    een. Typ de **naam** en de **E-mail** van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.

    b. Selecteer een **taal IDE**, een **taal** en een **profiel**.

    c. Selecteer **account maken e-mail sturen naar mij**.

    d. Klik op **Opslaan**.

    >[AZURE.NOTE] U kunt een andere RunMyProcess gebruiker account hulpmiddelen voor het maken of API's verstrekt door RunMyProcess inrichten Azure Active Directory gebruikersaccounts.
    

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan RunMyProcess.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan RunMyProcess, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **RunMyProcess**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. Klik op de linkerbenedenhoek, balk op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.
 
Wanneer u op de tegel RunMyProcess in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw RunMyProcess-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
