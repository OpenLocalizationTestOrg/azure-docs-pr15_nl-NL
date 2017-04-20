<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met NetDocuments | Microsoft Azure" 
    description="Meer informatie over het gebruiken van NetDocuments met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Zelfstudie: Azure Active Directory-integratie met NetDocuments
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en NetDocuments.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant NetDocuments
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan NetDocuments één Meld u aan bij de toepassing op uw bedrijfssite NetDocuments (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor NetDocuments inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scenario")
##<a name="enabling-the-application-integration-for-netdocuments"></a>De toepassingsintegratie van de voor NetDocuments inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor NetDocuments.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor NetDocuments door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **NetDocuments**.

    ![Galerie van toepassing] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Galerie van toepassing")

7.  Selecteer **NetDocuments**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij NetDocuments met hun account in Azure AD met Federatie op basis van het SAML-protocol.  
Eenmalige aanmelding voor NetDocuments configureren, moet u een vingerafdrukwaarde ophalen uit een certificaat.  
Als u niet bekend met deze procedure bent, raadpleegt u [het ophalen van de waarde van een certificaat](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **NetDocuments** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij NetDocuments** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Eenmalige aanmelding configureren")

3.  Klik op de pagina **App-URL configureren** , moet u de volgende stappen uitvoeren:

    ![URL van App configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "URL van App configureren")

    1.  Typ in het tekstvak **Aanmeldingsadres op URL** de URL die wordt gebruikt door uw gebruikers aanmelden bij uw NetDocuments-toepassing (bijvoorbeeld: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  Typ in het tekstvak **NetDocuments beantwoorden URL** de dezelfde waarde die u hebt getypt in de het tekstvak **Aanmeldingsadres op URL** .  

        >[AZURE.NOTE]U vindt de juiste waarde aan het einde van het dialoogvenster **Identiteit federatieve** (Zie de schermafbeelding voor stap 9).

    3.  Klik op **volgende**

4.  Klik op de pagina **configureren eenmalige aanmelding bij NetDocuments** op **certificaat downloaden**als u wilt downloaden van het certificaat, en sla het certificaatbestand lokaal op uw computer.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Eenmalige aanmelding configureren")

5.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite NetDocuments als beheerder.

6.  Ga naar **beheerder**.

7.  Klik op **toevoegen en verwijderen gebruikers en groepen**.

    ![Opslagplaats] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Opslagplaats")

8.  Klik op **configureren geavanceerde verificatieopties voor**.

    ![Configureren geavanceerde verificatieopties] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Configureren geavanceerde verificatieopties")

9.  Klik op **De identiteit federatieve** dialoogvenster, moet u de volgende stappen uitvoeren:

    ![Federatieve Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Federatieve Identitty")

    1.  Als het **servertype voor federatieve identiteit**, selecteert u **Active Directory Federation Services**.
    2.  Klik op **bestand kiezen**om het gedownloade metagegevensbestand te uploaden.
    3.  Klik op **OK**.

10. In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Eenmalige aanmelding configureren")
##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij NetDocuments, moeten ze worden ingericht in NetDocuments.  
Bij NetDocuments, met de inrichting van is een handmatige taak.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Als u wilt configureren de inrichting van de gebruiker, moet u de volgende stappen uitvoeren:

1.  Eenmalige bij de site van uw **NetDocuments** bedrijf als beheerder.

2.  In het menu aan de bovenkant, klikt u op **beheerder**.

    ![Beheerder] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Beheerder")

3.  Klik op **toevoegen en verwijderen gebruikers en groepen**.

    ![Opslagplaats] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Opslagplaats")

4.  Typ in het tekstvak **E-mailadres** het e-mailadres van een geldige Azure Active Directory-account dat u wilt inrichten, en klik op **Gebruiker toevoegen**.

    ![E-mailadres] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "E-mailadres")

    >[AZURE.NOTE]De eigenaar van een account Azure Active Directory krijgt een e-mailbericht met een koppeling om te bevestigen van het account voordat deze geactiveerd wordt.

>[AZURE.NOTE]U kunt een andere NetDocuments gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door NetDocuments aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan NetDocuments, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **NetDocuments **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.