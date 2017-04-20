<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Gigya | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Gigya met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-gigya"></a>Zelfstudie: Azure Active Directory-integratie met Gigya
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Gigya.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Gigya eenmalige aanmelding ingeschakeld abonnement
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Gigya één Meld u aan bij de toepassing op uw bedrijfssite Gigya (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Gigya inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789512.png "Eenmalige aanmelding configureren")
##<a name="enabling-the-application-integration-for-gigya"></a>De toepassingsintegratie van de voor Gigya inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Gigya.

###<a name="to-enable-the-application-integration-for-gigya-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Gigya door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-gigya-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-gigya-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-gigya-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-gigya-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Gigya**.

    ![Galerie van toepassing] (./media/active-directory-saas-gigya-tutorial/IC789513.png "Galerie van toepassing")

7.  Selecteer **Gigya**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Gigya] (./media/active-directory-saas-gigya-tutorial/IC789527.png "Gigya")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Gigya met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Als onderdeel van deze procedure bent u verplicht voor het maken van een bestand base 64 gecodeerde certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **Gigya** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789528.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Gigya** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789529.png "Eenmalige aanmelding configureren")

3.  Typ de URL voor het gebruik van de volgende patroon "*http://company.gigya.com*" op de pagina **App-URL configureren** in het tekstvak **Gigya aanmelding op URL** en klik vervolgens op **volgende**.

    ![URL van App configureren] (./media/active-directory-saas-gigya-tutorial/IC789530.png "URL van App configureren")

4.  Klik op de pagina **configureren eenmalige aanmelding bij Gigya** op **certificaat downloaden**en sla het certificaatbestand op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789531.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite Gigya als beheerder.

6.  Ga naar **instellingen \> SAML Login**, en klik vervolgens op de knop **toevoegen** .

    ![Op SAML Login] (./media/active-directory-saas-gigya-tutorial/IC789532.png "Op SAML Login")

7.  Klik in de sectie **SAML Login** kunt u de volgende stappen uitvoeren:

    ![Op SAML-configuratie] (./media/active-directory-saas-gigya-tutorial/IC789533.png "Op SAML-configuratie")

    1.  Typ een naam voor uw configuratie in **het tekstvak** .
    2.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Gigya** de waarde **Uitgever URL** kopiëren en plak deze in het tekstvak **uitgever** .
    3.  Klik in de Azure klassieke portal op de pagina **configureren eenmalige aanmelding bij Gigya** de waarde voor **Eenmalige aanmelding Service URL** kopiëren en plak deze in het tekstvak **Eenmalige aanmelding Service URL** .
    4.  In de klassieke Azure portal, klik op de pagina **configureren eenmalige aanmelding bij Gigya** de **Naamindeling id** -waarde kopiëren en plak deze in het tekstvak **Naam ID opmaken** .
    5.  Een **basis-64-codering** -bestand uit uw gedownloade certificaat maken.
        
        >[AZURE.TIP]Zie voor meer informatie, [hoe u converteert een binair certificaat naar een tekstbestand](http://youtu.be/PlgrzUZ-Y1o)

    6.  Open uw base 64 gecodeerde certificaat in Kladblok, Kopieer de inhoud van deze naar het Klembord en plakt u deze naar het tekstvak **X.509-certificaat** .
    7.  Klik op **Instellingen opslaan**.

8.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-gigya-tutorial/IC789534.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij Gigya, moeten ze worden ingericht in Gigya.  
Bij Gigya, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Gigya** .

2.  Ga naar **beheerder \> gebruikers beheren**, en klik vervolgens op **Gebruikers uitnodigen**.

    ![Gebruikers beheren] (./media/active-directory-saas-gigya-tutorial/IC789535.png "Gebruikers beheren")

3.  Klik in het dialoogvenster gebruikers uitnodigen, moet u de volgende stappen uitvoeren:

    ![Gebruikers uitnodigen] (./media/active-directory-saas-gigya-tutorial/IC789536.png "Gebruikers uitnodigen")

    1.  Typ het e-mailalias van een geldige Azure Active Directory-account inrichten gewenste in het tekstvak **e-mailbericht** .
    2.  Klik op **uitnodiging van de gebruiker**.
    
        >[AZURE.NOTE] De eigenaar van een account Azure Active Directory, ontvangt een e-mailbericht met een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Gigya gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Gigya aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-gigya-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Gigya, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Gigya **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-gigya-tutorial/IC789537.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-gigya-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.