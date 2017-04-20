<properties
    pageTitle="Azure leuke opslag voor BLOB's | Microsoft Azure"
    description="Opslag niveaus voor Azure-blobopslag bieden kosten-efficiënte opslag voor gegevens van het object op basis van access patronen. De laag leuke opslag is geoptimaliseerd voor gegevens die minder vaak wordt geopend."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure-blobopslag: Stijgen en cool opslag lagen

## <a name="overview"></a>Overzicht

Azure opslagruimte biedt nu twee opslag lagen voor blobopslag (object opslag), zodat u uw gegevens meest voordelig afhankelijk van hoe u deze gebruiken kunt opslaan. De Azure **warm opslag laag** is geoptimaliseerd voor het opslaan van gegevens die vaak wordt geopend. De Azure **leuke opslag laag** is geoptimaliseerd voor het opslaan van gegevens die niet vaak worden geopend en lange levensduur. Gegevens in de laag leuke opslag mogelijk zonder een iets lager beschikbaarheid, maar nog steeds uitgerust met hoge levensduur en vergelijkbare tijd voor toegang tot en doorvoer kenmerken als warm gegevens. Voor indrukwekkende gegevens zijn iets lager beschikbaarheid SERVICEOVEREENKOMST en hoger access kosten aanvaardbaar voor-en nadelen voor veel lagere kosten voor de opslag.

Tegenwoordig kunnen groeit een exponentiële tempo gegevens die zijn opgeslagen in de cloud. Als u wilt beheren kosten voor uw groeiende opslagbehoeften, is het handig zijn uw gegevens op basis van kenmerken zoals frequentie van dialoogvenster toegang en geplande bewaarperiode ordenen. Gegevens die zijn opgeslagen in de cloud kan afwijken helemaal wat betreft hoe deze wordt gegenereerd, verwerkt, en via de levensduur. Sommige gegevens is actief geopend en is gewijzigd tijdens de verdere levensduur. Sommige gegevens is zeer vaak vroeg in de levensduur, toegankelijk met access neer te zetten aanzienlijk als de leeftijd gegevens. Sommige gegevens niet actief geweest blijft in de cloud en zelden is, als ooit, één keer hebt geopend dat is opgeslagen.

Elk van deze voordelen van een gesplitste laag van opslag die is geoptimaliseerd voor een bepaald access-patroon hierboven access scenario's. Door de introductie van warm- en leuke opslag niveaus zijn afzonderlijk Azure Blob storage adressen nu deze nodig voor gesplitste opslag lagen met modellen prijzen.

## <a name="blob-storage-accounts"></a>BLOB storage-accounts

**BLOB storage accounts** zijn gespecialiseerde opslagruimte voor uw ongestructureerde gegevens opslaat als BLOB's (objects) in de opslagruimte van de Azure-mailaccounts. Met Blob storage accounts, kunt u nu kiezen tussen warm- en leuke opslag lagen voor de opslag van uw minder vaak gebruikt leuke gegevens bij de opslag van een lagere kosten en meer veelgebruikte warm gegevens aan een lagere toegang krijgen tot het kosten op te slaan. BLOB storage accounts zijn vergelijkbaar met uw bestaande algemene opslag-accounts en de effectieve levensduur, beschikbaarheid, schaalbaarheid en prestatiefuncties die u vandaag gebruikt, inclusief consistentie van 100%-API voor blok BLOB's delen en BLOB's toevoegen.

> [AZURE.NOTE] BLOB storage accounts ondersteunen alleen blokkeren en BLOB's en BLOB niet pagina's toevoegen.

BLOB storage accounts laten zien het kenmerk **Access laag** , waarmee u de laag opslag opgeven als **warm** of **leuke** afhankelijk van de gegevens die zijn opgeslagen in het account. Als er een wijziging in het gebruikspatroon van uw gegevens, kunt u ook schakelen tussen deze lagen opslagruimte op elk gewenst moment.

