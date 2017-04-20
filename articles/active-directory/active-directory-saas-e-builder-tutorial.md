<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met e-opbouwfunctie voor | Microsoft Azure" 
    description="Leer hoe u met e-Builder met Azure Active Directory inschakelen eenmalige aanmelding, geautomatiseerd inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Zelfstudie: Azure Active Directory-integratie met e-Builder
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en e-Builder.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een e-Builder-tenant
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan e-Builder één Meld u aan bij de toepassing op uw e-Builder-bedrijfssite (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor e-Builder inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Scenario")
##<a name="enabling-the-application-integration-for-e-builder"></a>De toepassingsintegratie van de voor e-Builder inschakelen
  
Het doel van deze sectie is het inschakelen van de toepassingsintegratie van de voor e-opbouwfunctie voor een overzicht maken.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor e-opbouwfunctie voor door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **E-opbouwfunctie voor**.

    ![Galerie van toepassing] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Galerie van toepassing")

7.  Selecteer **E-opbouwfunctie voor**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![e-Builder] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "e-Builder")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij e-samenstellen met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **e-Builder** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij e-opbouwfunctie voor** **Eenmalige aanmelding Microsoft Azure AD**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **e-opbouwfunctie voor aanmelden URL** de URL voor het gebruik van de volgende patroon "*https://\<tenant-naam\>CF,o.e Builder.com*', en klik op **volgende**.

    ![URL van app configureren] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "URL van app configureren")

4.  Klik op de **configureren eenmalige aanmelding bij e-opbouwfunctie voor** pagina wilt downloaden van de metagegevens van uw, klikt u op het **downloaden van metagegevens**en klikt u vervolgens de gegevens lokaal opslaan als **c:\\e-BuilderMetaData.xml**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Eenmalige aanmelding configureren")

5.  Doorsturen dat metagegevensbestand naar e-Builder ondersteuningsteam. De behoeften van de team ondersteuning Hiermee configureert u eenmalige aanmelding voor u.

6.  Selecteer de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Er is geen actie-item voor u van gebruikers naar e-Builder configureren.  
Wanneer een toegewezen gebruiker probeert aan te melden bij e-Builder via het Configuratiescherm van access, e-Builder Hiermee wordt gecontroleerd of de gebruiker bestaat.  
Als er nog geen gebruikersaccount beschikbaar, wordt deze automatisch gemaakt door e-Builder.
##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan e-Builder, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **e-Builder **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
