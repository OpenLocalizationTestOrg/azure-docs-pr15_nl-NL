<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite | Microsoft Aure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Questetra BPM-Suite."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Zelfstudie: Azure Active Directory-integratie met Questetra BPM Suite

Het doel van deze zelfstudie is om aan te geven hoe u Questetra BPM Suite integreren met Azure Active Directory (Azure AD).  
Questetra BPM Suite integreren met Azure AD biedt de volgende voordelen: 

- U kunt bepalen in Azure AD wie toegang tot Questetra BPM Suite heeft 
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Questetra BPM Suite (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor 

Als u wilt configureren Azure AD-integratie met Questetra BPM Suite, moet u de volgende items:

- Een Azure AD-abonnement
- Een [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 

 
## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Questetra BPM Suite toevoegen vanuit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Questetra BPM Suite toevoegen vanuit de galerie
Als u wilt de integratie van Questetra BPM Suite in Azure AD configureren, moet u Questetra BPM Suite uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Questetra BPM Suite vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Questetra BPM-Suite**.

    ![Toepassingen][5]

7. Klik in het resultatendeelvenster Selecteer **Questetra BPM Suite**en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Questetra BPM Suite op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Questetra BPM Suite aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Questetra BPM Suite moet met andere woorden, tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Questetra BPM-Suite.
 
Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Questetra BPM Suite, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Questetra BPM-Suite testen](#creating-a-questetra-bpm-suite-test-user)** - hebben een tegenhanger van Britta Simon in Questetra BPM Suite dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Questetra BPM-Suite.

**Als u wilt configureren eenmalige aanmelding Azure AD met Questetra BPM Suite, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **Questetra BPM Suite** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][8]

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Questetra BPM Suite** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][9]


3. In een browservenster verschillende web en meld u aan bij uw bedrijfssite **Questetra BPM Suite** als beheerder.

4. In het menu aan de bovenkant, klikt u op **Systeeminstellingen**. 

    ![Azure AD voor eenmalige aanmelding][10]

5. Open de pagina **SingleSignOnSAML** , klikt u op **Eenmalige aanmelding (SAML)**. 

    ![Azure AD voor eenmalige aanmelding][11]


6. Klik in de klassieke Azure portal op de pagina van het dialoogvenster **App-instellingen configureren** de volgende stappen uitvoeren: 

    ![App-instellingen configureren][13]
 
    een. Op u **Questetra BPM Suite** bedrijfssite, in de sectie SP de **ACS URL**kopiëren en plak deze in het tekstvak **Aanmeldingsadres op URL** .

    b. Op u **Questetra BPM Suite** bedrijfssite, in de sectie SP de **Entiteit-ID**Kopieer en plak deze in het tekstvak **URL van de uitgever** .

    c. Op u **Questetra BPM Suite** bedrijfssite, in de sectie SP de **ACS URL**kopiëren en plak deze in het tekstvak **URL beantwoorden** .

    d. Klik op **volgende**.

 
7. Klik op de pagina **configureren eenmalige aanmelding bij Questetra BPM Suite** op **certificaat downloaden**en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren][14]


8. Op uw **Questetra BPM Suite** bedrijfssite, moet u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren][15]

    een. Selecteer **inschakelen eenmalige aanmelding**.
     
    b. In de klassieke Azure portal, de **Uitgever URL** -waarde Kopieer en plak deze in het tekstvak **Entiteit-ID** .

    c. In de klassieke Azure portal, de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **aanmeldingsproblemen URL voor de aanmeldingspagina** .

    d. Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure portal, en plak deze in het tekstvak **URL voor de aanmeldingspagina op Afmelden** .

    e. Typ in het tekstvak **opmaken NameID** **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: emailAddress**.


    f. Een basis-64 gecodeerd-bestand uit uw gedownloade certificaat maken. 

    >[AZURE.TIP] Zie [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.

    g. Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plak deze in het tekstvak **validatiecertificaat** . 

    h. Klik op **Opslaan**.


9. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**. 

    ![Wat is Azure AD Connect][17]


10. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Wat is Azure AD Connect][18]




### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Azure AD testgebruiker maken][100] 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Azure AD testgebruiker maken][101] 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**. 

    ![Azure AD testgebruiker maken][102] 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Azure AD testgebruiker maken][103]
 
    een. Als het **Type van gebruiker**, selecteert u **nieuwe gebruiker in uw organisatie**.
  
    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op volgende.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Azure AD testgebruiker maken][104] 
  
    een. Typ in het tekstvak **Voornaam** **Britta**. 
 
    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Azure AD testgebruiker maken][105]  

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Azure AD testgebruiker maken][106]   
  
    een. Noteer de waarde van het **Nieuwe wachtwoord**in.
  
    b. Klik op **Voltooien**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Maken van een gebruiker Questetra BPM Suite testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in Questetra BPM-Suite genoemd.

**Als u wilt maken van een gebruiker Britta Simon in Questetra BPM-Suite genoemd, moet u de volgende stappen uitvoeren:**

1.  Aanmelding bij uw bedrijfssite Questetra BPM Suite als beheerder.
2.  Ga naar **Systeeminstellingen > gebruikerslijst > nieuwe gebruiker**. 
3.  Klik in het dialoogvenster Nieuwe gebruiker, moet u de volgende stappen uitvoeren: 

    ![Testgebruiker maken][300] 

    een. Typ in **het tekstvak** van Britta gebruikersnaam in te voeren in Azure AD.

    b. Typ in het tekstvak **e-mailbericht** van Britta gebruikersnaam in te voeren in Azure AD.

    c. Typ in het tekstvak **wachtwoord** een wachtwoord.

4.  Klik op **nieuwe gebruiker toevoegen**.



### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van haar toegang aan Questetra BPM-Suite.

![Wat is Azure AD Connect][200]

**Als u wilt toewijzen Britta Simon Questetra BPM Suite, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Wat is Azure AD Connect][201]

2. Selecteer in de lijst met toepassingen **Questetra BPM-Suite**.

    ![Wat is Azure AD Connect][205]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Wat is Azure AD Connect][202]

1. Selecteer in de lijst gebruikers **Britta Simon**.

    ![Wat is Azure AD Connect][203]

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Wat is Azure AD Connect][204]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de tegel Questetra BPM Suite in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Questetra BPM Suite-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 