> [AZURE.NOTE] Wijzigen van de laag opslag, kan dit leiden tot extra kosten. Zie de sectie [prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.

Voorbeeld van gebruik scenario's voor de laag warm opslag zijn:

- Gegevens die in gebruik is of moet worden geopend (van gelezen en geschreven naar) vaak.
- Gegevens die is gefaseerde voor verwerking en eventuele migratie naar de laag leuke opslag.

Voorbeeld van gebruik scenario's voor de laag leuke opslag zijn:

- Back-up, archivering en noodgevallen herstel gegevenssets.
- Oudere media-inhoud vaak meer weergegeven niet, maar kan worden gecontroleerd bij toegankelijk direct naar verwachting.
- Grote gegevenssets die worden opgeslagen kosten effectief moeten terwijl er meer gegevens worden verzameld voor toekomstige verwerking. (*bijvoorbeeld*langdurige opslag van wetenschappelijke gegevens, onbewerkte telemetriegegevens van een fabriek)
- Oorspronkelijke (onbewerkte) gegevens die moeten worden behouden, zelfs nadat deze is verwerkt in bruikbare eindvorm. (*bijvoorbeeld*onbewerkte media-bestanden na transcodering in andere indelingen)
- Naleving en archivering gegevens die moet worden opgeslagen voor erg lang en weinig wordt geopend. (*bijvoorbeeld*, beveiliging camera-opnamen, oude X-Rays/MRIs voor gezondheidszorg organisaties, audio-opnamen en transcripties van klant oproepen voor financiële services)

Zie [over Azure opslag accounts](storage-create-storage-account.md) voor meer informatie over de opslag-accounts.

Voor toepassingen waarbij alleen blokkeren of blobopslag toevoegen, wordt u aangeraden Blob storage-accounts gebruiken om te profiteren van de gesplitste prijzen model van gelaagde opslag. Maar wij begrijpen dat dit mogelijk niet onder bepaalde omstandigheden als via algemene opslag accounts de manier om te gaan zijn zou, zoals:

- U moet werken met tabellen, wachtrijen of bestanden en wilt uw BLOB's die zijn opgeslagen in hetzelfde opslag-account. Let erop dat er geen technische voordeel van het opslaan van deze in hetzelfde account dan met dezelfde is gedeeld toetsen.
- Nog steeds moet u het model Klassiek implementatie gebruiken. BLOB storage accounts zijn alleen beschikbaar via het implementatiemodel resourcemanager Azure.
- U moet gebruiken BLOB van pagina's. BLOB storage accounts ondersteund BLOB van pagina's niet. In het algemeen het beste blokkeren BLOB's gebruiken, tenzij u een specifieke dat u een pagina BLOB's hebt.
- U een versie van de [Opslagruimte Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) die ouder is dan 2014-02-14 of een clientbibliotheek gebruiken met een versie die lager zijn dan 4.x en kan de toepassing niet bijwerken.

> [AZURE.NOTE] BLOB storage accounts worden momenteel ondersteund in een meeste Azure regio's met meer wilt volgen. U vindt de bijgewerkte lijst met beschikbare regio's op de pagina [Services Azure per regio](https://azure.microsoft.com/regions/#services) .

## <a name="comparison-between-the-storage-tiers"></a>Vergelijking tussen de lagen opslag

De volgende tabel worden de vergelijking tussen de twee opslag lagen gemarkeerd:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Warm opslag laag</center></strong></td>
    <td><strong><center>Leuke opslag laag</center></strong></td
</tr>
<tr>
    <td><strong><center>Beschikbaarheid</center></strong></td>
    <td><center>99,9%</center></td>
    <td><center>99%</center></td>
</tr>
<tr>
    <td><strong><center>Beschikbaarheid<br>(AB-GRS leest)</center></strong></td>
    <td><center>99,99%</center></td>
    <td><center>99,9%</center></td>
</tr>
<tr>
    <td><strong><center>Gebruikskosten</center></strong></td>
    <td><center>Hogere kosten voor de opslag<br>Lagere kosten voor access en transactie</center></td>
    <td><center>Lagere kosten voor de opslag<br>Hogere kosten voor toegang en transactie</center></td>
</tr>
<tr>
    <td><strong><center>Grootte van minimum aantal objecten<center></strong></td>
    <td colspan="2"><center>N/B</center></td>
</tr>
<tr>
    <td><strong><center>Minimale opslagruimte duur<center></strong></td>
    <td colspan="2"><center>N/B</center></td>
</tr>
<tr>
    <td><strong><center>Latentie<br>(Time to eerste byte)<center></strong></td>
    <td colspan="2"><center>milliseconden</center></td>
</tr>
<tr>
    <td><strong><center>Schaalbaarheid en prestaties doelen<center></strong></td>
    <td colspan="2"><center>Komt overeen algemene opslag-accounts</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] BLOB storage accounts ondersteunt de dezelfde prestaties en schaalbaarheid doelen als algemene opslag-accounts. Zie [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md) voor meer informatie.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Een nieuw prijzen model BLOB storage accounts gebruiken voor op basis van de laag opslag-blobopslag. Wanneer een Blob storage-account gebruikt, wordt de volgende aandachtspunten voor de facturering toepassen:

- **Kosten voor opslag**: naast de hoeveelheid gegevens die zijn opgeslagen, worden de kosten van het opslaan van gegevens varieert afhankelijk van de laag opslag. De kosten per gigabyte is lagere voor de laag leuke opslagruimte dan voor de laag warm opslag.
- **Data access-kosten**: voor gegevens in de laag leuke opslag, moet u een boete voor toegang tot gegevens per gigabyte betalen voor lezen en schrijven.
- **Transactiekosten**: Er is een kosten per transactie voor beide lagen. De kosten per transactie voor de laag leuke opslag is echter hoger zijn dan die voor de laag warm opslag.
- **Geografische herhaling gegevens overbrengen kosten**: dit is alleen van toepassing op accounts met geografische-herhaling geconfigureerd, inclusief GRS en AB-GRS. Geografische herhaling gegevens doorverbinden bijhoudt kosten per gigabyte.
- **Uitgaande gegevens kosten overbrengen**: uitgaande overdracht (gegevens die worden overgebracht afmelden bij een Azure regio) oplopen facturering voor bandbreedtegebruik op per gigabyte basis, consistent zijn met algemene opslag-accounts.
- **Wijzigen van de laag opslag**: wijzigen van de laag opslag van fraaie naar direct, wordt een boete gelijk is aan het lezen van alle gegevens in het account opslag voor elke overgang bestaande oplopen. Aan de andere kant, te wijzigen in de laag opslag van warm fraaie te betalen van de kosten.

> [AZURE.NOTE] Als u wilt toestaan dat gebruikers kunnen de nieuwe opslag lagen uitproberen en functionaliteit bericht starten, de kosten voor het wijzigen van de opslag valideren laag uit fraaie naar direct uitschakelen doorloopt tot 30 juni-2016. Begin juli 1e 2016 en wordt de kosten toegepast op alle overgangen vanuit fraaie naar direct. Voor meer informatie over het model prijzen voor Blob storage accounts, [Prijzen van Azure-opslag](https://azure.microsoft.com/pricing/details/storage/) pagina weergegeven. Voor meer informatie over de uitgaande gegevens doorverbinden kosten, [Prijzen detailgegevens overdrachten](https://azure.microsoft.com/pricing/details/data-transfers/) pagina weergegeven.

## <a name="quick-start"></a>Snel aan de slag

In deze sectie wordt gedemonstreerd de volgende scenario's met behulp van de Azure portal:

- Het maken van een Blob storage-account.
- Het beheren van een Blob storage-account.

### <a name="using-the-azure-portal"></a>Met behulp van de Azure portal

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Maak een Blob storage-account met behulp van de Azure portal

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).

2. Selecteer **Nieuw**op het menu Hub > **gegevens + opslagruimte** > **opslag-account**.

3. Voer een naam voor uw account opslag.

    Deze naam moet uniek; Deze worden gebruikt als onderdeel van de URL die wordt gebruikt voor toegang tot de objecten in het opslag-account.  

4. Selecteer **Resource Manager** als het implementatiemodel.

    Gelaagde opslag kan alleen worden gebruikt met resourcemanager opslag accounts; Dit is het aanbevolen implementatiemodel voor nieuwe resources. Bekijk het [overzicht van de Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)voor meer informatie.  

5. Selecteer in de vervolgkeuzelijst type Account **-Blobopslag**.

    Dit is waarin u het type account opslagruimte selecteren. Gelaagde opslag is niet beschikbaar in de algemene opslag; Dit is alleen beschikbaar in het Blob storage type account.    

    Houd er rekening mee dat wanneer u deze optie selecteert, de laag prestaties is ingesteld op standaard. Gelaagde opslag is niet beschikbaar met de Premium prestaties laag.

6. Selecteer de optie herhaling voor de opslag-account: **LRS**, **GRS**of **AB-GRS**. De standaardinstelling is **AB-GRS**.

    LRS = lokaal redundante opslag; GRS = geografische-redundante opslag (2 regio's); AB-GRS is leestoegang geografische-redundante opslag (2 regio's met alleen toegang tot de tweede).

    Voor meer informatie over replicatieopties van Azure-opslag, raadpleegt u [Azure Storage herhaling](storage-redundancy.md).

7. Selecteer de laag rechts opslagruimte voor uw behoeften: de **Access-laag** ingesteld op **leuke** of **warm**. De standaardinstelling is **warm**.

8. Selecteer het abonnement waarin u wilt maken van het nieuwe opslag-account.

9. Een nieuwe resourcegroep opgeven of Selecteer een bestaande resourcegroep. Zie [overzicht van de Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)voor meer informatie over resourcegroepen.

10. Selecteer het gebied voor uw account opslag.

11. Klik op **maken** om de opslag-account maken.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>De laag opslag van een Blob storage-account met behulp van de Azure portal wijzigen

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).

