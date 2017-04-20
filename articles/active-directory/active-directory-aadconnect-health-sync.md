
<properties
    pageTitle="Azure AD verbinding status bij het synchroniseren met | Microsoft Azure"
    description="Dit is de status verbinding maken met Azure AD-pagina die wordt uitgelegd hoe het controleren van Azure AD Connect synchroniseren."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Met behulp van Azure AD verbinding servicestatus voor synchronisatie
De volgende documentatie hoort bij Azure AD Connect (-synchronisatie) met Azure AD verbinding servicestatus bewaken.  Zie voor informatie over het controleren van AD FS met Azure AD verbinding systeemstatus [Gebruiken Azure AD status verbinding maken met AD FS](active-directory-aadconnect-health-adfs.md). Zie bovendien [Gebruiken Azure AD status verbinding maken met AD DS](active-directory-aadconnect-health-adds.md)voor informatie over het controleren van Active Directory Domain Services met Azure AD verbinding status.

![Azure AD Connect servicestatus voor synchronisatie](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Waarschuwingen voor Azure AD verbinding servicestatus voor synchronisatie
De Azure AD verbinding systeemstatus sectie waarschuwingen voor synchronisatie vindt u de lijst met actieve waarschuwingen. Elke melding bevat relevante informatie, resolutie stappen en koppelingen naar gerelateerde documentatie. Door te selecteren van een melding actief of opgelost ziet u een nieuwe blade met aanvullende informatie, evenals de stappen voor het oplossen van de waarschuwing en koppelingen naar aanvullende documentatie die u kunt uitvoeren. U kunt ook historische gegevens weergeven op waarschuwingen die zijn opgelost in het verleden.

U kunt uitvoeren door te selecteren van een melding u met aanvullende informatie, evenals de stappen krijgt voor het oplossen van de waarschuwing en koppelingen naar aanvullende documentatie.

![Azure AD Connect synchronisatiefout is opgetreden](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Beperkte evaluatie van meldingen
Als Azure AD Connect is niet werkt met de standaardconfiguratie (bijvoorbeeld als kenmerk filteren in een aangepaste configuratie uit de standaardconfiguratie wordt gewijzigd), klikt u vervolgens in de status verbinding maken met Azure AD-agent niet de foutgebeurtenissen die betrekking hebben op Azure AD Connect uploaden.

Hiermee wordt de evaluatie van waarschuwingen beperkt door de service. Zou ziet u een banner waarmee wordt aangegeven met deze voorwaarde in de Portal Azure onder uw service.

![Azure AD Connect servicestatus voor synchronisatie](./media/active-directory-aadconnect-health-sync/banner.png)

U kunt dit wijzigen door te klikken "Instellingen" en Azure AD verbinding systeemstatus agent alle foutenlogboeken wil uploaden.

![Azure AD Connect servicestatus voor synchronisatie](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Synchronisatie inzicht
Beheerders vaak wilt u weten over de tijd die nodig is om te synchroniseren in Azure AD en de hoeveelheid wijzigingen plaatsvindt. Deze functie biedt een eenvoudige manier om dit te visualiseren met de onder grafieken:   

- Latentie van synchroniseren-bewerkingen
- Trend van object wijzigen

### <a name="sync-latency"></a>Synchronisatie latentie
Deze functie biedt een grafische trend van latentie van de synchronisatie-bewerkingen (importeren, exporteren, enzovoort) voor connectors.  Hier vindt u een snelle en eenvoudige manier om te begrijpen niet alleen de latentie van uw activiteiten (groter als er een groot aantal veranderingen), maar ook een manier om te bepalen van afwijkingen in de latentie waardoor mogelijk verder onderzoek.

![Synchronisatie latentie](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Alleen de latentie van de ' exportbewerking ' voor de verbindingslijn Azure AD wordt standaard weergegeven.  Als u wilt zien van meer bewerkingen op de verbindingslijn of als u wilt bewerkingen vanuit andere verbindingslijnen weergeven met de rechtermuisknop op de grafiek, selecteer grafiek bewerken of klik op de knop 'Latentie grafiek bewerken' en kies de specifieke bewerking en verbindingslijnen.

### <a name="sync-object-changes"></a>Synchronisatie Object wordt gewijzigd
Deze functie biedt een grafische trend van het aantal wijzigingen die worden geëvalueerd en geëxporteerd naar Azure AD.  Vandaag, is het moeilijk wilt deze gegevens van de synchronisatie-logboeken verzamelen.  De grafiek kunt u, niet alleen een eenvoudigere van het aantal wijzigingen die optreden in uw omgeving, maar ook een visuele weergave van de fouten die optreden cmdlets voor controle.

![Synchronisatie latentie](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Object synchronisatie op niveau foutrapport (Preview)
Deze functie biedt een rapport over synchronisatiefouten die optreden mogelijk bij identiteitsgegevens worden gesynchroniseerd tussen Windows Server AD en Azure AD met Azure AD Connect.

- Het rapport behandelt fouten vastgelegd door de synchronisatieclient (Azure AD Connect versie 1.1.281.0 of hoger)
- Het bevat de fouten die zijn aangebracht in de laatste synchronisatiebewerking op de synchronisatie-engine. ("Exporteren' op de Azure AD-Connector.)
- Azure AD verbinding systeemstatus agent voor synchronisatie moet uitgaande verbinding hebt met de vereiste eindpunten voor het rapport om de meest recente gegevens te nemen. Zie het [gedeelte vereisten](active-directory-aadconnect-health-agent-install.md#Requirements) voor meer informatie.
- Het rapport is **bijgewerkt na elke 30 minuten** met de gegevens die zijn geüpload, door Azure AD verbinding systeemstatus agent voor synchronisatie.
Deze biedt de volgende belangrijke mogelijkheden

    - Categorisatie van fouten
    - Lijst met objecten met fout per categorie
    - Alle gegevens over de fouten op één plaats
    - Vergelijking van objecten met fout vanwege een conflict naast
    - Download de foutrapport als een CVS (binnenkort)

### <a name="categorization-of-errors"></a>Categorisatie van fouten
Het rapport wordt gecategoriseerd de bestaande synchronisatieproblemen in de volgende categorieën:

| Categorie | Beschrijving |
| -------------- | ----------- |
| Dubbel kenmerk | Fouten bij het Azure AD Connect probeert maken of bijwerken van objecten met dubbele waarden van een of meer kenmerken in Azure AD die uniek in een Tenant, zoals proxyAddresses, UserPrincipalName zijn. |
| Gegevens niet overeenkomen | Fouten bij de vloeiende-overeenkomst niet overeenkomen met de objecten die in synchronisatiefouten resulteren. |
| Fout bij valideren van gegevens | Fouten als gevolg van ongeldige gegevens, zoals niet-ondersteunde tekens in kritieke kenmerken zoals UserPrincipalName, opmaken fouten die validatie voordat deze werd geschreven in Azure AD is mislukt.|
| Grote kenmerk | Fouten bij een of meer kenmerken groter dan de toegestane grootte, lengte of tellen zijn.|
| Andere | Alle andere fouten die niet in de bovenstaande categorieën passen. Op basis van feedback, wordt deze categorie in subcategorieën worden gesplitst.

![Overzicht van de fout rapport synchroniseren](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![fout rapportcategorieën synchroniseren](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Lijst met objecten met fout per categorie
Inzoomen op elke categorie, vindt u in de lijst met objecten met de fout in die categorie.
![Lijst met fouten rapport synchroniseren](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Meer informatie over fout
Gegevens volgen is beschikbaar in de gedetailleerde weergave voor elke fout

- -Id's voor het nodig zijn voor *AD-Object*
- -Id's voor het *Azure AD Object* betrokken (indien van toepassing)
- Beschrijving van fout en oplossing
- Verwante artikelen

![Rapportdetails voor synchronisatie-fout](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>De foutrapport als CSV downloaden
Deze functie is binnenkort beschikbaar. Blijf lopende voor meer updates.



## <a name="related-links"></a>Verwante koppelingen
* [Fouten corrigeren tijdens het synchroniseren](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect systeemstatus](active-directory-aadconnect-health.md)
* [Azure AD Connect systeemstatus Agent installatie](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect systeemstatus bewerkingen](active-directory-aadconnect-health-operations.md)
* [Gebruik van Azure AD status verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Gebruik van Azure AD status verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect systeemstatus Veelgestelde vragen](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect systeemstatus versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
