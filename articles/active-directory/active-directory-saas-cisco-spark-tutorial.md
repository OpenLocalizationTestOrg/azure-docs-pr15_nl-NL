<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Cisco elektrische | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Cisco elektrische configureren."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Zelfstudie: Azure Active Directory-integratie met Cisco elektrische

In deze zelfstudie leert u hoe u een Cisco integreren met Azure Active Directory (Azure AD).

Cisco elektrische integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot een Cisco heeft
- U kunt uw gebruikers kunnen automatisch ophalen die zijn aangemeld bij een Cisco (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met Cisco elektrische, moet u de volgende items:

- Een Azure AD-abonnement
- Een **Cisco een** eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. Cisco elektrische toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco elektrische toevoegen vanuit de galerie
Als u wilt de integratie van Cisco elektrische in Azure AD configureren, moet u Cisco elektrische uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u wilt toevoegen zoals Cisco elektrische vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak, **Zoals Cisco elektrische**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. In het deelvenster met resultaten **Cisco elektrische**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met Cisco elektrische op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in een Cisco in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Cisco elektrische moet met andere woorden, tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Cisco elektrische. Als u wilt configureren en te testen Azure AD eenmalige aanmelding met een Cisco, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een elektrische Cisco testen](#creating-a-cisco-spark-test-user)** - hebben een tegenhanger van Britta Simon in Cisco elektrische dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Cisco elektrische.

Cisco een toepassing verwacht de bevestigingen SAML naar specifieke kenmerken bevatten. Configureer de volgende kenmerken van deze toepassing. U kunt de waarden van de volgende kenmerken beheren op het tabblad **"Atrributes"** van de toepassing. De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Als u wilt configureren eenmalige aanmelding Azure AD met Cisco elektrische, moet u de volgende stappen uitvoeren:**

1. Klik in het Azure op klassieke-portal op de pagina **Cisco een** toepassing integratie, in het menu aan de bovenkant, **kenmerken**.
     
    ![Eenmalige aanmelding configureren][5]


2. Klik in het dialoogvenster **SAML token kenmerken** , moet u de volgende stappen uitvoeren:

    een. Klik op **gebruikerskenmerk toevoegen** om het dialoogvenster **Gebruiker Attribure toevoegen** .

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Typ in het tekstvak **Kenmerk** **uid**.
    
    c. Selecteer in de lijst **Kenmerkwaarde** **user.userprincipal**.
    
    d. Klik op **Voltooien**. Vervolgens **Apply Changes** onder aan de pagina.

3. Klik in het menu aan de bovenkant, op **Snel starten**.

    ![Eenmalige aanmelding configureren][6]

4. Klik in de klassieke-portal, klik op de pagina **Cisco een** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][7] 

5. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij een Cisco** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.
    
    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    een. Typ in het tekstvak Aanmeldingsadres op URL een URL waarbij het volgende patroon: `https://web.ciscospark.com/#/signin`.

    b. Klik op **volgende**.


7. Klik op de pagina **configureren eenmalige aanmelding bij Cisco elektrische** op **metagegevens downloaden**, en sla het bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Meld u aan bij [Cisco Cloud samenwerking Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.
9. Selecteer **Instellingen** en klik onder de sectie **verificatie** op **Modify**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Selecteer **integreren een identiteitsprovider van 3e van derden. (Geavanceerd)** en Ga naar het volgende scherm.
11. Klik op **Metagegevens-bestand downloaden** en sla het bestand op uw computer.
12. Klik op de pagina **Importeren Idp metagegevens** sleept en neerzetten Azure AD metagegevens naar de pagina of gebruik de optie voor browser te zoeken en upload het bestand Azure AD-metagegevens. Selecteer **certificaat ondertekend door een certificeringsinstantie in metagegevens (veiliger) vereisen** vervolgens en klik op **volgende**. 

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Selecteer **Testverbinding voor eenmalige aanmelding**en wanneer een nieuw browsertabblad wordt geopend, verificatie met Azure AD door te melden bij.
14. Ga terug naar het tabblad van de browser **Cisco Cloud samenwerking Management** . Als de test geslaagd is, selecteert u **deze test is geslaagd. Eenmalige aanmelding optie inschakelen** en klik op **volgende**.

7. In de klassieke Azure AD-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.
    
    ![Azure AD voor eenmalige aanmelding][10]

8. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.  
    
    ![Azure AD voor eenmalige aanmelding][11]

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   



### <a name="creating-a-cisco-spark-test-user"></a>Maken van een gebruiker een Cisco testen

In deze sectie, kunt u een gebruiker met de naam van Britta Simon in Cisco elektrische maken. In deze sectie, kunt u een gebruiker met de naam van Britta Simon in Cisco elektrische maken.

1. Ga naar het [Cisco Cloud samenwerking Management](https://admin.ciscospark.com/) met uw volledige beheerdersreferenties.
2. Klik op **gebruikers** en klik vervolgens op **gebruikers beheren**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. Klik in het venster **Beheren gebruiker** **handmatig toevoegen of wijzigen van de gebruikers** selecteren en op **volgende**.
4. Selecteer **namen en e-mailadres**. Vul vervolgens het tekstvak als volgt:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**

    b. Typ in het tekstvak **Achternaam** **Simon**

    c. Typ in het tekstvak **e-mailadres****britta.simon@contoso.com**

5. Klik op het plusteken (+) om toe te voegen Britta Simon. Klik vervolgens op **volgende**.
6. In het venster **Services voor gebruikers toevoegen** , klikt u op **Opslaan** en vervolgens op **Voltooien**.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan Cisco elektrische inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan Cisco elektrische, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen, **Zoals Cisco elektrische**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel Cisco elektrische in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw Cisco een toepassing.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
