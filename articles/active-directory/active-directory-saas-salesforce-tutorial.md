<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Salesforce | Microsoft Azure"
    description="Meer informatie over het gebruiken van Salesforce met Azure Active Directory om in te schakelen voor eenmalige aanmelding, geautomatiseerde inrichting en meer!"
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

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Zelfstudie: Hoe u Salesforce integreren met Azure Active Directory

Deze zelfstudie leert u hoe u uw Salesforce-omgeving verbinden met uw Azure Active Directory. U leert hoe u eenmalige aanmelding naar Salesforce configureert, het inschakelen van de inrichting van geautomatiseerde gebruiker en hoe u toewijzen aan gebruikers toegang hebben tot Salesforce.

##<a name="prerequisites"></a>Vereisten voor

1. Voor toegang tot Azure Active Directory via de [portal van Azure klassieke](https://manage.windowsazure.com), moet u eerst een geldig abonnement op Azure hebben.

2. U moet een geldige tenant in [Salesforce.com](https://www.salesforce.com/)hebben.

> [AZURE.IMPORTANT] Als u **een proefaccount Salesforce.com** gebruikt, klikt zult u niet voor het configureren van de inrichting van geautomatiseerde gebruiker. Proefabonnement accounts bent niet gemachtigd de benodigde API ingeschakeld totdat ze worden gekocht.
> 
> U kunt deze beperking omzeilen via een [gratis ontwikkelaarsaccount](https://developer.salesforce.com/signup) om te voltooien van deze zelfstudie.

Als u een Salesforce Sandbox-omgeving gebruikt, raadpleegt u de [zelfstudie voor Salesforce Sandbox-integratie](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Video zelfstudies

U kunt deze zelfstudie met de onderstaande video's kunt volgen.

**Videozelfstudie deel één: het inschakelen van eenmalige aanmelding**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Videozelfstudie deel twee: Het automatiseren van de inrichting van gebruiker**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Stap 1: Salesforce toevoegen aan uw adreslijst

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Selecteer Active Directory in het linkernavigatiedeelvenster.][0]

2. Selecteer de map die u wilt toevoegen van Salesforce aan in de lijst **Directory** .

3. Klik op **toepassingen** in het bovenste menu.

    ![Klik op toepassingen.][1]

4. Klik op **toevoegen** aan de onderkant van de pagina.

    ![Klik op toevoegen om een nieuwe toepassing.][2]

5. Klik in het dialoogvenster **Wat wilt u doen** , klikt u op **toevoegen een toepassing in de galerie**.

    ![Klik op toevoegen een toepassing in de galerie.][3]

6. Typ in het **zoekvak** **Salesforce**. Selecteer **Salesforce** uit de resultaten en klik op **voltooid** als de toepassing wilt toevoegen.

    ![Salesforce toevoegen.][4]

7. U ziet nu de pagina snel aan de slag voor Salesforce:

    ![Snel aan de slag van de SalesForce-pagina in Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Stap 2: Inschakelen eenmalige aanmelding

1. Voordat u eenmalige aanmelding configureren kunt, moet u een aangepast domein implementeren voor uw Salesforce-omgeving en instellen. Zie voor instructies over hoe u dat doen, [Instellen van een domeinnaam](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Klik op de Salesforce snel aan de slag pagina in Azure AD op de knop **eenmalige aanmelding configureren** .

    ![De eenmalige aanmelding knop configureren][6]

3. Een dialoogvenster wordt geopend en ziet u een scherm met de vraag "Hoe wilt u gebruikers aanmelden bij Salesforce dat?" Selecteer **Azure AD eenmalige aanmelding**, en klik vervolgens op **volgende**.

    ![Selecteer Azure AD voor eenmalige aanmelding][7]

    > [AZURE.NOTE] Voor meer informatie over de verschillende eenmalige aanmelding opties, [klikt u hier](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work) over

4. Vul op de pagina **App-instellingen configureren** de **Aanmelding op URL** in door te typen in de URL van uw Salesforce-domein in de volgende notatie:
 - Enterprise-account:`https://<domain>.my.salesforce.com`
 - Ontwikkelaarsaccount:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Typ uw Aanmeldingsadres in URL][8]

5. Klik op de pagina **configureren eenmalige aanmelding bij Salesforce** Klik op het **certificaat downloaden**en sla het certificaatbestand lokaal op uw computer.

    ![Download-certificaat][9]

6. Open een nieuw tabblad in uw browser en meld u aan op uw Salesforce-beheerdersaccount.

7. Klik onder het navigatiedeelvenster van de **beheerder** , klikt u op **Beveiliging besturingselementen** om uit te vouwen van de sectie gerelateerde. Klik op **Instellingen voor eenmalige aanmelding**.

    ![Klik op instellingen voor eenmalige aanmelding bij beveiliging besturingselementen][10]

8. Klik op de knop **bewerken** op de pagina **Instellingen voor eenmalige aanmelding** .

    ![Klik op de knop bewerken][11]

    > [AZURE.NOTE] Als u nog geen instellingen voor eenmalige aanmelding voor uw account Salesforce inschakelen, moet u mogelijk contact opnemen met de Salesforce-ondersteuning om te kunnen de functie voor u is ingeschakeld.

9. Selecteer **SAML ingeschakeld**en klik vervolgens op **Opslaan**.

    ![Selecteer SAML ingeschakeld][12]

10. Uw SAML eenmalige aanmelding om instellingen te configureren, klikt u op **Nieuw**.

    ![Selecteer SAML ingeschakeld][13]

11. Controleer de volgende configuraties op de pagina **SAML eenmalige aanmelding instelling bewerken** :

    ![Schermafbeelding van de configuraties die u moet aanbrengen][14]

 - Typ een beschrijvende naam voor deze configuratie voor het veld **naam** . Een waarde op te geven voor **de naam van** het tekstvak **API** voor het automatisch ingevuld.

 - Kopieer de **URL van de uitgever** -waarde in Azure AD, en plak deze in het veld **uitgever** in Salesforce.

 - Typ de naam van uw Salesforce-domein met het volgende patroon in het **tekstvak entiteit-Id**:
     - Enterprise-account:`https://<domain>.my.salesforce.com`
     - Ontwikkelaarsaccount:`https://<domain>-dev-ed.my.salesforce.com`

 - Klik op **Bladeren** of **Bestand kiezen** om het dialoogvenster **Bestand selecteren voor uploaden** openen, selecteer uw Salesforce-certificaat en klik vervolgens op **openen** als u wilt uploaden van het certificaat.

 - Selecteer de **bevestiging bevat gebruikersnaam van salesforce.com**voor **SAML identiteitstype**.

 - Selecteer voor **SAML identiteit locatie**, **identiteit wordt in het element NameIdentifier van de instructie onderwerp**

 - In Azure AD, de waarde van de **Externe aanmeldings-URL** kopiëren en plak deze in het veld **Identiteit Provider aanmeldings-URL** in Salesforce.

 - Selecteer de **HTTP-omleiding**voor de **Service Provider gestart aanvragen binden**en.

 - Klik tot slot op **Opslaan** als u wilt uw SAML eenmalige aanmelding instellingen toepassen.

12. Klik op het linker navigatiedeelvenster in Salesforce, **Domain Management** om uit te vouwen van de sectie gerelateerde op en klik vervolgens op **Mijn domein**.

    ![Klik op mijn domein][15]

13. Schuif omlaag naar de sectie **Verificatie-configuratie** en klik op de knop **bewerken** .

    ![Klik op de knop bewerken][16]

14. Selecteer de beschrijvende naam van uw configuratie SAML SSO in de sectie **Verificatieservice** en klik vervolgens op **Opslaan**.

    ![Selecteer de configuratie van uw eenmalige aanmelding][17]

    > [AZURE.NOTE] Als meer dan één verificatieservice is geselecteerd, wordt klikt u vervolgens wanneer gebruikers proberen te initiëren eenmalige aanmelding bij uw Salesforce-omgeving, ze gevraagd om te selecteren welk verificatieservice ze willen zich aanmeldt. Als u niet wilt dat dit gebeurt, klikt u vervolgens moet u **alle andere verificatieservices uitgeschakeld laat**.

15. Schakel het selectievakje in de configuratie voor eenmalige aanmelding bevestiging om in te schakelen van het certificaat dat u hebt geüpload naar Salesforce in Azure AD. Klik vervolgens op **volgende**.

    ![Schakel het selectievakje bevestiging][18]

16. Typ op de laatste pagina van het dialoogvenster, in een e-mailadres als u wilt ontvangen van e-mailmeldingen voor fouten en waarschuwingen die zijn gerelateerd aan het onderhoud van deze configuratie voor eenmalige aanmelding. 

    ![Typ in uw e-mailadres.][19]

17. Klik op **Voltooien** om het dialoogvenster te sluiten. Als u wilt testen van uw configuratie, raadpleegt u de sectie [Gebruikers toewijzen aan Salesforce](#step-4-assign-users-to-salesforce)getiteld.

##<a name="step-3-enable-automated-user-provisioning"></a>Stap 3: Inschakelen geautomatiseerde gebruiker inrichting

1. In de pagina Azure AD snel aan de slag voor Salesforce, klik u op de knop **inrichting van de gebruiker configureren** .

    ![Klik op de knop inrichting van gebruiker configureren][20]

2. Typ in het dialoogvenster **inrichting van configureren gebruiker** in uw Salesforce beheerdersgebruikersnaam en wachtwoord.

    ![Typ in uw beheerdersgebruikersnaam of wachtwoord][21]

    > [AZURE.NOTE] Als u een productieomgeving configureert, wordt het wordt aanbevolen te maken van een nieuwe beheerdersaccount in Salesforce specifiek voor deze stap is. Dit account moet de **Systeembeheerder** profiel toegekend in Salesforce hebben.

3. Als u de beveiliging van uw Salesforce token, opent u een nieuw tabblad en meld u in de dezelfde Salesforce-beheerdersaccount. In de rechterbovenhoek van de pagina, klikt u op uw naam en klik op **Instellingen voor mijn**.

    ![Klik op uw naam en klik op instellingen voor mijn][22]

4. Klik op het linker navigatiedeelvenster, klik op **persoonlijke** om uit te vouwen van de gerelateerde sectie en klik op **Opnieuw mijn beveiligingstoken**.

    ![Klik op uw naam en klik op instellingen voor mijn][23]

5. Klik op de pagina **Opnieuw mijn beveiligingstoken** klik u op de knop **Beveiligingstoken opnieuw instellen** .

    ![Lees de waarschuwingen.][24]

6. Controleer de e-mailpostvak die is gekoppeld aan dit beheerdersaccount. Een e-mailbericht van Salesforce.com met het nieuwe beveiligingstoken zoeken.

7. Kopieer de token, gaat u naar uw Azure AD-venster en plak deze in het veld **Gebruiker beveiligingstoken** . Klik vervolgens op **volgende**.

    ![Plak in het beveiligingstoken][25]

8. Klik op de bevestigingspagina kunt u kiezen voor het ontvangen van e-mailmeldingen voor als inrichten fouten optreden. Klik op **Voltooien** om het dialoogvenster te sluiten.

    ![Typ in uw e-mailadres meldingen wilt ontvangen][26]

##<a name="step-4-assign-users-to-salesforce"></a>Stap 4: Gebruikers toewijzen aan Salesforce

1. Als u wilt testen van uw configuratie, begint u met het maken van een nieuwe testaccount in de adreslijst.

2. Klik op de knop **Gebruikers toewijzen** op de pagina Salesforce snel starten.

    ![Klik op gebruikers toewijzen][27]

3. Selecteer uw gebruikers-test en klik op de knop **toewijzen** aan de onderkant van het scherm:

 - Als u nog niet inschakelen geautomatiseerde gebruiker is geïnstalleerd, en ziet u het volgende bericht om te bevestigen:

        ![Confirm the assignment.][28]

 - Als u automatische gebruiker inrichting hebt ingeschakeld, ziet u een prompt om te bepalen welk type Salesforce-profiel de gebruiker moet hebben. Nieuw ingerichte gebruikers moeten worden weergegeven in uw omgeving Salesforce na een paar minuten.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Als u zijn in een Salesforce-omgeving voor **ontwikkelaars** is geïnstalleerd, wordt er een beperkt aantal licenties beschikbaar voor elk profiel. Daarom aanbevolen om gebruikers te richten aan de **babbelt gratis** gebruikersprofiel, waarvoor 4,999 licenties beschikbaar.

4. Als u wilt testen uw instellingen voor eenmalige aanmelding, open het paneel Access op [https://myapps.microsoft.com](https://myapps.microsoft.com/), en meld u aan de testaccount en klik op **Salesforce**.

##<a name="related-articles"></a>Verwante artikelen

- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Lijst met zelfstudies over het integreren van SaaS Apps](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
