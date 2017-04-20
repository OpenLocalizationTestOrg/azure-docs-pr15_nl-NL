<properties
    pageTitle="Synchronisatie van Azure AD Connect: technische concepten | Microsoft Azure"
    description="Dit artikel wordt uitgelegd de technische concepten van Azure AD Connect synchroniseren."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-technical-concepts"></a>Synchronisatie van Azure AD Connect: technische concepten
In dit artikel volgt een overzicht van het onderwerp [lidmaatschap architectuur](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect-synchronisatie gebaseerd op een effen metadirectory synchronisatie-platform.
De volgende secties introduceren de concepten voor metadirectory-synchronisatie.
Samenstellen van MIIS, ILM en FIM, biedt de Azure Active Directory Sync Services de volgende platform voor verbinding maken met gegevensbronnen, gegevens synchroniseren tussen gegevensbronnen, evenals de inrichting van en deprovisioning van identiteiten.

![Technische concepten](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

De volgende secties vindt meer informatie over de volgende aspecten van de synchronisatieservice FIM:

- Verbindingslijn
- Kenmerk stroom
- Connector ruimte
- Metaverse
- Inrichten

## <a name="connector"></a>Verbindingslijn

De codemodules die worden gebruikt om te communiceren met een verbonden directory heten verbindingslijnen (voorheen bekend als management agenten (MAs)).

Deze zijn geïnstalleerd op de computer met Azure AD Connect synchroniseren.
De verbindingslijnen kunnen u agentless manier converseren met behulp van extern systeemprotocollen in plaats van te vertrouwen op de implementatie van gespecialiseerde agenten. Dit betekent dat beperktere risico's en -tijd van de implementatie, met name bij het afhandelen van kritieke toepassingen en systemen.

In de bovenstaande afbeelding is de verbindingslijn is synoniem met de ruimte verbindingslijn, maar omvat alle communicatie met het externe systeem.

De verbindingslijn is verantwoordelijk voor alle importeren en exporteren functionaliteit aan het systeem en ontwikkelaars van hoeft te weten hoe u kunt verbinding maken met elk systeem native bij gebruik van de inrichting van declaratieve om aan te passen gegevenstransformaties maakt.

En uitvoer alleen optreden bij gepland, waardoor verdere onafhankelijk van veranderingen die zich in het systeem, aangezien wijzigingen niet automatisch aan de verbonden gegevensbron doorgegeven kunnen. Bovendien kunnen ontwikkelaars ook hun eigen verbindingslijnen om verbinding te maken met vrijwel elke gegevensbron maken.

## <a name="attribute-flow"></a>Kenmerk stroom

De metaverse is de gecombineerde weergave van alle gekoppelde identiteiten van de omringende verbindingslijn spaties. In de bovenstaande afbeelding wordt kenmerk stroom door regels met pijlpunten voor stroom inkomende en uitgaande weergegeven. Kenmerk stroom is het proces van kopiëren of gegevens van het ene systeem naar een andere transformeren en alle kenmerk loopt (inkomend of uitgaand).

Kenmerk stroom treedt op tussen de ruimte verbindingslijn en de metaverse bi andersom wanneer synchronisatie (volledig of delta) bewerkingen zijn gepland om uit te voeren.

Kenmerk stroom wordt alleen weergegeven wanneer deze synchronisatie worden uitgevoerd. Kenmerk loopt zijn gedefinieerd in de regels van synchronisatie. Deze zijn inkomende (ISR in de bovenstaande afbeelding) of uitgaande (OSR in de bovenstaande afbeelding).

## <a name="connected-system"></a>Verbonden systeem

Verbonden systeem (of verbonden directory) is verwijzen naar het externe systeem Azure AD Connect synchronisatie is verbonden met en lezen en schrijven identiteitsgegevens en naar.

## <a name="connector-space"></a>Connector ruimte

Elke verbonden gegevensbron wordt weergegeven als een gefilterde subset van de objecten en kenmerken in de ruimte verbindingslijn.
Hiermee kan de synchronisatieservice lokaal gewerkt, hebben zonder dat u een contact van het externe systeem bij het synchroniseren van de objecten op te nemen en wordt beperkt interactie voor de invoer en Hiermee exporteert u alleen.

Wanneer de gegevensbron en de verbindingslijn hebt de mogelijkheid te geven van een lijst van wijzigingen (een delta importeren), klikt u vervolgens de operationele efficiëntie Hiermee verhoogt u aanzienlijk als alleen wijzigingen sinds de laatste polling-cyclus worden uitgewisseld. De verbindingslijn ruimte schermt de verbonden gegevensbron tegen verdere wijzigingen automatisch doorgevoerd door te vereisen dat de planning verbindingslijn importeert en exporteert. Deze toegevoegde verzekeringen verleent u gemoedsrust tijdens het testen, voorbeeld of de acceptatie van de volgende update.

## <a name="metaverse"></a>Metaverse

De metaverse is de gecombineerde weergave van alle gekoppelde identiteiten van de omringende verbindingslijn spaties.

Terwijl identiteiten aan elkaar zijn gekoppeld en instantie voor verschillende kenmerken tot en met importeren stroom toewijzingen is toegewezen, wordt de status van het object centraal metaverse overeenkomsten statistische gegevens uit meerdere systemen. Toewijzingen uitvoeren van deze overdracht object-kenmerk informatie naar uitgaande systemen.

Objecten worden gemaakt wanneer een gezaghebbend systeem ze naar de metaverse projecten. Zodra alle verbindingen worden verwijderd, wordt het object metaverse verwijderd.

Objecten in de metaverse worden niet rechtstreeks bewerkt. Alle gegevens in het object moet worden bijgedragen tot en met kenmerk stroom. De metaverse onderhoudt permanente verbindingslijnen met elke verbindingslijn-ruimte. Deze verbindingslijnen hoeft geen nieuwe evaluatie voor elke synchronisatie uitvoeren. Dit betekent dat Azure AD Connect synchroniseren niet hoeft te vinden het overeenkomende externe object telkens wanneer. Hiermee voorkomt u dat u een dure agenten om te voorkomen dat kenmerken die normaal zou die verantwoordelijk is voor het correleren van de objecten wordt gewijzigd.

Wanneer ontdekken nieuwe gegevensbronnen die wellicht reeds bestaande objecten die moeten worden beheerd, wordt een proces een join-regel genoemd in Azure AD Connect-synchronisatie gebruikt potentiële kandidaten waarmee u tot stand brengen van een koppeling wordt berekend.
Wanneer de koppeling tot stand is gebracht, deze evaluatie niet meer optreedt en normale kenmerk stroom kan zich voordoen tussen de externe gegevensbron voor verbonden en de metaverse.

## <a name="provisioning"></a>Inrichten

Wanneer een gemachtigde bronprojecten kunt een nieuw object in de metaverse een nieuw verbindingslijn ruimte object worden gemaakt in een andere verbindingslijn dat staat voor een gegevensbron voor het volgende verbonden.

Dit inherent tot stand brengt een koppeling en kenmerk stroom bi andersom kunt gaan.

Wanneer u een regel vaststelt dat er een nieuw verbindingslijn ruimte object moet worden gemaakt, wordt deze inrichting genoemd. Echter omdat deze bewerking alleen plaats in de ruimte verbindingslijn vindt, bevat deze niet uitvoeren op in de verbonden gegevensbron totdat een exporteren wordt uitgevoerd.

## <a name="additional-resources"></a>Aanvullende informatie

* [Azure AD verbinden met synchronisatie: Opties voor het aanpassen van synchronisatie](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
