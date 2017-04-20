<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Proofpoint op aanvraag | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Proofpoint op aanvraag."
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
    ms.date="10/05/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a>Zelfstudie: Azure Active Directory-integratie met Proofpoint op aanvraag

In deze zelfstudie leert u hoe u Proofpoint op aanvraag integreren met Azure Active Directory (Azure AD).

Proofpoint op aanvraag integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Proofpoint op aanvraag heeft.
- Hier kunt u uw gebruikers kunnen automatisch ophalen aangemeld bij Proofpoint op aanvraag (eenmalige aanmelding of SSO) met hun Azure AD-accounts.
- U kunt uw accounts op één centrale locatie, de Azure klassieke-portal beheren.

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Proofpoint op aanvraag, moet u de volgende items:

- Een Azure AD-abonnement
- Een Proofpoint op aanvraag eenmalige aanmelding-abonnement


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u [ophalen van een proefversie één maand](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Proofpoint op aanvraag toevoegen vanuit de galerie.
2. Configureer en test eenmalige aanmelding Azure AD.


## <a name="add-proofpoint-on-demand-from-the-gallery"></a>Proofpoint op aanvraag toevoegen vanuit de galerie
Als u wilt configureren de integratie van Proofpoint op aanvraag in Azure AD, moet u Proofpoint op aanvraag uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Klik in het Azure op klassieke portal in het linkernavigatiedeelvenster, **Active Directory**.

    ![Active Directory-pictogram][1]
2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u **toepassingen** op in het menu aan de bovenkant.

    ![TOEPASSINGEN menu-item][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Knop toevoegen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** op **toevoegen een toepassing in de galerie**.

    ![Keuze van het toevoegen van een toepassing in de galerie][4]

6. Typ in het zoekvak **Proofpoint op aanvraag**.

    ![Vak waarin u "Proofpoint op aanvraag" typt](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_01.png)

7. Selecteer **Proofpoint op aanvraag**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.



##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configureer en test eenmalige aanmelding Azure AD
In deze sectie, configureert en Azure AD eenmalige aanmelding met Proofpoint op aanvraag op basis van een testgebruiker met de naam Britta Simon testen.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Proofpoint op aanvraag is aan een gebruiker in Azure AD. Met andere woorden, moet u een koppeling relatie definiëren tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Proofpoint op aanvraag.

U maken deze koppeling-relatie door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Proofpoint op aanvraag.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Proofpoint op aanvraag, voert u de volgende procedures uit:

1. [Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on), zodat uw gebruikers kunnen deze functie gebruiken.
2. [Maken van een gebruiker van Azure AD testen](#creating-an-azure-ad-test-user), testen Azure AD eenmalige aanmelding met Britta Simon.
3. [Maken van een Proofpoint op aanvraag testgebruiker](#creating-a-proofpoint-ondemand-test-user), moet een tegenhanger van Britta Simon in Proofpoint op aanvraag dat is gekoppeld aan de Azure AD-weergave van haar.
4. [De gebruiker Azure AD-test toewijzen](#assigning-the-azure-ad-test-user), om in te schakelen Britta Simon gebruik eenmalige aanmelding Azure AD.
5. [Eenmalige aanmelding testen](#testing-single-sign-on)om te bevestigen dat de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw Proofpoint op aanvraag-toepassing.

1. Klik in de klassieke-portal, klik op de pagina **Proofpoint op aanvraag** -toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** te openen.

    ![Knop "Configureren eenmalige aanmelding"][6]

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Proofpoint op aanvraag** **Eenmalige aanmelding Microsoft Azure AD**selecteren en klik vervolgens op **volgende**.

    !["Microsoft Azure AD voor eenmalige aanmelding" keuzerondje](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_03.png)

3. Klik op de pagina **App-instellingen configureren** de volgende stappen uitvoeren:

    ![Pagina met vakken ingevuld "App-instellingen configureren"](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_04.png)

    een. Typ in het vak **AANMELDINGSADRES op URL** de URL waar gebruikers meldt u zich aan bij uw Proofpoint op aanvraag-toepassing. Het volgende patroon volgen: **https://\<hostname\>.pphosted.com/ppssamlsp_hostname**

    b. Typ in het vak **id** de URL met behulp van de volgende patroon: **https://\<hostname / >.pphosted.com/ppssamlsp**

    c. Typ in het vak **Beantwoorden URL** de URL met behulp van de volgende patroon: **https://\<hostname / >.pphosted.com:portnumber/v1/samlauth/samlconsumer**

    d. Klik op **volgende**.

4. Klik op de pagina **configureren eenmalige aanmelding bij Proofpoint op aanvraag** moet u de volgende stappen uitvoeren:

    !['Configureren eenmalige aanmelding op Proofpoint op aanvraag' pagina met "Certificaat als Download"-knop](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met de Proofpoint op aanvraag-ondersteuningsteam en geef met de volgende items:

    • Het gedownloade certificaat

    • De afdelings-ID

    • De SAML SSO-URL

6. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Selectievakje in dat u bevestigt dat u hebt geconfigureerd eenmalige aanmelding][10]

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Bevestigingspagina][11]


### <a name="create-an-azure-ad-test-user"></a>Een gebruiker Azure AD-test maken
In dit gedeelte maakt u een testgebruiker met de naam Britta Simon in de klassieke portal.


![De testgebruiker in de lijst met gebruikers][20]

1. Klik in het Azure op klassieke portal in het linkernavigatiedeelvenster, **Active Directory**.

    ![Active Directory-pictogram](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![GEBRUIKERS menu-item](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png)

4. Klik op **Gebruiker toevoegen**om te openen op de werkbalk onderaan van het dialoogvenster **Gebruiker toevoegen** .

    ![Knop gebruiker toevoegen](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png)

5. Klik op de pagina **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  !['Laat het ons weten deze gebruiker' pagina met vakken ingevuld](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_05.png)

    een. Selecteer in het vak **TYPE van gebruiker** , **nieuwe gebruiker in uw organisatie**.

    b. Typ in het vak **Gebruikersnaam in te voeren** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **gebruikersprofiel** de volgende stappen uitvoeren: ![de pagina "gebruikersprofiel" met vakken ingevuld](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_06.png)

    een. Typ in het vak **VOORNAAM** **Britta**.  

    b. Typ in het vak **ACHTERNAAM** **Simon**.

    c. Typ in het vak **WEERGAVENAAM** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina **krijgen tijdelijke wachtwoord** .

    ![Knop voor het maken van een tijdelijk wachtwoord](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_07.png)

8. Klik op de pagina **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Pagina met een wachtwoord info 'Krijgt tijdelijke wachtwoord'](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_08.png)

    een. Noteer de waarde in het vak **Nieuw wachtwoord** .

    b. Klik op **Voltooien**.   



### <a name="create-a-proofpoint-on-demand-test-user"></a>Een Proofpoint op aanvraag testgebruiker maken

In dit gedeelte maakt u een gebruiker Britta Simon in Proofpoint op aanvraag genoemd. Neem werken met Proofpoint op aanvraag-ondersteuningsteam u gebruikers kunt toevoegen in de Proofpoint op aanvraag-platform.


### <a name="assign-the-azure-ad-test-user"></a>Toewijzen aan de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Proofpoint op aanvraag inschakelen.

![Gebruikersgegevens, met de toegang tot en met de directe methode ingeschakeld][200]

1. Klik op **toepassingen** in het menu boven aan de weergave van toepassingen openen in de klassieke-portal, klik in de directoryweergave.

    ![TOEPASSINGEN menu-item][201]

2. Selecteer in de lijst met toepassingen, **Proofpoint op aanvraag**.

    ![Lijst met toepassingen met Proofpoint op aanvraag is geselecteerd](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_50.png)

3. Klik op **gebruikers**in het menu aan de bovenkant.

    ![GEBRUIKERS menu-item][203]

4. Selecteer in de lijst met gebruikers, **Britta Simon**.

5. Klik op de werkbalk onderaan op **toewijzen**.

    ![Knop toewijzen][205]


### <a name="test-single-sign-on"></a>Test eenmalige aanmelding

In deze sectie, kunt u uw Azure AD eenmalige aanmelding configuratie testen met behulp van het Access-deelvenster.

Wanneer u op de tegel **Proofpoint op aanvraag** in het deelvenster Access, moet u worden automatisch aangemeld bij uw Proofpoint op aanvraag-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_205.png
