<properties
    pageTitle="Geneste verkeer Manager profielen | Microsoft Azure"
    description="In dit artikel wordt uitgelegd van de functie 'Profielen genest' van Azure verkeer Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="nested-traffic-manager-profiles"></a>Geneste verkeer Manager-profielen

Verkeer Manager bevat een bereik van verkeer-routing methoden waarmee u kunt bepalen hoe verkeer Manager bepaalt welke eindpunt verkeer uit elke eindgebruiker moet ontvangen. Zie [verkeer Manager verkeer-routing methoden](traffic-manager-routing-methods.md)voor meer informatie.

Elk profiel verkeer Manager Hiermee geeft u één verkeer-routing methode. Er zijn echter scenario's waarvoor meer geavanceerde verkeersroutering dan de routering door één verkeer Manager profiel verstrekt. U kunt functies nesten verkeer Manager profielen als u wilt combineren van de voordelen van meer dan één verkeer-routing methode. Geneste profielen kunnen u het standaardgedrag van de Manager van verkeer naar ondersteuning voor grotere en meer complexe implementaties van de toepassing te overschrijven.

De volgende voorbeelden laten zien voor het gebruik van geneste verkeer Manager profielen in verschillende scenario.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Voorbeeld 1: Combineren 'Prestaties' en 'Gewogen' verkeersroutering

Stel dat u een toepassing in de volgende Azure regio's worden geïmplementeerd: West VS, West Europe en Oost-Azië. Distributie van verkeer naar het gebied dat zich het dichtst bij de gebruiker van verkeer Manager 'Prestaties' verkeer-routing methode te gebruiken.

![Enkel verkeer Manager profiel][1]

Stel nu dat u wilt een update van uw service te testen voordat u deze sterk uiteen schuivend voor meer informatie. U wilt de 'gewogen' verkeer-routing methode gebruiken om te leiden van een klein percentage van verkeer naar uw test-implementatie. U instellen de implementatie test samen met de bestaande productie-implementatie in West Europa.

Kan niet worden gecombineerd beide 'gewogen' en ' prestaties verkeer-mailroutering in één profiel. Voor dit scenario, kunt u een verkeer Manager-profiel met de eindpunten van West Europe en de methode 'Gewogen' verkeer-routing maken. Vervolgens toevoegen u dit profiel 'onderliggende' als een eindpunt aan het profiel 'parent'. Het profiel van de bovenliggende nog steeds wordt de prestaties verkeer-routing methode gebruikt en de andere globale implementaties als eindpunten bevat.

In het volgende diagram ziet u in dit voorbeeld:

![Geneste verkeer Manager-profielen][2]

In deze configuratie wordt verdeelt verkeer via het profiel van de bovenliggende wordt gestuurd verkeer over regio's normaal. Binnen West Europa, wordt verkeer naar de eindpunten productie en testen op basis van het gewicht dat is toegewezen verdeeld door het geneste profiel.

