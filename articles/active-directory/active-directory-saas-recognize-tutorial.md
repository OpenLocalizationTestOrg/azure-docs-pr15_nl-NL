<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met herkennen | Microsoft Azure"
    description="Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en herkennen."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Zelfstudie: Azure Active Directory-integratie met herkennen

Het doel van deze zelfstudie is om aan te geven hoe u kunt herkennen integreren met Azure Active Directory (Azure AD).

Herkennen integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot herkennen heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot herkennen (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met herkennen, moet u de volgende items:

- Een Azure AD-abonnement
- Een herkennen eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Herkennen toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-recognize-from-the-gallery"></a>Herkennen toevoegen vanuit de galerie
Als u wilt de integratie van herkennen in Azure AD configureren, moet u herkennen uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen herkennen vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .
    
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.
    
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **herkennen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)

7. Selecteer **herkennen**in het deelvenster resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![De app selecteren in de galerie](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met herkennen op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in herkennen aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in herkennen moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in herkennen.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met herkennen, moet u voltooien van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een herkennen testen](#creating-a-recognize-test-user)** - hebben een tegenhanger van Britta Simon in herkennen dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding in de klassieke portal inschakelen en configureren eenmalige aanmelding in uw toepassing herkennen.

**Als u wilt configureren eenmalige aanmelding Azure AD met herkennen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke-portal, klik op de pagina **herkennen** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .
     
    ![Eenmalige aanmelding configureren][6] 

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij herkennen** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)

3. De volgende stappen uitvoeren en klik op **volgende**op de pagina van het dialoogvenster **App-instellingen configureren** :

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)

    een. Typ in het tekstvak **Aanmeldingsadres op URL** een URL waarbij het volgende patroon: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. Typ een URL waarbij het volgende patroon in het tekstvak **id** : `https://recognizeapp.com/<your-domain>/saml/metadata`.

    c. Klik op **volgende**

    > [AZURE.NOTE] Als u niet over deze URL's weet, typt u voorbeeld-URL's met voorbeeld patroon. Als u deze waarden, kunt u verwijzen stap 9 voor meer informatie of neem contact op met het ondersteuningsteam herkennen via <mailto:support@recognizeapp.com>.

4. Klik op de pagina **configureren eenmalige aanmelding bij herkennen** op **certificaat downloaden** en sla het bestand op uw computer:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)

5. In een andere webbrowservenster, eenmalige aanmelding aan bij uw tenant herkennen als beheerder.

6. Klik in de rechterbovenhoek op **Menu**. Ga naar **bedrijfsbeheerder**.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

7. Klik op het linker navigatiedeelvenster, klikt u op **Instellingen**.

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

8. De volgende stappen uitvoeren op de sectie **Instellingen voor eenmalige aanmelding** .

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)

    een. Als **SSO inschakelen**, selecteert u **op aan**.

    b. In de **IDP entiteit-ID** plaatst tekstvak de waarde van de **URL van de uitgever** van de configuratiewizard van Azure AD-toepassing.

    c. In de **doel-url voor eenmalige aanmelding** plaatst tekstvak de waarde van **Eenmalige aanmelding Service URL** van de configuratiewizard van Azure AD-toepassing.

    d. In de **doel-url trage** plaatst tekstvak de waarde van **Één Sign-Out Service-URL** van de configuratiewizard van Azure AD-toepassing.

    e. Open uw gedownloade certificaatbestand in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **certificaat** . 

    f. Klik op de knop **Instellingen opslaan** . 

9. Kopieer de URL onder de **url van de Service Provider metagegevens**naast de sectie **Instellingen voor eenmalige aanmelding** .
    
    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

10. Open de **URL voor metagegevens koppeling** onder een lege browser om het metagegevensdocument te downloaden. Gebruik de waarde van de EntityDescriptor die herkennen toegekend u **id** in het dialoogvenster **App-instellingen configureren** .

    ![Eenmalige aanmelding op App kant configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

11. In de klassieke-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

12. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]



### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
Het doel van dit gedeelte is het opzetten van een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-Portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png)

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Typ in het tekstvak **Achternaam** **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png)

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-recognize-test-user"></a>Maken van een gebruiker herkennen testen

Als u wilt dat gebruikers Azure AD Meld u aan bij herkennen, moeten ze worden ingericht in herkennen. Bij herkennen, met de inrichting van is een handmatige taak.

Deze app biedt geen ondersteuning voor SCIM inrichting, maar de synchronisatie van een alternatieve gebruiker die de bepalingen van de gebruikers heeft. 

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Als u wilt een gebruikersaccount inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw bedrijfssite herkennen als beheerder.

2.  Klik in de rechterbovenhoek op **Menu**. Ga naar **bedrijfsbeheerder**.

3.  Klik op het linker navigatiedeelvenster, klikt u op **Instellingen**.

4.  De volgende stappen uitvoeren op **Gebruiker synchroniseren** sectie.
    
    ![Nieuwe gebruiker] (./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Nieuwe gebruiker")

    een. Als u **Synchronisatie ingeschakeld**, selecteert u **op**.

    b. Als **de synchronisatie-provider kiezen**, selecteert u **Microsoft / Office 365**.

    c. Klik op **gebruiker synchronisatie uitvoeren**

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan herkennen.
    
![Gebruiker toewijzen][200]

**Als u wilt toewijzen Britta Simon aan herkennen, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.
    
    ![Gebruiker toewijzen][201]

2. Selecteer in de lijst met toepassingen **herkennen**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)

3. In het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.
    
    ![Gebruiker toewijzen][205]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.
 
Wanneer u op de tegel herkennen in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw toepassing herkennen.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png
