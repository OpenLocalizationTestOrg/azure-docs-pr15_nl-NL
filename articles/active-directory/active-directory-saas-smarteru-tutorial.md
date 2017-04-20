<properties 
    pageTitle="Zelfstudie: Azure Active Directory-integratie met SmarterU | Microsoft Azure" 
    description="Meer informatie over het gebruiken van SmarterU met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!" 
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
    ms.date="09/19/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Zelfstudie: Azure Active Directory-integratie met SmarterU
  
Het doel van deze zelfstudie is om weer te geven van de integratie van Azure en SmarterU.  
Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:

-   Een geldig abonnement op Azure
-   Een tenant SmarterU
  
Na het voltooien van deze zelfstudie, kunnen de Azure AD-gebruikers die u hebt toegewezen aan SmarterU één Meld u aan bij de toepassing op uw bedrijfssite SmarterU (serviceprovider gestart Meld u aan op) of de [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)gebruiken.
  
Het scenario die worden beschreven in deze zelfstudie bestaat uit de volgende elementen:

1.  De toepassingsintegratie van de voor SmarterU inschakelen
2.  Eenmalige aanmelding configureren
3.  Configuratie van de inrichting van gebruiker
4.  Gebruikers toewijzen

![Scenario] (./media/active-directory-saas-smarteru-tutorial/IC777320.png "Scenario")

##<a name="enabling-the-application-integration-for-smarteru"></a>De toepassingsintegratie van de voor SmarterU inschakelen
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van de toepassingsintegratie van de voor SmarterU.

###<a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>Schakel de toepassingsintegratie van de voor SmarterU door de volgende stappen uitvoeren:

1.  Klik in het Azure op klassieke-portal op het linker navigatiedeelvenster, **Active Directory**.

    ![Active Directory] (./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

    ![Toepassingen] (./media/active-directory-saas-smarteru-tutorial/IC700994.png "Toepassingen")

4.  Klik op **toevoegen** aan de onderkant van de pagina.

    ![De toepassing toevoegen] (./media/active-directory-saas-smarteru-tutorial/IC749321.png "De toepassing toevoegen")

5.  Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Toepassing van gallerry toevoegen] (./media/active-directory-saas-smarteru-tutorial/IC749322.png "Toepassing van gallerry toevoegen")

6.  Typ in het **zoekvak** **SmarterU**.

    ![Toepassing fallery] (./media/active-directory-saas-smarteru-tutorial/IC777321.png "Toepassing fallery")

7.  Selecteer **SmarterU**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.

    ![SmarterU] (./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

##<a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren
  
Het doel van dit gedeelte is overzicht maken van het inschakelen van gebruikers worden geverifieerd bij SmarterU met hun account in Azure AD met Federatie op basis van het SAML-protocol.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal, klik op de pagina **SmarterU** toepassing integratie op **eenmalige aanmelding configureren** om het dialoogvenster **Configureren eenmalige aanmelding** .

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777323.png "Eenmalige aanmelding configureren")

2.  Klik op de pagina **Hoe wilt u dat gebruikers aanmelden bij SmarterU** selecteert u **Eenmalige aanmelding Microsoft Azure AD**en klik vervolgens op **volgende**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777324.png "Eenmalige aanmelding configureren")

3.  Klik op de **configureren eenmalige aanmelding bij SmarterU** pagina wilt downloaden van de metagegevens van uw, klikt u op het **downloaden van metagegevens**en klikt u vervolgens de gegevens lokaal opslaan als **c:\\SmarterUMetaData.cer**.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777325.png "Eenmalige aanmelding configureren")

4.  In een browservenster verschillende web en meld u aan bij uw bedrijfssite SmarterU als beheerder.

5.  Klik in de werkbalk op de voorgrond op **Accountinstellingen**.

    ![Accountinstellingen] (./media/active-directory-saas-smarteru-tutorial/IC777326.png "Accountinstellingen")

6.  Klik op de accountpagina configuratie, kunt u de volgende stappen uitvoeren:

    ![Externe autorisatie] (./media/active-directory-saas-smarteru-tutorial/IC777327.png "Externe autorisatie")

    1.  Selecteer **externe autorisatie inschakelen**.
    2.  Selecteer het tabblad **SmarterU** in de sectie **Outmodel Login besturingselement** .
    3.  Selecteer het tabblad **SmarterU** in de sectie **Standaard gebruikersaanmelding** .
    4.  Selecteer **Okta inschakelen**.
    5.  Kopieer de inhoud van het metagegevensbestand gedownloade en plak deze in het tekstvak **Okta metagegevens** .
    6.  Klik op **Opslaan**.

7.  In de klassieke Azure portal, selecteert u de Configuratiebevestiging voor eenmalige aanmelding en klik vervolgens op **Voltooien** om het dialoogvenster **Configureren eenmalige aanmelding** te sluiten.

    ![Eenmalige aanmelding configureren] (./media/active-directory-saas-smarteru-tutorial/IC777328.png "Eenmalige aanmelding configureren")

##<a name="configuring-user-provisioning"></a>Configuratie van de inrichting van gebruiker
  
Als u wilt dat gebruikers Azure AD Meld u aan bij SmarterU, moeten ze worden ingericht in SmarterU.  
Bij SmarterU, met de inrichting van is een handmatige taak.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Als u wilt een gebruikersaccounts inrichten, moet u de volgende stappen uitvoeren:

1.  Meld u aan bij uw tenant **SmarterU** .

2.  Ga naar **gebruikers**.

3.  Klik in de sectie gebruikers de volgende stappen uitvoeren:

    ![Nieuwe gebruiker] (./media/active-directory-saas-smarteru-tutorial/IC777329.png "Nieuwe gebruiker")

    1.  Klik op **+ gebruiker**.
    2.  Typ de waarden gerelateerde kenmerk van de Azure AD-gebruikersaccount in de volgende tekstvakken: **E-primair**, **Werknemer-ID**, **wachtwoord**, **Bevestig het wachtwoord**, **naam**, **Achternaam**.
    3.  Klik op **actieve**.
    4.  Klik op **Opslaan**.

>[AZURE.NOTE] U kunt een andere SmarterU gebruiker account hulpmiddelen voor het maken of API's die is verstrekt door SmarterU aan inrichten AAD-gebruikersaccounts.

##<a name="assigning-users"></a>Gebruikers toewijzen
  
Als u wilt testen van uw configuratie, moet u de Azure AD-gebruikers verlenen die u wilt toestaan dat uw toepassing toegang tot het via toe te wijzen.

###<a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>Als u wilt gebruikers toewijzen aan SmarterU, moet u de volgende stappen uitvoeren:

1.  Klik in de klassieke Azure portal een testaccount te maken.

2.  Klik op de pagina **SmarterU **toepassing integratie op **toewijzen aan gebruikers**.

    ![Toewijzen aan gebruikers] (./media/active-directory-saas-smarteru-tutorial/IC777330.png "Toewijzen aan gebruikers")

3.  Selecteer uw gebruikers-test op **toewijzen**en klik vervolgens op **Ja** om te bevestigen van uw toewijzing.

    ![Ja] (./media/active-directory-saas-smarteru-tutorial/IC767830.png "Ja")
  
Als u testen van uw instellingen voor eenmalige aanmelding wilt, opent u het deelvenster Access. Zie [Inleiding tot het deelvenster Access](active-directory-saas-access-panel-introduction.md)voor meer informatie over het Access-deelvenster.