<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Concur | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Concur met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Zelfstudie: Azure Active Directory-integratie met Concur  


Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Concur.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant in Concur

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Concur inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-concur-tutorial/IC769766.png "Scenario")

>[AZURE.NOTE] De configuratie van uw abonnement Concur voor federatieve SSO via SAML is een afzonderlijke taak, moet u contact opnemen met Concur om uit te voeren.

##<a name="enabling-the-application-integration-for-concur"></a>De toepassingsintegratie van de voor Concur inschakelen

Het doel van dit gedeelte is om een overzicht van het inschakelen van de toepassingsintegratie van de voor Concur te.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Concur inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.  In de **Directory** lijst, selecteer de map waarvoor wilt directory-integratie inschakelen.

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-concur-tutorial/IC700994.png "Toepassingen")

4.  Als u wilt openen in de **Galerie van toepassing**, klik op **Een App toevoegen**en klik op **toevoegen een toepassing voor mijn organisatie om te gebruiken**.

    ![Wat wilt u doen?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Wat wilt u doen?")

5.  Typ in het **zoekvak** **Concur**.

    ![Galerie van toepassing] (./media/active-directory-saas-concur-tutorial/IC721727.png "Galerie van toepassing")

6.  Selecteer **Concur**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Instemming] (./media/active-directory-saas-concur-tutorial/IC721728.png "Instemming")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Concur met hun account in Azure AD met Federatie op basis van het SAML-protocol.

>[AZURE.NOTE] De configuratie van uw abonnement Concur voor federatieve SSO via SAML is een afzonderlijke taak, moet u contact opnemen met Concur om uit te voeren.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Concur **toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-concur-tutorial/IC769767.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Concur** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-concur-tutorial/IC769768.png "Eenmalige aanmelding configureren")

3.  Op de pagina **App-URL configureren** in het tekstvak **Sign In URL instemming** uw concur tenant aanmeldingsproblemen URL hebt getypt en klik vervolgens op **volgende**: 

    ![Meld u aan de URL configureren] (./media/active-directory-saas-concur-tutorial/IC769769.png "Meld u aan de URL configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Concur** kunt u de volgende stappen uitvoeren.

    ![Meld u aan de URL configureren] (./media/active-directory-saas-concur-tutorial/IC769770.png "Meld u aan de URL configureren")

    1.  Klik op de metagegevens, ingeschakeld en vervolgens het gegevensbestand naar uw computer downloaden.
    2.  Neem contact op met het ondersteuningsteam Concur eenmalige aanmelding configureren voor uw tenant.
    3.  Selecteer de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.  

    >[AZURE.NOTE] De configuratie van uw abonnement Concur voor federatieve SSO via SAML is een afzonderlijke taak, moet u contact opnemen met Concur om uit te voeren.

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Het doel van dit gedeelte is om een overzicht van het inschakelen van de inrichting van Active Directory-gebruikersaccounts naar Concur te.

Heeft om in te schakelen apps in de Service onkosten, er worden beginletters instellen en het gebruik van een profiel Web servicebeheerder. Niet de WS-beheerdersrol gewoon toevoegen aan uw bestaande beheerdersprofiel dat u voor T & E beheerfuncties gebruikt.

Consultants instemming of de beheerder van de client moet een unieke Web Service-beheerder-profiel maken en de beheerder van de Client moet dit profiel gebruiken voor de beheerder van de Web Services-functies (bijvoorbeeld waardoor apps). Deze profielen moeten worden bewaard los van de client dagelijkse T & E beheerder beheerdersprofiel (het profiel van de beheerder T & E mogen geen de WSAdmin rol).

Wanneer u het profiel moet worden gebruikt voor het inschakelen van de app hebt gemaakt, kunt u de naam van de beheerder van de client invoeren in de velden voor gebruikersprofiel. Hierbij worden eigendom toegewezen aan het profiel. Nadat de profiel(en) is gemaakt, moet de client Meld u aan met dit profiel klikken op de knop '*inschakelen*' voor een Partner-App in het menu-webservices.

Voor de volgende oorzaken hebben, moet deze actie niet worden uitgevoerd met het profiel dat ze voor normale T & E-beheer gebruiken.

1.  De client is de fase die "*Ja*" klikt in het venster dialoog die wordt weergegeven als een app is ingeschakeld. Klikt u op bevestigt dat de client is bereid voor de toepassing van de Partner voor toegang tot hun gegevens, zodat u of de Partner, kunt u niet op deze knop Ja.
2.  Als de beheerder van een client die door een app zijn ingeschakeld voor het gebruik van de T- en E-beheerder profiel het bedrijf verlaat (resulteert in het profiel dat wordt uitgeschakeld), apps ingeschakeld met behulp van dat profiel niet werkt totdat de app met een andere actieve WS beheerder profiel is ingeschakeld. Daarom u gewoonlijk distinct WS beheerder profielen maken.
3.  Als een beheerder van het bedrijf verlaat, kan de naam die is gekoppeld aan het profiel WS-beheerder worden gewijzigd in de beheerder van de vervangende indien gewenst zonder die invloed hebben op dat de ingeschakelde app omdat dit profiel niet hoeft deactiveren

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **Concur** .

2.  Selecteer in het menu **beheer** **Webservices**.

    ![Concur tenant] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur tenant")

3.  Selecteer aan de linkerkant, in het deelvenster **Webservices** **Partner-toepassing inschakelen**.

    ![Partnertoepassing inschakelen] (./media/active-directory-saas-concur-tutorial/IC721730.png "Partnertoepassing inschakelen")

4.  Selecteer **Azure Active Directory**in de lijst **Toepassing inschakelen** en klik vervolgens op **inschakelen**.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Klik op **Ja** om het dialoogvenster **Actie bevestigen** te sluiten.

    ![Bevestigen] (./media/active-directory-saas-concur-tutorial/IC721732.png "Bevestigen")

6.  Selecteer in de portal voor Azure klassieke **Concur** uit de lijst met toepassingen om de pagina met **Concur** dialoogvenster te openen.

7.  U opent de pagina voor het dialoogvenster **Gebruiker inrichten configureren** , op de **inrichting van de gebruiker configureren**.

8.  Voer de gebruikersnaam en het wachtwoord van uw beheerder Concur en klik vervolgens op **volgende**.

9.  Klik op de knop **voltooid** om de configuratie, klik op **de bevestigingspagina** .

U kunt nu een testaccount maken, 10 minuten wachten en dat het account is gesynchroniseerd met Concur verifiëren.
##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Concur, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Concur **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-concur-tutorial/IC769771.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-concur-tutorial/IC767830.png "Ja")

U moet nu 10 minuten wachten en controleer of dat het account is gesynchroniseerd met Concur.

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
