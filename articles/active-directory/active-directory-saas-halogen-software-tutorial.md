<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met halogeen-Software"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en halogeen Software."
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


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Zelfstudie: Azure Active Directory-integratie met halogeen-Software

Het doel van deze zelfstudie is om aan te geven hoe u halogeen Software integreren met Azure Active Directory (Azure AD).

Halogeen Software integreren met Azure AD biedt de volgende voordelen: 

- U kunt bepalen in Azure AD wie toegang tot halogeen-Software heeft 
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot de Software halogeen (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor 

Als u wilt configureren Azure AD-integratie met halogeen Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een halogeen Software eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 

 
## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving. 

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Halogeen Software toevoegen vanuit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-halogen-software-from-the-gallery"></a>Halogeen Software toevoegen vanuit de galerie
Als u wilt de integratie van halogeen Software in Azure AD configureren, moet u halogeen Software vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen halogeen Software vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina. 

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **halogeen software**.

    ![Toepassingen][5]

7. Selecteer **Halogeen Software**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met halogeen Software op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in halogeen Software aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in halogeen Software moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in halogeen Software.
 
Als u wilt configureren en te testen Azure AD eenmalige aanmelding met halogeen Software, die u moet uitvoeren van de volgende elementen:

1. **[Configureren Azure AD één eenmalige aanmelding](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Software halogeen testen](#creating-a-halogen-software-test-user)** - hebben een tegenhanger van Britta Simon in halogeen-Software die is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Eenmalige aanmelding Azure AD één configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing halogeen.


**Als u wilt configureren eenmalige aanmelding Azure AD met halogeen Software, moet u de volgende stappen uitvoeren:**

1. Klik in de Azure klassieke portal op de pagina van de integratie in de **Halogeen Software** -toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][8]

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de Software halogeen** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][9]

3. Klik op de pagina **App-instellingen configureren** dialoogvenster de volgende stappen uitvoeren:  ![App-instellingen configureren][10]
 
     een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw halogeen softwaretoepassing met het volgende patroon: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Klik op **volgende**.
 
4. Klik op de pagina **configureren eenmalige aanmelding bij halogeen Software** op **metagegevens downloaden**en sla het metagegevensbestand lokaal op uw computer.
    
    ![Wat is Azure AD Connect][11]

5. In een ander browservenster, eenmalige aanmelding in uw toepassing **Halogeen Software** als beheerder.

6. Klik op het tabblad **Opties** . 

    ![Wat is Azure AD Connect][12]


7. Klik in het linkernavigatiedeelvenster op **SAML-configuratie**. 

    ![Wat is Azure AD Connect][13]

8. Klik op de pagina **Op SAML-configuratie** de volgende stappen uitvoeren:  ![wat Azure AD Connect is][14]

    een. Als **De unieke id**, selecteert u **NameID**.

    b. Selecteer de **gebruikersnaam**als **Unieke id gerelateerd aan**.

    c. Als u wilt uw gedownloade metagegevensbestand uploaden, klikt u op **Bladeren** om het bestand te selecteren en vervolgens op **Bestand uploaden**.

    d. Klik op **Uitvoeren testen**om de configuratie. 

    > [AZURE.NOTE] U moet u wachten voor het bericht '*de SAML test is voltooid. Sluit dit venster*". Sluit het browservenster geopend. Het selectievakje **SAML inschakelen** is alleen beschikbaar als de test is voltooid.

    e. Selecteer **SAML inschakelen**.
    
    f. Klik op **wijzigingen opslaan**. 


9. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten. 

    ![Wat is Azure AD Connect][15]

10. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Wat is Azure AD Connect][16]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Wat is Azure AD Connect][100] 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Wat is Azure AD Connect][101] 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**. 

    ![Wat is Azure AD Connect][102] 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Wat is Azure AD Connect][103] 
 
    een. Als het **Type van gebruiker**, selecteert u **nieuwe gebruiker in uw organisatie**.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op volgende.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Wat is Azure AD Connect][104] 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Achternaam** txtbox, type, **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Wat is Azure AD Connect][105]  

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Wat is Azure AD Connect][106]   

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.
    b. Klik op **Voltooien**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Maken van een gebruiker halogeen Software testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in halogeen Software genoemd.

**Als u wilt maken van een gebruiker Britta Simon in halogeen Software genoemd, moet u de volgende stappen uitvoeren:**

1. Meld u aan uw toepassing **Halogeen Software** als beheerder.

2. Klik op het tabblad van het **Midden van de gebruiker** en klik vervolgens op **Maak gebruiker**.

    ![Wat is Azure AD Connect][300]  

3. Klik op de pagina van het dialoogvenster **Nieuwe gebruiker** , moet u de volgende stappen uitvoeren:

    ![Wat is Azure AD Connect][301]

    een. Typ in het tekstvak **Voornaam** **Britta**. 
  
    b. Typ in het tekstvak **Achternaam** **Simon**.
  
    c. Typ in het tekstvak **gebruikersnaam** **van Brita Simon gebruikersnaam in te voeren in de portal van Azure klassieke**.
  
    d. Typ in het tekstvak **wachtwoord** een wachtwoord voor Britta.
  
    e. Klik op **Opslaan**.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan halogeen Software.

![Wat is Azure AD Connect][200]

**Als u wilt toewijzen Britta Simon naar halogeen Software, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Wat is Azure AD Connect][201]

2. Selecteer in de lijst met toepassingen **Halogeen Software**.

    ![Wat is Azure AD Connect][202]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Wat is Azure AD Connect][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

    ![Wat is Azure AD Connect][204]

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Wat is Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel halogeen Software in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw halogeen softwaretoepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png