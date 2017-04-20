<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Mimecast beheerconsole | Microsoft Azure" 
    description="Leer hoe u Mimecast beheerconsole gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerde inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Zelfstudie: Azure Active Directory-integratie met Mimecast beheerconsole
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Mimecast-beheerconsole.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Mimecast beheerconsole eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan de beheerconsole Mimecast één Meld u aan bij de toepassing op uw site in de beheerconsole Mimecast bedrijf (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor de beheerconsole Mimecast inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scenario")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>De toepassingsintegratie van de voor de beheerconsole Mimecast inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Mimecast-beheerconsole.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor de beheerconsole Mimecast inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak**, **Mimecast-beheerconsole**.

    ![Galerie van toepassing] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Galerie van toepassing")

7.  In het deelvenster met resultaten **Mimecast beheerconsole**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Mimecast beheerconsole] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Mimecast beheerconsole")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Mimecast beheerconsole met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **Beheerconsole Mimecast** toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij de beheerconsole Mimecast** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** , in het tekstvak **Mimecast beheerder Console Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Mimecast beheerconsole-toepassing (bijvoorbeeld: "https://webmail-uk.mimecast.com" of "https://webmail-us.mimecast.com"), en klik op **volgende**.

    >[AZURE.NOTE] Het teken in URL is specifieke regio.

    ![URL van App configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij de beheerconsole Mimecast** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw beheerconsole Mimecast als beheerder.

6.  Ga naar **Services \> toepassing**.

    ![Services] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Services")

7.  Klik op **verificatie profielen**.

    ![Verificatie-profielen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Verificatie-profielen")

8.  Klik op **Nieuw verificatie-profiel**.

    ![Nieuwe verificatie-profielen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Nieuwe verificatie-profielen")

9.  Klik in de sectie **Verificatie profiel** kunt u de volgende stappen uitvoeren:

    ![Verificatie-profiel] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Verificatie-profiel")

    1.  Typ in het tekstvak **Beschrijving** een naam voor uw configuratie.
    2.  Selecteer **SAML-verificatie voor Mimecast beheerconsole afdwingen**.
    3.  Als **Provider**, selecteer **Azure Active Directory**.
    4.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij de beheerconsole Mimecast** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **URL van de uitgever** .
    5.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij de beheerconsole Mimecast** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    6.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij de beheerconsole Mimecast** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **URL Meld u af** .  

        >[AZURE.NOTE]De waarde aanmeldings-URL en de afmelden URL-waarde zijn voor de beheerconsole Mimecast hetzelfde.

    7.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP]Zie [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.

    8.  Open uw base 64 gecodeerde certificaat in Kladblok verwijderen de eerste regel ("*--*") en de laatste regel ("*--*"), de resterende inhoud van deze naar het Klembord kopiëren en plak deze vervolgens naar het tekstvak **Identiteit Provider certificaat (metagegevens)** .
    9.  Selecteer **toestaan Single Sign in**.
    10. Klik op **Opslaan**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij de beheerconsole Mimecast, moeten ze ingericht in Mimecast-beheerconsole.  
Bij Mimecast beheerconsole, met de inrichting van is een handmatige taak.
  
U moet een domein hebt geregistreerd voordat u gebruikers kunt maken.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw **Mimecast beheerconsole** aan als beheerder.

2.  Ga naar **mappen \> interne**.

    ![Mappen] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Mappen")

3.  Klik op **nieuwe domein hebt geregistreerd**.

    ![Nieuwe domein hebt geregistreerd] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Nieuwe domein hebt geregistreerd")

4.  Nadat uw nieuwe domein is gemaakt, klikt u op **Nieuw adres**.

    ![Nieuw adres] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Nieuw adres")

5.  Klik in het dialoogvenster nieuwe adres van de volgende stappen uitvoeren:

    ![Opslaan] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Opslaan")

    1.  Typ de kenmerken van het **E-mailadres**, **De naam van de globale**, **wachtwoord** en **Bevestig het wachtwoord** van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE]U kunt een andere hulpmiddelen voor Mimecast beheerconsole gebruiker het account maken of verstrekt door Mimecast beheerconsole aan gebruikersaccounts van inrichten AAD-API's gebruiken.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan de beheerconsole Mimecast, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Op de pagina van de integratie in de **Beheerconsole Mimecast **toepassing, klikt u op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.