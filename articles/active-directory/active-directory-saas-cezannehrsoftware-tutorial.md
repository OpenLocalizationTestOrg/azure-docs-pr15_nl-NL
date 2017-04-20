<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cezanne HR Software | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Cezanne HR Software."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Zelfstudie: Azure Active Directory-integratie met Cezanne HR Software

Het doel van deze zelfstudie is om aan te geven hoe u Cezanne HR Software integreren met Azure Active Directory (Azure AD).

Cezanne HR Software integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Cezanne HR Software heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Cezanne HR Software (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Cezanne HR Software, moet u de volgende items:

- Een Azure AD-abonnement
- Een Cezanne HR Software eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cezanne HR Software toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Cezanne HR Software toevoegen vanuit de galerie
Als u wilt de integratie van Cezanne HR Software in Azure AD configureren, moet u Cezanne HR Software vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Cezanne HR Software vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .
    
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Cezanne HR Software**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. Selecteer **Cezanne HR Software**in het deelvenster resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Cezanne HR Software op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Cezanne HR Software aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Cezanne HR Software moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Cezanne HR Software.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Cezanne HR Software, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een Cezanne HR Software testen](#creating-a-cezanne-hr-software-test-user)** - hebben een tegenhanger van Britta Simon in Cezanne HR Software die is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte u Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing Cezanne HR.

**Als u wilt configureren eenmalige aanmelding Azure AD met Cezanne HR Software, moet u de volgende stappen uitvoeren:**

1. In de klassieke-portal op de pagina van de integratie in de **Cezanne HR Software** -toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Cezanne HR Software** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. De volgende stappen uitvoeren en klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    een. Typ in het tekstvak **Aanmeldingsadres op URL** een URL waarbij het volgende patroon: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Typ in het tekstvak **Antwoord URL** een URL waarbij het volgende patroon: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c. Klik op **volgende**

    > [AZURE.NOTE] Houd er rekening mee dat u moet deze waarden bijwerken met de werkelijke Aanmeldingsadres op URL en de URL van de reactie. U kunt deze waarden contact opnemen met het ondersteuningsteam Cezanne HR Software via <mailto:info@cezannehr.com>.

4. Klik op de pagina **configureren eenmalige aanmelding bij Cezanne HR Software** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

5. In een andere webbrowservenster, eenmalige aanmelding aan bij uw tenant Cezanne HR Software als beheerder.

6. Klik op het linker navigatiedeelvenster, op **Systeeminstellingen**. Ga naar de **Beveiligingsinstellingen**. Ga naar de **Configuratie voor eenmalige aanmelding**.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. In de **gebruikers toestaan deel te Meld u aan met de volgende Service voor eenmalige aanmelding (SSO)** deelvenster het selectievakje **SAML 2.0** in en selecteert u de optie **Geavanceerde configuratie** ernaast.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Klik op de knop **Nieuw toevoegen** .

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. De volgende stappen uitvoeren op **SAML 2.0-IDENTITEITSPROVIDER** sectie.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    een. Voer de naam van uw identiteitsprovider als de **Naam weer te geven**.

    b. In de **Entiteit-id** plaatst tekstvak de waarde van **Entiteit-ID** van de configuratiewizard van Azure AD-toepassing.

    c. Wijzig de **SAML binden** aan 'Bericht'.

    d. In de **Beveiligingstoken Service-eindpunt** plaatst tekstvak de waarde van **Eenmalige aanmelding Service URL** van de configuratiewizard van Azure AD-toepassing.

    e. Typ 'http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name' in de **naam van de gebruiker-ID-kenmerk**.

    f. Klik op **uploaden** pictogram om het gedownloade certificaat van Azure AD uploaden.

    g. Klik op de knop **Ok** . 

10. Klik op de knop **Opslaan** .

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

12. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Typ in het tekstvak **Achternaam** **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Maken van een gebruiker Cezanne HR Software testen

Als u wilt dat gebruikers Azure AD Meld u aan bij Cezanne HR Software, moeten ze worden ingericht in Cezanne HR Software. Bij Cezanne HR Software, met de inrichting van is een handmatige taak.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Als u wilt een gebruikersaccount inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw bedrijfssite Cezanne HR Software als beheerder.

2.  Klik op het linker navigatiedeelvenster, op **Systeeminstellingen**. Ga naar **gebruikers beheren**. Ga naar de **Nieuwe gebruiker toevoegen**.

    ![Nieuwe gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Nieuwe gebruiker")

3.  Voer op de sectie **Details van de persoon** onderstaande stappen te volgen:

    ![Nieuwe gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Nieuwe gebruiker")

    een. **Interne gebruiker** instellen als uit.

    b. Typ in het tekstvak **Voornaam** **Britta**.  

    c. Typ in het tekstvak **Achternaam** **Simon**.

    d. Typ het e-mailadres van Britta Simon account in het tekstvak **E-mail** .

4.  Voer op de sectie **Accountgegevens** onderstaande stappen te volgen:

    ![Nieuwe gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Nieuwe gebruiker")

    een. Typ in het tekstvak **gebruikersnaam** het e-mailadres van Britta Simon.

    b. Typ in het tekstvak **wachtwoord** het wachtwoord van account Britta Simon.

    c. Selecteer **HR Professional** als **rol**.

    d. Klik op **OK**.

5. Ga naar het tabblad **Eenmalige aanmelding** en selecteer **Nieuwe toevoegen** in het gebied **SAML 2.0-id's** .

    ![Gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Gebruiker")

6. Kies uw id-Provider voor de **Id-Provider** en in het tekstvak van de **Gebruikers-id**, voer het e-mailadres van Britta Simon-account.

    ![Gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Gebruiker")
    
7. Klik op de knop **Opslaan** .

    ![Gebruiker] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Gebruiker")


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Cezanne HR Software.
    
![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon naar Cezanne HR Software, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **Cezanne HR Software**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.
    
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.
 
Wanneer u op de tegel Cezanne HR Software in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Cezanne HR Software-toepassing.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
