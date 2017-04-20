<properties
    pageTitle="Azure resourcemanager ondersteuning voor verkeer Manager | Microsoft Azure "
    description="PowerShell gebruiken voor verkeer Manager met Azure resourcemanager"
    services="traffic-manager"
    documentationCenter="na"
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

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure resourcemanager ondersteuning voor Azure verkeer Manager

Azure resourcemanager is de voorkeur management interface voor services in Azure wordt aangegeven. Azure verkeer Manager profielen kunnen worden beheerd via Azure resourcemanager gebaseerde API's en hulpprogramma's voor.

## <a name="resource-model"></a>Resource-model

Azure verkeer Manager is geconfigureerd met behulp van een verzameling instellingen een profiel verkeer Manager genoemd. Dit profiel bevat DNS-instellingen, instellingen voor het routeren van verkeer controle-instellingen van eindpunt en een lijst met service-eindpunten waarop verkeer is doorgestuurd.

Elk profiel verkeer Manager wordt voorgesteld door een resource van het type 'TrafficManagerProfiles'. Op het niveau van de REST API is de URI voor elk profiel als volgt uit:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Vergelijking van de Azure verkeer Manager klassieke-API

De resourcemanager Azure-ondersteuning voor verkeer Manager andere terminologie dan het implementatiemodel klassieke gebruikt. De volgende tabel ziet u de verschillen tussen de resourcemanager en klassieke voorwaarden:

| Resourcemanager term | Klassieke term |
|-----------------------|--------------|
| Omleiding van verkeer methode | Methode van taakverdeling |
| Prioriteit methode | Failover-methode |
| Gewogen methode | Round robin methode |
| Prestaties-methode | Prestaties-methode |

Op basis van feedback van klanten, we de terminologie om te verbeteren, helderheid en algemene vaak iets verkeerd te verkleinen gewijzigd. Er is geen verschil in functionaliteit.

## <a name="limitations"></a>Beperkingen

Wanneer u verwijst naar een eindpunt van het type 'AzureEndpoints' voor een Web-App, verkeer Manager eindpunten kunnen alleen verwijzen naar de standaard (productie) [slot Web App](../app-service-web/web-sites-staged-publishing.md). Aangepaste sleuven worden niet ondersteund. Als een tijdelijke oplossing, kunnen aangepaste sleuven worden geconfigureerd met behulp van het type 'ExternalEndpoints'.

## <a name="setting-up-azure-powershell"></a>Bij het instellen van Azure PowerShell

Deze instructies Microsoft Azure PowerShell gebruiken. Het volgende artikel wordt uitgelegd hoe installeren en configureren van Azure PowerShell.

- [Het installeren en configureren van Azure PowerShell](../powershell-install-configure.md)

De voorbeelden in dit artikel is ervan uitgegaan dat er een bestaande resourcegroep. U kunt een resourcegroep met de volgende opdracht kunt maken:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure resourcemanager moet alle resourcegroepen beschikken over een locatie. Deze locatie wordt gebruikt als de standaardsjabloon voor resources die zijn gemaakt in die resourcegroep. Echter aangezien verkeer Manager profiel resources globale, niet regionale zijn, heeft de keuze van resource groep locatie geen invloed op Azure verkeer Manager.

## <a name="create-a-traffic-manager-profile"></a>Een verkeer Manager-profiel maken

Als u wilt een verkeer Manager-profiel maken, gebruikt u de cmdlet New-AzureRmTrafficManagerProfile:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

De volgende tabel worden de parameters beschreven:

| Parameter | Beschrijving |
|-----------|-------------|
| Naam | De resourcenaam voor de resource van verkeer Manager profiel. Profielen in dezelfde resourcegroep moeten een unieke naam hebben. Deze naam staat los van de DNS-naam die wordt gebruikt voor DNS-query's.|
| ResourceGroupName | De naam van de resourcegroep die de bron profiel bevat.|
| TrafficRoutingMethod | Geeft de omleiding van verkeer methode gebruikt om te bepalen welke eindpunt een DNS-query in antwoord wordt geretourneerd. Mogelijke waarden zijn 'Prestaties', 'Gewogen' of 'Prioriteit'.|
| RelativeDnsName | Hiermee geeft u het gedeelte hostname van de naam van de DNS-verstrekt door dit profiel verkeer Manager. Deze waarde wordt gecombineerd met de DNS-domeinnaam die wordt gebruikt door Azure verkeer Manager om de FQDN-naam (FQDN) van het profiel. De waarde van 'contoso' wordt bijvoorbeeld 'contoso.trafficmanager.net'.|
| TTL | Hiermee geeft u de DNS-Time to Live (TTL), in seconden. Deze TTL wordt geïnformeerd de lokale DNS-resolvers en de DNS-clients hoe lang cache DNS-antwoorden voor dit profiel verkeer Manager.|
| MonitorProtocol | Hiermee geeft u het protocol gebruiken om de status van eindpunt te houden. Mogelijke waarden zijn 'HTTP' en 'HTTPS'.|
| MonitorPort | Hiermee geeft u de TCP-poorten gebruikt voor het eindpunt servicestatus bewaken.|
| MonitorPath | Hiermee geeft u het pad ten opzichte van de naam van het domein gebruikt om te zoeken naar eindpunt status.|

