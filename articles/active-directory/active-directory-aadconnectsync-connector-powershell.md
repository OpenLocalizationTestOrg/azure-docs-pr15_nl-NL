<properties
   pageTitle="PowerShell verbindingslijn | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe van Microsoft Windows PowerShell-Connector configureren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="windows-powershell-connector-technical-reference"></a>Windows PowerShell-Connector technische verwijzing
In dit artikel worden de Windows PowerShell-Connector. Het artikel is van toepassing op de volgende producten:

- Microsoft identiteitsbeheer 2016 (MIM2016)
- Forefront identiteit Manager 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De verbindingslijn is voor MIM2016 en FIM2010R2, een downloaden via het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Overzicht van de PowerShell-Connector
De verbindingslijn PowerShell kunt u de synchronisatieservice integreren met externe systemen die aanbieden dat API 's op basis van Windows PowerShell. De verbindingslijn biedt een brug tussen de mogelijkheden van de oproep gebaseerde extensible connectivity management-agent 2 (ECMA2) framework en Windows PowerShell. Voor meer informatie over het kader ECMA, raadpleegt u [Extensible Connectivity 2.2 Management Agent verwijzing](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Vereisten voor
Voordat u de verbindingslijn gebruiken, te controleren of u de volgende handelingen uit op de synchronisatieserver:

- 4.5.2 van Microsoft .NET Framework of hoger
- Windows PowerShell 2.0, 3.0 of 4.0

Het beleid kan worden uitgevoerd op de server synchronisatieservice moet worden geconfigureerd toestaan de verbindingslijn met Windows PowerShell-scripts worden uitgevoerd. Tenzij de scripts de verbindingslijn wordt uitgevoerd, zijn digitaal ondertekend, configureert het beleid kan worden uitgevoerd door deze opdracht uit te voeren:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken
Als u wilt een Windows PowerShell-connector in de synchronisatieservice maakt, moet u een reeks van Windows PowerShell-scripts die de aangevraagd door de synchronisatieservice stappen worden uitgevoerd opgeven. Afhankelijk van de gegevensbron waarmee u verbinding met maakt en de functionaliteit die u nodig hebt, moet u implementeren scripts varieert. In deze sectie worden elk van de scripts die kan worden geïmplementeerd en wanneer ze zijn vereist.

De Windows PowerShell-verbindingslijn is bedoeld voor de opslag van elk van de scripts binnen de synchronisatie-database. Tijdens het uitvoeren van scripts die zijn opgeslagen in het bestandssysteem mogelijk is, is het eenvoudiger om in te voegen van de hoofdtekst van elke script rechtstreeks in om de configuratie van de verbindingslijn.

Als u wilt maken van een verbindingslijn PowerShell, selecteert u in **Synchronisatieservice** **Management Agent** en **maken**. Selecteer de verbindingslijn **PowerShell (Microsoft)** .

