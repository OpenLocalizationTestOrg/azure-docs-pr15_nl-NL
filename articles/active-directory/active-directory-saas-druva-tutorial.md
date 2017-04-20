<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie integratie met Druva | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Druva met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Zelfstudie: Azure Active Directory-integratie integratie met Druva

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Druva.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Druva eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Druva één Meld u aan bij de toepassing op uw bedrijfssite Druva (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Druva inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-druva-tutorial/IC795084.png "Scenario")
##<a name="enabling-the-application-integration-for-druva"></a>De toepassingsintegratie van de voor Druva inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Druva.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Druva door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-druva-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-druva-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-druva-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Druva**.

    ![Galerie van toepassing] (./media/active-directory-saas-druva-tutorial/IC795085.png "Galerie van toepassing")

7.  Selecteer **Druva**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Druva met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

Uw toepassing Druva verwacht de bevestigingen SAML een specifieke notatie, waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie **saml token kenmerken** .  
De volgende schermafbeelding ziet u een voorbeeld hiervoor.

![Op SAML Token kenmerken] (./media/active-directory-saas-druva-tutorial/IC795087.png "Op SAML Token kenmerken")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Druva** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795027.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Druva** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795088.png "Eenmalige aanmelding configureren")

3.  Typ op de pagina **App-URL configureren** , in het tekstvak **Druva aanmelding op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw Druva-toepassing (bijvoorbeeld: "*https://cloud.druva.com/home/*"), en klik op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-druva-tutorial/IC795089.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Druva** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795090.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Druva als beheerder.

6.  Ga naar **beheren \> instellingen**.

    ![Instellingen] (./media/active-directory-saas-druva-tutorial/IC795091.png "Instellingen")

7.  Klik in het dialoogvenster Instellingen voor eenmalige aanmelding, moet u de volgende stappen uitvoeren:

    ![Instellingen voor kel eenmalige aanmelding] (./media/active-directory-saas-druva-tutorial/IC795092.png "Instellingen voor kel eenmalige aanmelding")

    1.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Druva** de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het tekstvak **ID Provider aanmeldings-URL** .
    2.  Kopieer de waarde van de **URL van externe Meld u af** en plak deze in het tekstvak **ID Provider afmelden URL** in de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Druva** .
    3.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.  

        >[AZURE.TIP] Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    4.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **ID Provider certificaat**
    5.  Open de pagina **Instellingen** , klik op **Opslaan**.

8.  Klik op de pagina **Instellingen** op **SSO Token genereren**.

    ![Instellingen] (./media/active-directory-saas-druva-tutorial/IC795093.png "Instellingen")

9.  Klik in het dialoogvenster **Eenmalige aanmelding verificatie Token** moet u de volgende stappen uitvoeren:

    ![Eenmalige aanmelding Token] (./media/active-directory-saas-druva-tutorial/IC795094.png "Eenmalige aanmelding Token")

    1.  Klik op **kopiëren**.
    2.  Klik op **sluiten**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-druva-tutorial/IC795095.png "Eenmalige aanmelding configureren")

11. In het menu aan de bovenkant, klikt u op **kenmerken** om het dialoogvenster **SAML Token kenmerken** .

    ![Kenmerken] (./media/active-directory-saas-druva-tutorial/IC795096.png "Kenmerken")

12. Als u wilt de toewijzingen vereist kenmerk hebt toegevoegd, kunt u de volgende stappen uitvoeren:

  	|Kenmerknaam|Kenmerkwaarde|
  	|---|---|
  	|synchroon\_auth\_token|<*Klembord-waarde*>|

    1.  Klik op **gebruikerskenmerk toevoegen**voor elke gegevensrij met in de tabel hierboven.
    2.  Typ de naam van het kenmerk weergegeven voor die rij in het tekstvak **Kenmerk** .
    3.  Typ in het tekstvak **Kenmerkwaarde** de kenmerkwaarde weergegeven voor die rij.
    4.  Klik op **Voltooien**.

13. Klik op **wijzigingen toepassen**.
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Druva, moeten ze worden ingericht in Druva.  
Bij Druva, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Druva** .

2.  Ga naar **beheren \> gebruikers**.

    ![Gebruikers beheren] (./media/active-directory-saas-druva-tutorial/IC795097.png "Gebruikers beheren")

3.  Klik op **nieuwe maken**.

    ![Gebruikers beheren] (./media/active-directory-saas-druva-tutorial/IC795098.png "Gebruikers beheren")

4.  Klik in het dialoogvenster Nieuwe gebruiker maken de volgende stappen uitvoeren:

    ![Nieuwegebruiker maken] (./media/active-directory-saas-druva-tutorial/IC795099.png "Nieuwegebruiker maken")

    1.  Typ het e-mailadres en de naam van een geldige Azure Active Directory-gebruikersaccount die u wilt inrichten in de gerelateerde tekstvakken.
    2.  Klik op **gebruiker maken**.

>[AZURE.NOTE] U kunt een andere Druva gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Druva aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Druva, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Druva **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-druva-tutorial/IC795100.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-druva-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