De cmdlet wordt gemaakt van een profiel verkeer Manager in Azure wordt aangegeven en geeft als resultaat een bijbehorende profile-object met PowerShell. Op dit moment bevat het profiel geen eindpunten. Zie voor meer informatie over het toevoegen van eindpunten aan een profiel verkeer Manager [Verkeer Manager eindpunten toe te voegen](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Een profiel verkeer Manager ophalen

Als u wilt een bestaand verkeer Manager profiel object ophalen, gebruikt u de cmdlet Get-AzureRmTrafficManagerProfle:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Deze cmdlet retourneert een verkeer Manager profile-object.

## <a name="update-a-traffic-manager-profile"></a>Een profiel verkeer Manager bijwerken

Verkeer Manager profielen wijzigen, voert u een 3-stappen:

1. Het profiel met Get-AzureRmTrafficManagerProfile ophalen of gebruikt u het profiel die het resultaat van nieuw-AzureRmTrafficManagerProfile.
2. Wijzig het profiel. U kunt toevoegen en verwijderen van de eindpunten of eindpunt of profiel parameters wijzigen. Deze wijzigingen zijn offline bewerkingen. Verandert u alleen het lokale object in het geheugen dat staat voor het profiel.
3. Uw wijzigingen met de cmdlet Set-AzureRmTrafficManagerProfile.

Alle profieleigenschappen kunnen worden gewijzigd, met uitzondering van het profiel RelativeDnsName. Als u wilt wijzigen van de RelativeDnsName, moet u profiel- en een nieuw profiel met een nieuwe naam verwijderen.

Het volgende voorbeeld wordt getoond hoe wijzigen van het profiel TTL:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Er zijn drie typen verkeer Manager eindpunten:

1. **Azure eindpunten** zijn de services van Azure
2. **Externe eindpunten** worden services hosten buiten Azure
3. **Geneste eindpunten** worden gebruikt voor geneste hiërarchieën van verkeer Manager profielen. Geneste eindpunten inschakelen geavanceerde verkeer-routing configuraties voor complexe toepassingen.

In alle gevallen, kunnen de eindpunten worden toegevoegd op twee manieren:

1. Gebruik een 3 stappen eerder is beschreven. Het voordeel van deze methode is dat verschillende eindpunt wijzigingen kunnen worden aangebracht in een update.
2. Gebruik de cmdlet New-AzureRmTrafficManagerEndpoint. Een eindpunt toevoegt deze cmdlet aan een bestaand profiel verkeer Manager in één bewerking.

## <a name="adding-azure-endpoints"></a>Azure eindpunten toevoegen

Azure eindpunten verwijzingen maken naar de services van Azure. Drie soorten Azure eindpunten worden ondersteund:

1. Azure WebApps
2. 'Klassieke' cloud services (die kunnen bevatten een service PaaS of IaaS virtuele machines)
3. Azure PublicIpAddress-resources (die kunnen worden gekoppeld aan een taakverdeling of een virtuele machine NIC). De PublicIpAddress moet een DNS-naam die moet worden gebruikt in verkeer Manager is toegewezen.

In elk geval:

- De service is opgegeven met de parameter 'targetResourceId' van toevoegen-AzureRmTrafficManagerEndpointConfig of nieuw AzureRmTrafficManagerEndpoint.
- De 'Doel' en 'EndpointLocation' zijn geïmpliceerd door de TargetResourceId.
- Geven de "gewicht" is optioneel. Lijndikten worden alleen gebruikt als het profiel is geconfigureerd voor gebruik van de methode 'Gewogen' verkeer-mailroutering. Anders worden deze genegeerd. Als u opgeeft, moet de waarde een getal tussen 1 en 1000. De standaardwaarde is '1'.
- Geven de 'prioriteit' is optioneel. Prioriteiten worden alleen gebruikt als het profiel is geconfigureerd voor gebruik van de methode 'Prioriteit' verkeer-mailroutering. Anders worden deze genegeerd. Geldige waarden liggen tussen 1 en 1000 met lagere waarden die aangeeft dat u een hogere prioriteit. Als voor één eindpunt opgegeven, moet het worden opgegeven voor alle eindpunten. Indien weggelaten worden standaardwaarden, beginnend aan '1' zijn toegepast in de volgorde waarin de eindpunten worden weergegeven.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Voorbeeld 1: De Web-App toevoegen eindpunten toevoegen-AzureRmTrafficManagerEndpointConfig gebruiken

In dit voorbeeld wordt een verkeer Manager-profiel maken en toevoegen van de eindpunten van Web App, met de cmdlet toevoegen-AzureRmTrafficManagerEndpointConfig.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Een 'klassieke' cloud-service-eindpunt met nieuw AzureRmTrafficManagerEndpoint toevoegen

In dit voorbeeld wordt een 'klassieke' Cloudservice-eindpunt toegevoegd aan een profiel verkeer Manager. In dit voorbeeld opgegeven we het profiel met behulp van de namen van de groep profiel- en resourcekalenders, in plaats van de doorgeven van een object profiel. Beide methoden worden ondersteund.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 3: Een nieuw AzureRmTrafficManagerEndpoint met publicIpAddress-eindpunt toevoegen

In dit voorbeeld wordt de bron van een openbare IP-adres toegevoegd aan het verkeer Manager-profiel. Het openbare IP-adres moet een DNS-naam die is geconfigureerd en kan worden gekoppeld aan de formule voor het berekenen van een VM of aan een taakverdeling.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Externe eindpunten toevoegen

Verkeer Manager wordt externe eindpunten om verkeer door naar services hosten buiten Azure. Net als met Azure eindpunten, kunnen externe eindpunten worden toegevoegd door toevoegen-AzureRmTrafficManagerEndpointConfig gevolgd door de Set-AzureRmTrafficManagerProfile of nieuw AzureRMTrafficManagerEndpoint.

Wanneer u externe eindpunten opgeeft:

- De naam van het domein moet worden opgegeven met de parameter 'Target'
- Als de methode voor het verkeer routering van 'Prestaties' wordt gebruikt, is het 'EndpointLocation' is vereist. Anders is optioneel. De waarde moet een [geldige Azure regionaam](https://azure.microsoft.com/regions/).
- De 'Gewicht' en 'Prioriteit' zijn optioneel.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Externe eindpunten met toevoegen-AzureRmTrafficManagerEndpointConfig en Set-AzureRmTrafficManagerProfile toevoegen

In dit voorbeeld wordt een verkeer Manager-profiel maken, externe eindpunten toevoegen en de wijzigingen.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Externe eindpunten met nieuw AzureRmTrafficManagerEndpoint toevoegen

In dit voorbeeld wordt een externe eindpunt toevoegen aan een bestaand profiel. Het profiel is opgegeven met de namen van de groep profiel- en resourcekalenders.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>'Geneste' eindpunten toevoegen

Elk profiel verkeer Manager Hiermee geeft u één verkeer-routing methode. Er zijn echter scenario's waarvoor meer geavanceerde verkeersroutering dan de routering door één verkeer Manager profiel verstrekt. U kunt functies nesten verkeer Manager profielen als u wilt combineren van de voordelen van meer dan één verkeer-routing methode. Geneste profielen kunnen u het standaardgedrag van de Manager van verkeer naar ondersteuning voor grotere en meer complexe implementaties van de toepassing te overschrijven. Zie voor meer gedetailleerde voorbeelden [genest verkeer Manager profielen](traffic-manager-nested-profiles.md).

Geneste eindpunten worden geconfigureerd in de bovenliggende profiel, met een specifieke eindpunt type, 'NestedEndpoints'. Wanneer u opgeeft geneste eindpunten:

- Het eindpunt moet worden opgegeven met de parameter 'targetResourceId'
- Als de methode voor het verkeer routering van 'Prestaties' wordt gebruikt, is het 'EndpointLocation' is vereist. Anders is optioneel. De waarde moet een [geldige Azure regionaam](http://azure.microsoft.com/regions/).
- De 'Gewicht' en 'Prioriteit' zijn optioneel, als voor Azure eindpunten.
- De parameter 'MinChildEndpoints' is optioneel. De standaardwaarde is '1'. Als het aantal beschikbare eindpunten kleiner is dan deze drempelwaarde, wordt het profiel van de bovenliggende neemt het onderliggende profiel 'Doorgaan' en zorgt verkeer naar de andere eindpunten in het profiel van de bovenliggende site.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Geneste eindpunten met toevoegen-AzureRmTrafficManagerEndpointConfig en Set-AzureRmTrafficManagerProfile toevoegen

In dit voorbeeld we maken nieuwe verkeer Manager onderliggende en bovenliggende profielen, de onderliggende als een geneste eindpunt toevoegen aan de bovenliggende en de wijzigingen doorvoeren.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Voor kort te houden in dit voorbeeld hebt we niet een andere eindpunten toevoegen aan de onderliggende of bovenliggende profielen.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Geneste eindpunten met nieuw AzureRmTrafficManagerEndpoint toevoegen

In dit voorbeeld toevoegen we een bestaand profiel van de onderliggende als een geneste eindpunt aan een bestaand profiel van de bovenliggende site. Het profiel is opgegeven met de namen van de groep profiel- en resourcekalenders.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Een eindpunt verkeer Manager bijwerken

Er zijn twee manieren bijwerken van een bestaand verkeer Manager-eindpunt:

1. Het verkeer Manager profiel Get-AzureRmTrafficManagerProfile met gedownload, de eigenschappen van het eindpunt binnen het profiel bijwerken en het gebruik van de Set-AzureRmTrafficManagerProfile wijzigingen doorvoeren. Deze methode heeft het voordeel van meer dan één eindpunt in één keer bijwerken.
2. Het verkeer Manager eindpunt met Get-AzureRmTrafficManagerEndpoint gedownload, bijwerken van de eigenschappen van het eindpunt en het gebruik van de Set-AzureRmTrafficManagerEndpoint wijzigingen doorvoeren. Deze methode is eenvoudiger, aangezien hoeft niet in de matrix eindpunten in het profiel geïndexeerd.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: De eindpunten met Get-AzureRmTrafficManagerProfile en Set-AzureRmTrafficManagerProfile bijwerken

In dit voorbeeld wordt de prioriteit op twee eindpunten binnen een bestaand profiel wijzigen.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Een eindpunt met Get-AzureRmTrafficManagerEndpoint en Set-AzureRmTrafficManagerEndpoint bijwerken

In dit voorbeeld wordt de dikte van een enkelvoudig eindpunt in een bestaand profiel wijzigen.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Eindpunten en profielen inschakelen en uitschakelen

Verkeer Manager kunt afzonderlijke eindpunten worden ingeschakeld en uitgeschakeld, evenals toestaan in- en uitschakelen van de hele profielen.
Deze wijzigingen kunnen worden aangebracht door ophalen/bijwerken/instellingen de bronnen eindpunt of profiel. Als u wilt deze algemene stroomlijnen, worden ze ook ondersteund via speciale cmdlets.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Voorbeeld 1: In- en uitschakelen van een profiel verkeer Manager

Als u wilt een profiel verkeer Manager inschakelen, gebruikt u inschakelen-AzureRmTrafficManagerProfile. Het profiel u kunt opgeven met behulp van een object profiel. Het profiel-object kan worden doorgegeven via de pijplijn of met behulp van de '-TrafficManagerProfile' parameter. In dit voorbeeld wordt het profiel opgeven door de naam van de groep profiel- en resourcekalenders.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Een profiel verkeer Manager uitschakelen:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

De cmdlet uitschakelen-AzureRmTrafficManagerProfile vraagt om bevestiging. Deze vraag kan worden onderdrukt met de '-dwingen ' parameter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Voorbeeld 2: In- en uitschakelen van een eindpunt verkeer Manager

Als u wilt een eindpunt verkeer Manager inschakelen, gebruikt u inschakelen-AzureRmTrafficManagerEndpoint. Er zijn twee manieren om op te geven van het eindpunt

1. Met behulp van een TrafficManagerEndpoint-object doorgegeven via de pijplijn of met de '-TrafficManagerEndpoint' parameter
2. Gebruik de naam van het eindpunt, eindpunt type, naam van een profiel en de naam van de resource-groep:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Daarnaast een eindpunt verkeer Manager uitschakelen:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Als u met de uitschakelen-AzureRmTrafficManagerProfile, wordt de cmdlet uitschakelen-AzureRmTrafficManagerEndpoint vraagt om bevestiging. Deze vraag kan worden onderdrukt met de '-dwingen ' parameter.

## <a name="delete-a-traffic-manager-endpoint"></a>Een eindpunt verkeer Manager verwijderen

Als u wilt verwijderen afzonderlijke eindpunten, gebruikt u de cmdlet verwijderen AzureRmTrafficManagerEndpoint:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Deze cmdlet vraagt om bevestiging. Deze vraag kan worden onderdrukt met de '-dwingen ' parameter.

## <a name="delete-a-traffic-manager-profile"></a>Een profiel verkeer Manager verwijderen

Als u wilt verwijderen van een profiel verkeer Manager, gebruikt u de verwijderen-AzureRmTrafficManagerProfile-cmdlet, de namen van de groep profiel- en resourcekalenders opgeven:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Deze cmdlet vraagt om bevestiging. Deze vraag kan worden onderdrukt met de '-dwingen ' parameter.

Het profiel moet worden verwijderd kan ook worden opgegeven met behulp van een object profiel:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Kunt u de deze reeks ook eigenschappen:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Volgende stappen

[Verkeer Manager bewaken](traffic-manager-monitoring.md)

[Prestatieoverwegingen verkeer Manager](traffic-manager-performance-considerations.md)
