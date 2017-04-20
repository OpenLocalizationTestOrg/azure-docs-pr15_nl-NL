<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met AnswerHub | Microsoft Azure" 
    description="Meer informatie over het gebruiken van AnswerHub met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Zelfstudie: Azure Active Directory-integratie met AnswerHub

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software).  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan AnswerHub één Meld u aan bij de toepassing op uw bedrijfssite AnswerHub (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor AnswerHub inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Scenario")

##<a name="enabling-the-application-integration-for-answerhub"></a>De toepassingsintegratie van de voor AnswerHub inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor AnswerHub.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor AnswerHub door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **AnswerHub**.

    ![Galerie van toepassing] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Galerie van toepassing")

7.  Selecteer **AnswerHub**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij AnswerHub met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **AnswerHub** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij AnswerHub** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.answerhub.com*" op de pagina **App-URL configureren** in het tekstvak **AnswerHub Sign In URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij AnswerHub** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite AnswerHub als beheerder.
    >[AZURE.NOTE] Als u hulp bij de configuratie van AnswerHub nodig hebt, neemt u contact op met [het ondersteuningsteam van AnswerHub](mailto:success@answerhub.com. ).








6.  Ga naar **beheer**.

7.  Klik op het tabblad **gebruikers en groepen** .

8.  Klik in het navigatiedeelvenster aan de linkerkant, in de sectie **Sociale instellingen** op **SAML-instelling**.

9.  Klik op de tab **IDP Config** .

10. Klik op het tabblad **IDP Config** kunt u de volgende stappen uitvoeren:

    ![Op SAML-instelling] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "Op SAML-instelling")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij AnswerHub** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **IDP aanmeldings-URL** .
    2.  Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **IDP afmelden URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij AnswerHub** .
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij AnswerHub** de **Naamindeling id** -waarde kopiëren en plak deze in het tekstvak **IDP id naamindeling** .
    4.  Klik op **toetsen en certificaten**.

11. Klik op het tabblad sleutels en certificaten, moet u de volgende stappen uitvoeren:

    ![Sleutels en certificaten] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Sleutels en certificaten")

    1.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    2.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **IDP openbare sleutel (x 509-indeling)** .
    3.  Klik op **Opslaan**.

12. Klik op **Opslaan**op het tabblad **IDP Config** .

13. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij AnswerHub, moeten ze worden ingericht in AnswerHub.  
Bij AnswerHub, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **AnswerHub** .

2.  Ga naar **beheer**.

3.  Klik op het tabblad **gebruikers en groepen** .

4.  Klik in het navigatiedeelvenster aan de linkerkant, in de sectie **Gebruikers beheren** op **gebruikers maken of importeren**.

    ![Gebruikers en groepen] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Gebruikers en groepen")

5.  Typ het **e-mailadres**, **gebruikersnaam** en **wachtwoord** van een geldige Azure Active Directory-account inrichten in de gerelateerde tekstvakken gewenste en klik vervolgens op **Opslaan**.

>[AZURE.NOTE] U kunt een andere AnswerHub gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door AnswerHub aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan AnswerHub, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **AnswerHub **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
