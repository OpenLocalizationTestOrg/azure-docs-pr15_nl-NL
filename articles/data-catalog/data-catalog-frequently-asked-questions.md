<properties
   pageTitle="Veelgestelde vragen Azure gegevenscatalogus | Microsoft Azure"
   description="Veelgestelde vragen over de gegevenscatalogus Azure, met inbegrip van mogelijkheden voor gegevensdetectie bron, aantekeningen en management."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure gegevenscatalogus Veelgestelde vragen

In dit artikel vindt u antwoorden voor veelgestelde vragen met betrekking tot de service Microsoft **Azure-gegevenscatalogus** .

## <a name="q-what-is-azure-data-catalog"></a>V: Wat is de gegevenscatalogus Azure?

A: Microsoft Azure-gegevenscatalogus is een volledig beheerde service gehost in de cloud met Microsoft Azure die als een systeem van registratie en systeem van discovery voor enterprise-gegevensbronnen fungeert. Azure gegevenscatalogus bevat functies waarmee de elke gebruiker – in analisten en gegevens wetenschappers voor ontwikkelaars – registreren, ontdekken, begrijpen en gebruiken van gegevensbronnen.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>V: welke klant uitdagingen heeft Azure-gegevenscatalogus oplossen?

Azure gegevenscatalogus adressen bron gegevensdetectie en 'donker-gegevens' door gebruikers om te detecteren en enterprise-gegevensbronnen begrijpen toe te staan.

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>V: wie de doelgroepen voor Azure-gegevenscatalogus zijn?

Azure gegevenscatalogus biedt mogelijkheden voor technische en niet-technische gebruikers, waaronder:

- Gegevens ontwikkelaars, BI en Analytics-Professionals: die verantwoordelijk zijn voor het maken van gegevens en analyses inhoud zodat anderen deze kunnen gebruiken
-   Gegevens Stewards: Mensen die de beschikken over de gegevens, wat betekent dit en hoe deze is bedoeld om te worden gebruikt en voor welke doel
- Gebruikers van gegevens: Personen die kunnen eenvoudig ontdekken moeten, begrijpen en verbinding maken met gegevens die nodig zijn voor hun werk met de functie van hun keuze
- Centraal IT: die wie moet honderden gegevensbronnen detecteerbaar maken voor zakelijke gebruikers en wie moeten supervisie houden via hoe gegevens wordt gebruikt en door wie

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>V: Wat is de beschikbaarheid van Azure-gegevenscatalogus regio?

Azure gegevenscatalogus services zijn momenteel beschikbaar in de volgende datacenters:

- West VS
- Oost-VS
- West Europa
- Noord-Europa
- Australië Oost
- Zuidoost-Azië

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>V: wat zijn de beperkingen ten aanzien van het aantal gegevens activa in de catalogus van Azure-gegevens?

De gratis versie van Azure catalogus met gegevens is beperkt tot 5.000 geregistreerde gegevens activa.

De standaard-editie van Azure gegevenscatalogus ondersteunt maximaal 100.000 geregistreerde gegevens activa.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>V: wat zijn de ondersteunde gegevensbronnen en activa gegevenstypen?

Zie [Gegevenscatalogus DSA](data-catalog-dsr.md) voor de lijst met ondersteunde gegevensbronnen.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>V: hoe ik ondersteuning voor een andere gegevensbron aanvragen?

