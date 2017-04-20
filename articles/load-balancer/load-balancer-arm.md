<properties
   pageTitle="Azure resourcemanager ondersteuning voor taakverdeling | Microsoft Azure "
   description="Powershell gebruiken voor taakverdeling met Azure Resource Manager. Sjablonen gebruiken voor de verdeling van belasting"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Gebruik van Azure resourcemanager ondersteuning met Azure taakverdeling

Azure resourcemanager is de voorkeur management framework voor services in Azure wordt aangegeven. Azure taakverdeling kunnen worden beheerd via Azure resourcemanager gebaseerde API's en hulpprogramma's voor.

## <a name="concepts"></a>Concepten

Met resourcemanager Azure taakverdeling bevat de volgende bronnen voor de onderliggende:

- Front IP-configuratie – een taakverdeling kan een of meer front-end IP-adressen, ook bekend als een virtuele IP-adressen (VIP's) bevatten. Deze IP-adressen fungeren als ingress voor het verkeer.

- Groep met back-enddatabase adressen – hierna ziet u IP-adressen die is gekoppeld met de VM netwerk Interface kaart (NIC) waaraan belasting wordt verdeeld.

- Laden van taakverdeling regels: een regeleigenschap kaarten poortcombinatie van een bepaald front-end IP- en poortcombinatie tot een set met back-end IP-adressen en. Een enkel taakverdeling kan meerdere regels voor taakverdeling hebben. Elke regel is een combinatie van een front IP- en poort en back-enddatabase IP- en poort die is gekoppeld aan VMs.

- Sondes – sondes u in staat stellen om de status van de exemplaren die VM bij te houden. Als een systeemstatus-test is mislukt, het exemplaar VM beschouwd afmelden bij de draaiing automatisch.

- Regels voor binnenkomende NAT – NAT-regels definiëren het binnenkomende verkeer die doorloopt tot en met de voorgrond IP stopt en verdeeld over de back-end-IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>QuickStart sjablonen

Azure resourcemanager kunt u voor het inrichten van uw toepassingen met een declaratieve sjabloon. In een enkele sjabloon, kunt u meerdere services samen met de bijbehorende afhankelijkheden implementeren. Dezelfde sjabloon kunt u uw toepassing tijdens elke fase van de levenscyclus van de toepassing net zo vaak implementeren.

Sjablonen kunnen definities voor virtuele Machines, virtuele netwerken beschikbaarheid Sets, netwerk-Interfaces (NIC's), opslag-Accounts, netwerktaakverdelers, netwerk-beveiligingsgroepen en openbare IP-adressen bevatten. Met sjablonen kunt u alles wat die u nodig voor een complexe toepassing hebt maken. Het sjabloonbestand kan worden gecontroleerd in inhoudsbeheersysteem voor versiebeheer en samenwerking.

[Meer informatie over sjablonen](http://go.microsoft.com/fwlink/?LinkId=544798)

[Meer informatie over het netwerk Resources](../virtual-network/resource-groups-networking.md)

QuickStart sjablonen met Azure taakverdeling vindt u in een [bibliotheek GitHub](https://github.com/Azure/azure-quickstart-templates) hostingprovider een reeks gegenereerd community-sjablonen.

Voorbeelden van sjablonen:

- [2 VMs in een taakverdeling en de regels van taakverdeling](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 VMs in een VNET met een interne taakverdeling en taakverdeling regels](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 VMs in een taakverdeling en NAT regels op de kg configureren](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Bij het instellen van taakverdeling Azure met een PowerShell of CLI

Aan de slag met Azure resourcemanager cmdlets, hulpmiddelen voor de opdrachtregel en REST API 's

- [Cmdlets voor Azure toegang](https://msdn.microsoft.com/library/azure/mt163510.aspx) kan worden gebruikt om te maken van een taakverdeling.
- [Het maken van een taakverdeling met Azure Resource Manager](load-balancer-get-started-ilb-arm-ps.md)
- [Gebruik van de Azure CLI met Azure resourcebeheer](../xplat-cli-azure-resource-manager.md)
- [De belasting voor documentconversies REST API 's](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Volgende stappen

U kunt ook [aan de slag voor het maken van een internetverbinding van taakverdeling](load-balancer-get-started-internet-arm-ps.md) en welk type [verdeling modus](load-balancer-distribution-mode.md) voor een specifieke de verdeling van netwerkverkeer laadgedrag configureren.

Informatie over het beheren van [niet-actieve TCP time-outinstellingen voor een taakverdeling](load-balancer-tcp-idle-timeout.md). Dit is belangrijk wanneer uw toepassing moet verbindingen leven voor servers achter een taakverdeling behouden.
