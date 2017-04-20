<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met DocuSign | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en DocuSign configureren."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Zelfstudie: Azure Active Directory-integratie met DocuSign

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en DocuSign.
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

- Een geldig abonnement op Azure
- Een tenant in DocuSign



Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1. [De toepassingsintegratie van de voor DocuSign inschakelen](#enabling-the-application-integration-for-docusign) 


2. [Eenmalige aanmelding configureren](#configuring-single-sign-on) 


3. [De inrichting van account configureren](#configuring-account-provisioning) 


4. [Gebruikers toewijzen](#assigning-users) 

    ![Eenmalige aanmelding configureren][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>De toepassingsintegratie van de voor DocuSign inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor DocuSign.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor DocuSign door de volgende stappen uitvoeren:

1. Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Eenmalige aanmelding configureren][1]

2. Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst Directory.

3. De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Eenmalige aanmelding configureren][2]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Toepassingen][3]

5. Klik op de wat wilt u dialoogvenster doen, klikt u op **een toepassing in de galerie toevoegen**.

    ![Eenmalige aanmelding configureren][4]


6. Typ in het zoekvak **DocuSign**.

    ![Eenmalige aanmelding configureren][5]

7. Selecteer **DocuSign**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Eenmalige aanmelding configureren][6]


## <a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij DocuSign met hun account in Azure AD met Federatie op basis van het SAML-protocol.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1. Klik in de klassieke Azure portal, klik op de pagina **DocuSign toepassingsintegratie** op **eenmalige aanmelding configureren** om het dialoogvenster configureren eenmalige aanmelding.

    ![Eenmalige aanmelding configureren][7]

2. Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij DocuSign** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op volgende.

    ![Eenmalige aanmelding configureren][8]

3. Klik op de pagina **App-instellingen configureren** de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren][61]

    een. Typ in het tekstvak **zich aanmeldt op URL** `https://account.docusign.com/*`.  

    b. Typ in het tekstvak **id** `https://account.docusign.com/*`.  
   
    c. Klik op **volgende**. 


    > [AZURE.TIP] De aanmelding op URL en id-waarden zijn alleen tijdelijke aanduidingen. Instructies voor het ophalen van de werkelijke waarden voor uw omgeving vallen verderop in dit onderwerp.
 

4. Klik op de pagina **configureren eenmalige aanmelding bij DocuSign** op **certificaat downloaden**en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren][10]


5. In een browservenster verschillende web en meld u aan bij uw **DocuSign-beheerportal** als beheerder.


6. Klik in het navigatiemenu aan de linkerkant, op **domeinen**.

    ![Eenmalige aanmelding configureren][51]

7. Klik in het rechterdeelvenster op **Claimen domein**.

    ![Eenmalige aanmelding configureren][52]

8. Klik in het dialoogvenster **claimen een domein** in het tekstvak **Domeinnaam** , typt u uw bedrijfsdomein en klik op **claimen**. Zorg ervoor dat u het domein verifiëren en de status actief is.

    ![Eenmalige aanmelding configureren][53]

9. Klik in het menu aan de linkerkant op **Identiteitsproviders**  

    ![Eenmalige aanmelding configureren][54]

10. Klik in het rechterdeelvenster op **Identiteitsprovider toevoegen**. 
    
    ![Eenmalige aanmelding configureren][55]

11. Klik op de pagina **Instellingen voor de Provider identiteiten** kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren][56]


    een. Typ in het tekstvak **naam** een unieke naam voor de configuratie. Gebruik geen spaties.

    b. Kopieer de URL van de uitgever in de portal voor Azure klassieke en plak deze in het tekstvak **Identiteit Provider uitgever** .

    c. Klik in de klassieke Azure portal de **Externe aanmeldings-URL**kopiëren en plak deze in het tekstvak **Identiteit Provider aanmeldings-URL** .

    d. Kopieer de **URL van externe Meld u af**en plak deze in het tekstvak **Identiteit Provider afmelden URL** in de klassieke Azure portal.

    e. Selecteer **Aanmelden AuthN verzoek**.

    f. Als **de aanvraag AuthN verzenden door**, selecteert u **bericht**.

    g. Als het **verzoek Meld u af door te sturen**, selecteert u **bericht**. 


