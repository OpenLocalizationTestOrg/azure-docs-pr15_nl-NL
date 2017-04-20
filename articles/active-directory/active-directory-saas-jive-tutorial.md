<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Jive | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Jive configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Zelfstudie: Azure Active Directory-integratie met Jive

In deze zelfstudie leert u hoe u Jive integreren met Azure Active Directory (Azure AD).

Jive integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Jive heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Jive (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Jive, moet u de volgende items:

- Een Azure AD-abonnement
- Een Jive eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Jive toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-jive-from-the-gallery"></a>Jive toevoegen vanuit de galerie
Als u wilt de integratie van Jive in Azure AD configureren, moet u Jive uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Jive vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]
2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Jive**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. Selecteer **Jive**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Jive op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Jive in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Jive moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Jive.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Jive, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een Jive testen](#creating-a-jive-test-user)** - hebben een tegenhanger van Britta Simon in Jive dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Configuratie van de inrichting van de gebruiker](#configuring-user-provisioning)** - overzicht maken van het inschakelen van de gebruiker de inrichting van Active Directory-gebruiker-mailaccounts naar Jive.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
6. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Jive.

**Als u wilt configureren eenmalige aanmelding Azure AD met Jive, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **Jive** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u gebruikers aanmelden bij Jive** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Jive-toepassing met het volgende patroon: **https://\<klantnaam\>. jivecustom.com**.
    
    b. Klik op **volgende**
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Jive** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Eenmalige aanmelding aan bij uw tenant Jive als beheerder.

6. In het menu aan de bovenkant, klikt u op "**Saml**".

    ![Eenmalige aanmelding configureren op App-kant](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    een. Selecteer **ingeschakeld** onder het tabblad **Genaral** .

    b. Klik op de knop '**alle saml-instellingen opslaan**'.

7. Ga naar het tabblad "**Idp metagegevens**".

    ![Eenmalige aanmelding configureren op App-kant](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    een. De inhoud van het gedownloade metagegevens XML-bestand kopiëren en plak deze in het tekstvak **identiteit Provider (IDP)-metagegevens** .

    b. Klik op de knop '**alle saml-instellingen opslaan**'. 

8. Ga naar het tabblad '**User-kenmerk Mapping**'.

    ![Eenmalige aanmelding configureren op App-kant](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    een. In het tekstvak **e** Kopieer en plak de naam van het kenmerk van **e** -waarde.

    b. In het **eerste** tekstvak, kopieer en plak de naam van het kenmerk van **givenname** waarde.

    c. In het tekstvak **Achternaam** Kopieer en plak de naam van het kenmerk van **Achternaam** waarde.
    
9. Klik in de portal Azure AD selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
![Azure AD voor eenmalige aanmelding][10]

10. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
  ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster de volgende stappen uitvoeren: ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



###<a name="creating-a-jive-test-user"></a>Maken van een gebruiker Jive testen

In dit gedeelte maakt u een gebruiker Britta Simon in Jive genoemd. Neem werken met het ondersteuningsteam Jive om toe te voegen van de gebruikers in het platform Jive.


###<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de gebruiker de inrichting van Active Directory-gebruikersaccounts naar Jive.  
Als onderdeel van deze procedure bent u moet het beveiligingstoken van een gebruiker moet u aanvraagt bij Jive.com opgeven.
  
De volgende schermafbeelding ziet u een voorbeeld van het bijbehorende dialoogvenster in Azure AD:

![Gebruiker inrichting configureren] (./media/active-directory-saas-jive-tutorial/IC698794.png "Gebruiker inrichting configureren")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Klik in de beheerportal Azure, klik op de pagina **Jive** toepassing integratie op **inrichting van de gebruiker configureren** om het dialoogvenster **Gebruiker inrichting configureren** .

2.  Klik op de pagina **Voer uw referenties Jive om in te schakelen automatische gebruiker inrichting** bevatten de volgende configuratieinstellingen:

    1.  Typ een naam voor het account van Jive met het profiel van de **Systeembeheerder** in Jive.com toegewezen in het tekstvak **Jive beheerder gebruikersnaam in te voeren** .

    2.  Typ het wachtwoord voor dit account in het tekstvak **Jive beheerderswachtwoord** .

    3.  Typ in het tekstvak **Jive Tenant URL** de URL van de tenant Jive.

        >[AZURE.NOTE]De URL van de tenant Jive is de URL die wordt gebruikt door uw organisatie aan te melden bij Jive.  
        Meestal de URL heeft de volgende indeling: **www.\< organisatie\>. jive.com**.

    4.  Klik op **valideren** om te controleren of uw configuratie.

    5.  Klik op de knop **volgende** om de pagina **bevestiging** te openen.

3.  Klik op **de bevestigingspagina** op het vinkje om op te slaan van uw configuratie.
  
U kunt nu een testaccount maken, 10 minuten wachten en controleer of dat het account is gesynchroniseerd met Jive.com.




### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Jive inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Jive, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Jive**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de tegel Jive in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Jive-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