![Verbindingslijn maken](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
Van configuratieparameters om verbinding te maken met een extern systeem opgeven. Deze waarden worden veilig opgeslagen door de synchronisatie-Service en beschikbaar zijn op uw Windows PowerShell-scripts worden gemaakt wanneer de verbindingslijn wordt uitgevoerd.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

U kunt de volgende Connectivity parameters configureren:

**Connectiviteit**

Parameter | Standaardwaarde | Doel
--- | --- | ---
Server | <Blank> | De servernaam die de verbindingslijn verbinding met maken moet.
Domein | <Blank> | Domein van de referentie voor het opslaan voor gebruik bij de verbindingslijn wordt uitgevoerd.
Gebruiker | <Blank> | De gebruikersnaam van de referentie voor het opslaan voor gebruik bij de verbindingslijn wordt uitgevoerd.
Wachtwoord | <Blank> | Wachtwoord van de referentie voor het opslaan voor gebruik bij de verbindingslijn wordt uitgevoerd.
Nabootsen Connector-Account | ONWAAR | Waar is, wordt de Windows PowerShell-scripts in de synchronisatieservice uitgevoerd in de context van de referenties. Indien mogelijk, het wordt aanbevolen dat de parameter **$Credentials** wordt doorgegeven toe aan elk script wordt gebruikt in plaats van imitatie. Voor meer informatie over aanvullende machtigingen die vereist zijn voor u deze optie gebruikt, raadpleegt u [Aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation).
Gebruikersprofiel laden als u imitatie | ONWAAR | Hiermee geeft u Windows het gebruikersprofiel van de verbindingslijn referenties laden tijdens de imitatie. Als de nagebootste gebruiker een roaming-profiel heeft, de verbindingslijn het roaming profiel niet geladen. Voor meer informatie over aanvullende machtigingen die vereist zijn voor deze parameter gebruiken, raadpleegt u [Aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation).
Aanmeldingstype als u imitatie | Geen | Aanmeldingstype tijdens de imitatie. Voor meer informatie raadpleegt u de [dwLogonType] [ dw] documentatie.
Alleen ondertekende Scripts | ONWAAR | Als dit waar is, is de Windows PowerShell-connector gevalideerd dat elk script een geldige digitale handtekening heeft. Als de waarde false, zorgen ervoor dat de synchronisatieservice van Windows PowerShell execution serverbeleid RemoteSigned of onbeperkt.

**Algemene Module**  
De verbindingslijn kunt u een gedeelde Windows PowerShell-module opgeslagen in de configuratie. Wanneer de verbindingslijn een script uitvoert, wordt de Windows PowerShell-module uitgepakt naar het bestandssysteem, zodat deze kan worden geïmporteerd door elke script.

De algemene module wordt voor importeren, exporteren en Wachtwoordsynchronisatie scripts, naar de map van connector MAData uitgepakt. Voor discovery-scripts, Schema, validatie hiërarchie en Partition, wordt de algemene module geëxtraheerd naar de map % TEMP %. In beide gevallen heet het opgehaalde algemene Module-script op basis van de Module Script eigennaam-instelling.

Als u wilt een module FIMPowerShellConnectorModule.psm1 aangeroepen vanuit de map MAData hebt geladen, moet u de volgende instructie gebruiken:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Als u wilt een module FIMPowerShellConnectorModule.psm1 aangeroepen vanuit de map % TEMP % hebt geladen, moet u de volgende instructie gebruiken:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Parameter gegevensvalidatie**  
Het Script gegevensvalidatie is een optionele Windows PowerShell-script die kan worden gebruikt om ervoor te zorgen dat de verbindingslijn configuratieparameters opgegeven door de beheerder geldig zijn. Valideren server, verbindingsreferenties en connectivity parameters zijn algemene vormen van gebruik van het script gegevensvalidatie. Het script gegevensvalidatie wordt aangeroepen na de volgende tabbladen en dialoogvensters worden gewijzigd:

- Connectiviteit
- Globale Parameters
- Partitieconfiguratie

Het validatiescript worden de volgende parameters van de verbindingslijn ontvangen:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Het tabblad configuratie of dialoogvenster waarvoor de aanvraag gegevensvalidatie.
ConfigParameters | [KeyedCollection] [ keyk] [tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.

Het validatiescript moet één ParameterValidationResult object terug naar de pijplijn.

**Schema Discovery**  
Het Schema Discovery-script is verplicht. Dit script geeft als resultaat de objecttypen, kenmerken en kenmerk beperkingen die de synchronisatieservice wordt gebruikt tijdens het configureren van kenmerk stroomregels. Het Schema Discovery-script tijdens het maken van een verbindingslijn wordt uitgevoerd en worden ingevuld van de verbindingslijn schema. Dit wordt ook gebruikt door de actie Schema vernieuwen in het servicebeheer van synchronisatie.

Het schema discovery-script worden de volgende parameters van de verbindingslijn ontvangen:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.

Het script moet een enkel [Schema] retourneren[ schema] object naar de pijplijn. Het object Schema bestaat uit [SchemaType] [ schemaT] objecten die objecttypen vertegenwoordigen (bijvoorbeeld: gebruikers en groepen). Het object SchemaType bevat een verzameling [SchemaAttribute] [ schemaA] objecten met daarin de kenmerken (bijvoorbeeld: voornaam, achternaam en postadres) van het type.

**Aanvullende Parameters**  
Naast de standaard configuratie-instellingen, kunt u extra aangepaste configuratie-instellingen die specifiek voor het exemplaar van de verbindingslijn zijn definiëren. U kunt deze parameters opgeven op de verbindingslijn, partition, of uitvoeren stap worden geëffend en toegankelijk vanaf het betreffende Windows PowerShell-script. Aangepaste configuratie-instellingen kunnen worden opgeslagen in de database synchronisatieservice in tekst zonder opmaak of ze mogelijk worden gecodeerd. De synchronisatieservice voor het automatisch worden gecodeerd en ontsleuteld secure configuratie-instellingen als vereist.

Als u wilt opgeven aangepaste configuratie-instellingen, scheidt u de naam van elke parameter met een komma (,).

Voor toegang tot aangepaste configuratie-instellingen van een script, moet u de naam met een onderstrepingsteken achtervoegsel ( \_ ) en het bereik van de parameter (globale, Partition of RunStep). Bijvoorbeeld, voor toegang tot de parameter globale bestandsnaam, gebruikt u deze codefragment:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Mogelijkheden
Het tabblad van de mogelijkheden van de ontwerpfunctie voor Management Agent Hiermee definieert u de functionaliteit van de verbindingslijn en gedrag. Uw selecties op dit tabblad kunnen niet worden gewijzigd wanneer de verbindingslijn is gemaakt. Deze tabel bevat de mogelijkheid-instellingen.

![Mogelijkheden](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

De mogelijkheid | Beschrijving |
--- | --- |
[DN-naam-stijl][dnstyle] | Geeft aan of de verbindingslijn DN-namen ondersteunt en als dat zo is, wat de stijl.
[Type exporteren][exportT] | Bepaalt het type objecten die wordt aangeboden aan het script exporteren. <li>AttributeReplace – bevat de volledige set waarden voor een kenmerk met meerdere waarden als het kenmerk verandert.</li><li>AttributeUpdate – bevat alleen de delta naar een kenmerk met meerdere waarden als het kenmerk verandert.</li><li>MultivaluedReferenceAttributeUpdate - bevat een volledige reeks waarden voor niet-naslag met meerdere waarden kenmerken en alleen delta voor meerdere waarden verwijzing kenmerken.</li><li>ObjectReplace – bevat alle kenmerken voor een object wanneer willekeurig wijzigingen kenmerk</li>
[Gegevens normaliseren][DataNorm] | Hiermee geeft u de synchronisatieservice te normaliseren anker kenmerken voordat ze zijn bedoeld om scripts.
[Object bevestiging][oconf] | Het gedrag in behandeling importeren configureren in de synchronisatie-Service <li>Normaal – standaardgedrag die verwacht alle geëxporteerde wijzigingen worden bevestigd via importeren</li><li>NoDeleteConfirmation – is wanneer een object wordt verwijderd, er geen in behandeling importeren die zijn gegenereerd.</li><li>NoAddAndDeleteConfirmation – is wanneer een object wordt gemaakt of verwijderd, er geen in behandeling importeren die zijn gegenereerd.</li>
DN als anker gebruiken | Als de DN naamstijl is ingesteld op LDAP, is het ankerkenmerk voor de ruimte verbindingslijn ook de DN-naam.
Gelijktijdige bewerkingen van verschillende verbindingslijnen | Wanneer ingeschakeld, kunnen dat meerdere Windows PowerShell-connectors tegelijk worden uitgevoerd.
Partities | Wanneer ingeschakeld, kunt u de verbindingslijn meerdere partities en partition discovery.
Hiërarchie | Wanneer ingeschakeld, ondersteunt de verbindingslijn een hiërarchische structuur voor LDAP-stijl.
Importeren inschakelen | Wanneer ingeschakeld, worden de verbindingslijn gegevens via scripts die te importeren.
Delta importeren inschakelen | Wanneer ingeschakeld, kan de verbindingslijn delta vraag aan de scripts importeren.
Exporteren inschakelen | Wanneer ingeschakeld, exporteert de verbindingslijn gegevens via scripts die te exporteren.
Volledige exporteren inschakelen | Wanneer ingeschakeld, ondersteuning voor het exporteren-scripts voor het exporteren van de ruimte verbindingslijn volledig. Als u deze optie gebruikt, moet inschakelen exporteren ook worden gecontroleerd.
Geen waarden van de verwijzing In eerste exporteren keer | Wanneer ingeschakeld, worden in een tweede exporteren keer verwijzing kenmerken geëxporteerd.
Object naam inschakelen | Wanneer ingeschakeld, kunnen de DN-namen worden gewijzigd.
Delete toevoegen vervangen | Wanneer ingeschakeld, delete-toevoegen bewerkingen worden geëxporteerd als één vervanging.
Wachtwoord bewerkingen inschakelen | Wanneer ingeschakeld, wordt de synchronisatie-scripts wachtwoord worden ondersteund.
Wachtwoord exporteren in de eerste keer inschakelen | Wanneer ingeschakeld, worden de wachtwoorden instellen tijdens het inrichten worden geëxporteerd als het object wordt gemaakt.

### <a name="global-parameters"></a>Globale Parameters
Het tabblad globale Parameters in de ontwerpfunctie voor Management Agent kunt u de Windows PowerShell-scripts die worden uitgevoerd door de verbindingslijn configureren. U kunt ook globale waarden voor aangepaste configuratie-instellingen die zijn gedefinieerd op het tabblad connectiviteit configureren.

**Partition Discovery**  
Een partition is een aparte naamruimte binnen één gedeelde schema. Bijvoorbeeld, is elk domein in Active Directory een partition binnen één bos. Een partition is de logische groepering voor importeren en exporteren. Importeren / exporteren heeft partition Als een context en alle bewerkingen gebeurt er in deze context. Partities moeten om aan te geven van een hiërarchie in LDAP. De DN-naam van een partition wordt gebruikt in importeren en controleer of alle geretourneerde objecten binnen het bereik van een partition. De partition DN-naam wordt ook gebruikt tijdens het inrichten van de metaverse naar de ruimte verbindingslijn om te bepalen de partition die een object gekoppeld aan tijdens het exporteren worden moet.

Het partition discovery-script worden de volgende parameters van de verbindingslijn ontvangen:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.

Het script moet retourneren een ofwel één [Partition] [ part] object of een lijst [T] met partitieobjecten op de pijplijn.

**Hiërarchie Discovery**  
De hiërarchie discovery-script wordt alleen gebruikt als de mogelijkheid DN naamstijl LDAP. Het script wordt is toegestaan om te bladeren en selecteer een reeks containers die wordt beschouwd als in- of uitzoomen bereik voor importeren en exporteren bewerkingen gebruikt. Het script moet alleen geven voor een lijst met knooppunten die directe onderliggende van het knooppunt van de hoofdmap is in het script ingevoerd elementen.

De hiërarchie discovery-script worden de volgende parameters van de verbindingslijn ontvangen:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
ParentNode | [HierarchyNode][hn] | Het knooppunt van de hoofdsite van de hiërarchie waaronder het script directe kinderen moet worden geretourneerd.

Het script moet terugkeren een hetzij een enkel onderliggend HierarchyNode-object of een lijst [T] met onderliggende HierarchyNode objecten naar de pijplijn.

#### <a name="import"></a>Importeren
Verbindingslijnen die ondersteuning bieden voor importbewerkingen hanteert drie scripts.

**Beginnen met importeren**  
De begin-script importeren wordt uitgevoerd aan het begin van een stap importeren die zijn uitgevoerd. Tijdens deze stap kunt u geen verbinding maken met het bronsysteem en voorbereidende stappen doen voordat het importeren van gegevens uit het verbonden systeem.

De begin-script importeren worden de volgende parameters van de verbindingslijn ontvangen:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Wordt het script geïnformeerd over het type importeren uitvoeren (delta of volledige) partition, hiërarchie watermerk en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de ruimte verbindingslijn die is geïmporteerd.

Het script moet retourneren een enkel [OpenImportConnectionResults] [ oicres] object naar de pijplijn, bijvoorbeeld:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Gegevens importeren**  
Het script van de gegevens importeren wordt aangeroepen door de verbindingslijn totdat het script geeft aan dat er geen meer gegevens te importeren. De Windows PowerShell-connector heeft een paginaformaat 9999 objecten. Als uw script meer dan 9999 objecten voor importeren retourneert, moet u paginering ondersteunen. De verbindingslijn gegarandeerd de eigenschap van een aangepaste gegevens waarmee u een winkel een watermerk kunt zodat telkens wanneer het script van de gegevens importeren wordt genoemd, het script hervat waar deze gebleven was objecten importeren.

Het script van de gegevens importeren kunt u de volgende parameters van de verbindingslijn ontvangt:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
GetImportEntriesRunStep | [ImportRunStep][irs] | Bevat het watermerk (CustomData) die kan worden gebruikt tijdens invoer geheugen: en delta importeert.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Wordt het script geïnformeerd over het type importeren uitvoeren (delta of volledige) partition, hiërarchie watermerk en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de ruimte verbindingslijn die is geïmporteerd.

Een lijst moet schrijven door het script van de gegevens importeren [[CSEntryChange][csec]] object naar de pijplijn. Deze verzameling bestaat uit CSEntryChange kenmerken die elk object dat wordt geïmporteerd vertegenwoordigen. Tijdens de uitvoering van een volledige importeren, moet deze verzameling een volledige reeks CSEntryChange objecten met alle kenmerken voor elk object hebben. Tijdens een Delta importeren, moet het object CSEntryChange het kenmerk niveau delta voor elk object te importeren of een volledige weergave van de objecten die zijn gewijzigd (modus vervangen) bevatten.

**Einde importeren**  
Aan het einde van de importbewerking uitvoeren, wordt het script einde importeren uitgevoerd. Dit script moet een Opschoningstaken nodig (bijvoorbeeld sluiten verbindingen met systemen) en reageren op fouten uitvoeren.

Het einde importeren script ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Wordt het script geïnformeerd over het type importeren uitvoeren (delta of volledige) partition, hiërarchie watermerk en verwachte paginaformaat.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Wordt het script geïnformeerd over de reden dat het importeren is beëindigd.

Het script moet retourneren een enkel [CloseImportConnectionResults] [ cicres] object naar de pijplijn, bijvoorbeeld:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exporteren
Gelijk aan de architectuur van het importeren van de verbindingslijn, verbindingslijnen die ondersteuning bieden voor exporteren hanteert drie scripts.

**Begin exporteren**  
De begin-exportscript wordt uitgevoerd aan het begin van een stap exporteren uitvoeren. Tijdens deze stap kunt u geen verbinding maken met het bronsysteem en eventuele voorbereidende stappen uitvoeren voordat u gegevens exporteren naar het verbonden systeem.

De begin-exportscript ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Wordt het script geïnformeerd over het type exporteren uitvoeren (delta of volledige) partition, hiërarchie en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de verbindingslijn ruimte die is geëxporteerd.

Het script moet de pijplijn niet uitvoer weer.

**Gegevens exporteren**  
De synchronisatieservice oproepen het script gegevens exporteren zo vaak aan alle in behandeling uitvoer van proces. Als de ruimte verbindingslijn meer in behandeling exports dan van de verbindingslijn paginaformaat heeft, mag de gegevens exportscript worden aangeroepen meerdere keren en mogelijk meerdere keren voor hetzelfde object.

De gegevens exportscript ontvangt de volgende parameters van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
CSEntries | IList[CSEntryChange][csec] | Lijst met alle verbindingslijn ruimteobjecten met wachtende exports tijdens deze sessie worden verwerkt.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Wordt het script geïnformeerd over het type exporteren uitvoeren (delta of volledige) partition, hiërarchie en verwachte paginaformaat.
Typen | [Schema][schema] | Schema voor de verbindingslijn ruimte die is geëxporteerd.

De gegevens exportscript moet een [PutExportEntriesResults] retourneren[ peeres] object naar de pijplijn. Dit object hoeft niet informatie wilt tonen resultaat voor elke verbindingslijn die geëxporteerde tenzij een fout of een wijziging in het ankerkenmerk optreedt. Als u bijvoorbeeld een object PutExportEntriesResults terugkeren naar de pijplijn:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Einde exporteren**  
Aan het einde van de export wordt uitgevoerd, het script einde exporteren om uit te voeren. Dit script moet een Opschoningstaken nodig (bijvoorbeeld sluiten verbindingen met systemen) en reageren op fouten uitvoeren.

De volgende parameters ontvangt de exportscript einde van de verbindingslijn:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Wordt het script geïnformeerd over het type exporteren uitvoeren (delta of volledige) partition, hiërarchie en verwachte paginaformaat.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Wordt het script geïnformeerd over de reden dat de export is beëindigd.

Het script moet de pijplijn niet uitvoer weer.

#### <a name="password-synchronization"></a>Wachtwoordsynchronisatie
Windows PowerShell verbindingslijnen kunnen worden gebruikt als doel voor wachtwoorden wijzigingen.

Het script wachtwoord kunt u de volgende parameters van de verbindingslijn ontvangt:

Naam | Gegevenstype | Beschrijving
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] | Tabel met configuratieparameters die voor de verbindingslijn.
Referentie | [PSCredential][pscred] | Eventuele referenties die zijn opgegeven door de beheerder op het tabblad Connectivity bevat.
Partition | [Partition][part] | Directory partition waarop de CSEntry zich bevindt.
CSEntry | [CSEntry][cse] | Vermelding van de verbindingslijn spatie voor het object dat is ontvangen een wachtwoord wijzigen of opnieuw instellen.
OperationType | Tekenreeks | Geeft aan of de bewerking een opnieuw instellen (**SetPassword**) of een wijziging (**ChangePassword is**).
PasswordOptions | [PasswordOptions][pwdopt] | Vlaggen die het juiste wachtwoord aangeven opnieuw instellen van gedrag. Deze parameter is alleen beschikbaar als OperationType **SetPassword**is.
OudWachtwoord | Tekenreeks | Gevuld met de oude wachtwoord van het object voor wijzigen van wachtwoorden. Deze parameter is alleen beschikbaar als OperationType **ChangePassword**is.
NewPassword | Tekenreeks | Gevuld met nieuw wachtwoord in van het object dat het script moet instellen.

Het script wachtwoord wordt niet naar verwachting om terug te keren resultaten worden aan de Windows PowerShell-pijplijn. Als een fout in het script wachtwoord optreedt, moet een van de volgende uitzonderingen op de synchronisatieservice informeren over het probleem door het script genereren:

- [PasswordPolicyViolationException] [ pwdex1] – gegenereerd als het wachtwoord niet aan de wachtwoordbeleid in het verbonden systeem voldoet.
- [PasswordIllFormedException] [ pwdex2] – gegenereerd als het wachtwoord niet geschikt voor het verbonden systeem is.
- [PasswordExtension] [ pwdex3] – gegenereerd voor alle andere fouten in het script wachtwoord.

## <a name="sample-connectors"></a>Voorbeeld van verbindingslijnen
Zie [Windows PowerShell-Connector steekproef verbindingslijn collectie]voor een volledig overzicht van de verbindingslijnen beschikbaar voorbeeld,[samp].

## <a name="other-notes"></a>Andere notities

### <a name="additional-configuration-for-impersonation"></a>Aanvullende configuratie voor imitatie
Verleen de gebruiker die is nagebootst voor de synchronisatieservice-server de volgende machtigingen:

Alleen de toegang tot de volgende registersleutels staat:

- HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
- HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Environment

Om te bepalen de beveiliging id (beveiligings-id) van de synchronisatieservice serviceaccount, voert u de volgende PowerShell-opdrachten:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Alleen de toegang tot de volgende mappen van het bestandssysteem:

- %ProgramFiles%\Microsoft forefront identiteit Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront identiteit Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront identiteit Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Vervang de naam van de Windows PowerShell-connector voor de tijdelijke aanduiding van {ConnectorName}.

## <a name="troubleshooting"></a>Problemen oplossen

-   Zie voor informatie over het inschakelen van logboekregistratie om op te lossen de verbindingslijn, [hoe u ETW tracering inschakelen voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
