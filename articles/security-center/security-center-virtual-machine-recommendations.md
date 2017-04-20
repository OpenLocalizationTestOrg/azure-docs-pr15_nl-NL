<properties
   pageTitle="Beveiligen van uw virtuele machines in Azure Beveiligingscentrum | Microsoft Azure"
   description="In dit document adressen aanbevelingen in Azure Beveiligingscentrum die u helpen beveiligen uw virtuele machines en blijf overeenkomstig de beleidsregels van beveiligingsbeleid voor apparaten."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Beveiligen van uw virtuele machines in Azure Beveiligingscentrum

Azure Beveiligingscentrum analyseert de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden aangegeven mogelijke beveiligingsrisico's, wordt u begeleid bij het proces van het configureren van de benodigde besturingselementen aanbevelingen.  Aanbevelingen hebben betrekking op Azure resourcetypen: virtuele machines (VMs), netwerken, SQL en toepassingen.

In dit artikel biedt aanbevelingen die van toepassing op VMs.  VM aanbevelingen center rond gegevensverzameling, systeemupdates, antimalware, inrichting toepassen coderen uw schijven VM en meer.  De volgende tabel als een overzicht gebruiken om u te helpen u meer informatie over de beschikbare VM aanbevelingen en wat elke doen als u deze hebt toegepast.

## <a name="available-vm-recommendations"></a>Beschikbare VM aanbevelingen

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
| [Versie van het besturingssysteem bijwerken](security-center-update-os-version.md) | Wordt aanbevolen dat u de versie van het besturingssysteem (OS) voor uw Cloudservice naar de meest recente versie beschikbaar voor uw gezin OS bijwerkt.  Zie voor meer informatie over Cloud Services, de [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md). |
| [Beveiligingsprobleem beoordeling is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) | Wordt aanbevolen dat u een oplossing van een beveiligingsprobleem assessment op uw VM installeren. |
| [Problemen verhelpen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Kunt u zien van de systeem- en problemen gedetecteerd door de beveiligingsprobleem assessment oplossing geïnstalleerd op uw VM. |

## <a name="see-also"></a>Zie ook

Zie de volgende onderwerpen voor meer informatie over aanbevelingen die van toepassing op andere resourcetypen Azure:

- [Beveiligen van uw toepassingen in Azure Beveiligingscentrum](security-center-application-recommendations.md)
- [Beveiligen van uw netwerk in Azure Beveiligingscentrum](security-center-network-recommendations.md)
- [De SQL Azure-service in Azure Beveiligingscentrum beveiligen](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) --meer informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) --meer informatie over het beheren en reageren op beveiligingsmeldingen.
- [Veelgestelde vragen over azure beveiliging beheercentrum](security-center-faq.md) --zoeken vaak Veelgestelde vragen over het gebruik van de service.
