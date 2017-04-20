<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Qlik komen Enterprise | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Qlik komen Enterprise."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Zelfstudie: Azure Active Directory-integratie met Qlik komen Enterprise

In deze zelfstudie leert u hoe u Qlik komen Enterprise integreren met Azure Active Directory (Azure AD).

Qlik komen Enterprise integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot Qlik komen Enterprise heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Qlik komen Enterprise (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Qlik komen Enterprise, moet u de volgende items:

- Een Azure AD-abonnement
- Een Qlik komen Enterprise eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Qlik komen Enterprise toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik komen Enterprise toevoegen vanuit de galerie
Als u wilt de integratie van Qlik komen Enterprise in Azure AD configureren, moet u Qlik komen onderneming vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen Qlik komen onderneming vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]
2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **Qlik komen Enterprise**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. Selecteer **Qlik komen Enterprise**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Qlik komen Enterprise op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Qlik komen Enterprise in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Qlik komen Enterprise moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Qlik komen onderneming.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Qlik komen Enterprise, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een Qlik komen Enterprise testen](#creating-a-qliksense-enterprise-test-user)** - hebben een tegenhanger van Britta Simon in Qlik komen Enterprise dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte u Azure AD eenmalige aanmelding in de klassieke portal in inschakelen en configureren eenmalige aanmelding uw bedrijfstoepassing Qlik komen.


**Als u wilt configureren eenmalige aanmelding Azure AD met Qlik komen Enterprise, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **Qlik komen Enterprise** -toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Qlik komen Enterprise** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    een. Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw Qlik komen Enterprise-toepassing met het volgende patroon: **https://\<Qlik komen volledig Qualifed Hostname\>: 443 / < virtuele Proxy voorvoegsel\>/samlauthn/**.
    
    > [AZURE.NOTE] Opmerking de afsluitende slash aan het einde van dit URI.  Dit is vereist.

    b. Klik op **volgende**
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Qlik komen Enterprise** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.  Bereid dit metagegevensbestand bewerken voordat u uploadt naar de server Qlik komen.

    b. Klik op **volgende**.

5. Bereid het Federatie metagegevens XML-bestand zodat u die naar Qlik komen server uploaden kunt.

    > [AZURE.NOTE] Voordat u kunt de metagegevens IdP uploadt naar de server Qlik komen, het bestand moet worden bewerkt als u wilt verwijderen om ervoor te zorgen werking tussen Azure AD en Qlik komen server.

    ![QlikSense][qs24]

    een. Open het bestand FederationMetaData.xml is gedownload van Azure in een teksteditor.

    b. Zoek naar de waarde **RoleDescriptor**.  Er zijn vier gegevens (twee paren van openen en sluiten element tags).

    c. Verwijderen van de labels RoleDescriptor en alle gegevens uit het bestand.

    d. Sla het bestand en behouden in de buurt voor gebruik verderop in dit document.

6. Navigeer naar de Qlik komen Qlik Management Console (QMC) als een gebruiker aan wie virtuele proxyconfiguraties kunt maken.

7. Klik in de QMC, op het virtuele Proxy menu-item.

    ![QlikSense][qs6] 

8. Klik onder aan het scherm, op de nieuwe knop maken.

    ![QlikSense][qs7]

9. Het scherm virtuele proxy bewerken wordt weergegeven.  Aan de rechterkant van het scherm wordt een menu voor configuratieopties zichtbaar maken.

    ![QlikSense][qs9]

10. Met de optie van het type identificatie menu is ingeschakeld, voert u de identificatiegegevens voor de configuratie van de Azure virtuele proxy.

    ![QlikSense][qs8]  
    
    een. Het veld Beschrijving is een beschrijvende naam voor de configuratie van de virtuele proxy.  Voer een waarde voor een beschrijving.
    
    b. Het veld voorvoegsel kunt u het eindpunt virtuele proxyserver om verbinding te maken om Qlik komen met Azure AD eenmalige aanmelding te identificeren.  Voer de voorvoegselnaam van een unieke voor deze virtuele proxy.

    c. Inactiviteit sessietime (minuten) is de time-out voor verbindingen via deze virtuele proxy.

    d. De naam van de sessie cookie koptekst is de naam van de cookie opslaan van de sessie-id van de sessie Qlik komen van die een gebruiker na een succesvolle verificatie ontvangt.  Deze naam moet uniek zijn.

11. Klik op de menuoptie verificatie deze zichtbaar te maken.  De verificatie-scherm wordt weergegeven.

    ![QlikSense][qs10]

    een. De vervolgkeuzelijst **anonieme toegangsmodus** bepaalt als anonieme gebruikers Qlik krijgen via de virtuele proxy toegang kunnen krijgen.  De standaardoptie is geen anonieme gebruiker.

    b. De vervolgkeuzelijst **verificatiemethode** bepaalt dat het schema voor verificatie van de virtuele proxy wordt gebruikt.  Selecteer SAML in de vervolgkeuzelijst.  Meer opties wordt hierdoor weergegeven.

    c. In het **SAML host URI veld**worden ingevoerd voeren de hostname-gebruikers toegang tot Qlik komen via deze virtuele SAML-proxy.  De hostnaam is de uri van de server Qlik komen.

    d. Voer in het **SAML entiteit-ID**, dezelfde waarde voor het veld SAML host URI ingevoerd.

    e. De **metagegevens van SAML IdP** is het bestand bewerkt eerder in de sectie **Bewerken Federatie metagegevens van Azure AD-configuratie** .  Gegevens om ervoor te zorgen werking tussen Azure AD verwijderen **voordat u de metagegevens IdP uploadt het bestand moet worden bewerkt** en Qlik komen server.  **Raadpleeg de instructies hierboven als het bestand dat nog moet worden bewerkt.**  Klik op de knop Bladeren en selecteer het metagegevensbestand bewerkt te uploaden naar de virtuele proxyconfiguratie als het bestand is bewerkt.

    f. Voer de naam van het kenmerk of Naslaggids voor het schema voor het SAML-kenmerk dat staat voor de **gebruikers-id** Azure AD worden verzonden naar de server Qlik komen.  Schema naslaginformatie is beschikbaar in de configuratie van Azure app schermen posten.  Het kenmerk name, **voert u http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**gebruiken.

    g. Voer de waarde voor de **Gebruikerslijst** die wordt gekoppeld aan gebruikers wanneer ze worden geverifieerd bij Qlik komen server via Azure AD.  Vastgelegde waarden moeten tussen **vierkante haken []**.  Als u wilt gebruiken een kenmerk verzonden in de Azure AD SAML-bevestiging, voert u de naam van het kenmerk in deze tekst vak **zonder** vierkante haken.

    h. De **SAML-ondertekend algoritme** Hiermee stelt u de serviceprovider (in dit geval Qlik komen server)-certificaat zich aanmeldt voor de configuratie van de virtuele proxy.  Als Qlik komen server een vertrouwde certificaat dat is gegenereerd met Microsoft Enhanced RSA en AES Cryptographic Provider gebruikt, wijzigen in de algoritme van de SAML-ondertekend **SHA-256**.

    ik. De sectie SAML-kenmerk toewijzing kan voor extra kenmerken zoals groepen worden verzonden naar Qlik komen voor gebruik in beveiligingsregels.

12. Klik op het menuoptie voor het zichtbaar maken van taakverdeling.  Het scherm taakverdeling wordt weergegeven.

    ![QlikSense][qs11]

13. Klik op de nieuwe server knooppunt knop toevoegen, selecteer engine knooppunt of knooppunten Qlik komen worden sessies bij voor doeleinden van taakverdeling verzenden en klik op de knop toevoegen.

    ![QlikSense][qs12]

14. Klik op de menuoptie geavanceerde deze zichtbaar te maken. Het geavanceerde scherm wordt weergegeven.

    ![QlikSense][qs13]

    een. De Host wit-lijst bevat hostnamen die zijn geaccepteerd verbinding met de server Qlik komen.  **Voer de hostnaam gebruikers wordt opgeven wanneer u verbinding maakt met Qlik komen-server.** De hostnaam is hetzelfde resultaat als de SAML host uri zonder de https://.

15. Klik op de knop toepassen.

    ![QlikSense][qs14]

16. Klik op OK om de waarschuwing waarin wordt aangegeven proxy's die zijn gekoppeld aan de virtuele proxy wordt opnieuw worden gestart te accepteren.

    ![QlikSense][qs15]

17. Aan de rechterkant van het scherm, wordt het gekoppelde items menu weergegeven.  Klik op de menuoptie proxy's.

    ![QlikSense][qs16]

18. De proxy-scherm wordt weergegeven.  Klik op de knop koppelen aan het scherm om het koppelen van een proxy naar de virtuele proxy.

    ![QlikSense][qs17]

19. Selecteer het knooppunt proxy die ondersteuning voor deze virtuele proxyverbinding en klik op de knop koppelen.  Na het koppelen, wordt de proxy weergegeven onder de bijbehorende proxy's.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Na ongeveer vijf tot tien seconden verschijnt het bericht QMC vernieuwen.  Klik op de knop Vernieuwen QMC.

    ![QlikSense][qs20]

21. Wanneer de QMC wordt vernieuwd, klik op het virtuele proxy's menu-item. De nieuwe SAML virtuele proxy-vermelding wordt weergegeven in de tabel aan het scherm.  Één klik op de vermelding virtuele proxy.

    ![QlikSense][qs51]

22. Klik onderaan het scherm wordt de knop downloaden SP-metagegevens geactiveerd.  Klik op de knop downloaden SP metagegevens u kunt de metagegevens op een bestand opslaan.

    ![QlikSense][qs52]

23. Open het bestand sp-metagegevens.  De **id van de entiteit** -post en de post **AssertionConsumerService** toekijken.  Deze waarden zijn gelijk aan de **id** en de u **zich aanmeldt op URL** in de configuratie van de Azure AD-toepassing. Als ze komen niet overeen vervolgens vervangt u ze in de wizard van de configuratie Azure AD-App.

    ![QlikSense][qs53]

24. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

25. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
 
    ![Azure AD voor eenmalige aanmelding][11]


### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.


![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** de volgende stappen uitvoeren:  ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster de volgende stappen uitvoeren: ![maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Maken van een gebruiker Qlik komen Enterprise testen

In deze sectie, kunt u een gebruiker met de naam van Britta Simon in Qlik komen onderneming maken. Neem werken met het ondersteuningsteam Qlik komen Enterprise mag de gebruikers toevoegen aan het Qlik komen Enterprise-platform.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van haar toegang aan Qlik komen Enterprise inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon naar Qlik komen Enterprise, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **Qlik komen Enterprise**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


## <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de Qlik komen Enterprise-tegel in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw bedrijfstoepassing Qlik komen.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png