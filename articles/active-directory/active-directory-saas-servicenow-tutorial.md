<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ServiceNow en ServiceNow Express | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en ServiceNow en ServiceNow Express configureren."
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


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Zelfstudie: Azure Active Directory-integratie met ServiceNow en ServiceNow Express.


In deze zelfstudie leert u hoe u ServiceNow en ServiceNow Express integreren met Azure Active Directory (Azure AD).

ServiceNow en ServiceNow Express integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot ServiceNow en ServiceNow Express heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot ServiceNow en ServiceNow Express (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met ServiceNow en ServiceNow Express, moet u de volgende items:

- Een Azure AD-abonnement
- Voor ServiceNow, een exemplaar of de tenant van ServiceNow, Calgary versie of hoger
- Voor ServiceNow Express, een exemplaar van ServiceNow Express, Helsinki versie of hoger
- ServiceNow tenant moeten de [Meerdere Provider eenmalige aanmelding op Plug](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) ingeschakeld. Dit kan worden uitgevoerd door een serviceaanvraag indienen bij <https://hi.service-now.com/> indienen 


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ServiceNow toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding voor ServiceNow of ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow toevoegen vanuit de galerie
Als u wilt de integratie van ServiceNow of ServiceNow Express in Azure AD configureren, moet u ServiceNow uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps. 

**Als u wilt toevoegen ServiceNow vanuit de galerie, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassingen][4]

6. Typ in het zoekvak **ServiceNow**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. Selecteer **ServiceNow**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In dit gedeelte u Configureer en test eenmalige aanmelding Azure AD met ServiceNow of ServiceNow Express op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in ServiceNow in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ServiceNow moet met andere woorden, tot stand worden gebracht.
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in ServiceNow. Als u wilt configureren en te testen Azure AD eenmalige aanmelding met ServiceNow, die u moet uitvoeren van de volgende elementen:

1. **[Configureren Azure AD eenmalige aanmelding voor ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Configureren Azure AD eenmalige aanmelding voor ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - zodat uw gebruikers kunnen deze functie gebruiken.
3. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een ServiceNow testen](#creating-a-servicenow-test-user)** - hebben een tegenhanger van Britta Simon in ServiceNow dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
6. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

> [AZURE.NOTE] Als u wilt configureren ServiceNow weglaat stap 2. Op dezelfde manier als u wilt configureren ServiceNow Express weglaat stap 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow

1.  Klik in de klassieke portal van Azure AD, klik op de pagina **ServiceNow** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ServiceNow** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-instellingen configureren** de volgende stappen uitvoeren:

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "URL van app configureren")

    een. Typ in het tekstvak **ServiceNow aanmelding op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw ServiceNow-toepassing het patroon volgen: `https://<instance-name>.service-now.com`.

    b. Typ de URL die wordt gebruikt door uw gebruikers voor eenmalige aanmelding in uw ServiceNow-toepassing na het patroon in het tekstvak **id** : `https://<instance-name>.service-now.com`.

    c. Klik op **volgende**

4.  Als u wilt dat Azure AD ServiceNow automatisch te configureren voor SAML gebaseerde verificatie, voert u uw ServiceNow exemplaarnaam, beheerdersgebruikersnaam, en beheerderswachtwoord in het formulier **automatisch eenmalige aanmelding configureren** en op *configureren*. Opmerking die de beheerdersgebruikersnaam verstrekt hebben de **security_admin** rol in ServiceNow hiervoor om te werken. Anders om handmatig te configureren ServiceNow om Azure AD als een SAML-identiteitsprovider gebruiken, klik op **handmatig configureren de toepassing voor eenmalige aanmelding**, en vervolgens op **volgende** en de volgende stappen uit.

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "URL van app configureren")



5.  Klik op **downloaden certificaat**, sla het certificaatbestand lokaal op uw computer op de pagina **configureren eenmalige aanmelding bij ServiceNow** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Eenmalige aanmelding configureren")

1. Eenmalige aanmelding in uw toepassing ServiceNow als beheerder.
2. De _integratie - meerdere Provider eenmalige aanmelding Installer_ -invoegtoepassing activeren door de volgende stappen uit:

    een. In het navigatiedeelvenster aan de linkerkant, gaat u naar de sectie **System Definition** en klik vervolgens op **Plug-ins**.

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "De invoegtoepassing activeren")
    
    b. Zoeken naar _integratie - meerdere Provider eenmalige aanmelding Installer_.

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "De invoegtoepassing activeren")

    c. Selecteer de invoegtoepassing. Rigth op en selecteer **Activeren/Upgrade**.
    
    d. Klik op de knop **activeren** .

