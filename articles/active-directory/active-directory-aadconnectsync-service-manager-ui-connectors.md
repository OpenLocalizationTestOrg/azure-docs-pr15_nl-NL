<properties
    pageTitle="Synchronisatie van Azure AD Connect: synchronisatie servicebeheer UI | Microsoft Azure"
    description="Het tabblad verbindingslijnen in het servicebeheer van synchronisatie voor Azure AD Connect te begrijpen."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Synchronisatie van Azure AD Connect: synchronisatie servicebeheer

[Bewerkingen](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Verbindingslijnen](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Het tabblad verbindingslijnen wordt gebruikt voor het beheren van alle systemen die de synchronisatie-engine is verbonden met.

## <a name="connector-actions"></a>Connector acties

Actie | Opmerking
--- | ---
Maken | Gebruik geen. Gebruik de installatiewizard voor verbinding maakt met extra AD forests.
Eigenschappen | Wordt gebruikt voor het domein en organisatie-eenheid filteren.
[Verwijderen](#delete) | Gebruikt de gegevens in de ruimte verbindingslijn verwijderen of verwijderen van verbinding met een bos.
[Uitvoeren profielen configureren](#configure-run-profiles) | Behalve voor domeinen filteren, niets hier configureren. U kunt deze actie gebruiken om al geconfigureerde uitvoeren profielen weer te geven.
Uitvoeren | Wordt gebruikt voor het starten van een eenmalige uitvoeren van een profiel.
Stoppen | Een verbindingslijn dat moment actief zijn een profiel stopt.
Connector exporteren | Gebruik geen.
Connector importeren | Gebruik geen.
Connector bijwerken | Gebruik geen.
Schema vernieuwen | Hiermee vernieuwt u het schema in de cache opgeslagen. Het is voorkeur voor het gebruik van de optie in de installatiewizard is het in plaats daarvan sinds die ook updates regels synchroniseren.
[Zoeken verbindingslijn ruimte](#search-connector-space) | Gebruikt om objecten te zoeken en te [volgen van een object en de bijbehorende gegevens via het systeem](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Verwijderen
De verwijderactie wordt gebruikt voor twee verschillende dingen.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

De optie **alleen verbindingslijn spatie verwijderen** verwijdert alle gegevens, maar blijven de configuratie.

De optie **verwijderen verbindingslijn en verbindingslijn ruimte** Hiermee verwijdert u de gegevens en de configuratie. Deze optie wordt gebruikt wanneer u niet meer verbinding maken met een bos wilt.

Beide opties synchroniseren van alle objecten en de objecten metaverse bijwerken. Deze actie is een langdurige bewerking.

### <a name="configure-run-profiles"></a>Uitvoeren profielen configureren
Deze optie kunt u zien die de uitvoeren profielen die is geconfigureerd voor een verbindingslijn.

![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Zoeken verbindingslijn ruimte
De actie zoeken verbindingslijn ruimte is handig om te zoeken van objecten en problemen met gegevens.

![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Begin door een **bereik**te selecteren. U kunt zoeken op basis van gegevens (RDN, DN fixeerpunt onderliggende boomstructuur), of van het object (alle overige opties).  
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Als u bijvoorbeeld onderliggende boomstructuur zoeken wilt, krijgt u alle objecten in een organisatie-eenheid.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) in dit raster kunt u een object, selecteer **Eigenschappen**en [volgt u deze](#follow-an-object-and-its-data-through-the-system) uit de bron verbindingslijn ruimte, via de metaverse en naar de doelsite verbindingslijn ruimte te selecteren.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Ga als volgt een object en de bijbehorende gegevens via het systeem
Als u een probleem met de gegevens oplossen wilt, een object uit de bron verbindingslijn ruimte, naar de metaverse volgen en naar de doelsite verbindingslijn ruimte is een belangrijke procedure voor meer informatie over waarom gegevens niet de verwachte waarden bevatten.

### <a name="connector-space-object-properties"></a>Connector ruimte objecteigenschappen
**Importeren**  
Wanneer u een object cs opent, zijn er meerdere tabbladen aan de bovenkant. Het tabblad **importeren** ziet u de gegevens die is gefaseerde na een import.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) de **Oude waarde** ziet u wat momenteel is opgeslagen in het systeem en de **Nieuwe waarde** wat is ontvangen van het bronsysteem en nog niet is toegepast. Aangezien er een synchronisatiefout is, kunnen de wijziging in dit geval kan niet worden toegepast.

**Fout**  
De foutpagina is alleen zichtbaar als er een probleem met het object. De details bekijken op de pagina bewerkingen voor meer informatie over het [oplossen van synchronisatieproblemen](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**Over de afkomst**  
Het tabblad over de afkomst ziet u hoe de verbindingslijn ruimte-object is gerelateerd aan het object metaverse. U kunt zien wanneer een wijziging in de verbindingslijn laatst worden geïmporteerd uit het systeem verbonden en welke regels toegepast op gegevens in de metaverse vullen.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) In de kolom **actie** , kunt u zien er één regel van **Inkomend** synchroniseren met de actie **inrichten**. Die wordt aangegeven met het zo lang maken als dit object verbindingslijn-ruimte aanwezig is, wordt het object metaverse blijft. Als in de lijst met regels voor synchronisatie weergegeven in plaats daarvan een regel voor synchronisatie met richting **Uitgaand** en **inrichten**, geeft aan dat dit object wordt verwijderd wanneer het object metaverse wordt verwijderd.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) u kunt ook zien in de kolom **PasswordSync** dat de binnenkomende verbindingslijn ruimte wijzigingen in het wachtwoord bijdragen kan aangezien één regel voor synchronisatie met de waarde **True**. Dit wachtwoord wordt vervolgens verzonden naar Azure AD tot en met de uitgaande regel.

Op het tabblad over de afkomst kunt u de metaverse openen door te klikken op [Metaverse objecteigenschappen](#metaverse-object-properties).

Onder aan alle tabbladen worden twee knoppen: **Preview** en **Log**.

**Voorbeeld**  
De voorbeeldpagina wordt gebruikt voor het synchroniseren van één enkel object. Het is handig als u problemen met enkele regels voor het synchroniseren van klant en wilt zien van het effect van een wijziging op één object. U kunt kiezen uit **Volledige synchroniseren** en **Delta synchroniseren**. U kunt ook selecteren tussen **Preview genereren**, alleen de wijziging blijft in het geheugen, en **Doorvoeren Preview**, welke alle fasen wordt gewijzigd in target verbindingslijn spaties.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) kunt u het object en welke regel wordt toegepast voor een bepaald kenmerk-mailstroom controleren.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Log**  
De pagina Log wordt gebruikt om de synchronisatiestatus van wachtwoord en geschiedenis, raadpleegt u [problemen oplossen met de Wachtwoordsynchronisatie](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) voor meer informatie weer te geven.

### <a name="metaverse-object-properties"></a>Metaverse objecteigenschappen
**Kenmerken**  
Klik op het tabblad kenmerken ziet u de waarden en welke verbindingslijn bijgedragen deze.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**verbindingslijnen**  
Het tabblad verbindingslijnen ziet u alle verbindingslijn spaties waarvoor een weergave van het object.
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) dit tabblad kunt u navigeren naar het [verbindingslijn ruimte object](#connector-space-object-properties)ook.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
