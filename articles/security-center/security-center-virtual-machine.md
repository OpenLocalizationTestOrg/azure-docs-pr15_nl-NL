<properties
   pageTitle="Azure-Beveiligingscentrum en Azure virtuele Machines | Microsoft Azure"
   description="In dit document helpt u bij het begrijpen hoe Azure Beveiligingscentrum u Azure virtuele Machines kunt beschermen."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure-Beveiligingscentrum en Azure virtuele Machines

[Azure Beveiligingscentrum](https://azure.microsoft.com/services/security-center/) kunt u voorkomen dat, detecteren en reageren op threats. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

In dit artikel leest hoe Beveiligingscentrum kunt beveiligen van uw Azure virtuele Machines (VM).

## <a name="why-use-security-center"></a>Waarom Beveiligingscentrum gebruiken?

Beveiligingscentrum kunt u virtuele machine gegevens in Azure doordat inzicht in uw VM beveiligingsinstellingen. Wanneer Beveiligingscentrum uw VMs beveiligt, wordt de volgende mogelijkheden beschikbaar zijn:

- Beveiligingsinstellingen van besturingssysteem (OS) met de regels aanbevolen configuratie
- Systeem beveiligingsupdates en essentiële updates die verdwenen zijn
- Endpoint protection aanbevelingen
- Schijf versleuteling gegevensvalidatie
- Beveiligingsprobleem assessment en herstellen
- Bedreiging detectie

Naast het beveiligen van uw VMs Azure, biedt Beveiligingscentrum ook beveiliging voor controle en beheer voor Cloudservices, App-Services, virtuele netwerken en meer. 

>[AZURE.NOTE] Zie [Inleiding tot Azure Beveiligingscentrum](security-center-intro.md) voor meer informatie over Beveiligingscentrum Azure.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt beginnen met Azure Beveiligingscentrum, moet u kent en houd rekening met het volgende:

- U moet een abonnement op Microsoft Azure hebben. Zie [Beveiliging Center prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over de lagen gratis en de standaardversie van Beveiligingscentrum.
- Uw adoptie Beveiligingscentrum plannen, Zie [Azure Beveiligingscentrum planning en bedrijfsactiviteiten gids](security-center-planning-and-operations-guide.md) voor meer informatie over aandachtspunten voor de planning en bewerkingen.
- Zie voor informatie over besturingssysteem ondersteuning, [Azure Beveiligingscentrum Veelgestelde vragen (FAQ)](security-center-faq.md). 

## <a name="set-security-policy"></a>Beveiligingsbeleid instellen

Gegevens verzamelen moet worden ingeschakeld zodat deze Beveiligingscentrum Azure kunt verzamelen informatie die nodig is om te leveren aanbevelingen en waarschuwingen die zijn gegenereerd op basis van het beveiligingsbeleid dat u configureren. In de onderstaande afbeelding ziet u dat **het verzamelen van gegevens** is **ingeschakeld**.

Een beveiligingsbeleid definieert de reeks besturingselementen die worden aanbevolen voor bronnen binnen de opgegeven abonnement of de resourcegroep. Voordat u beveiligingsbeleid inschakelt, moet u verzamelen van gegevens is ingeschakeld, Beveiligingscentrum verzamelt gegevens uit uw virtuele machines om te kunnen hun beveiligingsstatus beoordelen, aanbevelingen voor beveiliging bieden en u attent maken op threats. In Beveiligingscentrum definieert u beleidsregels voor uw Azure abonnementen of resourcegroepen op basis van de behoeften van de beveiliging van uw bedrijf en het type van toepassingen of gevoeligheid van de gegevens in elk abonnement. 

![Beveiligingsbeleid](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Voor meer informatie over elke **beleid voor voorkoming van** beschikbaar, Zie artikel [beveiligingsbeleid voor apparaten instellen](security-center-policies.md) .

## <a name="manage-security-recommendations"></a>Aanbevelingen voor beveiliging beheren

Beveiligingscentrum analyseert de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden mogelijke beveiligingsrisico's aangegeven, wordt deze aanbevelingen gemaakt. De aanbevelingen helpt u bij het proces van het configureren van de benodigde besturingselementen.

Na het instellen van een beveiligingsbeleid, analyseert Beveiligingscentrum de beveiligingsstatus van uw resources identificeren mogelijke problemen. De aanbevelingen worden weergegeven in een tabel waarin elke regel één bepaalde aanbeveling vertegenwoordigt. De onderstaande tabel bevat enkele voorbeelden van aanbevelingen voor Azure VMs en wat elke doen als u deze hebt toegepast. Wanneer u een aanbeveling selecteert, krijgt u informatie die u hoe ziet u het implementeren van de aanbeveling in Beveiligingscentrum.

|Aanbeveling|Beschrijving|
|-----|-----|
|[Gegevens verzamelen voor abonnementen inschakelen](security-center-enable-data-collection.md)|Wordt aanbevolen dat u hebt ingeschakeld gegevensverzameling in het beveiligingsbeleid voor elk van uw abonnementen en alle virtuele machines (VMs) in uw abonnementen.|
|[Een OS problemen verhelpen](security-center-remediate-os-vulnerabilities.md)|Aanbevolen dat u uw configuraties OS met regels voor de aanbevolen configuratie uitlijnen bijvoorbeeld toegestaan niet wachtwoorden worden opgeslagen.|
|[Systeemupdates toepassen](security-center-apply-system-updates.md)|Raadt ontbrekende systeem beveiligingsupdates en essentiële updates naar VMs.|
|[Start opnieuw op na het systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates)|Wordt aanbevolen een VM om het proces van het toepassen van de systeemupdates te voltooien opnieuw te starten.|
|[Endpoint Protection installeren](security-center-install-endpoint-protection.md)|Wordt aanbevolen dat u antimalware-programma's voor VMs (alleen voor Windows-VMs) inrichten.|
|[Endpoint Protection systeemstatus waarschuwingen oplossen](security-center-resolve-endpoint-protection-health-alerts.md)|Wordt aanbevolen dat u endpoint protection fouten oplossen.|
|[VM Agent inschakelen](security-center-enable-vm-agent.md)|Kunt u zien welke VMs vereisen de VM-Agent. De VM-Agent moeten worden geïnstalleerd op VMs om te kunnen inrichten patch scannen, volgens de basislijn scannen en antimalware-programma's. De VM-Agent is al dan niet standaard geïnstalleerd voor VMs die zijn geïmplementeerd van Azure Marketplace. Het artikel [VM Agent en uitbreidingen – deel 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) vindt u informatie over het installeren van de VM-Agent.|
| [Schijfversleuteling toepassen](security-center-apply-disk-encryption.md) |Wordt aanbevolen dat u uw VM schijven Azure-schijf versleuteling gebruikt (Windows en Linux VMs) versleutelen. Versleuteling geschikt is voor de OS en de gegevens volumes op uw VM.|
| [Beveiligingsprobleem beoordeling is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) | Wordt aanbevolen dat u een oplossing van een beveiligingsprobleem assessment op uw VM installeren. |
| [Problemen verhelpen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Kunt u zien van de systeem- en problemen gedetecteerd door de beveiligingsprobleem assessment oplossing geïnstalleerd op uw VM. |

>[AZURE.NOTE] Voor meer informatie over aanbevelingen, Zie [aanbevelingen voor de beveiliging beheer](security-center-recommendations.md) artikel.

## <a name="monitor-security-health"></a>Monitor beveiliging systeemstatus

Nadat u [beveiligingsbeleid voor apparaten](security-center-policies.md) voor resources van een abonnement hebt ingeschakeld, wordt Beveiligingscentrum de beveiliging van uw resources identificeren potentiële problemen analyseren.  U kunt de beveiligingsstatus van uw bronnen, samen met eventuele problemen in het blad **Resource beveiliging status** weergeven. Wanneer u **virtuele machines** in de tegel van de servicestatus **resourcebeveiliging** klikt, wordt het blad **virtuele machines** met aanbevelingen voor uw VMs geopend. 

![Status van de beveiliging](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Beheren en reageren op beveiligingsmeldingen

Beveiligingscentrum automatisch worden verzameld, worden geanalyseerd en integreert logboekgegevens uit uw Azure resources, het netwerk en verbonden partneroplossingen (zoals firewall uit en endpoint protection oplossingen), om te detecteren reële threats en onrechte te verkleinen. Door gebruik te maken van een diverse aggregatie van [detectiemogelijkheden voor](security-center-detection-capabilities.md), kan Beveiligingscentrum genereren prioriteit beveiligingsmeldingen waarmee u kunt snel het probleem onderzoeken en vindt u aanbevelingen voor hoe u een mogelijke aanvallen te verhelpen.

![Beveiligingsmeldingen](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Selecteer een beveiligingswaarschuwing voor meer informatie over de gebeurtenis of gebeurtenissen die de waarschuwing en wat, geactiveerd als een, stappen u moet uitvoeren om te verhelpen van een aanval. Beveiligingsmeldingen zijn gegroepeerd op [type](security-center-alerts-type.md) en de datum.


## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