2. Klik in het navigatiedeelvenster aan de linkerkant op **Eigenschappen**.  

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "URL van app configureren")


3. Klik in het dialoogvenster **Eigenschappen van meerdere Provider eenmalige aanmelding van** de volgende stappen uitvoeren:

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "URL van app configureren")

    een. Als u **meerdere provider SSO inschakelen**, selecteert u **Ja**.

    b. Als de **logboekregistratie voor foutopsporing hebt u de meerdere provider SSO-integratie inschakelen**, selecteert u **Ja**.

    c. Typ in **het veld op de gebruiker van een tabel die...** tekstvak **gebruikersnaam**.

    d. Klik op **Opslaan**.



1. Klik in het navigatiedeelvenster aan de linkerkant op **x509 certificaten**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Eenmalige aanmelding configureren")


1. Klik in het dialoogvenster **X.509-certificaten** op **Nieuw**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Eenmalige aanmelding configureren")


1. Klik in het dialoogvenster **X.509-certificaten** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Eenmalige aanmelding configureren")

    een. Klik op **Nieuw**.

    b. Typ in het tekstvak **naam** een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).

    c. Selecteer **actieve**.

    d. Als **opmaken**, selecteert u **PEM**.

    e. Als het **Type**, selecteer **Store certificaat vertrouwen**.
    
    f. Een certificaat Base64-gecodeerde opent in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **PEM certificaat** .

    g. Klik op **bijwerken**.


1. Klik in het navigatiedeelvenster aan de linkerkant op **Identiteitsprovider**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Eenmalige aanmelding configureren")

1. Klik in het dialoogvenster **Identiteitsprovider** klikt u op **Nieuw**:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Eenmalige aanmelding configureren")


1. Klik in het dialoogvenster **Identiteitsprovider** op **SAML2 Update1?**:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Eenmalige aanmelding configureren")


1. Klik in het dialoogvenster Eigenschappen van SAML2 Update1 kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Eenmalige aanmelding configureren")


    een. Typ in het tekstvak **naam** een naam voor uw configuratie (bijvoorbeeld: **SAML 2.0**).

    b. In het **Veld van de gebruiker** tekstvak, type **e-mailbericht** of **user_id**, afhankelijk van welke veld wordt gebruikt om gebruikers in uw implementatie ServiceNow uniek te identificeren. 
    
    > [AZURE.NOTE] U kunt configue Azure AD als u wilt uitzenden van de Azure AD-gebruikers-ID (UPN) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > eenmalige aanmelding** sectie van de Azure klassieke portal en het gewenste veld aan het kenmerk **nameidentifier** toewijzen. De waarde voor het geselecteerde kenmerk van Azure AD (bijvoorbeeld UPN) moet overeenkomen met de waarde die zijn opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld user_id)

    c. De **Identiteit Provider-ID** -waarde kopiëren en plak deze in het tekstvak **Identiteit Provider URL** in de klassieke Azure AD-portal.

    d. De waarde **Verificatie aanvragen URL** kopiëren en plak deze in het tekstvak **id-Provider AuthnRequest** in de klassieke Azure AD-portal.

    e. Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure AD-portal en plak deze in het tekstvak **id-Provider SingleLogoutRequest** .

    f. Typ in het tekstvak **ServiceNow startpagina van** de URL van de startpagina van uw ServiceNow exemplaar.

    > [AZURE.NOTE] De startpagina van ServiceNow exemplaar is een samenvoeging van uw **ServieNow tenant URL** en **/navpage.do** (bijvoorbeeld:`https://fabrikam.service-now.com/navpage.do`).
 

    g. In de **entiteit-ID / uitgever** tekstvak, typ de URL van uw tenant ServiceNow.

    h. Typ in het tekstvak **Publiek URL** de URL van uw tenant ServiceNow. 

    ik. Typ in het tekstvak **Protocol Binding voor van de IDP SingleLogoutRequest** **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-omleiden**.

    j. Typ in het tekstvak NameID beleid **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: niet-opgegeven**.

    k. Hef de selectie **een AuthnContextClass maken**.

    l. Typ in de **Methode AuthnContextClassRef**, `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Dit is alleen nodig als u alleen cloud-organisatie. Als u een on-premises ADFS of MFA voor verificatie gebruikt moet u deze waarde niet configureren. 

    m. Typ in het tekstvak **Klok laten** **60**.

    n. Als **Eenmalige aanmelding op Script**, selecteer **MultiSSO_SAML2_Update1**.

    o. Als **x509 certificaat**, selecteer het certificaat dat u in de vorige stap hebt gemaakt.

    p. Klik op **verzenden**. 



6. In de klassieke Azure AD-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Eenmalige aanmelding configureren")

7. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.
 
    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Eenmalige aanmelding configureren")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD voor eenmalige aanmelding configureren voor ServiceNow Express

1.  Klik in de klassieke portal van Azure AD, klik op de pagina **ServiceNow** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ServiceNow** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-instellingen configureren** de volgende stappen uitvoeren:

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "URL van app configureren")

    een. Typ in het tekstvak **ServiceNow aanmelding op URL** de URL die wordt gebruikt door uw gebruikers eenmalige aanmelding in uw ServiceNow-toepassing het patroon volgen: `https://<instance-name>.service-now.com`.

    b. Typ in het tekstvak **Uitgever URL** de URL die wordt gebruikt door uw gebruikers voor eenmalige aanmelding in uw ServiceNow-toepassing na het patroon `https://<instance-name>.service-now.com`.

    c. Klik op **volgende**

