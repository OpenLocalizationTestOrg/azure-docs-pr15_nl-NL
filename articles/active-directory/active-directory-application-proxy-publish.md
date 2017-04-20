<properties
    pageTitle="Apps gebruiken met Azure AD-toepassingsproxy publiceren | Microsoft Azure"
    description="On-premises implementatie toepassingen in de cloud met Azure AD-toepassingsproxy publiceren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Toepassingen met Azure AD-toepassingsproxy publiceren

Azure AD-toepassingsproxy kunt u de ondersteuning van externe werknemers door te publiceren, on-premises implementatie toepassingen toegankelijk via internet. Met dit punt, moet u al hebt [ingeschakeld toepassingsproxy in de portal van Azure klassieke](active-directory-application-proxy-enable.md). In dit artikel begeleidt u bij de stappen voor het publiceren van toepassingen die op uw lokale netwerk worden uitgevoerd en geef de veilige externe toegang van buiten uw netwerk. Nadat u in dit artikel hebt voltooid, kunt u zult gereed is voor het configureren van de toepassing met persoonlijke informatie of beveiligingsvereisten die zijn.

> [AZURE.NOTE] Toepassingsproxy is een functie die is alleen beschikbaar als u een upgrade hebt uitgevoerd naar de Premium- of Basic edition van Azure Active Directory. Zie [Azure Active Directory-edities](active-directory-editions.md)voor meer informatie.

## <a name="publish-an-app-using-the-wizard"></a>Een app met de wizard Publiceren

