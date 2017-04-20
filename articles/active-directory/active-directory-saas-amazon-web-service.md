<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Amazon Web Service (AWS) | Microsoft Azure"
    description="Meer informatie over het gebruiken van Amazon Web Services (AWS) met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!"
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Zelfstudie: Azure Active Directory-integratie met Amazon Web Service (AWS)

Het doel van deze zelfstudie is om aan te geven hoe u Amazon Web Service (AWS) integreren met Azure Active Directory (Azure AD).  
Amazon Web Service (AWS) integreren met Azure AD biedt de volgende voordelen: 

- U kunt bepalen in Azure AD wie heeft er toegang naar Amazon Web Service (AWS) 
- U kunt uw gebruikers kunnen automatisch ophalen ondertekend-on naar Amazon Web Service (AWS) (eenmalige aanmelding) inschakelen met hun Azure AD-accounts
- U kunt uw accounts op één centrale locatie - de Azure klassieke-portal beheren

Als u weten meer informatie over SaaS app-integratie met Azure AD wilt, raadpleegt u [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten voor 

Als u wilt configureren Azure AD-integratie met Amazon Web Service (AWS), moet u de volgende items:

- Een Azure AD-abonnement
- Een Amazon Web Service (AWS) eenmalige aanmelding ingeschakeld abonnement


> [AZURE.NOTE] Als u wilt testen de stappen in deze zelfstudie, niet aangeraden een productieomgeving gebruiken.


Als u wilt testen de stappen in deze zelfstudie, moet u deze aanbevelingen volgt:

- Gebruik niet uw productieomgeving, tenzij dit nodig is.
- Als u een proefabonnement Azure AD-omgeving niet hebt, kunt u een één maand proefabonnement [hier](https://azure.microsoft.com/pricing/free-trial/)verkrijgen. 

 
## <a name="scenario-description"></a>Scenario beschrijving
Het doel van deze zelfstudie is om te kunnen eenmalige aanmelding Azure AD testen in een testomgeving.  
Het scenario die worden beschreven in deze zelfstudie bestaat uit drie belangrijkste bouwstenen:

1. Amazon Web Service (AWS) toe te voegen vanuit de galerie 
2. Configureren en testen van Azure AD eenmalige aanmelding


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Amazon Web Service (AWS) toe te voegen vanuit de galerie
Als u wilt de integratie van Amazon Web Service (AWS) in Azure AD configureren, moet u Amazon Web Service (AWS) in de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Als u wilt toevoegen Amazon Web Service (AWS) in de galerie, moet u de volgende stappen uitvoeren:

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**. 

    ![Active Directory][1] 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** . 
   
    ![Toepassingen][2]

4. Klik op **toevoegen** aan de onderkant van de pagina. 
   
    ![Toepassingen][3]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**. 
   
    ![Toepassingen][4]

6. Typ in het zoekvak **Amazon Web Service (AWS)**.
   
    ![Toepassingen][5]

7. Selecteer **Amazon Web Service (AWS)**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Toepassingen][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
Het doel van dit gedeelte is om aan te geven hoe configureren en test eenmalige aanmelding Azure AD met Amazon Web Service (AWS) op basis van een testgebruiker 'Britta Simon' genoemd.

Eenmalige aanmelding om te werken, moet Azure AD duidelijk de gebruiker die beschikbaar zijn in Amazon Web Service (AWS) aan een gebruiker in Azure AD is. Een relatie koppeling tussen een Azure AD-gebruiker en de gerelateerde gebruiker in Amazon Web Service (AWS) moet met andere woorden, tot stand worden gebracht.  
Deze relatie koppeling tot stand is gebracht door het toewijzen van de waarde van de **gebruikersnaam in te voeren** in Azure AD als de waarde van de **gebruikersnaam** in Amazon Web Service (AWS).
 
Als u wilt configureren en te testen Azure AD eenmalige aanmelding met Amazon Web Service (AWS), moet u voltooien van de volgende elementen:

1. **[Configureren Azure AD één eenmalige aanmelding](#configuring-azure-ad-single-single-sign-on)** - zodat uw gebruikers kunnen deze functie gebruiken.
2. **[Gebruiker maken van een Azure AD test](#creating-an-azure-ad-test-user)** - test Azure AD eenmalige aanmelding met Britta Simon.
4. **[Gebruiker maken van een Amazon Web Service (AWS) test](#creating-a-halogen-software-test-user)** - heeft een tegenhanger van Britta Simon in Amazon Web Service (AWS) dat is gekoppeld aan de Azure AD-weergave van haar.
5. **[Het toewijzen van de Azure AD testen gebruiker](#assigning-the-azure-ad-test-user)** - Britta Simon gebruik eenmalige aanmelding Azure AD inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)** - om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Eenmalige aanmelding Azure AD één configureren

Het doel van dit gedeelte is Azure AD eenmalige aanmelding in de portal van Azure klassieke inschakelen en configureren eenmalige aanmelding in uw toepassing Amazon Web Service (AWS).  
Uw toepassing Amazon Web Service (AWS) wordt de bevestigingen SAML verwacht een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** . De volgende schermafbeelding ziet u een voorbeeld hiervoor.


![Eenmalige aanmelding configureren][27]

**Als u wilt configureren eenmalige aanmelding Azure AD met Amazon Web Service (AWS), moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, klik op de pagina **Amazon Web Service (AWS)** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren][7]

2. Klik op de pagina **Hoe wilt u dat gebruikers aan te melden aan Amazon Web Service (AWS)** Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren][8]

3. Klik op volgende op de pagina van het dialoogvenster **App-instellingen configureren** . 

    ![App-instellingen configureren][9]
 
4. Klik op de pagina **configureren eenmalige aanmelding bij Amazon Web Service (AWS)** op **metagegevens downloaden**en sla het metagegevensbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren][10]

5. In een ander browservenster, eenmalige aanmelding bij uw bedrijfssite met Amazon Web Service (AWS) als beheerder.

6. Klik op **Start Console**.

    ![Eenmalige aanmelding configureren][11]

7. Klik op **identiteit en toegangsbeheer**. 

    ![Eenmalige aanmelding configureren][12]

8. **Identiteitsprovider**op en klik vervolgens op **Provider maken**. 

    ![Eenmalige aanmelding configureren][13]

9. Op de pagina van het dialoogvenster **Provider configureren** , moet u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren][14]

     een. Als het **Type van Provider**, selecteer **SAML**.

     b. Typ in het tekstvak **Naam Provider** de naam van een provider (bijvoorbeeld: *WAAD*).

     c. Klik op **Bestand kiezen**om het gedownloade metagegevensbestand.

     d. Klik op de **volgende stap**.


10. Klik op **maken**op de pagina **Gegevens controleren of** dialoogvenster. 

    ![Eenmalige aanmelding configureren][15]

11. Klik op **rollen**en klik vervolgens op **Nieuwe rol maken**. 

    ![Eenmalige aanmelding configureren][16]

12. Klik in het dialoogvenster **Naam van de rol instellen** , moet u de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren][17]

    een. Typ de rolnaam van een in het tekstvak **Rol** (bijvoorbeeld: *TestUser*).

    b. Klik op de **volgende stap**.

13. Klik in het dialoogvenster **Type rol selecteert u** de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren][18]

    een. Selecteer **de rol voor identiteit Provider-toegang**.

    b. Klik op **selecteren**in de sectie **verlenen Web eenmalige aanmelding (WebSSO) toegang tot SAML providers** .


