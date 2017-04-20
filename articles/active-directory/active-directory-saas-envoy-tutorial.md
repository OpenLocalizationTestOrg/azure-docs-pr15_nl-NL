<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Envoy | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Envoy met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Zelfstudie: Azure Active Directory-integratie met Envoy
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Envoy.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant Envoy
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Envoy één Meld u aan bij de toepassing op uw bedrijfssite Envoy (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Envoy inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Scenario")
##<a name="enabling-the-application-integration-for-envoy"></a>De toepassingsintegratie van de voor Envoy inschakelen
  
Het doel van dit gedeelte is om een overzicht van het inschakelen van de toepassingsintegratie van de voor Envoy te.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor Envoy inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-envoy-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Envoy**.

    ![Galerie van toepassing] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Galerie van toepassing")

7.  Selecteer **Envoy**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Envoy met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Envoy configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Envoy** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding inschakelen] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Eenmalige aanmelding inschakelen")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Envoy** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Envoy Sign In URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>. Envoy.com*', en klik op **volgende**.

    ![URL van app configureren] (./media/active-directory-saas-envoy-tutorial/IC776780.png "URL van app configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Envoy** als wilt downloaden van het certificaat, klikt u op **certificaat downloaden**en sla het certificaatbestand lokaal als **c:\\Envoy.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Envoy als beheerder.

6.  Op de werkbalk op de voorgrond, klikt u op **Instellingen**.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  Klik op **bedrijf**.

    ![Bedrijf] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Bedrijf")

8.  Klik op **SAML**.

    ![Op SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "Op SAML")

9.  In de sectie **SAML verificatie** configuratie van de volgende stappen uitvoeren:

    ![Op SAML-verificatie] (./media/active-directory-saas-envoy-tutorial/IC776785.png "Op SAML-verificatie")

    >[AZURE.NOTE] De waarde voor de ID van de locatie hoofdkantoor is automatisch gegenereerd door de toepassing.

    1.  De waarde **vingerafdruk** van het geëxporteerde certificaat kopiëren en plak deze in het tekstvak **vingerafdruk** .  

        >[AZURE.TIP] Zie voor meer informatie [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI)

    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Envoy** de waarde **SAML SSO URL** kopiëren en plak deze in het tekstvak **Identiteit Provider HTTP SAML URL** .
    3.  Klik op **wijzigingen opslaan**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Er is geen actie-item voor u van gebruikers aan Envoy configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij Envoy via het Configuratiescherm van access, Envoy Hiermee wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door Envoy.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Envoy, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Envoy **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.