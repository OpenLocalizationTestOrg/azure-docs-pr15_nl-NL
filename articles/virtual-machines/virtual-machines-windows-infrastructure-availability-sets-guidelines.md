<properties
    pageTitle="Beschikbaarheid instellen richtlijnen | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren richtlijnen voor het implementeren van beschikbaarheid Sets in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="availability-sets-guidelines"></a>Beschikbaarheid stelt richtlijnen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

In dit artikel ligt de nadruk met informatie over de vereiste stappen van de planning voor beschikbaarheid sets om ervoor te zorgen uw toepassingen van toegankelijke tijdens of niet gepland gebeurtenissen blijft.

## <a name="implementation-guidelines-for-availability-sets"></a>Implementatie van richtlijnen voor beschikbaarheid sets

Beslissingen:

- Hoeveel beschikbaarheid sets hebt u nodig voor de verschillende rollen en niveaus in de infrastructuur van uw toepassing?

Taken:

- Het aantal VMs definiëren in elke toepassingslaag die u nodig hebt.
- Bepalen of u kunt het aantal foutenstructuuranalyse aanpassen of bijwerken van domeinen moet worden gebruikt voor uw toepassing nodig.
- De beschikbaarheid van de vereiste sets met uw naamgevingsconventie en wat definiëren VMs bevinden zich in deze. Een VM kan alleen worden opgenomen in één beschikbaarheid instellen. 

## <a name="availability-sets"></a>Beschikbaarheid van sets

In Azure kunnen virtuele machines (VMs) worden geplaatst naar een logische groepering een set beschikbaarheid genoemd. Wanneer u VMs binnen een set beschikbaarheid maakt, worden de plaatsing van deze VMs door het Azure platform over de onderliggende infrastructuur verdeeld. Moet ik een gebeurtenis gepland onderhoud aan het Azure platform of een onderliggende hardware / infrastructuur foutenstructuuranalyse, het gebruik van beschikbaarheid sets zorgt ervoor dat ten minste één VM actief blijft.

Als een goede gewoonte toepassingen moeten bevindt zich niet op een enkele VM. Een set beschikbaarheid met een enkel VM krijgen niet alle bescherming tegen of niet gepland gebeurtenissen binnen het Azure platform. De [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) vereist twee of meer VMs binnen een beschikbaarheid toe te staan dat de verdeling van VMs in de onderliggende infrastructuur instellen.

De onderliggende Azure-infrastructuur is verdeeld domeinen en domeinen voor foutenstructuuranalyse bijwerken. Deze domeinen zijn gedefinieerd door welke hosts delen een gemeenschappelijke update cyclus of delen dezelfde fysieke infrastructuur zoals power en netwerken. Azure distribueert automatisch uw VMs binnen een beschikbaarheid ingesteld voor domeinen op voor het behoud van beschikbaarheid en fouttolerantie. Afhankelijk van de grootte van uw toepassing en het aantal VMs binnen een set beschikbaarheid, kunt u het aantal domeinen die u wilt gebruiken. U vindt meer informatie over [beheren beschikbaarheid en het gebruik van domeinen voor bijwerken en foutenstructuuranalyse](virtual-machines-windows-manage-availability.md).

Bij het ontwerpen van de infrastructuur van uw toepassing, u moet ook van plan bent de lagen die u gebruikt. Groep VMs die hetzelfde doel in beschikbaarheid dienen wordt ingesteld, zoals een beschikbaarheid instellen voor uw front VMs waarop IIS wordt uitgevoerd. Maak een afzonderlijke beschikbaarheid instellen voor uw back-enddatabase VMs met SQL Server. Het doel is om ervoor te zorgen dat elk onderdeel van uw toepassing is uitgeschakeld door een set beschikbaarheid en ten minste eenmaal exemplaar altijd niet beëindigd.

Netwerktaakverdelers kunnen worden gebruikt voor elke toepassingslaag kunt werken samen met een set beschikbaarheid en te zorgen dat netwerkverkeer kan altijd worden doorgestuurd naar een actief exemplaar. Zonder een taakverdeling uw VMs mogelijk afgebroken overal in de geplande en niet-gepland onderhoud gebeurtenissen, maar uw eindgebruikers mogelijk niet voor het oplossen van deze als de primaire VM niet beschikbaar is.


## <a name="next-steps"></a>Volgende stappen
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 