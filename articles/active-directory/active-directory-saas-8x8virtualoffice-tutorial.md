<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en 8 x 8 virtuele Office configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Zelfstudie: Azure Active Directory-integratie met 8 x 8 virtuele Office

Het doel van deze zelfstudie is om aan te geven hoe u Office van 8 x 8 virtuele integreren met Azure Active Directory (Azure AD).

Integratie van 8 x 8 biedt virtuele Office met Azure AD de volgende voordelen:

- U kunt bepalen in Azure AD die toegang tot Office van 8 x 8 virtuele heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot Office van 8 x 8 virtuele (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u meer informatie over SaaS app-integratie met Azure AD weet wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met 8 x 8 virtuele Office, moet u de volgende items:

- Een Azure AD-abonnement
- Een 8 x 8 virtuele Office eenmalige aanmelding van ingeschakeld-abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Microsoft Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. 8 x 8 virtuele Office uit de galerie toe te voegen
2. Configureren en testen van Microsoft Azure AD eenmalige aanmelding


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 virtuele Office uit de galerie toe te voegen
Als u wilt de integratie van 8 x 8 virtuele Office in Azure AD configureren, moet u 8 x 8 virtuele Office in de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen van 8 x 8 virtuele Office vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .
    
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ **8 x 8 virtuele Office**in het zoekvak.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. Selecteer **8 x 8 virtuele Office**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Configureren en testen van Microsoft Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test Microsoft Azure AD eenmalige aanmelding met 8 x 8 is die Virtual Office op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD weten welke in de gebruiker die beschikbaar zijn in 8 x 8 Virtual Office aan een gebruiker in Azure AD is. Met andere woorden, een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in 8 x 8 virtuele Office moet tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in 8 x 8 virtuele Office.

Als u wilt configureren en te testen Microsoft Azure AD eenmalige aanmelding met 8 x 8 virtuele Office, moet u voltooien van de volgende elementen:

1. **[Configureren Microsoft Azure AD eenmalige aanmelding](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Microsoft Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een gebruiker van 8 x 8 virtuele Office test](#creating-a-8x8-virtual-office-test-user)** - hebben een tegenhanger van Britta Simon in 8 x 8 virtuele Office dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Microsoft Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Configuratie van Microsoft Azure AD voor eenmalige aanmelding

In deze sectie, die u kunt Microsoft Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw 8 x 8 virtuele Office-toepassing.

**Als u wilt configureren eenmalige aanmelding Microsoft Azure AD met 8 x 8 virtuele Office, moet u de volgende stappen uitvoeren:**

1. In de klassieke-portal op de pagina van de integratie in de **8 x 8 virtuele Office** -toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u gebruikers aan te melden bij Office van 8 x 8 virtuele** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. De volgende stappen uitvoeren en klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    een. Typ in het tekstvak **URL beantwoorden** :`https://sso.8x8.com/saml2`

    b. Klik op **volgende**

4. Klik op de pagina **configureren eenmalige aanmelding bij 8 x 8 virtuele Office** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

5. Eenmalige aanmelding aan bij uw tenant 8 x 8 virtuele Office als een beheerder.
6. Selecteer **virtuele Office Account Mgr** op toepassing Configuratiescherm.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Selecteer **bedrijven** -account om te beheren en klik op de knop **Aanmelden** .

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Klik op het tabblad **Accounts** in de menulijst.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Klik op **Eenmalige aanmelding** in de lijst met Accounts.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. Selecteer **Signle aanmelding** onder verificatiemethode en klik op **SAML**.

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. **Eenmalige aanmelding van SAML-URL**, **één eenmalige Out Service URL** en **uitgever URL** kopiëren van Azure AD **aanmelden bij URL**, **URL afmelden** en **uitgever URL** in de virtuele Office 8 x 8. 

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Aan de kant App configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Klik op de knop **Browser** om het certificaat dat u hebt gedownload van Azure AD uploaden.

13. Klik op de knop **Opslaan** .

14. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Azure AD voor eenmalige aanmelding][10]

15. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  

    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.
    
![Azure AD-gebruiker maken][20]

**U maakt een testgebruiker in Azure AD door de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>Maken van een gebruiker van 8 x 8 virtuele Office testen

Het doel van dit gedeelte is het opzetten van een gebruiker Britta Simon in 8 x 8 virtuele Office genoemd. 8 x 8 virtuele Office ondersteunt just-in-time is geïnstalleerd, die is standaard ingeschakeld.

Er is geen actie-item voor u in deze sectie. Een nieuwe gebruiker wordt gemaakt tijdens een poging voor toegang tot Office van 8 x 8 virtuele als dit nog niet bestaat. 

> [AZURE.NOTE] Als u een gebruiker handmatig maken moet, moet u contact opnemen met het ondersteuningsteam van 8 x 8 virtuele Office.


### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van haar toegang aan 8 x 8 virtuele Office.
    
![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon bij 8 x 8 virtuele Office, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen, **8 x 8 virtuele Office**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Microsoft Azure AD eenmalige aanmelding configuratie via het Configuratiescherm toegang testen.

Wanneer u op de tegel van de 8 x 8 virtuele Office in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw 8 x 8 virtuele Office-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
