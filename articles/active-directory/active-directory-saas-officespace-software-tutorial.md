<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met OfficeSpace Software | Microsoft Azure" 
    description="Leer hoe u OfficeSpace Software gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerd inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Zelfstudie: Azure Active Directory-integratie met OfficeSpace-Software
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en OfficeSpace Software.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een OfficeSpace Software eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan OfficeSpace Software één Meld u aan bij de toepassing op uw bedrijfssite OfficeSpace Software (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor OfficeSpace Software inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Scenario")
##<a name="enabling-the-application-integration-for-officespace-software"></a>De toepassingsintegratie van de voor OfficeSpace Software inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor OfficeSpace Software.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor OfficeSpace Software inschakelen, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **OfficeSpace Software**.

    ![Galerie van toepassing] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Galerie van toepassing")

7.  Selecteer **OfficeSpace Software**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![OfficeSpace-Software] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace-Software")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij OfficeSpace Software met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor OfficeSpace Software configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **OfficeSpace Software** -toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Configureren eenmalige = op] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Configureren eenmalige = op")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de OfficeSpace Software** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** , in het tekstvak **OfficeSpace Software Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw OfficeSpace Software-toepassing (bijvoorbeeld: "*https://company.officespacesoftware.com*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij OfficeSpace Software** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite OfficeSpace Software als beheerder.

6.  Ga naar **beheerder \> verbindingslijnen**.

    ![Beheerder] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Beheerder")

7.  Klik op **SAML autorisatie**.

    ![Verbindingslijnen] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Verbindingslijnen")

8.  Klik in de sectie **SAML autorisatie** moet u de volgende stappen uitvoeren:

    ![Op SAML-configuratie] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "Op SAML-configuratie")

    1.  De waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Afmelden provider url** in de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij OfficeSpace Software** .
    2.  Kopieer de **URL van externe afmelden** -waarde in de klassieke Azure portal, klik op de dialoog pagina **configureren eenmalige aanmelding bij OfficeSpace Software** en plak deze in het tekstvak **Client idp doel-url** .
    3.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Client idp certificaat vingerafdruk** .  

        >[AZURE.TIP]
        Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    4.  Klik op **Instellingen opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij OfficeSpace Software, moeten ze worden ingericht in OfficeSpace Software. Bij OfficeSpace Software, met de inrichting van is een geautomatiseerde taak.  
Er is geen actie-item voor u.  
Gebruikers worden automatisch zo nodig gemaakt tijdens de eerste eenmalige aanmelding poging.

>[AZURE.NOTE]U kunt een andere OfficeSpace Software gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door OfficeSpace Software aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers naar OfficeSpace Software, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina van de integratie in de **OfficeSpace Software **-toepassing, op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.