4.  Klik op **handmatig configureren de toepassing voor eenmalige aanmelding**, en vervolgens op **volgende** en voer de volgende stappen uit.

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "URL van app configureren")

5.  Klik op de pagina **configureren eenmalige aanmelding bij ServiceNow** **certificaat**downloaden, het certificaatbestand lokaal op uw computer opslaan en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Eenmalige aanmelding configureren")

6. Eenmalige aanmelding in uw toepassing ServiceNow Express als beheerder.

7. Klik in het navigatiedeelvenster aan de linkerkant op **Eenmalige aanmelding**.  

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "URL van app configureren")


8. Klik in het dialoogvenster **Eenmalige aanmelding** op het configuratiepictogram in de rechterbovenhoek en de volgende eigenschappen instellen:

    ![URL van app configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "URL van app configureren")

    een. Schakelen tussen **meerdere provider SSO inschakelen** naar rechts.

    b. Schakelen tussen **inschakelen van logboekregistratie voor de integratie voor eenmalige aanmelding van meerdere provider voor foutopsporing** naar rechts.

    c. Typ in **het veld op de gebruiker van een tabel die...** tekstvak **gebruikersnaam**.


9. Klik in het dialoogvenster **Eenmalige aanmelding** op **Nieuw certificaat toevoegen**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Eenmalige aanmelding configureren")



