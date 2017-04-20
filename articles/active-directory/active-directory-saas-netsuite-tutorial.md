<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met NetSuite | Microsoft Azure"
    description="Meer informatie over het gebruiken van NetSuite met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Zelfstudie: Hoe u NetSuite integreren met Azure Active Directory

Deze zelfstudie leert u hoe u uw omgeving NetSuite verbinden met uw Azure Active Directory (Azure AD). U leert hoe u eenmalige aanmelding naar NetSuite configureert, het inschakelen van de inrichting van geautomatiseerde gebruiker en hoe u toewijzen aan gebruikers toegang hebben tot NetSuite. 

##<a name="prerequisites"></a>Vereisten voor

1. Voor toegang tot Azure Active Directory via de [portal van Azure klassieke](https://manage.windowsazure.com), moet u eerst een geldig abonnement op Azure hebben.

2. U moet beheerderstoegang hebben tot een [NetSuite](http://www.netsuite.com/portal/home.shtml) -abonnement. U kunt een gratis proefabonnement account voor een van deze services.

##<a name="step-1-add-netsuite-to-your-directory"></a>Stap 1: NetSuite toevoegen aan uw adreslijst

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Selecteer Active Directory in het linkernavigatiedeelvenster.][0]

2. Selecteer de map die u wilt toevoegen van NetSuite aan in de lijst **Directory** .

3. Klik op **toepassingen** in het bovenste menu.

    ![Klik op toepassingen.][1]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Klik op toevoegen om een nieuwe toepassing.][2]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Klik op toevoegen een toepassing in de galerie.][3]

6. Typ in het **zoekvak** **NetSuite**. Selecteer **NetSuite** uit de resultaten en klik op **voltooid** als de toepassing wilt toevoegen.

    ![NetSuite toevoegen.][4]

7. U ziet nu de pagina snel aan de slag voor NetSuite:

    ![Snel aan de slag van de NetSuite-pagina in Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Stap 2: Inschakelen eenmalige aanmelding

1. Azure AD, klikt u op de pagina snel aan de slag voor NetSuite, klik op de knop **eenmalige aanmelding configureren** .

    ![De eenmalige aanmelding knop configureren][6]

2. Een dialoogvenster wordt geopend en ziet u een scherm met de vraag "Hoe wilt u gebruikers aanmelden bij NetSuite dat?" Selecteer **Azure AD eenmalige aanmelding**, en klik vervolgens op **volgende**.

    ![Selecteer Azure AD voor eenmalige aanmelding][7]

    > [AZURE.NOTE] Voor meer informatie over de verschillende eenmalige aanmelding opties, [klikt u hier](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work) over

3. Typ op de pagina **App-instellingen configureren** voor het veld **Antwoord-URL** de URL van uw NetSuite-tenant met een van de volgende indelingen:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Typ de URL van uw tenant][8]

4. Klik op de pagina **configureren eenmalige aanmelding bij NetSuite** Klik op het **downloaden van metagegevens**en sla het certificaatbestand lokaal op uw computer.

    ![Download de metagegevens.][9]

5. Open een nieuw tabblad in uw browser en meld u aan bij uw site van het bedrijf Netsuite als beheerder.

6. De werkbalk boven aan de pagina **Instellingen**, klik op **Beheer van Setup**.

    ![Ga naar beheer van Setup][10]

7. Selecteer in de lijst **Configuratietaken** **integratie**.

    ![Ga naar integratie.][11]

8. Klik in de sectie **Verificatie beheren** op **SAML eenmalige aanmelding**.

    ![Ga naar het vak SAML eenmalige aanmelding][12]

9. Klik op de pagina **Op SAML-instelling** kunt u de volgende stappen uitvoeren:

    - Klik in de Azure Active Directory, de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het veld **Identiteit Provider aanmeldingspagina** in NetSuite.

        ![De pagina op SAML-instelling.][13]

    - In NetSuite, **Primaire verificatiemethode**te selecteren.

    - Selecteer voor het veld gelabelde **SAMLV2 identiteit Provider metagegevens** **IDP metagegevens-bestand uploaden**. Klik vervolgens op **Bladeren** om de metagegevensbestand dat u in stap 4 # gedownload te uploaden.

        ![De metagegevens uploaden][16]

    - Klik op **verzenden**.

9. Schakel het selectievakje in de configuratie voor eenmalige aanmelding bevestiging om in te schakelen van het certificaat dat u hebt geüpload naar NetSuite in Azure AD. Klik vervolgens op **volgende**.

    ![Schakel het selectievakje bevestiging][14]

10. Typ op de laatste pagina van het dialoogvenster, in een e-mailadres als u wilt ontvangen van e-mailmeldingen voor fouten en waarschuwingen die zijn gerelateerd aan het onderhoud van deze configuratie voor eenmalige aanmelding. 

    ![Typ in uw e-mailadres.][15]

11. Klik op **Voltooien** om het dialoogvenster te sluiten. Klik op **kenmerken** boven aan de pagina.

    ![Klik op kenmerken.][17]

12. Klik op het **gebruikerskenmerk toevoegen**.

    ![Klik op gebruikerskenmerk toevoegen.][18]

