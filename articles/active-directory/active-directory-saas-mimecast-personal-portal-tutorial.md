<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Mimecast persoonlijk Portal | Microsoft Azure" 
    description="Leer hoe u persoonlijke Mimecast-Portal gebruiken met Azure Active Directory om te schakelen van eenmalige aanmelding, geautomatiseerd inrichting en meer." 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a>Zelfstudie: Azure Active Directory-integratie met Mimecast Personal-Portal
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Mimecast persoonlijke Portal.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een persoonlijke Mimecast-Portal eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan persoonlijke Portal Mimecast één Meld u aan bij de toepassing op uw persoonlijke Mimecast-Portal-site van het bedrijf (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor persoonlijke Portal Mimecast inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794991.png "Scenario")
##<a name="enabling-the-application-integration-for-mimecast-personal-portal"></a>De toepassingsintegratie van de voor persoonlijke Portal Mimecast inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor persoonlijke Mimecast-Portal.

###<a name="to-enable-the-application-integration-for-mimecast-personal-portal-perform-the-following-steps"></a>Als u wilt de toepassingsintegratie van de voor persoonlijke Portal Mimecast inschakelt, moet u de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Mimecast persoonlijke Portal**.

    ![Galerie van toepassing] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794992.png "Galerie van toepassing")

7.  In het deelvenster met resultaten **Mimecast persoonlijke Portal**selecteren en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Mimecast Personal-Portal] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794993.png "Mimecast Personal-Portal")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers om te verifiëren bij persoonlijke Mimecast-Portal met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de Azure klassieke portal op de pagina van de integratie in de **Persoonlijke Portal Mimecast** toepassing, klik op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794994.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aan te melden bij Mimecast persoonlijke Portal** **Eenmalige aanmelding Microsoft Azure AD**selecteren en klik op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794995.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** in het tekstvak **Persoonlijke Portal-teken Mimecast op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw persoonlijke Portal Mimecast-toepassing (bijvoorbeeld: "https://webmail-uk.mimecast.com" of "https://webmail-us.mimecast.com"), en klik op **volgende**.

    >[AZURE.NOTE] Het teken in URL is specifieke regio.

    ![URL van App configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794996.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij persoonlijke Mimecast-Portal** op **Download certificaat**om te downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794997.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw persoonlijke Mimecast-Portal als beheerder.

6.  Ga naar **Services \> toepassing**.

    ![Toepassingen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794998.png "Toepassingen")

7.  Klik op **verificatie profielen**.

    ![Verificatie-profielen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC794999.png "Verificatie-profielen")

8.  Klik op **Nieuw verificatie-profiel**.

    ![Nieuwe verificatie-profiel] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795000.png "Nieuwe verificatie-profiel")

9.  Klik in de sectie **Verificatie profiel** kunt u de volgende stappen uitvoeren:

    ![Verificatie-profiel] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795001.png "Verificatie-profiel")

    1.  Typ in het tekstvak **Beschrijving** een naam voor uw configuratie.
    2.  Selecteer **SAML-verificatie voor Mimecast Personal Portal afdwingen**.
    3.  Als **Provider**, selecteer **Azure Active Directory**.
    4.  Kopieer de **URL van de uitgever** -waarde in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij persoonlijke Mimecast-Portal** en plak deze in het tekstvak **URL van de uitgever** .
    5.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij persoonlijke Portal Mimecast** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **Aanmeldings-URL** .
    6.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij persoonlijke Portal Mimecast** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **URL Meld u af** .  

        >[AZURE.NOTE] De waarde aanmeldings-URL en de afmelden URL-waarde zijn voor de - Klik op aan persoonlijke Portal Mimecast hetzelfde.

    7.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP]Zie [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)voor meer informatie.

    8.  Open uw base 64 gecodeerde certificaat in Kladblok verwijderen de eerste regel ("*--*") en de laatste regel ("*--*"), de resterende inhoud van deze naar het Klembord kopiëren en plak deze vervolgens naar het tekstvak **Identiteit Provider certificaat (metagegevens)** .
    9.  Selecteer **toestaan Single Sign in**.
    10. Klik op **Opslaan**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795002.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij persoonlijke Mimecast-Portal, moeten ze ingericht in Mimecast persoonlijke Portal.  
Bij persoonlijke Mimecast-Portal met de inrichting van is een handmatige taak.
  
U moet een domein hebt geregistreerd voordat u gebruikers kunt maken.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u aan uw **Persoonlijke Portal Mimecast** aan als beheerder.

2.  Ga naar **mappen \> interne**.

    ![Mappen] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795003.png "Mappen")

3.  Klik op **nieuwe domein hebt geregistreerd**.

    ![Nieuwe domein hebt geregistreerd] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795004.png "Nieuwe domein hebt geregistreerd")

4.  Nadat uw nieuwe domein is gemaakt, klikt u op **Nieuw adres**.

    ![Nieuw adres] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795005.png "Nieuw adres")

5.  Klik in het dialoogvenster nieuwe adres van de volgende stappen uitvoeren:

    ![Opslaan] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795006.png "Opslaan")

    1.  Typ de kenmerken van het **E-mailadres**, **De naam van de globale**, **wachtwoord** en **Bevestig het wachtwoord** van een geldige AAD-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **Opslaan**.

>[AZURE.NOTE]U kunt een andere hulpmiddelen voor persoonlijke Portal Mimecast gebruiker het account maken of verstrekt door Mimecast persoonlijke Portal aan gebruikersaccounts van inrichten AAD-API's gebruiken.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-mimecast-personal-portal-perform-the-following-steps"></a>Als u wilt toewijzen aan gebruikers bij persoonlijke Mimecast-Portal, kunt u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Mimecast persoonlijke Portal **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC795007.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-mimecast-personal-portal-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.