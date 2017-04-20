<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Coupa | Microsoft Azure" 
    description="Meer informatie over het gebruiken van Coupa met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Zelfstudie: Azure Active Directory-integratie met Coupa

Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en Coupa.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een Coupa eenmalige aanmelding ingeschakeld abonnement

Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan Coupa één Meld u aan bij de toepassing de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruikt.

Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor Coupa inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")
##<a name="enabling-the-application-integration-for-coupa"></a>De toepassingsintegratie van de voor Coupa inschakelen

Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor Coupa.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor Coupa door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-coupa-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **Coupa**.

    ![Galerie van toepassing] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Galerie van toepassing")

7.  Selecteer **Coupa**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij Coupa met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor Coupa configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Meld u bij uw bedrijfssite Coupa aan als beheerder.

2.  Ga naar **Setup \> beveiliging besturingselement**.

    ![Besturingselementen voor beveiliging] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Besturingselementen voor beveiliging")

3.  Het bestand te downloaden Coupa metagegevens naar uw computer, klikt u op **downloaden en SP-metagegevens importeren**.

    ![Coupa SP-metagegevens] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP-metagegevens")

4.  In een ander browservenster, moet u zich aanmelden bij de portal van Azure klassieke.

5.  Klik op de pagina **Coupa** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Eenmalige aanmelding configureren")

6.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij Coupa** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Eenmalige aanmelding configureren")

7.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-coupa-tutorial/IC791904.png "URL van App configureren")

    1.  Typ in het tekstvak **Aanmeldingsadres op URL** URL die wordt gebruikt door uw gebruikers aanmelden bij uw Coupa-toepassing (bijvoorbeeld: "*http://company.Coupa.com*").
    2.  Opent u het gedownloade bestand voor Coupa-metagegevens en kopieer de **AssertionConsumerService index/URL**.
    3.  Plak de **AssertionConsumerService index/URL** -waarde in het tekstvak **Coupa antwoord URL** .
    4.  Klik op **volgende**.

8.  Klik op de pagina **configureren eenmalige aanmelding bij Coupa** op **metagegevens downloaden**uw metagegevensbestand te downloaden en sla het bestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Eenmalige aanmelding configureren")

9.  Op de site van het bedrijf Coupa, gaat u naar **Setup \> beveiliging besturingselement**.

    ![Besturingselementen voor beveiliging] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Besturingselementen voor beveiliging")

10. Klik in de sectie **Meld u aan met referenties Coupa** kunt u de volgende stappen uitvoeren:

    ![Meld u aan met referenties van Coupa] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Meld u aan met referenties van Coupa")

    1.  **Meld u aan met SAML**selecteren
    2.  Klik op **Bladeren** om het gedownloade bestand met Azure Active metagegevens te uploaden.
    3.  Klik op **Opslaan**.

11. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker

Als u wilt dat gebruikers Azure AD Meld u aan bij Coupa, moeten ze worden ingericht in Coupa.  
Bij Coupa, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Meld u als beheerder aan bij uw bedrijfssite **Coupa** .

2.  In het menu aan de bovenkant, klik op **Instellingen**en klik vervolgens op **gebruikers**.

    ![Gebruikers] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Gebruikers")

3.  Klik op **maken**.

    ![Gebruikers maken] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Gebruikers maken")

4.  Klik in de sectie **Gebruiker maken** de volgende stappen uitvoeren:

    ![Gebruikersgegevens] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Gebruikersgegevens")

    1.  Typ de **aanmelding**, **Voornaam**, **Achternaam**, **Eenmalige aanmelding-ID**, **e** -kenmerken van een geldige Azure Active Directory-account inrichten in de gerelateerde tekstvakken gewenste.
    2.  Klik op **maken**.

    >[AZURE.NOTE] De eigenaar van een account Azure Active Directory krijgt een e-mailbericht met een koppeling naar het account bevestigen voordat deze geactiveerd wordt.

>[AZURE.NOTE] U kunt een andere Coupa gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door Coupa aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen

Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan Coupa, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **Coupa **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Ja")

Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.
