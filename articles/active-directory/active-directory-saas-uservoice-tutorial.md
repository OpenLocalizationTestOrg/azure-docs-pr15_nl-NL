<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met UserVoice | Microsoft Azure" 
    description="Informatie over het gebruiken van UserVoice met Azure Active Directory om te schakelen eenmalige aanmelding, geautomatiseerde inrichting en meer!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Zelfstudie: Azure Active Directory-integratie met UserVoice
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en UserVoice.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant UserVoice
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan UserVoice één Meld u aan bij de toepassing op uw bedrijfssite UserVoice (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor UserVoice inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Scenario")

##<a name="enabling-the-application-integration-for-uservoice"></a>De toepassingsintegratie van de voor UserVoice inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor UserVoice.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor UserVoice door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **UserVoice**.

    ![Galerie van toepassing] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Galerie van toepassing")

7.  Selecteer **UserVoice**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![UserVoice] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij UserVoice met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor UserVoice configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **UserVoice** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij UserVoice** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **UserVoice Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. UserVoice.com*', en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij UserVoice** als u wilt downloaden van het certificaat, klikt u op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\UserVoice.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite UserVoice als beheerder.

6.  Op de werkbalk op de voorgrond, klikt u op instellingen en selecteer Web-portal in het menu.

    ![Instellingen] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Instellingen")

7.  Klik op het tabblad **Web-portal** , klik in de sectie **gebruikersverificatie** op **bewerken** om het dialoogvenster-pagina **Gebruikersverificatie bewerken** te openen

    ![Web-portal] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Web-portal")

8.  Klik op de pagina **Gebruikersverificatie bewerken** dialoogvenster kunt u de volgende stappen uitvoeren:

    ![Gebruikersverificatie bewerken] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Gebruikersverificatie bewerken")

    1.  Klik op **Eenmalige aanmelding (SSO)**.
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij UserVoice** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **SSO externe aanmeldingsproblemen** .
    3.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij UserVoice** Kopieer de waarde van de **URL van externe Meld u af** en plak deze in de **externe Sign-Out SSO tekstvak**.
    4.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **huidige certificaat SHA1 vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    5.  Klik op **Opslaan verificatie-instellingen**.

9.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij UserVoice, moeten ze worden ingericht in UserVoice.  
Bij UserVoice, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **UserVoice** .

2.  Ga naar **Instellingen**.

    ![Instellingen] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Instellingen")

3.  Klik op **Algemeen**.

4.  Klik op **agenten en machtigingen**.

    ![Agenten en machtigingen] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Agenten en machtigingen")

5.  Klik op **beheerders toevoegen**.

    ![Beheerders toevoegen] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "Beheerders toevoegen")

6.  Klik in het dialoogvenster **beheerders uitnodigen** , moet u de volgende stappen uitvoeren:

    ![Beheerders uitnodigen] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "Beheerders uitnodigen")

    1.  Typ in het e-mailberichten texbox, het e-mailadres van het account dat u wilt inrichten en klik vervolgens op **toevoegen**.
    2.  Klik op **uitnodigen**.

>[AZURE.NOTE] U kunt een andere UserVoice gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door UserVoice aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan UserVoice, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **UserVoice **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.