14. Klik in het dialoogvenster **Vertrouwen stand brengen van** de volgende stappen uitvoeren:  

    ![Eenmalige aanmelding configureren][19]
     
     een. Als SAML-provider, selecteert u de SAML-provider u previousley hebt gemaakt (bijvoorbeeld: *WAAD*) 

     b. Klik op de **volgende stap**.


15. Klik op de **Volgende stap**in het dialoogvenster **Rol vertrouwen verifiëren** . 

    ![Eenmalige aanmelding configureren][32]


16. Klik op de **Volgende stap**in het dialoogvenster **Beleid bijvoegen** .  

    ![Eenmalige aanmelding configureren][33]


17. Klik in het dialoogvenster **controleren** kunt u de volgende stappen uitvoeren:   

    ![Eenmalige aanmelding configureren][34]

     een. Kopieer de waarde van de **Rol informatie** .

     b. Kopieer de waarde van de informatie **Vertrouwde entiteiten** .

     c. Klik op **rol maken**. 

18. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **volgende**.

    ![Wat is Azure AD Connect][20]

19. Klik op de pagina **confirmation voor eenmalige aanmelding** op **Voltooien** om het dialoogvenster **eenmalige aanmelding configureren** te sluiten.

    ![Wat is Azure AD Connect][22]


20. In het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** . 

    ![Eenmalige aanmelding configureren][21]

21. Klik op **gebruikerskenmerk toevoegen**. 

    ![Eenmalige aanmelding configureren][23]

22. Klik in het dialoogvenster gebruikerskenmerk toevoegen de volgende stappen uitvoeren. 

    ![Eenmalige aanmelding configureren][24] 

    een. Typ in het tekstvak **Kenmerk** **https://aws.amazon.com/SAML/Attributes/Role**.

    b. Typ in het tekstvak **Kenmerkwaarde** **[de rol informatie waarde], [de waarde van de vertrouwde entiteit informatie]**.

    >[AZURE.TIP] Dit zijn de waarden die u hebt gekopieerd in het dialoogvenster controleren wanneer u uw rol hebt gemaakt. 

    c. Klik op **Voltooien** om het dialoogvenster **Gebruikerskenmerk toevoegen** te sluiten.

23. Klik op **gebruikerskenmerk toevoegen**. 

    ![Eenmalige aanmelding configureren][23]