2. Naar uw account opslagruimte wilt gaan, selecteert u alle Resources en vervolgens uw opslag-account.

3. Klik in het blad instellingen op **configuratie** als u wilt weergeven en/of de accountconfiguratie te wijzigen.

4. Selecteer de laag rechts opslagruimte voor uw behoeften: de **Access-laag** ingesteld op **leuke** of **warm**.

5. Klik op opslaan aan de bovenkant van het blad.

> [AZURE.NOTE] Wijzigen van de laag opslag, kan dit leiden tot extra kosten. Zie de sectie [prijzen en facturering](storage-blob-storage-tiers.md#pricing-and-billing) voor meer informatie.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Evaluatie van en migreren naar Blob storage-accounts

Het doel van dit gedeelte is om gebruikers om te maken van een soepele overgang naar via Blob storage accounts te helpen. Er zijn twee scenario's van gebruikers:

- U hebt een bestaand algemene opslag-account en wilt evalueren van een wijziging in een Blob storage-account met de juiste opslag laag.
- U hebt besloten een Blob storage-account of al hebt en wilt evalueren of u de laag warm of leuke opslag moet gebruiken.

In beide gevallen is het eerste volgorde van bedrijf schatten van de kosten van opslaan en toegang tot uw gegevens die zijn opgeslagen in een Blob storage-account en vergelijk deze met de huidige kosten.

### <a name="evaluating-blob-storage-account-tiers"></a>Evaluatie van Blob storage account lagen

Om te kunnen schatten van de kosten van opslaan en toegang tot gegevens die zijn opgeslagen in een Blob storage-account, moet u naar uw bestaande gebruikspatroon beoordelen of het gebruikspatroon Verwachte benaderen. U wilt in het algemeen, weten:

- Uw opslag verbruik - hoe veel gegevens worden opgeslagen en hoe verandert dit de maandelijks managementrapporten?
- Uw opslag access patroon - hoeveel gegevens wordt lezen van en naar het account (inclusief de nieuwe gegevens) geschreven? Hoeveel transacties worden gebruikt voor toegang tot gegevens en welke soorten transacties zijn ze?

#### <a name="monitoring-existing-storage-accounts"></a>Cmdlets voor controle bestaande opslag-accounts

U kunt om te controleren van uw bestaande accounts voor de opslag en deze gegevens verzamelen, maken gebruik van Azure opslag Analytics die worden uitgevoerd logboekregistratie en gegevens van de doelstellingen voor een account opslagruimte biedt.
Opslag Analytics kunt aan de doelstellingen met geaggregeerde transactie statistieken en capaciteit gegevens over aanvragen voor de Blob storage-service voor zowel algemene opslag accounts als Blob storage accounts opslaan.
Deze gegevens zijn opgeslagen in bekende tabellen in hetzelfde opslag-account.

Voor meer informatie raadpleegt u [Over Analytics opslageenheden](https://msdn.microsoft.com/library/azure/hh343258.aspx) en [Opslag Analytics aan de doelstellingen tabel schemabestanden](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] BLOB storage accounts laten zien het eindpunt van de service tabel alleen voor het opslaan en toegang tot de gegevens aan de doelstellingen voor dat account.

Als u wilt controleren het verbruik opslag voor de Blob storage-service, moet u de doelstellingen van de capaciteit inschakelen.
Met deze ingeschakeld, wordt capaciteit dagelijkse voor een opslag-account Blob service, en opgenomen als de invoer in een tabel die is geschreven aan de tabel *$MetricsCapacityBlob* in hetzelfde account opslag geregistreerd.

Als u wilt controleren het patroon dat in access gegevens voor de Blob storage-service, moet u de doelstellingen van de per uur transactie op een niveau API inschakelen.
Met deze ingeschakeld, per API zijn transacties samengevoegd per uur en opgenomen als de invoer in een tabel die is geschreven aan de tabel *$MetricsHourPrimaryTransactionsBlob* in hetzelfde account opslag. De tabel *$MetricsHourSecondaryTransactionsBlob* records de transacties naar het secundaire eindpunt voor het geval AB-GRS opslag-accounts.

> [AZURE.NOTE] Als u een algemene opslag-account waarin u hebt opgeslagen BLOB van pagina's en VM schijven samen met blokkeren en blobgegevens toevoegen hebt, is dit proces schatting niet van toepassing. Dit is omdat er geen manier van opvallende capaciteit en op basis van het type blob voor alleen aan de doelstellingen transactie blokkeren en BLOB's die kunnen worden gemigreerd naar een Blob storage-account toevoegen.

Als u een goede benadering van u gegevens verbruik en access patroon, raden u kiest u een bewaarperiode voor de criteria die is een voorbeeld van uw normaal gebruik en afleiden.
Eén optie is om te bewaren van de gegevens aan de doelstellingen zeven dagen en de gegevens verzamelen elke week voor analyse aan het einde van de maand.
Er is een andere optie voor het bewaren van de gegevens aan de doelstellingen voor de afgelopen 30 dagen en verzamelen en de gegevens analyseren aan het einde van de periode van 30 dagen.

Voor meer informatie over het inschakelen van Zie verzamelen en weergeven van gegevens aan de doelstellingen, [Azure-opslageenheden inschakelen en weergeven van gegevens aan de doelstellingen](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Opslaan, openen en downloaden analytics-gegevens is ook net als normale gebruikersgegevens betalen.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Gebruik van gebruik aan de doelstellingen voor het schatten van kosten

##### <a name="storage-costs"></a>Kosten voor opslag

De meest recente vermelding in de capaciteit aan de doelstellingen tabel *$MetricsCapacityBlob* met de rij key *'gegevens'* ziet u de opslagcapaciteit dat door de gebruikersgegevens.
De meest recente vermelding in de capaciteit aan de doelstellingen tabel *$MetricsCapacityBlob* met de rij key *'analytics'* ziet u de opslagcapaciteit dat door de logboeken analytics.

Dit totale capaciteit dat door beide gebruikersgegevens en analyses Logboeken (indien ingeschakeld) kunnen vervolgens worden gebruikt voor het schatten van de kosten van het opslaan van gegevens in de opslagruimte-account.
Dezelfde methode kan ook worden gebruikt voor het schatten van opslagkosten voor blokkeren en BLOB's in de algemene opslag-accounts toevoegen.

##### <a name="transaction-costs"></a>Transactiekosten

De som van *'TotalBillableRequests'*, in alle items voor een API in de transactietabel aan de doelstellingen wordt aangegeven dat het totale aantal transacties voor dat bepaalde API. *bijvoorbeeld*het totale aantal *'GetBlob'* transacties in een bepaalde termijn kan worden berekend door de som van totale factureerbare aanvragen voor alle items met de toets rij *' gebruiker. GetBlob'*.

Om te kunnen schatten transactiekosten voor Blob storage-accounts, moet u Splits op de transacties in drie groepen, aangezien ze anders prijs.

- Schrijf transacties zoals *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'*en *'CopyBlob'*.
- Transacties zoals *'DeleteBlob'* en *'DeleteContainer'*verwijderen.
- Alle andere transacties.

Om te kunnen schatten transactiekosten voor algemene opslag-accounts, moet u alle transacties ongeacht de bewerking/API aggregeren.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Data access en geografische herhaling gegevens overbrengen kosten

Terwijl opslag analytics geen de hoeveelheid gegevens lezen en geschreven met een opslag-account, kan het ongeveer door te kijken in de transactietabel aan de doelstellingen geschatte.
De som van *'TotalIngress'* in alle items voor een API in de transactietabel aan de doelstellingen geeft de totale hoeveelheid ingress gegevens in bytes voor dat bepaalde API.
Op dezelfde manier is de som van *'TotalEgress'* geeft aan de totale hoeveelheid egress gegevens, in bytes.

Om te kunnen schatten van de kosten van de toegang tot gegevens voor Blob storage-accounts, moet u de transacties in twee groepen uitsplitsen.

- De hoeveelheid gegevens die zijn opgehaald uit de opslag-account kan worden geschat door te kijken de som van *'TotalEgress'* voor hoofdzakelijk de bewerkingen *'GetBlob'* en *'CopyBlob'* .
- De hoeveelheid gegevens die worden geschreven naar het opslag-account kan worden geschat door te kijken in de som van *'TotalIngress'* voor voornamelijk *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* en *'AppendBlock'* bewerkingen.

De kosten van de overdracht van gegevens geografische herhaling voor Blob storage accounts kan ook worden berekend met behulp van de schatting voor de hoeveelheid gegevens die zijn geschreven voor het geval een GRS of AB-GRS opslag-account.

> [AZURE.NOTE] Neem een Kennismaking met de veelgestelde vragen over het getiteld *"wat warm en fraaie access lagen zijn en hoe moet ik bepalen welk lettertype moet use?"* bijvoorbeeld een meer gedetailleerde informatie over het berekenen van de kosten voor het gebruik van de laag warm of leuke opslag in de [Azure opslag prijzen van de pagina](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Migreren van bestaande gegevens

Een Blob storage-account is speciaal voor het opslaan van alleen blokkeren en BLOB's toevoegen. Bestaande accounts voor algemene opslag, waarmee u tabellen, wachtrijen, bestanden en schijven, evenals BLOB's wordt opgeslagen, kunnen niet worden geconverteerd naar Blob storage-accounts. Als u wilt gebruiken voor de opslag niveaus, moet u maken van nieuwe Blob storage accounts en migreren van uw bestaande gegevens naar de nieuwe accounts.
U kunt de volgende methoden gebruiken om te migreren van bestaande gegevens naar Blob storage accounts vanaf on-premises opslagapparaten, van derden cloud opslag providers of van uw bestaande algemene opslag-accounts in Azure wordt aangegeven:

#### <a name="azcopy"></a>AzCopy

AzCopy is een opdrachtregel hulpprogramma ontworpen voor krachtige kopiëren van gegevens naar en vanuit Azure opslag van Windows. U kunt AzCopy gebruiken om gegevens te kopiëren naar uw Blob storage-account van uw bestaande algemene opslag-accounts of voor het uploaden van gegevens uit uw on-premises implementatie opslag-apparaten in uw Blob storage-account.

Zie [overbrengen van gegevens met het hulpprogramma AzCopy-opdrachtregelopties](storage-use-azcopy.md)voor meer informatie.

#### <a name="data-movement-library"></a>Gegevens verkeer bibliotheek

Azure opslag gegevens verkeer bibliotheek voor .NET is gebaseerd op de core gegevens verkeer framework die AzCopy bevoegdheden. De bibliotheek is bedoeld voor krachtige, betrouwbare en eenvoudig doorverbinden gegevensbewerkingen vergelijkbaar met AzCopy. Hiermee kunt u de voordelen van de functies van AzCopy in uw toepassing native zonder te handelen uitgevoerd en externe exemplaren van AzCopy cmdlets voor controle.

Zie voor meer informatie, [Azure opslag gegevens verkeer bibliotheek voor .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST API of clientbibliotheek

U kunt een maatwerktoepassing op uw gegevens migreren naar een Blob storage-account met behulp van een van de Azure clientbibliotheken of de opslagservices Azure REST API maken. Azure opslagruimte biedt rijke bibliotheken voor meerdere talen en platforms zoals .NET, Java, C++, Node.JS, PHP, Ruby en Python. De clientbibliotheken bieden geavanceerde mogelijkheden zoals opnieuw logica, logboekregistratie en parallelle uploads. U kunt ook ontwikkelen rechtstreeks tegen de REST API, die kan worden aangeroepen door een taal die HTTP/HTTPS-verzoeken maakt.

Zie [Aan de slag met Azure-blobopslag](storage-dotnet-how-to-use-blobs.md)voor meer informatie.

> [AZURE.NOTE] BLOB's die zijn versleuteld met aan de clientzijde versleuteling opslaan versleuteling-gerelateerde metagegevens die zijn opgeslagen met de blob. Het is absoluut kritieke dat welke manier kopie ervoor zorgen moet dat de blob-metagegevens en met name de versleuteling-gerelateerde metagegevens, blijft behouden. Als u de BLOB's zonder deze metagegevens kopieert, de blob inhoud wordt niet opvraagbare opnieuw. Zie [Azure Storage client kant codering](storage-client-side-encryption.md)voor meer informatie aangaande metagegevens met betrekking tot versleuteling.

## <a name="faqs"></a>Veelgestelde vragen

1. **Bestaande accounts van opslag nog steeds beschikbaar?**

    Ja, bestaande opslag-accounts zijn nog steeds beschikbaar en in de prijs of functionaliteit blijven ongewijzigd.  Ze niet de mogelijkheid om te kiezen een laag opslagruimte hebben en hebben geen trapsgewijs mogelijkheden in de toekomst.

2. **Waarom en wanneer moet ik beginnen via Blob storage accounts?**

    BLOB storage accounts voor het opslaan van BLOB's zijn gespecialiseerd en kunnen we introduceren nieuwe blob centraal functies. In de toekomst zullen Blob storage accounts zijn de beste manier voor het opslaan van BLOB's, als toekomstige mogelijkheden zoals hiërarchische opslag en trapsgewijs schakelen geïntroduceerd op basis van dit accounttype. Het is echter tot u wanneer u wilt migreren op basis van uw vereisten voor bedrijven.

3. **Kan ik mijn bestaande opslag-account omzetten in een Blob storage-account?**

    Nee. BLOB storage-account is een ander soort opslag-account en moet u deze nieuwe hebt gemaakt en migreren van uw gegevens: Zie uitleg hierboven.

4. **Kan ik objecten opslaan in beide lagen opslagruimte in hetzelfde account?**

    Het kenmerk *' Access laag '* waarmee wordt aangegeven de laag opslag is ingesteld op het niveau van een account en is van toepassing op alle objecten in dat account. U kunt het kenmerk van de laag access niet instellen op het objectniveau van een.

5. **Kan ik de laag opslag van mijn Blob storage-account wijzigen?**

    Ja. U kunnen de laag opslagruimte wijzigen door het instellen van het kenmerk *' Access laag '* op het account opslag. Wijzigen van de laag opslag van toepassing op alle objecten die zijn opgeslagen in het account. De laag opslag van warm naar fraaie niet de eventuele tarieven, gelden wordt tijdens het wijzigen van fraaie naar direct wijzigen wiens rekening een per GB kosten voor het lezen van alle gegevens in het account.

6. **Hoe vaak kan ik de laag opslag van mijn Blob storage-account wijzigen?**

    Terwijl we niet een beperking op hoe vaak de laag opslag kan worden gewijzigd afdwingen, zorg ervoor dat het wijzigen van de laag opslag van fraaie naar direct aanzienlijk kosten wordt oplopen. We raden niet vaak wijzigen van de laag opslag.

7. **Wordt het BLOB's in de laag leuke opslag gedragen zich anders dan de lettertypen in de laag warm opslagruimte?**

    BLOB's in de laag warm opslagruimte hebben de dezelfde latentie als BLOB's in de algemene opslag-accounts. BLOB's in de laag leuke opslagruimte hebben een soortgelijke latentie (in milliseconden) als BLOB's in de algemene opslag-accounts.

    BLOB's in de laag leuke opslag heeft een iets lager beschikbaarheid serviceniveau (SLA) dan de BLOB's die zijn opgeslagen in de laag warm opslag. Zie voor meer informatie, [SLA voor opslag](https://azure.microsoft.com/support/legal/sla/storage).

8. **Kan ik BLOB van pagina's en VM schijven opslaan in Blob storage accounts?**

    BLOB storage accounts ondersteunen alleen blokkeren en BLOB's en BLOB niet pagina's toevoegen. Azure virtuele machines schijven worden ondersteund door BLOB van pagina's en daardoor Blob storage accounts kunnen niet worden gebruikt om op te slaan VM schijven. Het is echter mogelijk om op te slaan back-ups van de schijven VM als blok BLOB's in een Blob storage-account.

9. **Moet ik mijn bestaande toepassingen gebruikt Blob storage accounts wijzigen?**

    BLOB storage accounts zijn 100% API consistent zijn met algemene opslag-accounts voor blokkeren en BLOB's toevoegen. Als uw toepassing is met BLOB's blokkeren of BLOB's, toevoegen en u de 2014-02-14-versie van de [Opslagruimte Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) of groter is uw toepassing moet werken. Als u een oudere versie van het protocol gebruikt, moet u uw toepassing voor het gebruik van de nieuwe versie kunnen werken naadloos samen met beide typen opslag accounts bijwerken. In het algemeen, we altijd de meest recente versie ongeacht welk account opslagruimte met aanbevolen u typt gebruiken.

10. **Is er een wijziging in gebruikerservaring?**

    BLOB storage accounts zijn vergelijkbaar met een algemene opslag-accounts voor het opslaan van blokkeren en BLOB's toevoegen en ondersteuning voor de belangrijkste functies van Azure-opslag, inclusief hoog levensduur en beschikbaarheid, schaalbaarheid, prestaties en beveiliging. Dan de functies en beperkingen die specifiek zijn voor Blob storage accounts en de opslag-lagen die zijn sectienummers bovenstaande rest blijft ongewijzigd.

## <a name="next-steps"></a>Volgende stappen

### <a name="evaluate-blob-storage-accounts"></a>Evalueren Blob storage-accounts

[Beschikbaarheid controleren Blob storage accounts per regio](https://azure.microsoft.com/regions/#services)

[Gebruik van uw huidige opslag-accounts doordat Azure opslageenheden evalueren](storage-enable-and-view-metrics.md)

[Controleer Blob storage prijzen per regio](https://azure.microsoft.com/pricing/details/storage/)

[Selectievakje overdracht prijzen](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Aan de slag met Blob storage-accounts

[Aan de slag met Azure-blobopslag](storage-dotnet-how-to-use-blobs.md)

[Gegevens verplaatsen naar en van Azure-opslag](storage-moving-data.md)

[Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md)

[Blader en uw accounts opslag verkennen](http://storageexplorer.com/)
