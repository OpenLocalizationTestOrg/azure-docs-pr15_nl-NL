<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Kintone | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Kintone met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Zelfstudie: Azure Active Directory-integratie met Kintone
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Kintone.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Kintone eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Kintone één Meld u aan bij de toepassing op uw bedrijfssite Kintone (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Kintone inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Scenario")
##<a name="enabling-the-application-integration-for-kintone"></a>De toepassingsintegratie van de voor Kintone inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Kintone.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Kintone door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-kintone-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Kintone**.

    ![Galerie van toepassing] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Galerie van toepassing")

7.  Selecteer **Kintone**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Kintone met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Kintone** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Kintone** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*https://company.kintone.com*" op de pagina **App-URL configureren** in het tekstvak **Kintone aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-kintone-tutorial/IC785875.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Kintone** op **Download-certificaat**te downloaden van het certificaat, en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite **Kintone** als beheerder.

6.  Klik op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Instellingen")

7.  Klik op **gebruikers en Systeembeheer**.

    ![Gebruikers en Systeembeheer] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Gebruikers en Systeembeheer")

8.  Klik onder **Systeembeheer \> beveiliging** Klik op **Login**.

    ![Aanmelden] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Aanmelden")

9.  Klik op **inschakelen op SAML-verificatie**.

    ![Op SAML-verificatie] (./media/active-directory-saas-kintone-tutorial/IC785882.png "Op SAML-verificatie")

10. In de sectie SAML verificatie, moet u de volgende stappen uitvoeren:

    ![Op SAML-verificatie] (./media/active-directory-saas-kintone-tutorial/IC785883.png "Op SAML-verificatie")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Kintone** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    2.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Kintone** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL Meld u af** .
    3.  Klik op **Bladeren** om te uploaden van uw gedownloade certificaat.
    4.  Klik op **Opslaan**.

11. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Kintone, moeten ze worden ingericht in Kintone.  
Bij Kintone, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Kintone** .

2.  Klik op **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Instellingen")

3.  Klik op **gebruikers en Systeembeheer**.

    ![Gebruiker & Systeembeheer] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Gebruiker & Systeembeheer")

4.  Klik onder **Beheer van de gebruiker**, klikt u op **afdelingen en gebruikers**.

    ![Afdeling & gebruikers] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Afdeling & gebruikers")

5.  Klik op **nieuwe gebruiker**.

    ![Nieuwe gebruikers] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Nieuwe gebruikers")

6.  Klik in de sectie **Nieuwe gebruiker** moet u de volgende stappen uitvoeren:

    ![Nieuwe gebruikers] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Nieuwe gebruikers")

    1.  Typ een **Weergavenaam**, **Aanmeldingsnaam**, **Een nieuw wachtwoord**, **Bevestig het wachtwoord**, **E-mailadres** en andere details van een geldige AAD-account inrichten in de gerelateerde texboxes gewenste.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere Kintone gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Kintone aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Kintone, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Kintone **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.