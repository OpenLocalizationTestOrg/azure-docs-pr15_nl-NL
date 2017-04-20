<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met ITRP | Microsoft Azure" 
    description="Meer informatie over het gebruiken van ITRP met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Zelfstudie: Azure Active Directory-integratie met ITRP
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en ITRP.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant ITRP
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan ITRP één Meld u aan bij de toepassing op uw bedrijfssite ITRP (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor ITRP inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Scenario")
##<a name="enabling-the-application-integration-for-itrp"></a>De toepassingsintegratie van de voor ITRP inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor ITRP.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor ITRP door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-itrp-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **ITRP**.

    ![Galerie van toepassing] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Galerie van toepassing")

7.  Selecteer **ITRP**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij ITRP met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor ITRP configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **ITRP** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij ITRP** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **ITRP Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. ITRP.com*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-itrp-tutorial/IC775568.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij ITRP** als u wilt downloaden van het certificaat, klikt u op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\ITRP.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite ITRP als beheerder.

6.  Op de werkbalk op de voorgrond, klikt u op **Instellingen**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  Selecteer in het linkernavigatiedeelvenster, **Eenmalige aanmelding**.

    ![Eenmalige aanmelding] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Eenmalige aanmelding")

8.  In de eenmalige aanmelding configuratiesectie, kunt u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Eenmalige aanmelding")

    ![Eenmalige aanmelding] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Eenmalige aanmelding")

    1.  Klik op **inschakelen**.
    2.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij ITRP** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **URL van externe Meld u af** .
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij ITRP** de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **SAML SSO-URL** .
    4.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **Certificaat vingerafdruk** .
        
        >[AZURE.TIP]Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    5.  Klik op **Opslaan**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij ITRP, moeten ze worden ingericht in ITRP.  
Bij ITRP, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **ITRP** .

2.  Klik op **Records**in de werkbalk op de voorgrond.

    ![Beheerder] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Beheerder")

3.  Selecteer **personen**in het pop-upmenu.

    ![Personen] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Personen")

4.  Klik op **nieuwe persoon toevoegen** ('en').

    ![Beheerder] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Beheerder")

5.  Klik in het dialoogvenster nieuwe persoon toevoegen de volgende stappen uitvoeren:

    ![Gebruiker] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Gebruiker")

    1.  Typ de **naam**, **e-mailbericht** van een geldige AAD-account dat u wilt inrichten.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere ITRP gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door ITRP aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan ITRP, moet u de volgende stappen uitvoeren:

1.  Klik in de portal Azure AD een testaccount te maken.

2.  Klik op de pagina **ITRP **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.