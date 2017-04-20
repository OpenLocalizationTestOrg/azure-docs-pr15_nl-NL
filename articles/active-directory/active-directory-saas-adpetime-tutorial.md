<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ADP eTime | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en ADP eTime configureren."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Zelfstudie: Azure Active Directory-integratie met ADP eTime

Het doel van deze zelfstudie is om aan te geven hoe u ADP eTime integreren met Azure Active Directory (Azure AD).  
ADP eTime integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot ADP eTime heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot ADP eTime (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren


Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met ADP eTime, moet u de volgende items:

- Een Azure AD-abonnement
- Een ADP eTime eenmalige aanmelding van ingeschakeld-abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ADP eTime toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-adp-etime-from-the-gallery"></a>ADP eTime toevoegen vanuit de galerie
Als u wilt de integratie van ADP eTime in Azure AD configureren, moet u ADP eTime uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ADP eTime vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **ADP eTime**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. Selecteer **ADP eTime**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met ADP eTime op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in ADP eTime aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ADP eTime moet met andere woorden, tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in ADP eTime.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met ADP eTime, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een eTime ADP testen](#creating-a-adpetime-test-user)** - heeft een tegenhanger van Britta Simon in ADP eTime dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD voor eenmalige aanmelding configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw ADP eTime-toepassing.

Uw ADP eTime toepassing verwacht de bevestigingen SAML een specifieke notatie, waarin u aangepaste kenmerktoewijzingen toevoegen aan uw SAML token kenmerken configuratie is vereist. De volgende schermafbeelding ziet u een voorbeeld hiervoor. De naam van de claim zijn altijd **"PersonImmutableID"** en de waarde van de plaats waar we hebt toegewezen aan ExtensionAttribute2 waarin het veld Werknemer-id van de gebruiker. Hier de gebruiker toewijzing voorgrond Azure AD naar ADP eTime op het veld Werknemer-id wordt uitgevoerd, maar u kunt dit toewijzen aan een andere waarde ook op basis van de instellingen van de toepassing. Dus u moet werken met ADP eTime team eerst naar de juiste id van een gebruiker gebruiken en in kaart brengen die waarde met het claimen **"PersonImmutableID"** .  

![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Voordat u de SAML-bevestiging configureren kunt, moet u contact opnemen met uw ondersteuningsteam voor ADP eTime en de waarde van de unieke id-kenmerk aanvragen voor uw tenant. Moet u deze waarde voor het configureren van de aangepaste claim voor uw toepassing.


**Als u wilt configureren eenmalige aanmelding Azure AD met ADP eTime, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **ADP eTime** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ADP eTime** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. Klik op de pagina **App-instellingen configureren** dialoogvenster de volgende stappen uitvoeren:.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    een. Typ in het tekstvak **Beantwoorden URL** de URL die wordt gebruikt door uw gebruikers voor eenmalige aanmelding in uw ADP eTime-toepassing met het volgende patroon: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Klik op **volgende**.

4. Klik op de pagina **configureren eenmalige aanmelding bij ADP eTime** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    een. Klik op **metagegevens downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.


5. Als u eenmalige aanmelding configureren voor uw toepassing, neem contact op met uw ondersteuningsteam voor ADP eTime en e-mail in het bestand bijvoegen gedownload metagegevens, zodat deze kunnen worden geconfigureerd voor integratie voor eenmalige aanmelding.

    > [AZURE.NOTE] Nadat het exemplaar **ADP eTime** team hebt geconfigureerd, krijgen de waarde **RelayState** hiertegen. Ga als volgt de hieronder vermeld stappen om deze te configureren. U kunt de integratie testen na deze configuratie. Dus houd er rekening mee dat dit is belangrijk configuratie voor de toepassingsintegratie van deze om te werken.

6. Als u wilt configureren de waarde RelayState in Azure AD, moet u de volgende stappen uitvoeren: 
    
    een. Meld u aan bij de [Azure Management Portal](https://portal.azure.com) als beheerder.

    b. Klik in het linkernavigatiedeelvenster op **Meer Services**. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c. Typ **Azure Active Directory**in het tekstvak **Zoeken** en klik vervolgens op de gerelateerde koppeling.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Klik op **zakelijke toepassingen**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. Klik in de sectie **beheren** op **Alle toepassingen**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. Typ **ADP eTime**in het tekstvak **Zoeken** en klik vervolgens op de gerelateerde koppeling. 
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. Klik in de sectie **beheren** op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Selecteer **weergeven geavanceerde instellingen van de URL**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    ik. Typ een waarde aan de hand van de volgende patronen in het tekstvak **Relay staat** :
    
    - Productieomgeving:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Tijdelijke omgeving:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. De instellingen opslaan.

7. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

8. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.  
Selecteer in de lijst gebruikers **Britta Simon**.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    een. Als het **Type van gebruiker**, selecteert u **nieuwe gebruiker in uw organisatie**.

    b. Typ in het tekstvak **Gebruikersnaam in te voeren** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-adp-etime-test-user"></a>Maken van een gebruiker ADP eTime testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in ADP eTime genoemd. Neem werken met ADP eTime ondersteuningsteam de gebruikers in het ADP eTime-account toevoegen. 


> [AZURE.NOTE]Als u een gebruiker handmatig maken moet, moet u contact opnemen met het ondersteuningsteam van ADP eTime.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang tot ADP eTime verlenen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan ADP eTime, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ADP eTime**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de tegel ADP eTime in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw ADP eTime-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
