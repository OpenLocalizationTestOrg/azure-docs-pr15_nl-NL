
<properties
   pageTitle="Interne taakverdeling overzicht | Microsoft Azure"
   description="Overzicht voor interne taakverdeling en de bijbehorende functies. De werking van een verdeling van de belasting voor Azure en mogelijke scenario's voor het configureren van interne eindpunten"
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


# <a name="internal-load-balancer-overview"></a>Overzicht van de verdeling van interne laden

In tegenstelling tot vervolgens Internet omlaag taakverdeling, de interne taakverdeling (ILB) wordt u omgeleid zodat verkeer alleen naar bronnen in de cloudservice of het gebruik van VPN voor toegang tot de Azure infrastructuur. De infrastructuur beperkt toegang tot de taakverdeling virtuele IP-adressen (VIP's) van een Cloudservice of een virtueel netwerk, zodat ze wordt nooit rechtstreeks zichtbaar voor een Internet-eindpunt. Hiermee worden interne lijn bedrijfstoepassingen (LOB) om te worden uitgevoerd in Azure en zijn toegankelijk vanaf de cloud of vanuit lokale bronnen.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Waarom moet u mogelijk een interne taakverdeling

Azure interne laden verdelen (ILB) biedt taakverdeling tussen virtuele machines die zich zich in een cloudservice of een virtueel netwerk met een regionale bereik bevinden. Zie voor informatie over het gebruik en configuratie van virtuele netwerken met een regionale bereik, [Regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in de blog van Azure. Bestaande virtuele netwerken die zijn geconfigureerd voor een groep affiniteit niet ILB gebruiken.

ILB schakelt de volgende typen van taakverdeling:

- Binnen een cloudservice, van virtuele machines naar een reeks virtuele machines die zich binnen de dezelfde cloudservice bevinden (Zie afbeelding 1).
- Binnen een virtueel netwerk, van virtuele machines in het virtuele netwerk naar een reeks virtuele machines die zich binnen de dezelfde cloudservice van de virtuele bevinden netwerk (Zie afbeelding 2).
- Voor een virtueel cross-premises-netwerk, van lokale computers naar een reeks virtuele machines die zich binnen de dezelfde cloudservice van de virtuele bevinden netwerk (Zie afbeelding 3).
- Internetgerichte toepassingen met meerdere lagen waarin de back-enddatabase lagen zijn niet internetgerichte maar vereisen van taakverdeling voor verkeer van de laag internetgerichte.
- Taakverdeling voor LOB-toepassingen die zijn ingesloten in een Azure zonder extra belasting voor gelijkmatige verdeling hardware en software. On-premises-servers op te nemen in de reeks computers waarvan u het verkeer laden is verdeeld.

## <a name="internet-facing-multi-tier-applications"></a>Internet die tegenover elkaar liggen toepassingen met meerdere niveaus


De weblaag Internet omlaag eindpunten heeft voor Internet-clients en maakt deel uit van een set verdeeld. De taakverdeling worden binnenkomende verkeer van webclients voor TCP-poort 443 (HTTPS) naar de webservers.

De databaseservers zijn achter een eindpunt ILB waarin de endwebservers voor de opslag gebruikt. Deze database service laden gelijkmatig eindpunt, welke verkeer gelijkmatig verdeeld over de database-servers in de set ILB is.

De volgende afbeelding ziet u de internetverbinding met meerdere niveaus toepassing binnen de dezelfde cloudservice.

Afbeelding 1 - Internet die tegenover elkaar liggen met meerdere niveaus-toepassing

![Interne taakverdeling één cloudservice](./media/load-balancer-internal-overview/IC736321.png)

Een andere mogelijke gebruiken voor een toepassing met meerdere niveaus is wanneer de ILB geïmplementeerd in een andere cloudservice dan de tabel door de service voor het ILB andere.

Cloudservices met hetzelfde virtuele netwerk hebben toegang tot het eindpunt ILB.

Afbeelding 2 wordt front web servers zich in een andere cloudservice van de database back-enddatabase en het gebruik van het eindpunt ILB in hetzelfde virtuele netwerk.

![Interne taakverdeling tussen cloudservices](./media/load-balancer-internal-overview/IC744147.png)

Afbeelding 2 - front-servers in een andere cloudservice

## <a name="intranet-line-of-business-applications"></a>Intranet-regel van zakelijke toepassingen

Verkeer van clients op de on-premises netwerk dan verdeeld over het instellen van LOB-servers via VPN-verbinding met Azure netwerk.

De clientcomputer heeft toegang tot een IP-adres van Azure VPN service met punt naar site VPN. Deze kan worden gebruikt de LOB-toepassing die worden gehost achter het eindpunt ILB.

![Interne taakverdeling punt naar site VPN gebruiken](./media/load-balancer-internal-overview/IC744148.png)

Afbeelding 3 - LOB-toepassingen die worden gehost achter het eindpunt kg

Een ander scenario voor de LOB is dat een VPN-site naar de site naar het virtuele netwerk waarin het eindpunt ILB is geconfigureerd. Hierdoor wordt de netwerkverkeer on-premises implementatie moet worden doorgestuurd naar het eindpunt ILB.

![Interne taakverdeling via VPN van de site naar site](./media/load-balancer-internal-overview/IC744150.png)

Afbeelding 4 - On-premises netwerkverkeer doorgestuurd naar het eindpunt ILB


## <a name="next-steps"></a>Volgende stappen

[Azure resourcemanager ondersteuning voor taakverdeling van Azure](load-balancer-arm.md)

[Aan de slag een Internet die tegenover elkaar liggen taakverdeling configureren](load-balancer-get-started-internet-arm-ps.md)

[Een interne taakverdeling configureren aan de slag](load-balancer-get-started-ilb-arm-ps.md)

[Een modus voor het laden gelijkmatige verdeling configureren](load-balancer-distribution-mode.md)

[Niet-actieve TCP time-outinstellingen voor uw taakverdeling configureren](load-balancer-tcp-idle-timeout.md)