1. Meld u aan als beheerder in de [klassieke Azure-portal](https://manage.windowsazure.com/).
2. Ga naar Active Directory en selecteer de map waarvoor u toepassingsproxy hebt ingeschakeld.

    ![Active Directory - pictogram](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Klik op het tabblad **toepassingen** en klik op de knop **toevoegen** onder aan het scherm

    ![Toepassing toevoegen](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Selecteer **publiceren een toepassing die toegankelijk van buiten uw netwerk is**.

    ![Publiceren van een toepassing die toegankelijk zijn vanuit buiten uw netwerk](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Geef de volgende informatie over het gebruik van de toepassing:

    - **Naam**: de beschrijvende naam voor uw toepassing. Het moet uniek zijn binnen uw adreslijst.
    - **Interne URL**: het adres dat de toepassing Proxy verbindingslijn wordt gebruikt voor toegang tot de toepassing binnen uw privé-netwerk. U kunt een specifiek pad op de backend-server te publiceren, terwijl de rest van de server niet-gepubliceerde is opgeeft. Op deze manier kunt u verschillende sites op de server publiceren en ieder een eigen regels naam en toegang geven.

        > [AZURE.TIP] Als u een pad publiceert, zorg dat deze alle benodigde afbeeldingen, scripts en opmaakmodellen voor uw toepassing bevat. Bijvoorbeeld: als uw app https://yourapp/app loopt en afbeeldingen te vinden op https://yourapp/media gebruikt, u moet publiceren https://yourapp/ als het pad.

    - **Vooraf-verificatie methode**: hoe toepassingsproxy gebruikers worden geverifieerd voordat ze toegang geven tot uw toepassing. Kies een van de opties in de vervolgkeuzelijst.

        - Azure Active Directory: Toepassingsproxy leidt gebruikers aan te melden met Azure AD, die wordt geverifieerd door hun machtigingen voor de map en de toepassing.
        - Indirecte: Gebruikers niet hoeft te verifiëren voor toegang tot de toepassing.

    ![Eigenschappen van een servicetoepassing](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Klik op het vinkje onderaan in het scherm om de wizard. De toepassing is nu in Azure AD gedefinieerd.


## <a name="assign-users-and-groups-to-the-application"></a>Gebruikers en groepen toewijzen aan de toepassing

In de volgorde voor uw gebruikers toegang hebben tot uw gepubliceerde toepassing, moet u ze toewijzen individueel of in groepen. (Inclusief uzelf toewijzen access te.) Hiervoor is vereist dat elke gebruiker een licentie voor eenvoudige Azure of hoger hebben. U kunt licenties toewijzen aan groepen of afzonderlijk. Zie [gebruikers toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md) voor meer informatie. 

Hiermee verkrijgt machtigingen om de app gebruiken voor apps die verificatie vereisen. Apps waarvoor verificatie niet voor kunnen gebruikers nog steeds worden toegewezen aan de app zodat het wordt weergegeven in de toepassing, zoals Apps.

1. Nadat de wizard van de App toevoegen is voltooid, ziet u de pagina snel aan de slag voor uw toepassing. Als u wilt beheren wie toegang heeft tot de app, selecteert u **gebruikers en groepen**.

    ![Schermafbeelding van gebruikers - toewijzen toepassing Proxy snel aan de slag](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Zoeken naar specifieke groepen in uw adreslijst of alle gebruikers weergeven. Als u wilt weergeven in de lijst met zoekresultaten, klikt u op het vinkje.

    ![Zoeken naar groepen of gebruikers - schermafbeelding](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Selecteer elke gebruiker of groep die u wilt toewijzen aan deze app en klikt u op **toewijzen**. U wordt gevraagd deze actie te bevestigen.

> [AZURE.NOTE] Voor geïntegreerde Windows-verificatie-apps, kunt u alleen gebruikers en groepen die worden gesynchroniseerd vanuit uw lokale Active Directory toewijzen. Gebruikers die Meld u aan met een Microsoft-account en gasten kunnen niet worden toegewezen voor apps die zijn gepubliceerd met Azure Active Directory-toepassingsproxy. Zorg ervoor dat uw gebruikers Meld u aan met referenties die deel uitmaken van hetzelfde domein als de app die u wilt publiceren.

## <a name="test-your-published-application"></a>Test uw gepubliceerde toepassing

Wanneer u uw toepassing hebt gepubliceerd, kunt u dit kunt testen door te gaan naar de URL die u hebt gepubliceerd. Zorg ervoor dat u toegang hebt tot deze, dat deze wordt correct weergegeven en of alles werkt zoals verwacht. Als u problemen hebt of een foutbericht wordt weergegeven, kunt u de [gids voor probleemoplossing](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Uw toepassing configureren

U kunt gepubliceerde apps wijzigen of instellen van geavanceerde opties op de pagina configureren. Klik op deze pagina kunt u uw app aanpassen door te wijzigen van de naam of het uploaden van een logo. U kunt ook toegangsregels zoals de verificatie-methode of meervoudige verificatie beheren.

![Geavanceerde configuratie](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Nadat u toepassingen publiceren toepassingsproxy van Azure Active Directory, met deze worden weergegeven in de lijst met toepassingen in Azure AD en kunt u ze er beheren.

Als u toepassingsproxy services uitschakelt nadat u toepassingen hebt gepubliceerd, zijn ze niet meer toegankelijk van buiten uw privé-netwerk. Hiermee verwijdert u de toepassingen.

Als u wilt weergeven van een toepassing en ervoor zorgen dat dit toegankelijk is, dubbelklikt u op de naam van de toepassing. Als de toepassingsproxy-service is uitgeschakeld en de toepassing niet beschikbaar is, wordt een waarschuwing weergegeven boven aan het scherm.

Een toepassing verwijderen, selecteert u een toepassing in de lijst en klik vervolgens op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- [Toepassingen met uw eigen domeinnaam publiceren](active-directory-application-proxy-custom-domains.md)
- [Inschakelen voor eenmalige aanmelding](active-directory-application-proxy-sso-using-kcd.md)
- [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)
- [Werken met op claims-toepassingen](active-directory-application-proxy-claims-aware-apps.md)

Voor het laatste nieuws en updates, raadpleegt u de [toepassingsproxy-blog](http://blogs.technet.com/b/applicationproxyblog/)