13. Typ voor het **Kenmerk** veldnaam in `account`. Voor het veld **Kenmerkwaarde** , typt u in uw NetSuite-account-ID. Hieronder vindt u instructies voor het zoeken naar uw account-ID:

    ![Toevoegen van uw NetSuite-Account-ID.][19]

    - Klik op de **instelling** in het bovenste navigatiemenu in NetSuite.
    - Klik onder de sectie **Configuratietaken** van het linkernavigatiemenu, selecteert u de sectie **integratie** en klik op **Web Services-voorkeuren**.
    - Uw Account-ID van NetSuite Kopieer en plak deze in het veld **Kenmerkwaarde** in Azure AD.

        ![Uw account-ID ophalen][20]

14. Klik op **Voltooien** om te voltooien de SAML-kenmerk toe te voegen in Azure AD. Klik vervolgens op **Apply Changes** in het menu onder.

    ![Sla uw wijzigingen op.][21]

15. Voordat u gebruikers kunnen uitvoeren eenmalige aanmelding in NetSuite, moeten worden ze eerst de juiste machtigingen in NetSuite toegewezen. Volg de onderstaande instructies voor het toewijzen van deze machtigingen.

    - Klik op de bovenste navigatiemenu **instellen**Klik op **Beheer van Setup**.

        ![Ga naar beheer van Setup][10]

    - Klik op het linkernavigatiemenu, selecteert u **Gebruikers/rollen**en kies **Rollen beheren**.

        ![Ga naar rollen beheren][22]

    - Klik op **nieuwe rol**.

    - Typ een **naam** voor de nieuwe rol en schakel het selectievakje **Eenmalige aanmelding alleen** .

        ![De naam van de nieuwe functie.][23]

    - Klik op **Opslaan**.

    - In het menu aan de bovenkant, klikt u op **machtigingen**. Klik vervolgens op **instellen**.

        ![Ga naar machtigingen][24]

    - Selecteer **instellen omhoog SAM eenmalige aanmelding**en klik vervolgens op **toevoegen**.

    - Klik op **Opslaan**.

    - Klik op de bovenste navigatiemenu **instellen**Klik op **Beheer van Setup**.

        ![Ga naar beheer van Setup][10]

    - Klik op het linkernavigatiemenu, selecteert u **Gebruikers/rollen**en kies **Gebruikers beheren**.

        ![Ga naar gebruikers beheren][25]

    - Selecteer de testgebruiker van een. Klik vervolgens op **bewerken**.

        ![Ga naar gebruikers beheren][26]

    - Klik in het dialoogvenster rollen, selecteer de rol die u hebt gemaakt en klik op **toevoegen**.

        ![Ga naar gebruikers beheren][27]

    - Klik op **Opslaan**.

19. Als u wilt testen van uw configuratie, raadpleegt u de sectie [Gebruikers toewijzen aan NetSuite](#step-4-assign-users-to-netsuite)getiteld.

##<a name="step-3-enable-automated-user-provisioning"></a>Stap 3: Inschakelen geautomatiseerde gebruiker inrichting

> [AZURE.NOTE] Standaard wordt uw ingerichte gebruikers worden toegevoegd aan de vestiging van de hoofdsite van uw omgeving NetSuite.

1. Azure Active Directory, op de pagina snel aan de slag voor NetSuite, klik op de **inrichting van de gebruiker configureren**.

    ![Gebruiker inrichting configureren][28]

2. Klik in het dialoogvenster dat wordt geopend, typt u in uw beheerdersreferenties voor NetSuite en klik op **volgende**.

    ![Typ in uw beheerdersreferenties NetSuite.][29]

3. Typ op de pagina confirmation in uw e-mailadres van de inrichting van fouten-meldingen wilt ontvangen.

    ![Bevestig.][30]

4. Klik op **Voltooien** om het dialoogvenster te sluiten.

##<a name="step-4-assign-users-to-netsuite"></a>Stap 4: Gebruikers toewijzen aan NetSuite

1. Als u wilt testen van uw configuratie, beginnen met het maken van een nieuwe testaccount in de adreslijst.

2. Klik op de knop **Gebruikers toewijzen** op de pagina NetSuite snel starten.

    ![Klik op gebruikers toewijzen][31]

3. Selecteer uw gebruikers-test en klik op de knop **toewijzen** aan de onderkant van het scherm:

 - Als u nog niet inschakelen geautomatiseerd gebruiker is geïnstalleerd, en ziet u het volgende bericht om te bevestigen:

        ![Confirm the assignment.][32]

 - Als u automatische gebruiker inrichting hebt ingeschakeld, ziet u een prompt om te bepalen welk type rol de gebruiker in NetSuite moet hebben. Nieuw ingerichte gebruikers moeten worden weergegeven in uw omgeving NetSuite na een paar minuten.

4. Test uw instellingen voor eenmalige aanmelding, open het paneel Access op [https://myapps.microsoft.com](https://myapps.microsoft.com/), meld u aan bij de testaccount en klikt u op **NetSuite**.

##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Lijst met zelfstudies over het integreren van SaaS Apps](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