Functieverzoeken verzenden en andere feedback kunnen worden verzonden in het [forum van Azure-gegevenscatalogus](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>V: hoe ga ik aan de slag met Azure-gegevenscatalogus?

De beste plaats aan de slag is volgens de instructies in [Aan de slag met de gegevenscatalogus](data-catalog-get-started.md). In dit artikel is een end-to-end-overzicht van de mogelijkheden van de service.

## <a name="q-how-do-i-register-my-data"></a>V: hoe kan ik mijn gegevens registreren?

Als u wilt uw gegevens hebt geregistreerd in Azure-gegevenscatalogus, start u het hulpprogramma Azure-gegevenscatalogus registratie in het gedeelte "Publiceren" van de portal Azure-gegevenscatalogus. Meld u aan met dezelfde referenties die u gebruiken voor toegang tot de portal Azure-gegevenscatalogus in de publicatie gegevenscatalogus Azure-toepassing, en selecteer vervolgens de gegevensbron en de specifieke elementen die u wilt registreren.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>V: welke eigenschappen worden opgehaald voor activa van gegevens die zijn geregistreerd?

De specifieke eigenschappen van gegevensbron tot de gegevensbron wordt verschillen, maar in het algemeen de volgende informatie wordt opgehaald door de service voor het publiceren van Azure-gegevenscatalogus:

- De Activumnaam van de
- Activatype
- Activumbeschrijving
- Kenmerk/kolomnamen
- Kenmerk/kolom-gegevenstypen
- Kenmerk/kolombeschrijving

> [AZURE.IMPORTANT] Gegevens activa registreren met Azure-gegevenscatalogus niet verplaatsen of kopiëren van uw gegevens in de cloud. De activa metagegevens registreren activa uit een gegevensbron wordt gekopieerd naar Azure, maar blijven de gegevens in de bestaande gegevensbronlocatie. Alleen geldt deze regel als een gebruiker wil uploaden preview records of een profiel gegevens bij het registreren van activa. Wanneer een voorbeeld, maximaal 20 records inclusief uit elke actief worden gekopieerd en worden opgeslagen als een momentopname in Azure-gegevenscatalogus. Wanneer waaronder een gegevens profiel, statistische gegevens (zoals de grootte van tabellen, het percentage null-waarden per kolom en de minimum, maximum aantal en gemiddelde waarden voor kolommen) worden berekend en opgenomen in de metagegevens die zijn opgeslagen in de catalogus.

<br/>

> [AZURE.NOTE] Voor gegevensbronnen zoals SQL Server Analysis Services waarvoor een eersteklas **Beschrijving** eigenschap, wordt die waarde onroerend goed worden opgehaald door de Azure-gegevenscatalogus toepassing publiceren. Voor SQL Server-relationele databases, die niet over een eersteklas **Beschrijving** eigenschap, wordt de publicerende gegevenscatalogus Azure-toepassing de waarde ophalen uit het ms_description uitgebreide eigenschap voor objecten en kolommen. Zie TechNet voor [Uitgebreide eigenschappen op databaseobjecten gebruiken](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx)voor meer informatie.

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>V: hoe lang moet het voordat voor nieuw ingeschreven activa wordt weergegeven in de catalogus van Azure-gegevens?

Nadat u activa hebt geregistreerd met de gegevenscatalogus Azure is het mogelijk dat er een periode van 5-10 seconden voordat deze worden weergegeven in de portal Azure-gegevenscatalogus.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>V: hoe ik aantekeningen toevoegen aan en Verrijk de metagegevens voor Mijn activa geregistreerde gegevens?

Er is de eenvoudigste manier om aan te bieden van metagegevens voor geregistreerde activa selecteren van de activa in de gegevenscatalogus van Azure-portal en voert de metagegevenswaarden in het deelvenster Eigenschappen of schema voor het geselecteerde object.

U kunt ook enkele metagegevens, zoals tags en experts geven tijdens het registratieproces. De waarden in de gegevenscatalogus van Azure-service voor het publiceren van toepassing op alle activa op dat moment wordt geregistreerd. Als u wilt de objecten onlangs geregistreerd in de portal voor aanvullende aantekeningen weergeven, selecteert u de knop **Weergave-Portal** op het laatste scherm van de publicerende gegevenscatalogus Azure-toepassing.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>V: hoe kan ik mijn geregistreerde gegevensobjecten verwijderen?

U kunt een object verwijderen uit de gegevenscatalogus Azure door het object te selecteren in de portal, klikt op de knop **verwijderen** . Dit de metagegevens voor het object worden verwijderd uit de gegevenscatalogus Azure maar het is niet van invloed op de werkelijke onderliggende gegevensbron.

## <a name="q-what-is-an-expert"></a>V: Wat is een expert?

Een expert is een persoon die een perspectief op de hoogte over een gegevensobject heeft. Een object kan meerdere experts hebben. Een expert hoeft niet de ' eigenaar ' voor een object zijn. de expert is gewoon iemand die weet hoe de gegevens kunt en moet worden gebruikt.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>V: hoe kan ik gegevens delen met de gegevenscatalogus van Azure-team als ik problemen?

Gebruik van de gegevenscatalogus van Azure-forum problemen rapporteren, informatie te delen, en vragen kunt stellen. Het forum kunt u vinden op http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>V: Azure gegevenscatalogus werkt met deze andere gegevensbron die ik ben op?
We werken actief over het toevoegen van meer gegevensbronnen aan Azure-gegevenscatalogus. Als er een gegevensbron die u wilt zien ondersteund, neemt suggestie voor deze (of ondersteuning van uw voicemail, als deze is al voorgesteld) in het [forum van Azure-gegevenscatalogus](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>V: hoe Azure-gegevenscatalogus gerelateerd is aan de gegevenscatalogus van Power BI voor Office 365?

U kunt de gegevenscatalogus Azure zien als een ontwikkeling van de gegevenscatalogus. Azure gegevenscatalogus levert vergelijkbare mogelijkheden voor gegevensbron publiceren en discovery, maar is richten op bredere scenario's en niet afhankelijk is van de Office 365. Kort nadat de catalogus met Azure-gegevens in het algemeen beschikbaar wordt de twee catalogi samenvoegen in één service.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>V: welke machtigingen moet een gebruiker worden activa met Azure-gegevenscatalogus registreren?

De gebruiker die het Azure-gegevenscatalogus registratiehulpprogramma mailflow moet machtigingen voor de gegevensbron waarmee hem te lezen van de metagegevens uit de bron. Als de gebruiker ook selecteert een voorbeeld opnemen, moet klikt u vervolgens de gebruiker ook machtigingen hebben waarmee hem te lezen in de gegevens uit de objecten wordt geregistreerd.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>V: Azure gegevenscatalogus komen beschikbaar voor on-premises implementatie als u ook?

Azure gegevenscatalogus is een cloudservice die kunt werken met zowel de cloud en de on-premises gegevensbronnen, geven van een hybride gegevensbron discovery-oplossing. Er zijn momenteel geen plannen voor een versie van de gegevenscatalogus van Azure-service die on-premises implementatie wordt uitgevoerd.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>V: kunnen we meer / rijkere metagegevens uitpakken uit de gegevensbronnen die we registreren?

We werken actief om uit te vouwen van de mogelijkheden van Azure-gegevenscatalogus. Als er extra metagegevens die u wilt zien opgehaalde uit de gegevensbron tijdens de registratie, neemt u aangeraden deze (of stem voor deze als deze is al voorgesteld) in het [forum van Azure-gegevenscatalogus](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). In de toekomst mag derden nieuwe typen gegevensbronnen via een uitbreiding API toevoegen.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>V: hoe ik de zichtbaarheid van geregistreerde gegevens activa, beperken, zodat alleen bepaalde personen deze kunnen vinden?

A: selecteert u de activa van de gegevens in de catalogus met Azure-gegevens en klik op de knop 'Eigenaar'. Eigenaren van activa van de gegevens in de gegevenscatalogus Azure kunnen wijzigen van de instellingen van de zichtbaarheid, klikt u op dat alle catalogus gebruikers te vinden van de eigendom activa of de zichtbaarheid beperken tot specifieke gebruikers.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>V: hoe werk ik de registratie voor een activum gegevens naar die wijzigingen in de gegevensbron worden doorgevoerd in de catalogus?

A: als u de metagegevens voor gegevens activa die al zijn geregistreerd in de catalogus bijwerken, gewoon opnieuw te registreren de gegevensbron met de activa. Wijzigingen in de gegevensbron, zoals kolommen wordt toegevoegd of verwijderd uit de tabellen of weergaven, wordt bijgewerkt in de catalogus, maar alle aantekeningen die door gebruikers worden onderhouden.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>V: mijn vraag hier niet wordt beantwoord: wat moet ik doen?

Hoofd op boven aan het [forum van Azure-gegevenscatalogus](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Er vragen vindt u hier hun manier.
