<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met voorgrond | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de voorgrond."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-front"></a>Zelfstudie: Azure Active Directory-integratie met voorgrond

Het doel van deze zelfstudie is om aan te geven hoe u voorgrond integreren met Azure Active Directory (Azure AD).

Voorgrond integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot voorgrond heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen ondertekend-on naar voorgrond (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met voorgrond, moet u de volgende items:

- Een Azure AD-abonnement
- Een voorgrond eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Voorgrond toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-front-from-the-gallery"></a>Voorgrond toevoegen vanuit de galerie
Als u wilt de integratie van voorgrond in Azure AD configureren, moet u voorgrond uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen voorgrond vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .
    
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ **voorgrond**in het zoekvak.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/tutorial_front_01.png)

7. Selecteer **voorste**in het deelvenster resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-front-tutorial/tutorial_front_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met voorgrond op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn op de voorgrond aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker vóór moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** vóór.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met voorgrond, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een voorgrond testen](#creating-a-front-test-user)** - heeft een tegenhanger van Britta Simon vóór dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte u Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing voorgrond.

**Als u wilt configureren eenmalige aanmelding Azure AD met voorgrond, moet u de volgende stappen uitvoeren:**

1. In de klassieke-portal op de **voorgrond** pagina in de integratie van toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u gebruikers aan te melden op naar voorgrond** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_03.png)

3. Klik op de pagina van het dialoogvenster **App-instellingen configureren** als u configureren van de toepassing in **IDP modus gestart wilt**, de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_04.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **id** :`https://<company name>.frontapp.com`

    b. Typ een URL waarbij het volgende patroon in het tekstvak **URL beantwoorden** :`https://<company name>.frontapp.com/sso/saml/callback`

    c. Klik op **volgende**

4. Als u configureren van de toepassing in **SP modus gestart** op de pagina van het dialoogvenster **App-instellingen configureren wilt** , klik op de **"Weergeven geavanceerde instellingen (optioneel)"** en voer vervolgens de **Aanmelding op URL** en klik op **volgende**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_05.png)

    een. Typ een URL waarbij het volgende patroon in het tekstvak **Aanmeldingsadres op URL** :`https://<company name>.frontapp.com`

    b. Klik op **volgende**

    > [AZURE.NOTE] Houd er rekening mee dat deze niet de reële waarden zijn. U moet deze waarden bijwerken met de werkelijke Aanmeldingsadres op URL, id en beantwoorden URL. Als u deze waarden, kunt u verwijzen **stap 12** voor meer informatie of neem contact op met voorgrond via [support@frontapp.com](emailTo:support@frontapp.com).

5. Klik op de pagina **configureren eenmalige aanmelding voor** de volgende stappen uitvoeren en klik op **volgende**:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_06.png)

    een. Klik op **certificaat downloaden**en sla het bestand op uw computer.

    b. Klik op **volgende**.

6. Eenmalige aanmelding aan bij uw tenant voorgrond als beheerder.

7. Ga naar **instellingen (tandwiel pictogram onderaan in de sidebar links) > Voorkeuren**.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

8. Klik op de koppeling **Eenmalige aanmelding** .

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

9. Selecteer **SAML** in de vervolgkeuzelijst van **Eenmalige aanmelding**.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

10. In de **Ingangspunt** plaatst tekstvak de waarde van **Eenmalige aanmelding Service URL** van de configuratiewizard van Azure AD-toepassing.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

11. De inhoud van het gedownloade certificaatbestand Kopieer en plak deze in het tekstvak **handtekeningcertificaat** .

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

12. Controleer deze URL's overeenkomen met uw configuratie in stap 3.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

13. Klik op de knop **Opslaan** .

14. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

15. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/create_aaduser_05.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/create_aaduser_07.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-front-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-front-test-user"></a>Maken van een gebruiker voorgrond testen

Het doel van dit gedeelte is het opzetten van een gebruiker met de naam van Britta Simon in Front.Please werken met uw ondersteuningsteam voorgrond om toe te voegen van de gebruikers in de voorgrond-account.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van haar toegang naar voorgrond.
    
![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon naar voorgrond, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Klik in de lijst toepassingen Selecteer **voorgrond**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-front-tutorial/tutorial_front_50.png)

1. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.
    
    ![Gebruiker toewijzen][205]



### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.
 
Wanneer u op de voorgrond-tegel in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw toepassing voorgrond.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-front-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-front-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-front-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-front-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-front-tutorial/tutorial_general_205.png