12. Kies het veld dat u wilt toewijzen met Azure AD claimen in de sectie **Aangepast kenmerk toewijzen** . In dit voorbeeld is het claimen **emailaddress** met de waarde van **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**toegewezen. Dit is de standaardnaam claimen van Azure AD voor e-mailbericht claimen. 

    > [AZURE.NOTE] Gebruik de juiste **gebruikers-id** aan de gebruiker van Azure AD Docusign gebruiker toewijzen aan toewijzen. Selecteer het juiste veld en voer de gewenste waarde op basis van uw organisatie-instellingen.

    ![Eenmalige aanmelding configureren][57]

13. Klik in de sectie **Identiteit Provider certificaat** op **Certificaat toevoegen**en upload het certificaat dat u hebt gedownload van klassieke Azure AD-portal.   

    ![Eenmalige aanmelding configureren][58]

14. Klik op **Opslaan**.

15. Klik in de sectie **Identiteitsprovider** op **Acties**en klik vervolgens op **eindpunten**.   

    ![Eenmalige aanmelding configureren][59]



10. In de klassieke Azure portal, Ga terug naar de pagina **App-instellingen configureren** . 

16. In **de beheerportal DocuSign**, in de sectie **Weergave SAML 2.0 eindpunten** , de volgende stappen uitvoeren:

    ![Eenmalige aanmelding configureren][60]

    een. Kopieer de **URL van de uitgever Service Provider**en plak deze in het tekstvak **id** in de klassieke Azure-portal.

    b. De **Service Provider aanmeldings-URL**kopiëren en plak in het tekstvak **Aanmelden op de URL** van de Azure klassieke portal.

    c.  Klik op **sluiten**  


10. In de klassieke Azure portal, klik op **volgende**. 


15. In de klassieke Azure portal, selecteert u de **voor eenmalige aanmelding Configuratiebevestiging**en klik vervolgens op **volgende**.

    ![Toepassingen][14]

10. Klik op de pagina **confirmation voor eenmalige aanmelding** op **voltooid**.

    ![Toepassingen][15]
 

## <a name="configuring-account-provisioning"></a>De inrichting van account configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van de gebruiker de inrichting van Active Directory-gebruikersaccounts naar DocuSign.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1. Klik op van de **inrichting van de account configureren** om het dialoogvenster gebruiker inrichting configureren in de **klassieke Azure-portal**, klik op de pagina **DocuSign toepassingsintegratie** .

    ![De inrichting van account configureren][30]

2. Klik op de pagina **instellingen en beheerdersreferenties** de referenties van een account DocuSign opgeven met voldoende rechten als u wilt dat automatische gebruiker inrichting, en klik op **volgende**. 

    ![De inrichting van account configureren][31]

3. Klik in het dialoogvenster **verbinding testen** op **test starten**en klik op **volgende**na een geslaagde test.

    ![De inrichting van account configureren][32]

3. Klik op **de bevestigingspagina** op **voltooid**.

    ![De inrichting van account configureren][33]
 

## <a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan DocuSign, moet u de volgende stappen uitvoeren:

1. Klik in de **portal van Azure klassieke**een testaccount te maken.

2. Klik op de pagina **DocuSign toepassingsintegratie** op **toewijzen aan gebruikers**.

    ![Gebruikers toewijzen][40]
 

3. Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Gebruikers toewijzen][41]


U moet nu 10 minuten wachten en controleer of dat het account is gesynchroniseerd met DocuSign.

Als eerste stap verificatie, kunt u de inrichten status controleren door te klikken op Dashboard op het tabblad D op de pagina DocuSign toepassing-integratie in de klassieke Azure-portal.

![Gebruikers toewijzen][42]

De inrichting van cyclus van een met succes zijn gebruiker wordt aangegeven door een gerelateerde status:

![Gebruikers toewijzen][43]


Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access.

Zie Inleiding tot het deelvenster Access voor meer informatie over het Access-deelvenster.


## <a name="additional-resources"></a>Aanvullende informatie

* [Lijst met zelfstudies over het SaaS Apps integreren met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toepassing toegang en eenmalige aanmelding met Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png