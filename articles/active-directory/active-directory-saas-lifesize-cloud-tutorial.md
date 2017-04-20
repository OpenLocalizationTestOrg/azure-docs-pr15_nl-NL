<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Lifesize Cloud | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Lifesize Cloud."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Zelfstudie: Azure Active Directory-integratie met Lifesize Cloud

In deze zelfstudie leert u hoe u Lifesize Cloud integreren met Azure Active Directory (Azure AD).

Lifesize Cloud integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot Lifesize Cloud heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Lifesize Cloud (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Lifesize Cloud, moet u de volgende items:

- Een Azure AD-abonnement
- Een Lifesize Cloud eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Lifesize Cloud toe te voegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize Cloud toe te voegen vanuit de galerie
Als u wilt de integratie van Lifesize Cloud in Azure AD configureren, moet u Lifesize Cloud uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Lifesize Cloud vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]
2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Lifesize Cloud**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. Selecteer **Lifesize Cloud**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In dit gedeelte u Configureer en test eenmalige aanmelding Azure AD met Lifesize Cloud op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in de Lifesize Cloud in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in de Lifesize Cloud moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in de Lifesize Cloud.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Lifesize Cloud, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een wolk Lifesize testen](#creating-a-lifesize-cloud-test-user)** - hebben een tegenhanger van Britta Simon in Lifesize Cloud dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In deze sectie, Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing Lifesize Cloud.


**Als u wilt configureren eenmalige aanmelding Azure AD met Lifesize Cloud, kunt u de volgende stappen uitvoeren:**

1. In de klassieke-portal op de pagina van de integratie in de **Lifesize Cloud** toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Lifesize Cloud** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Lifesize Cloud-toepassing met het volgende patroon: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Klik op **volgende**
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Lifesize Cloud** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    een. Klik op **downloaden**certificaat en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Om eenmalige aanmelding configureren voor uw toepassing, login in de toepassing Lifesize Cloud met beheerdersbevoegdheden.

6. In de rechterbovenhoek klikt u op uw naam en klik op de **Volgende instellingen**

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. In de geavanceerde instellingen nu Klik op de koppeling **SSO configuratie** . Hiermee opent u de pagina configuratie voor eenmalige aanmelding voor uw exemplaar.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Nu de volgende waarden in de gebruikersinterface voor eenmalige aanmelding configuratie configureren.    

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • De waarde van de URL van de uitgever van Azure AD kopiëren en plakken die in **Identiteit Provider uitgever** tekstvak.

    • De waarde van externe aanmeldings-URL van Azure AD kopiëren en plakken die in tekstvak **Aanmeldings-URL** .

    • Opent u het gedownloade certificaat in Kladblok en kopieer de inhoud van het certificaat, met uitzondering van de lijnen certificaat Begin en einde het certificaat en plak deze in het tekstvak **X.509-certificaat** .

    • In de toewijzing SAML-kenmerk voor het tekstvak **Voornaam** Voer de waarde als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • In de toewijzing SAML-kenmerk voor het tekstvak **Achternaam** Voer de waarde als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • In de toewijzing SAML-kenmerk voor het tekstvak **e** Voer de waarde als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Als u wilt controleren van de configuratie kunt u klikken op de knop **testen** .

    > [AZURE.NOTE] Voor het testen van succesvolle moet u de configuratiewizard voltooid in Azure AD en Daarnaast bieden toegang aan gebruikers of groepen die de test kunnen uitvoeren.
    
10. De SSO inschakelen door te schakelen op de knop **SSO inschakelen** .

11. Klik nu op de knop **bijwerken** zodat alle instellingen worden opgeslagen. Hiermee wordt de waarde RelayState genereren. Kopieer de waarde RelayState dat in het tekstvak wordt gegenereerd. Moeten we deze waarde in de volgende stappen.

12. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

13. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
 
    ![Azure AD voor eenmalige aanmelding][11]

14. Nu aanmelden in de beheerportal van Azure **https://portal.azure.com** met de beheerdersreferenties

15. Klik op **Meer Services** de koppeling in het linkernavigatiedeelvenster
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Zoeken naar Azure Active Directory en klik op de koppeling **Azure Active Directory**
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. U vindt u al uw toepassingen van SaaS onder de knop **Bedrijfstoepassingen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Klik nu op de koppeling **Alle toepassingen** in het volgende blad
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Lifesize-toepassing waarvoor u voor het instellen van de RelayState wilt zoeken. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Klik nu op de koppeling naar **eenmalige aanmelding** in het blad

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Hier ziet u het selectievakje **weergeven geavanceerde instellingen van de URL** in. Klik op het selectievakje in.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Nu de RelayState voor de toepassing, zoals u kunt in de configuratiepagina van Lifesize toepassing eenmalige aanmelding zien configureren. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. De instellingen opslaan.

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster de volgende stappen uitvoeren: ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Maken van een gebruiker Lifesize Cloud testen

In dit gedeelte maakt u een gebruiker Britta Simon genoemd in de Lifesize Cloud. Lifesize cloud biedt ondersteuning voor automatische gebruiker inrichten. Na een succesvolle verificatie bij Azure AD de gebruiker automatisch de gegevens in de toepassing. 


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Lifesize Cloud inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon Lifesize cloud, kunt u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Lifesize Cloud**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de tegel Lifesize Cloud in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Lifesize Cloud-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
