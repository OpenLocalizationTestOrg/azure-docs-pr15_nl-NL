
<properties
   pageTitle="Internet die tegenover elkaar liggen laden de verdeling van overzicht | Microsoft Azure "
   description="Overzicht voor Internet die tegenover elkaar liggen taakverdeling en de bijbehorende functies. Hoe werkt een taakverdeling voor Azure virtuele machines en cloudservices gebruiken."
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


# <a name="internet-facing-load-balancer-overview"></a>Internet omlaag laden de verdeling van overzicht

Azure taakverdeling kaarten het openbare IP-adres en poort aantal binnenkomende verkeer naar het IP-adres en poort privénummer van de virtuele machine en vice versa voor het verkeer antwoord van de virtuele machine. Laden taakverdeling regels kunt u distributie van specifieke soorten verkeer tussen meerdere virtuele machines of services. Bijvoorbeeld, kunt u het selectievakje laden web verzoek om verkeer spreiden over meerdere endwebservers of web rollen.

Voor een cloudservice die exemplaren van web rollen of werknemer rollen bevat, kunt u een openbare eindpunt definiëren in de service-definitiebestand (.csdef).

Het bestand _servicedefinition.csdef_ bevat de eindpuntconfiguratie en wordt als er meerdere exemplaren van de rol van een rol web of werknemer implementatie, verdeling van de belasting voor deze. De manier waarop exemplaren toevoegen aan uw cloud-implementatie is het aantal exemplaar op het configuratiebestand service (.csfg) wijzigen.

De volgende afbeelding ziet een eindpunt taakverdeling voor versleutelde webverkeer die wordt gedeeld in drie virtuele machines voor de openbare en persoonlijke TCP-poort van 443. Deze drie virtuele machines zijn opgeslagen in een set verdeeld.

![Voorbeeld van de verdeling van openbare laden](./media/load-balancer-internet-overview/IC727496.png))

Afbeelding 1 - eindpunt taakverdeling voor versleutelde webverkeer

Wanneer Internet-clients webpagina aanvragen voor het openbare IP-adres van de cloudservice op TCP-poort 443 verzendt, verdeelt taakverdeling Azure de aanvragen tussen de drie virtuele machines in de set verdeeld. Zie voor meer informatie over laden verdeling algoritmen, het [laden van pagina voor het overzicht van de verdeling](load-balancer-overview.md#load-balancer-features).

Standaard taakverdeling Azure netwerkverkeer gelijkmatig over meerdere exemplaren van virtuele machine. U kunt ook sessie affiniteit, voor meer informatie, Zie [laden gelijkmatige verdeling modus](load-balancer-distribution-mode.md)configureren.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [interne belasting voor documentconversies](load-balancer-internal-overview.md) beter te begrijpen welke taakverdeling is beter geschikt zijn voor de cloud-implementatie.

U kunt ook [aan de slag voor het maken van een internetverbinding van taakverdeling](load-balancer-get-started-internet-arm-ps.md) en welk type [verdeling modus](load-balancer-distribution-mode.md) voor een specifieke de verdeling van netwerkverkeer laadgedrag configureren.

Als uw toepassing verbindingen leven voor servers achter een taakverdeling behouden moet, kunt u meer weten over [niet-actieve TCP time-outinstellingen voor een taakverdeling](load-balancer-tcp-idle-timeout.md). Deze helpen voor meer informatie over niet-actieve Verbindingsgedrag wanneer u taakverdeling Azure gebruikt.
