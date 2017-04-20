<properties
    pageTitle="Overzicht van automatisch schalen in Microsoft Azure virtuele Machines, Cloudservices en Web Apps | Microsoft Azure"
    description="Overzicht van automatisch schalen in Microsoft Azure. Geldt voor virtuele Machines, Cloudservices en Webapps."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Overzicht van automatisch schalen in Microsoft Azure virtuele Machines, Cloudservices en Web Apps

In dit artikel wordt beschreven welke Microsoft Azure automatisch schalen is, wordt de voordelen, en hoe u aan de slag met deze.  

Azure Monitor automatisch schalen alleen van toepassing op [VM schaal Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/)en [App-Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure heeft twee automatisch schalen methoden. Een oudere versie van automatisch schalen is van toepassing op virtuele Machines (beschikbaarheid sets). Deze functie biedt beperkte ondersteuning en het is raadzaam te migreren naar VM schaal Sets voor snellere en meer betrouwbare automatisch schalen ondersteuning. Een koppeling voor het gebruik van de oudere technologie is opgenomen in dit artikel.  


## <a name="what-is-autoscale"></a>Wat is automatisch schalen?

Automatisch schalen kunt u de juiste hoeveelheid resources is uitgevoerd voor het verwerken van de belasting van uw toepassing. U kunt resources toevoegen om te verwerken stijging laden en ook Bespaar door te verwijderen van resources die zijn zit niet actief geweest. U opgeven een minimum en maximum aantal keren wilt uitvoeren en toevoegen of verwijderen van VMs automatisch op basis van een set regels. Met een minimale maken die ervoor dat uw toepassing wordt altijd uitgevoerd zelfs geen laden. Met een beperkt mogelijke van de totale kosten per uur. U schalen automatisch tussenin met behulp van regels die u maakt.

 ![Automatisch schalen beschreven. Toevoegen en verwijderen van VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Als regelvoorwaarden is voldaan, worden een of meer automatisch schalen acties wordt geactiveerd. U kunt toevoegen en verwijderen van VMs of andere acties uitvoeren. De volgende conceptueel diagram ziet u dit proces.  

 ![Conceptuele automatisch schalen gegevensstroom-Diagram](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Automatisch schalen proces uitleg over
De volgende uitleg van toepassing op de onderdelen van het vorige diagram.   

### <a name="resource-metrics"></a>De doelstellingen van de resource
Resources verzenden aan de doelstellingen, die later door regels worden verwerkt. Aan de doelstellingen afkomstig zijn via verschillende methoden.
VM schaal Sets gebruikt telemetriegegevens van Azure diagnostisch hulpprogramma agenten dat telemetrielogboek voor WebApps en cloudservices wordt geleverd rechtstreeks vanuit de infrastructuur van Azure. Enkele veelgebruikte statistieken opnemen CPU-gebruik, geheugengebruik thread-tellingen komen, lengte en gebruik van de schijf. Zie voor een lijst met welke telemetriegegevens die u kunt gebruiken, [Automatisch schalen algemene doelstellingen](insights-autoscale-common-metrics.md).

### <a name="time"></a>Tijd
Planning gebaseerde regels zijn gebaseerd op UTC. U moet uw tijdzone correct ingesteld bij het instellen van uw regels.  

### <a name="rules"></a>Regels
Het diagram ziet u slechts één automatisch schalen regel, maar u kunt veel van deze hebben. Als u nodig hebt voor uw situatie, kunt u complexe overlappende regels maken.  Regeltypen opnemen  

 - **Metrisch gebaseerde** - bijvoorbeeld handeling wanneer CPU-gebruik groter dan 50 is %.
 - **Op tijd gebaseerde** - bijvoorbeeld trigger een webhook elke 8 am op zaterdag in een bepaalde tijdzone.

Metrisch gebaseerde regels meten toepassing laden en toevoegen of verwijderen van VMs op basis van die laden. Planning gebaseerde regels kunt u aan de nieuwe schaal wanneer u tijdnotatie weergeven in uw laden en wilt schalen voordat u een stijging van mogelijke laden of verkleinen.  


### <a name="actions-and-automation"></a>Acties en automatisering

Regels kunnen resulteren in een of meer typen acties.

- **Schaal** - schaal VMs in- of uitzoomen
- **E** - e-mail verzenden aan beheerders van abonnement, co-beheerders en/of extra e-mailadres dat u opgeeft
- **Automatiseren via webhooks** - oproep webhooks, waarin meerdere complexe acties binnen of buiten Azure kunt activeren. Binnen Azure, kunt u een Azure automatisering runbook, Azure-functie, of Azure logica App starten. Voorbeeld 3e partijen URL buiten Azure services zoals toegestane vertraging en Twilio opnemen.


## <a name="autoscale-settings"></a>Instellingen voor automatisch schalen
Automatisch schalen gebruik de volgende terminologie en structuur.

- Een **automatisch schalen instelling** wordt gelezen door de engine automatisch schalen om te bepalen of aan de nieuwe schaal omhoog of omlaag. De presentatie bevat een of meer profielen, informatie over de doeltoepassing resource en instellingen voor meldingen.
    - Een **automatisch schalen profiel** is een combinatie van een capaciteit instellen, een set regels voor de triggers en schaal acties voor het profiel en een terugkeerpatroon. U kunt meerdere profielen, waarmee u kunt verschillende overlappende vereisten kunt verrichten hebben.
        - Een **capaciteit instelling** geeft aan het minimum, maximum en standaardwaarden voor het aantal exemplaren. [figuur 1 gebruiken om de juiste plaats]
        - Een **regel** bevat een trigger, een metrische trigger of een tijd-trigger, en een actie schaal, die aangeeft of automatisch schalen moet schaal omhoog of omlaag wanneer deze regel wordt voldaan.
        - Een **Terugkeerpatroon** wordt aangegeven wanneer automatisch schalen moet dit profiel tot stand is gebracht. U kunt verschillende automatisch schalen profielen voor verschillende tijden overdag als dagen van de week, bijvoorbeeld hebben.
- Een **instelling voor melding** wordt gedefinieerd welke meldingen moeten worden uitgevoerd wanneer een gebeurtenis automatisch schalen op basis van die voldoet aan de criteria van een van de instelling van de automatisch schalen profielen plaatsvindt. Automatisch schalen kunt hoogte van een of meer e-mailadressen of oproepen naar een of meer webhooks.

![Azure automatisch schalen-instelling, profiel en regelstructuur](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

De volledige lijst met velden configureerbare en beschrijvingen is beschikbaar in de [Automatisch schalen REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Zie voor voorbeelden van programmacode

* [Geavanceerde automatisch schalen configuratie met resourcemanager sjablonen voor VM schaal Sets](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Automatisch schalen REST API](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Horizontale tegenover verticaal schalen

Automatisch schalen Hiermee verhoogt u resources in alleen schaal horizontaal, die een grotere ('uit') of ('in') in het aantal exemplaren VM verkleinen.  Horizontaal schaalbaarheid, dat wil flexibeler in een situatie cloud zeggen omdat u om uit te voeren mogelijk duizenden VMs worden afgehandeld laden. Verticale schaalbaarheid verschilt. Deze hetzelfde aantal VMs blijft, maar zorgt ervoor dat de VM ("omhoog") meer of minder ("omlaag") krachtige. Power wordt gemeten in het geheugen, processorsnelheid, schijfruimte, enzovoort.  Verticale schaalbaarheid heeft meer beperkingen. Dit is afhankelijk van de beschikbaarheid van grotere hardware die kan verschillen per regio en snel treffers en bovengrens. Verticale schaalbaarheid vereist ook meestal een VM stoppen en starten. Zie [verticaal schalen Azure virtuele machines met Azure automatisering](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md)voor meer informatie.


## <a name="methods-of-access"></a>Methoden van access
U kunt automatisch schalen via instellen

- [Azure-portal](insights-how-to-scale.md)
- [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Meerdere platforms opdrachtregelinterface)](insights-cli-samples.md#autoscale )
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Ondersteunde services voor automatisch schalen


| Service                              | Schema en documenten                                       |
|--------------------------------------|-----------------------------------------------------|
| WebApps                             | [Schaalbaarheid van WebApps](insights-how-to-scale.md)              |
| Cloudservices                       | [Automatisch een Cloudservice schalen](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuele Machines: klassieke           | [Klassieke VM beschikbaarheid Sets schaalbaarheid](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuele Machines: Windows schaal Sets| [VM schaal schaalbaarheid Hiermee stelt u in Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Virtuele Machines: Hiermee stelt u Linux schaal  | [VM schaal schaalbaarheid Hiermee stelt u in Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuele Machines: Windows-voorbeeld   | [Geavanceerde automatisch schalen configuratie met resourcemanager sjablonen voor VM schaal Sets](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over automatisch schalen, gebruik de Autoscale Walkthroughs eerder is vermeld of verwijzen naar de volgende bronnen:

- [Azure Monitor automatisch schalen algemene doelstellingen](insights-autoscale-common-metrics.md)
- [Aanbevolen procedures voor het automatisch schalen Azure Monitor](insights-autoscale-best-practices.md)
- [Automatisch schalen acties gebruiken voor het verzenden van e-mail en webhook waarschuwingen](insights-autoscale-to-webhook-email.md)
- [Automatisch schalen REST API](https://msdn.microsoft.com/library/dn931953.aspx)
- [Probleemoplossing VM schaal Sets automatisch schalen](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