24. Klik in het dialoogvenster gebruikerskenmerk toevoegen de volgende stappen uitvoeren. 

    ![Eenmalige aanmelding configureren][25]


     een. Typ in het tekstvak **Kenmerk** **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. In het tekstvak **Kenmerkwaarde** typt of selecteert u **user.userprincipalname** in de vervolgkeuzelijst.
     
    ![Eenmalige aanmelding configureren][35]
    

     c. Klik op **Voltooien** om het dialoogvenster **Gebruikerskenmerk toevoegen** te sluiten.


25. Klik op **wijzigingen toepassen**. 

    ![Eenmalige aanmelding configureren][26]





### <a name="creating-an-azure-ad-test-user"></a>Maken van een gebruiker Azure AD-test

Het doel van dit gedeelte is het opzetten van een testgebruiker in de Azure klassieke portal Britta Simon genoemd.

![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Als u wilt maken van een testgebruiker in Azure AD, moet u de volgende stappen uitvoeren:**

1. Klik in de **klassieke Azure-portal**op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3. De lijst met gebruikers, in het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. U opent het dialoogvenster **Gebruiker toevoegen** in de werkbalk op de onderkant, op **Gebruiker toevoegen**. 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Klik op de pagina dialoogvenster **Vertel ons over deze gebruiker** , moet u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Als het Type van gebruiker, selecteert u nieuwe gebruiker in uw organisatie.
  2. Typ in de gebruikersnaam in te voeren **tekstvak** **BrittaSimon**.
  3. Klik op volgende.

6.  Klik op de pagina **Gebruikersprofiel** dialoogvenster kunt u de volgende stappen uitvoeren: 

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    een. Typ in het tekstvak **Voornaam** **Britta**.  

    b. In de **Achternaam** txtbox, type, **Simon**.
  
    c. Typ in het tekstvak **Naam weer te geven** **Britta Simon**.

    d. Selecteer de **gebruiker**in de lijst **functie** .
  
    e. Klik op **volgende**.

7. Klik op **maken**op de pagina dialoogvenster **krijgen tijdelijke wachtwoord** .

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. Klik op de pagina dialoogvenster **tijdelijke wachtwoord krijgen** kunt u de volgende stappen uitvoeren:

    ![Maken van een gebruiker Azure AD-test](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    een. Noteer de waarde van het **Nieuwe wachtwoord**in.
  
    b. Klik op **Voltooien**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Maken van een testgebruiker Amazon Web Service (AWS)

Het doel van dit gedeelte is het opzetten van een gebruiker met de naam Britta Simon in Amazon Web Service (AWS).

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Als u wilt maken van een gebruiker met de naam Britta Simon in Amazon Web Service (AWS), moet u de volgende stappen uitvoeren:

1. Meld u als beheerder aan bij uw bedrijfssite **Amazon Web Service (AWS)** .

2. Klik op het pictogram **Console start** . 

    ![Eenmalige aanmelding configureren][11]

3. Klik op identiteit en toegang tot Management. 

    ![Eenmalige aanmelding configureren][28]

4. In het Dashboard, klikt u op gebruikers en klik vervolgens op nieuwe gebruikers maken. 

    ![Eenmalige aanmelding configureren][29]

5. Klik in het dialoogvenster gebruiker maken de volgende stappen uitvoeren: 

    ![Eenmalige aanmelding configureren][30]

     een. Typ in de tekstvakken **Gebruikersnamen invoeren** van Brita Simon gebruikersnaam in te voeren (userprincipalname) in Azure AD.

     b. Klik op **maken**.




### <a name="assigning-the-azure-ad-test-user"></a>Het toewijzen van de gebruiker Azure AD-test

Het doel van dit gedeelte is aan het inschakelen van Britta Simon gebruiken Azure eenmalige aanmelding door haar toegang verlenen aan Amazon Web Service (AWS).

![Gebruiker toewijzen][31]

**Als u wilt toewijzen Britta Simon aan CloudPassage, moet u de volgende stappen uitvoeren:**

1. Klik in de klassieke Azure portal, de toepassingen om weergave te openen, in de mapweergave, **toepassingen** in het bovenste menu.

    ![Gebruiker toewijzen][26]

2. Selecteer in de lijst met toepassingen **Amazon Web Service (AWS)**.

    ![Gebruiker toewijzen][27]

1. In het menu aan de bovenkant, klikt u op **gebruikers**.

    ![Gebruiker toewijzen][25]

1. Selecteer in de lijst gebruikers **Britta Simon**.

2. In de werkbalk op de onderkant, klikt u op **toewijzen**.

    ![Gebruiker toewijzen][29]

### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

Het doel van deze sectie is uw Azure AD eenmalige aanmelding configuratie testen met het deelvenster Access.  
Wanneer u op de tegel Amazon Web Service (AWS) in het deelvenster Access, u moet krijgen automatisch aangemeld voor toegang tot uw toepassing Amazon Web Service (AWS).


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















