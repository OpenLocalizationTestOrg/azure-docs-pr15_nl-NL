<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met AirWatch | Microsoft Azure" 
    description="Meer informatie over het gebruiken van AirWatch met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerd inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Zelfstudie: Azure Active Directory-integratie met AirWatch

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en AirWatch.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een AirWatch eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan AirWatch één Meld u aan bij de toepassing op uw bedrijfssite AirWatch (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor AirWatch inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>De toepassingsintegratie van de voor AirWatch inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor AirWatch.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor AirWatch door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **AirWatch**.

    ![Galerie van toepassing] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Galerie van toepassing")

7.  Selecteer **AirWatch**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij AirWatch met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **AirWatch** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij AirWatch** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **AirWatch aanmelding op URL** de URL die wordt gebruikt door uw gebruikers te melden bij uw AirWatch-toepassing (bijvoorbeeld: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij AirWatch** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite AirWatch als beheerder.

6.  In het linkernavigatiedeelvenster **Accounts**op en klik vervolgens op **beheerders**.

    ![Beheerders] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Beheerders")

7.  Uitvouwen van het menu **Instellingen** en klik vervolgens op **Directory Services**.

    ![Instellingen] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Instellingen")

8.  Klik op het tabblad **gebruiker** in het tekstveld **Base DN** , typ uw domeinnaam en klik vervolgens op **Opslaan**.

    ![Gebruiker] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Gebruiker")

9.  Klik op het tabblad **Server** .

    ![Server] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")

10. De volgende stappen uitvoeren:

    ![Uploaden] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Uploaden")

    1.  Als het **Type adreslijst**, selecteert u **geen**.
    2.  Selecteer **gebruiken SAML voor verificatie**.
    3.  Klik op **uploaden**om het gedownloade certificaat.

11. Klik in de sectie **aanvragen** kunt u de volgende stappen uitvoeren:

    ![Aanvragen] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Aanvragen")

    1.  Als het **Type binden aanvraag**, selecteert u **bericht**.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Airwatch** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **Identiteit Provider eenmalige aanmelding op URL** .
    3.  Als **NameID opmaken**, selecteert u **E-mailadres**.
    4.  Klik op **Opslaan**.

12. Klik nogmaals op het tabblad van de **gebruiker** .

    ![Gebruiker] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Gebruiker")

13. Klik in de sectie **kenmerk** moet u de volgende stappen uitvoeren:

    ![Kenmerk] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Kenmerk")

    1.  Typ in het tekstvak **Object-id** **http://schemas.microsoft.com/identity/claims/objectidentifier**.
    2.  Typ in het tekstvak **gebruikersnaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  Typ **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**in het tekstvak **Naam weer te geven** .
    4.  Typ in het tekstvak **Voornaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  Typ in het tekstvak **Achternaam** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6.  Typ in het tekstvak **e** **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Klik op **Opslaan**.

14. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij AirWatch, moeten ze worden ingericht in AirWatch.  
Bij AirWatch, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **AirWatch** .

2.  Klik in het navigatiedeelvenster aan de linkerkant op **Accounts**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Gebruikers")

3.  In het menu **gebruikers** op **De weergave lijst**en klik vervolgens op **toevoegen \> gebruiker toevoegen**.

    ![Gebruiker toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Gebruiker toevoegen")

4.  Klik in het dialoogvenster **toevoegen / gebruiker bewerken** , moet u de volgende stappen uitvoeren:

    ![Gebruiker toevoegen] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Gebruiker toevoegen")

    1.  Typ de **gebruikersnaam**, **wachtwoord**, **Bevestig het wachtwoord**, **Voornaam**, **Achternaam**, **E-mailadres** van een geldig Azure Active Directory-account dat u inrichten in de gerelateerde tekstvakken wilt.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere AirWatch gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door AirWatch aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan AirWatch, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **AirWatch **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
