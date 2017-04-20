<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ClickTime | Microsoft Azure" 
    description="Meer informatie over het gebruiken van ClickTime met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Zelfstudie: Azure Active Directory-integratie met ClickTime

In deze zelfstudie leert u hoe u ClickTime integreren met Azure Active Directory (Azure AD).

ClickTime integreren met Azure AD biedt de volgende voordelen:

- U kunt bepalen in Azure AD wie toegang tot ClickTime heeft
- U kunt dat uw gebruikers kunnen automatisch ophalen die zijn aangemeld voor toegang tot ClickTime (eenmalige aanmelding) met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt configureren Azure AD-integratie met ClickTime, moet u de volgende items:

- Een Azure AD-abonnement
- Een ClickTime eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen.


## <a name="scenario-description"></a>Scenario beschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving.

Het scenario die worden beschreven in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ClickTime toevoegen vanuit de galerie
2. Configureren en testen van Azure AD eenmalige aanmelding

##<a name="adding-clicktime-from-the-gallery"></a>ClickTime toevoegen vanuit de galerie

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor ClickTime.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor ClickTime door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **ClickTime**.

    ![Galerie van toepassing] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Galerie van toepassing")

7.  Selecteer **ClickTime**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie, die u kunt configureren en testen eenmalige aanmelding Azure AD met ClickTime op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in ClickTime in Azure AD aan een gebruiker is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ClickTime moet met andere woorden, tot stand worden gebracht.

Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in ClickTime.

Als u wilt configureren en te testen Azure AD eenmalige aanmelding met ClickTime, die u moet uitvoeren van de volgende elementen:

1. **[Eenmalige aanmelding configureren Azure AD](#configuring-azure-ad-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
3. **[Gebruiker maken van een ClickTime testen](#creating-a-clicktime-test-user)** - hebben een tegenhanger van Britta Simon in ClickTime dat is gekoppeld aan de Azure AD-weergave van haar.
4. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.


### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij ClickTime met hun account in Azure AD met Federatie op basis van het SAML-protocol.  


>[AZURE.IMPORTANT] Om u te kunnen eenmalige aanmelding configureren op uw tenant ClickTime, moet u contact opnemen met eerst de ClickTime technische ondersteuning als u deze functie is ingeschakeld.

**Als u wilt configureren eenmalige aanmelding Azure AD met ClickTime, moet u de volgende stappen uitvoeren:**

1.  Klik in de klassieke Azure portal, klik op de pagina **ClickTime** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Eenmalige aanmelding inschakelen")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ClickTime** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Eenmalige aanmelding configureren")

3. Klik op de pagina **App-instellingen configureren** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    een. Typ de URL met het volgende patroon in het tekstvak **IdentifierL** : **https://app.clicktime.com/sp/**
    
    b. In het tekstvak **Antwoord URL** typt u de URL voor het gebruik van de volgende patroon: **https://app.clicktime.com/Login/**

    c. Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij ClickTime** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Eenmalige aanmelding configureren")

4.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite ClickTime als beheerder.

5.  In de werkbalk op de bovenkant, op **Voorkeuren**en klik vervolgens op **Beveiligingsinstellingen**.

6.  In de sectie **Voorkeuren voor eenmalige aanmelding** configuratie van de volgende stappen uitvoeren:

    ![Beveiligingsinstellingen] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Beveiligingsinstellingen")

    een.  Selecteer **toestaan** aanmelden met eenmalige aanmelding (SSO) met **Azure AD**.
    
    b.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij ClickTime** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **Identiteit Provider-eindpunt** .

    c.  Open het base 64 gecodeerde certificaat in **Kladblok**, Kopieer de inhoud en plak deze in het tekstvak **X.509-certificaat** .
    
    d.  Klik op **Opslaan**.

7.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij ClickTime, moeten ze worden ingericht in ClickTime.  
Bij ClickTime, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **ClickTime** .

2.  Op de werkbalk op de voorgrond, klikt u op **bedrijf**en klik vervolgens op **personen**.

    ![Personen] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Personen")

3.  Klik op de **persoon toevoegen**.

    ![Persoon toevoegen] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Persoon toevoegen")

4.  Klik in de sectie nieuwe persoon moet u de volgende stappen uitvoeren:

    ![Personen] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Personen")

    een.  Typ in het tekstvak **e-mailadres** het e-mailadres van uw Azure AD-account.
    
    b.  Typ in het tekstvak **volledige naam** de naam van uw Azure AD-account.  

    >[AZURE.NOTE] Als u wilt, kunt u aanvullende eigenschappen van het nieuwe persoon-object instellen.

    c.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere ClickTime gebruiker account hulpmiddelen voor het maken of API's verstrekt door ClickTime voor het inrichten van Azure AD-gebruikersaccounts.

### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

In deze sectie, kunt u Britta Simon gebruiken Azure eenmalige aanmelding door het verlenen van toegang aan ClickTime inschakelen.

![Gebruiker toewijzen][200]

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

**Als u wilt toewijzen Britta Simon aan ClickTime, de volgende stappen uitvoeren**

1. Klik in de klassieke portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ClickTime**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][203]

4. Selecteer in de lijst gebruikers **Britta Simon**.

5. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][205]

## <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding
In deze sectie test u uw Azure AD eenmalige aanmelding configuratie via het Configuratiescherm van Access.

Wanneer u op de tegel ClickTime in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw ClickTime-toepassing.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png