10. Klik in het dialoogvenster **X.509-certificaten** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Eenmalige aanmelding configureren")

    een. Typ in het tekstvak **naam** een naam voor uw configuratie (bijvoorbeeld: **TestSAML2.0**).

    b. Selecteer **actieve**.

    c. Als **opmaken**, selecteert u **PEM**.

    d. Als het **Type**, selecteer **Store certificaat vertrouwen**.

    e. Een bestand Base64 codering van uw gedownloade certificaat maken.
   
    > [AZURE.NOTE] Zie [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.
    
    f. Een certificaat Base64-gecodeerde opent in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **PEM certificaat** .

    g. Klik op **bijwerken**.


11. Klik in het dialoogvenster **Eenmalige aanmelding** op **Nieuwe IdP toevoegen**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Eenmalige aanmelding configureren")


12. Klik in het dialoogvenster **Nieuwe identiteitsprovider toevoegen** onder **Identiteitsprovider configureren**, moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Eenmalige aanmelding configureren")


    een. Typ in het tekstvak **naam** een naam voor uw configuratie (bijvoorbeeld: **SAML 2.0**).

    b. De **Identiteit Provider-ID** -waarde kopiëren en plak deze in het tekstvak **Identiteit Provider URL** in de klassieke Azure AD-portal.

    c. De waarde **Verificatie aanvragen URL** kopiëren en plak deze in het tekstvak **id-Provider AuthnRequest** in de klassieke Azure AD-portal.

    d. Kopieer de **URL van de Service één Sign-Out** -waarde in de klassieke Azure AD-portal en plak deze in het tekstvak **id-Provider SingleLogoutRequest** .

    e. Als **Het certificaat Provider identiteit**, selecteer het certificaat dat u in de vorige stap hebt gemaakt.


13. Klik op **Geavanceerde instellingen**en klik onder **Aanvullende identiteit Provider eigenschappen**de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Eenmalige aanmelding configureren")

    een. Typ in het tekstvak **Protocol Binding voor van de IDP SingleLogoutRequest** **urn: oasis: namen: tc: SAML:2.0:bindings:HTTP-omleiden**.

    b. Typ in het tekstvak **NameID beleid** **urn: oasis: namen: tc: SAML:1.1:nameid-indeling: niet-opgegeven**.    

    c. Typ in de **Methode AuthnContextClassRef**, **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
    
    d. Hef de selectie **een AuthnContextClass maken**.

14. Klik onder **Aanvullende eigenschappen van de Service Provider**, moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Eenmalige aanmelding configureren")

    een. Typ in het tekstvak **ServiceNow startpagina van** de URL van de startpagina van uw ServiceNow exemplaar.

    > [AZURE.NOTE] De startpagina van ServiceNow exemplaar is een samenvoeging van uw **ServieNow tenant URL** en **/navpage.do** (bijvoorbeeld: `https://fabrikam.service-now.com/navpage.do`).

    b. In de **entiteit-ID / uitgever** tekstvak, typ de URL van uw tenant ServiceNow.

    c. Typ de URL van uw tenant ServiceNow in het tekstvak **Publiek URI** . 

    d. Typ in het tekstvak **Klok laten** **60**.

    e. In het **Veld van de gebruiker** tekstvak, type **e-mailbericht** of **user_id**, afhankelijk van welke veld wordt gebruikt om gebruikers in uw implementatie ServiceNow uniek te identificeren.
    
    > [AZURE.NOTE] U kunt configue Azure AD als u wilt uitzenden van de Azure AD-gebruikers-ID (UPN) of het e-mailadres als de unieke id in het SAML-token door te gaan naar de **ServiceNow > kenmerken > eenmalige aanmelding** sectie van de Azure klassieke portal en het gewenste veld aan het kenmerk **nameidentifier** toewijzen. De waarde voor het geselecteerde kenmerk van Azure AD (bijvoorbeeld UPN) moet overeenkomen met de waarde die zijn opgeslagen in ServiceNow voor het opgegeven veld (bijvoorbeeld user_id)

    f. Klik op **Opslaan**. 


15. In de klassieke Azure AD-portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**. 

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Eenmalige aanmelding configureren")

16. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.
 
    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de gebruiker de inrichting van Active Directory-gebruikersaccounts naar ServiceNow.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1. Klik op **configureren gebruiker inrichting**in de klassieke portal van Azure Management, klik op de pagina **ServiceNow** toepassing integratie. 

    ![De inrichting van gebruiker] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "De inrichting van gebruiker")


2. Klik op de pagina **Voer uw referenties ServiceNow om in te schakelen automatische gebruiker inrichting** bieden de volgende configuratieinstellingen: inrichting van gebruiker configureren 

     een. Typ de naam van het ServiceNow exemplaar in het tekstvak **ServiceNow exemplaar** .

     b. Typ de naam van het beheerdersaccount ServiceNow in het tekstvak **ServiceNow beheerder gebruikersnaam in te voeren** .

     c. Typ het wachtwoord voor dit account in het tekstvak **ServiceNow beheerderswachtwoord** .

     d. Klik op **valideren** om te controleren of uw configuratie.

     e. Klik op de knop **volgende** om de pagina van de **volgende stappen** te openen.

     f. Als u inrichten van alle gebruikers voor deze toepassing wilt, selecteert u "**Automatisch inrichten van alle accounts in de map naar deze toepassing**". 

    ![Volgende stappen] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Volgende stappen")

     g. Klik op de pagina **Vervolgstappen** op **voltooid** als u wilt opslaan van uw configuratie.

### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test
In dit gedeelte maakt u een testgebruiker in de klassieke portal Britta Simon genoemd.

![Azure AD-gebruiker maken][20]

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.
    
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren:
 
    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    een. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.

    b. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.

    c. Klik op **volgende**.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. Klik in het **Laatste** tekstvak type **Simon**.

    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .

    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.

    b. Klik op **Voltooien**.   


### <a name="creating-a-servicenow-test-user"></a>Maken van een gebruiker ServiceNow testen

In dit gedeelte maakt u een gebruiker Britta Simon in ServiceNow genoemd. In dit gedeelte maakt u een gebruiker Britta Simon in ServiceNow genoemd. Als u niet hoe weet u een gebruiker in uw ServiceNow of ServiceNow Express-account toevoegt, neemt u contact op met het ondersteuningsteam ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan ServiceNow inschakelen.

![Gebruiker toewijzen][200] 

**Als u wilt toewijzen Britta Simon aan ServiceNow, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ServiceNow**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203] 

1. Selecteer in de lijst alle gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]


### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.

Wanneer u op de tegel ServiceNow in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw ServiceNow-toepassing.

## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