Als het profiel van de bovenliggende gebruikmaakt van de methode 'Prestaties' verkeer-mailroutering, moet elke eindpunt een locatie worden toegewezen. De locatie wordt toegewezen wanneer u het eindpunt configureren. Kies het dichtst bevindt bij de implementatie Azure regio. De Azure gebieden zijn de locatie-waarden die worden ondersteund door de Internet latentie tabel. Zie voor meer informatie [verkeer Manager 'Prestaties' verkeer-routing methode](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Voorbeeld 2: Eindpunt cmdlets voor controle in geneste profielen

Verkeer Manager bewaakt actief de status van elke service-eindpunt. Als een eindpunt beschadigd is, verwijst verkeer Manager gebruikers om alternatieve eindpunten te behouden de beschikbaarheid van uw service. Dit eindpunt controle en failover gedrag is van toepassing op alle verkeer-routing methoden. Zie voor meer informatie [Verkeer Manager eindpunt controle](traffic-manager-monitoring.md). Eindpunt monitoring werkt anders voor geneste profielen. Met geneste profielen, het profiel van de bovenliggende niet systeemstatus voeren controles uit op de onderliggende rechtstreeks. In plaats daarvan wordt de status van de eindpunten van het profiel van de onderliggende gebruikt voor het berekenen van de algemene status van het kind-profiel. Deze status informatie wordt doorgegeven de hiërarchie geneste profiel. Het bovenliggende profiel dit systeemstatus om te bepalen of u wilt de directe verkeer naar het profiel van de onderliggende samengevoegd. Zie het gedeelte [Veelgestelde vragen](#faq) van dit artikel voor meer informatie op de statuscontrole van geneste profielen.

Terugkeren naar het vorige voorbeeld, Stel dat de productie-implementatie in West Europa mislukt. Standaard het profiel 'onderliggende' wordt u omgeleid zodat al het verkeer naar de test-implementatie. Als de test-implementatie ook mislukt, bepaalt het bovenliggende profiel dat het onderliggende profiel ontvangen geen over verkeer berichten moet omdat alle onderliggende eindpunten beschadigd. Klik, wordt het profiel van de bovenliggende verkeer naar de andere gebieden verdeeld.

![Geneste profiel failover (standaardinstelling)][3]

Het is mogelijk dat u tevreden zijn met deze optie kiest. Of wilt u misschien betrokken al het verkeer voor West Europa dient nu aan de test-implementatie in plaats van een beperkte subset-verkeer is toegestaan. Ongeacht de status van de test-implementatie wilt u naar de andere regio's mislukken wanneer de productie-implementatie in West Europa mislukt. Als u wilt deze failover inschakelt, kunt u de parameter 'MinChildEndpoints' opgeven tijdens het configureren van het kind profiel als een eindpunt in het profiel van de bovenliggende site. De parameter bepaalt het minimum aantal beschikbare eindpunten in het onderliggende profiel. De standaardwaarde is '1'. In dit scenario stelt u de waarde MinChildEndpoints op 2. Onder deze drempel, is het profiel van de bovenliggende acht van het profiel van de gehele onderliggende niet beschikbaar is, en wordt u omgeleid zodat verkeer naar de andere eindpunten.

De volgende afbeelding ziet u deze configuratie:

![Geneste profiel failover met 'MinChildEndpoints' = 2][4]

>[AZURE.NOTE]
>De methode 'Prioriteit' verkeer-routing distribueert al het verkeer naar een enkel eindpunt. Er is dus weinig doel in een instelling MinChildEndpoints dan '1' onderliggende profiel.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Voorbeeld 3: Prioriteit failover regio's in 'Prestaties' verkeersroutering

Het standaardgedrag voor de methode 'Prestaties' verkeer routering is bedoeld om te weinig laden van de volgende dichtstbijzijnde eindpunt en veroorzaakt door een trapsgewijze reeks fouten te voorkomen. Wanneer een eindpunt mislukt, wordt al het verkeer dat zou hebben is doorgestuurd naar dat eindpunt gelijkmatig verdeeld over de andere eindpunten in alle regio's.

!['Prestaties' verkeer routering met standaard failover][5]

Stel echter dat u wilt u de overname van West Europe verkeer naar West Amerikaans, en alleen-verkeer door naar andere gebieden als beide eindpunten niet beschikbaar zijn. U kunt deze oplossing met een onderliggend profiel en de methode voor het verkeer routering van 'Prioriteit' maken.

!['Prestaties' verkeer routering met preferentiële failover][6]

Aangezien het eindpunt West Europa met hogere prioriteit dan het eindpunt West Amerikaans, worden al het verkeer wordt verzonden naar het eindpunt West Europe wanneer beide eindpunten online bent. Als West Europe mislukt, wordt het verkeer doorgestuurd naar West VS. Met de geneste profiel wordt verkeer geleid Oost-Azië alleen wanneer zowel West Europa en West Amerikaans mislukt.

U kunt dit patroon herhalen voor alle regio's. Vervang alle drie eindpunten in het profiel van de bovenliggende door drie onderliggende profielen, elk voor een reeks prioriteit failover.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Voorbeeld 4: Bepalen 'Prestaties' verkeer routeren tussen meerdere eindpunten in dezelfde regio

Stel dat u de prestaties die verkeer-routing methode wordt gebruikt in een profiel dat meer dan één eindpunt in een bepaalde regio heeft. Standaard verkeer doorgestuurd naar die regio gelijkmatig verdeeld over alle beschikbare eindpunten in die regio.

!['Prestaties' verkeer routering in regio verkeer verdeling (standaardinstelling)][7]

In plaats van het toevoegen van meerdere eindpunten in West Europa, zijn deze eindpunten ingesloten in een afzonderlijk onderliggend-profiel. Het profiel van het kind wordt toegevoegd aan de bovenliggende als het enige eindpunt in West Europa. De instellingen in het onderliggende profiel kunnen u de verdeling verkeer met West Europa bepalen doordat op basis van prioriteit of gewogen verkeersroutering binnen die regio.

!['Prestaties' verkeer routering met aangepaste in regio verkeer verdeling][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Voorbeeld 5: Per eindpunt controle-instellingen

Stel dat u gebruikt verkeer Manager soepel migreren verkeer van een verouderd on-premises implementatie website naar een nieuwe cloudgebaseerde versie die wordt gehost in Azure wordt aangegeven. Voor de oude site, die u wilt de startpagina van Lotus URI gebruiken om de status van de site te houden. Maar voor de nieuwe cloudgebaseerde versie, u een aangepaste controleren pagina implementeert (pad ' / monitor.aspx') die extra controles bevat.

![Verkeer Manager eindpunt monitoring (standaardinstelling)][9]

De controle-instellingen in een profiel verkeer Manager toepassen op alle eindpunten binnen één profiel. Met geneste profielen kunt u verschillende onderliggende profiel per site definiëren verschillende controle-instellingen.

![Verkeer Manager eindpunt bewaken met per eindpunt instellingen][10]

## <a name="faq"></a>FAQ

### <a name="how-do-i-configure-nested-profiles"></a>Hoe configureer ik geneste profielen?

Geneste verkeer Manager profielen kunnen worden geconfigureerd met behulp van de Azure resourcemanager en de klassieke Azure REST API's, Azure PowerShell-cmdlets en platforms Azure CLI opdrachten. Ze worden ook ondersteund via de nieuwe portal van Azure. Ze worden niet ondersteund in de klassieke portal.

### <a name="how-many-layers-of-nesting-does-traffic-manger-support"></a>Hoeveel lagen van nesten doet verkeer Manager ondersteund?

U kunt profielen maximaal 10 niveaus nesten. 'Lussen' zijn niet toegestaan.

### <a name="can-i-mix-other-endpoint-types-with-nested-child-profiles-in-the-same-traffic-manager-profile"></a>Kan ik andere typen eindpunt meng met geneste onderliggende profielen in hetzelfde profiel zijn dat verkeer Manager?

Ja. Er zijn geen beperkingen van hoe u de eindpunten van verschillende typen binnen een profiel combineren.

### <a name="how-does-the-billing-model-apply-for-nested-profiles"></a>Hoe geldt het billing model voor geneste profielen?

Er is geen negatieve gevolgen van het gebruik van geneste profielen prijzen.

Verkeer Manager facturering bestaat uit twee onderdelen: eindpunt systeemstatus controles en miljoenen DNS-query's

- Controles voor servicestatus van eindpunt: Er is gratis bij geconfigureerd als een eindpunt in een bovenliggende-profiel een onderliggende-profiel. Cmdlets voor controle van de eindpunten in het profiel van de onderliggende zijn gefactureerd op de gebruikelijke manier.
- DNS-query's: elke query is slechts één keer worden geteld. Een query ten opzichte van een bovenliggende-profiel dat geeft als een eindpunt van een profiel kind resultaat wordt geteld ten opzichte van het profiel van bovenliggende alleen.

Zie voor meer informatie, de [pagina voor het prijzen van verkeer beheren](https://azure.microsoft.com/pricing/details/traffic-manager/).

### <a name="is-there-a-performance-impact-for-nested-profiles"></a>Is er invloed op de prestaties voor geneste profielen?

Nee. Er is geen invloed op de prestaties weergegeven die zijn gemaakt bij het gebruik van geneste profielen.

De naamservers van het verkeer Manager door de hiërarchie profiel intern bladeren tijdens het verwerken van elke DNS-query. Een DNS-query naar een bovenliggende-profiel kunt een DNS-antwoord aan een eindpunt ontvangen van een profiel kind. Een enkel CNAME-record wordt gebruikt of u met één profiel of geneste profielen. Er is geen nodig om een CNAME-record voor elk profiel maken in de hiërarchie.

### <a name="how-does-traffic-manager-compute-the-health-of-a-nested-endpoint-in-a-parent-profile"></a>Hoe wordt verkeer Manager berekend van de status van een geneste eindpunt in een bovenliggende-profiel?

Het profiel van de bovenliggende uitvoeren niet rechtstreeks systeemstatus controles op de onderliggende. In plaats daarvan de status van de eindpunten van het profiel van de onderliggende worden gebruikt voor het berekenen van de algemene status van het kind-profiel. Deze informatie wordt doorgegeven de hiërarchie geneste profiel om te bepalen de status van het geneste eindpunt. Het profiel van de bovenliggende gebruikt deze geaggregeerde systeemstatus om te bepalen of het verkeer kunt doorgestuurd naar de onderliggende.

De volgende tabel wordt het gedrag van verkeer Manager, systeemstatus Hiermee wordt gecontroleerd op een geneste-eindpunt.

|Status van de onderliggende profiel Monitor|Bovenliggende eindpunt Monitor status|Notities|
|---|---|---|
|Uitgeschakeld. Het profiel van de onderliggende is uitgeschakeld.|Gestopt|De status van de bovenliggende is gestopt, niet uitgeschakeld. De status uitgeschakeld is gereserveerd voor dat aangeeft dat u het eindpunt in het profiel van de bovenliggende hebt uitgeschakeld.|
|Niet beschikbaar is weergegeven. Ten minste één onderliggende profiel eindpunt heeft een gedegradeerd staat.| Online: het aantal Online eindpunten in het onderliggende profiel is ten minste de waarde van MinChildEndpoints.<BR>CheckingEndpoint: het aantal Online plus CheckingEndpoint eindpunten in het onderliggende profiel is ten minste de waarde van MinChildEndpoints.<BR>Niet beschikbaar is weergegeven: anders.|Verkeer wordt doorgestuurd naar een eindpunt van de status CheckingEndpoint. Als MinChildEndpoints te hoog is ingesteld, het eindpunt is altijd niet beschikbaar is weergegeven.|
|Online. Ten minste één onderliggende profiel eindpunt is Online provincie. Er is geen eindpunt in de stand gedegradeerd.|Zie hierboven.||
|CheckingEndpoints. Ten minste één onderliggende profiel eindpunt is 'CheckingEndpoint'. Geen eindpunten zijn 'Online' of 'Doorgaan'|Hetzelfde als hierboven.||
|Inactief. Alle onderliggende profiel eindpunten zijn uitgeschakeld of gestopt, of dit profiel bevat geen eindpunten.|Gestopt||


## <a name="next-steps"></a>Volgende stappen

Meer informatie over [de werking van verkeer Manager](traffic-manager-how-traffic-manager-works.md)

Informatie over het [maken van een profiel verkeer Manager](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png

