<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met & eerlijk gezegd | Microsoft Azure"
    description="Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en & eerlijk gezegd."
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
    ms.date="08/12/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Zelfstudie: Azure Active Directory-integratie met & eerlijk gezegd

Het doel van deze zelfstudie is om aan te geven hoe u kunt integreren & eerlijk gezegd met Azure Active Directory (Azure AD).

Integratie van & eerlijk gezegd met Azure AD bevat de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot & eerlijk gezegd heeft
- U kunt uw gebruikers u automatisch aangemeld op naar & eerlijk gezegd (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD nodig-integratie met & eerlijk gezegd u het volgende:

- Een Azure AD-abonnement
- A & eerlijk gezegd eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Toevoegen en eerlijk gezegd vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-frankly-from-the-gallery"></a>Toevoegen en eerlijk gezegd vanuit de galerie
Als u wilt de integratie van & eerlijk gezegd in Azure AD configureren, moet u toevoegen & eerlijk gezegd vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Tot toevoegen & eerlijk gezegd uit de galerie met de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .
    
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **en eerlijk gezegd**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_01.png)

7. In het deelvenster resultaten **& eerlijk gezegd**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe u configureert en test Azure AD eenmalige aanmelding met & eerlijk gezegd op basis van de testgebruiker van een 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in & eerlijk gezegd aan een gebruiker in Azure AD is. Met andere woorden, moet een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in en eerlijk gezegd tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in & eerlijk gezegd.

Als u wilt configureren en testen van Azure AD moet eenmalige aanmelding met & eerlijk gezegd, u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een & eerlijk gezegd testgebruiker](#creating-a-&frankly-test-user)** - heeft een tegenhanger van Britta Simon in & eerlijk gezegd dat wil zeggen gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte u Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw & eerlijk gezegd toepassing.

**Voor het configureren van Azure AD één aanmelding met & eerlijk gezegd, de volgende stappen uitvoeren:**

1. In de klassieke-portal op de eerlijk gezegd toepassing Integratiepagina **en** Klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden op & eerlijk gezegd** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_03.png)

3. Klik op de pagina van het dialoogvenster **App-instellingen configureren** als u configureren van de toepassing in **IDP modus gestart wilt**, de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_04.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **id** :`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. Typ een URL waarbij het volgende patroon in het tekstvak **URL beantwoorden** :`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

    c. Klik op **volgende**

4. Als u configureren van de toepassing in **SP modus gestart** op de pagina van het dialoogvenster **App-instellingen configureren wilt** , klik op de **"Weergeven geavanceerde instellingen (optioneel)"** en voer vervolgens de **Aanmelding op URL** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_05.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **Aanmeldingsadres op URL** :`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`

    b. Klik op **volgende**

    > [AZURE.NOTE] Houd er rekening mee dat deze niet de reële waarden zijn. U moet deze waarden bijwerken met de werkelijke Aanmeldingsadres op URL, id en beantwoorden URL. Contactpersoon [help@andfrankly.com](emailTo:help@andfrankly.com) om deze waarden.

5. Klik op de **configureren eenmalige aanmelding bij & eerlijk gezegd** pagina de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_06.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

6. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw & eerlijk gezegd ondersteuningsteam via [help@andfrankly.com](emailTo:help@andfrankly.com). De gedownloade metagegevensbestand bijvoegen en delen en eerlijk gezegd teamsites instellen van eenmalige aanmelding in hun kant.

7. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

8. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_05.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_07.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-frankly-test-user"></a>Maken van een & eerlijk gezegd testgebruiker

In dit gedeelte maakt u een gebruiker met de naam van Britta Simon in & eerlijk gezegd. Neem werken met & eerlijk gezegd ondersteuningsteam via [help@andfrankly.com](emailTo:help@andfrankly.com) om toe te voegen van de gebruikers in & eerlijk gezegd platform.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen tot & eerlijk gezegd.
    
![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon naar & eerlijk gezegd, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst toepassingen **en eerlijk gezegd**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_50.png)

1. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.
    
    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.
 
Wanneer u klikt op & eerlijk gezegd-tegel in het deelvenster Access, ontvangt u automatisch aangemeld voor toegang tot uw & eerlijk gezegd toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_205.png
