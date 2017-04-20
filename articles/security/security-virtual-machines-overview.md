<properties
   pageTitle="Overzicht van de beveiliging Azure virtuele Machines | Microsoft Azure"
   description=" Azure virtuele Machines flexibiliteit de van virtualization zonder te kopen en voor het behoud van de fysieke hardware die wordt uitgevoerd de virtuele machine.  Dit artikel bevat een overzicht van de core Azure beveiligingsfuncties die kunnen worden gebruikt met Azure virtuele Machines. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Overzicht van de beveiliging Azure virtuele Machines

Azure virtuele Machines kunt u een groot aantal oplossingen computing in een agile manier implementeren. Met ondersteuning voor Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP en Azure BizTalk-Services, kunt u een werkbelasting en een taal op vrijwel elk besturingssysteem implementeren.

Een Azure virtuele machines hebt u de flexibiliteit van virtualization zonder te kopen en voor het behoud van de fysieke hardware die wordt uitgevoerd de virtuele machine.  U kunt bouwen en implementeren van uw toepassingen met de assurance dat uw gegevens beveiligd en veilig in onze zeer veilige datacenters is.

Met Azure, kunt u die beveiligde, compatibele oplossingen bouwen:

- Uw virtuele machines beschermen tegen virussen en schadelijke software
- Uw gevoelige gegevens versleutelen
- Beveiligd netwerkverkeer
- Identificeren en threats detecteren
- Voldoet aan nalevingsvereisten

Het doel van dit artikel is voor een overzicht van de core Azure beveiligingsfuncties die kunnen worden gebruikt met virtuele machines. We bieden ook koppelingen naar artikelen met details van elke functie hebt doorgegeven zodat u kunt meer informatie.  

De core Azure virtuele machines beveiligingsmogelijkheden om te worden beschreven in dit artikel:

- Antimalware
- Hardware Security Module
- VM schijfversleuteling
- VM back-up maken
- Herstel van Azure-Site
- Virtuele netwerken
- Beheer van beveiligingsbeleid en rapporteren
- Naleving

## <a name="antimalware"></a>Antimalware

Met Azure, kunt u de software antimalware van beveiliging leveranciers zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky uw virtuele machines beschermen tegen schadelijke bestanden, adware en andere threats. Zie de sectie informatie onderstaande artikelen op partner om oplossingen te vinden.

Microsoft Antimalware voor Azure-Cloudservices en virtuele Machines is een mogelijkheid realtime beveiliging waarmee u kunt identificeren en virussen, spyware en andere kwaadaardige software verwijderen.  Microsoft Antimalware biedt configureerbare waarschuwingen wanneer bekende schadelijke of ongewenste software probeert te installeren zelf of op uw Azure systemen worden uitgevoerd.

Microsoft Antimalware is een enkel-agent-oplossing voor toepassingen en tenant omgevingen, ontworpen om uit te voeren op de achtergrond zonder menselijke tussenkomst. Beveiliging op basis van de behoeften van uw toepassing-werkbelastingen, met een eenvoudige secure--standaard of geavanceerde aangepaste configuratie, inclusief antimalware monitoring, kunt u implementeren.

Wanneer u implementeren en Microsoft Antimalware inschakelt, wordt de volgende core-functies zijn beschikbaar:

- Realtime bescherming - beeldschermen activiteit in de Cloud Services en klik op virtuele Machines detecteren en blokkeren malware worden uitgevoerd.
- Geplande scannen - uitvoert regelmatig gerichte scannen om op te sporen schadelijke software, waaronder programma's actief wordt uitgevoerd.
- Malware remediation - krijgt automatisch actie gevonden schadelijke software, zoals het verwijderen of schadelijke bestanden in quarantaine plaatsen en opschonen van schadelijke registervermeldingen.
- Updates van de handtekening - automatisch installaties de meest recente beveiliging handtekeningen (virusdefinities) ter bescherming is bijgewerkt op een vooraf bepaalde frequentie.
- Antimalware-Engine automatisch wordt bijgewerkt, werkt de Microsoft Antimalware-engine.
- Antimalware Platform automatisch wordt bijgewerkt – het platform Microsoft Antimalware wordt bijgewerkt.
- Actieve beveiliging - rapporten in Azure telemetrielogboek metagegevens over gevonden threats en verdachte bronnen om ervoor te zorgen snelle respons en realtime synchroon handtekening aflevering van toepassingen via de Microsoft Active beveiliging systeem (kaarten).
- Voorbeelden rapportage - biedt en voorbeelden rapporten met de service Microsoft Antimalware om te helpen Verfijn de service en probleemoplossing inschakelen.
- Uitsluitingen – kunnen-toepassing en servicebeheerders voor het configureren van bepaalde bestanden, processen, en tot deze uitsluiten van beveiliging en het zoeken naar de prestaties en de andere.
- Antimalware gebeurtenis verzamelen - records van de servicestatus antimalware, verdachte activiteiten en remediation acties die u hebt gemaakt in het gebeurtenislogboek van besturingssysteem en ze worden verzameld in de opslag van Azure-account van de klant.

Meer informatie: meer informatie over antimalware software voor het beveiligen van uw virtuele machines, Zie:

- [Microsoft Antimalware voor Azure Cloudservices en virtuele Machines](../security/azure-security-antimalware.md)
- [Implementatie van oplossingen Antimalware op Azure virtuele Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Het installeren en configureren van Trend Micro uitgebreide Security als een Service op een Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Het installeren en configureren van Symantec Endpoint Protection op een Windows-VM](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nieuwe Antimalware opties voor het beschermen van Azure virtuele Machines – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Beveiligingsoplossingen in het Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardwarebeveiliging Module

Versleuteling en verificatie Verbeter niet beveiliging tenzij de sleutels zelf zijn beveiligd. U kunt de management en de beveiliging van uw kritieke geheimen en sleutels vereenvoudigen door ze te slaan in Azure-toets kluis. Toets kluis biedt de optie voor de opslag van uw toetsen in hardware beveiligingsmodules (HSM's) gecertificeerd voor FIPS 140-2-standaarden voor niveau 2. Uw SQL Server-versleutelingssleutels voor back-up of [transparante gegevensversleuteling](https://msdn.microsoft.com/library/bb934049.aspx) kunnen alle worden opgeslagen in sleutel kluis met alle sleutels of geheimen vanuit uw toepassingen. Machtigingen en toegang tot deze beveiligde items worden beheerd via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Meer informatie:

- [Wat is Azure-toets kluis?](../key-vault/key-vault-whatis.md)
- [Aan de slag met Azure-toets kluis](../key-vault/key-vault-get-started.md)
- [Azure-toets kluis-blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>VM schijfversleuteling

Azure schijfversleuteling is een nieuwe mogelijkheid waarmee u uw Windows- en Linux Azure virtuele machines schijven versleutelen. Azure schijf-versleuteling gebruikt de functie standaard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) industrie van Windows en de functie [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) van Linux voor volume-versleuteling voor het besturingssysteem en de gegevensschijven.

De oplossing is geïntegreerd met Azure-toets kluis om te bepalen en de schijf versleuteling toetsen en geheimen in uw belangrijkste kluis-abonnement en ervoor zorgen dat dat alle gegevens in de virtuele machine schijven worden gecodeerd in rust in Azure opslag beheren.

Meer informatie:

- [Azure schijfversleuteling voor Windows en Linux IaaS VMs](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Azure schijfversleuteling voor Linux en Windows virtuele Machines](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Een virtuele machine versleutelen](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>VM back-up maken

Azure back-up is een scalable oplossing die uw toepassingsgegevens met de minimale gebruikskosten en nul hoofdletter investering beschermen. Toepassingsfouten kunnen uw gegevens beschadigd raken en menselijke fouten bugs bij te werken kunnen introduceren in uw toepassingen. Met back-up van Azure, zijn uw virtuele machines met Windows en Linux beveiligd.

Meer informatie:

- [Wat Azure back-up is?](../backup/backup-introduction-to-azure-backup.md)
- [Azure back-up leerpad](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure back-Service - Veelgestelde vragen](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Herstel van Azure-Site

Een belangrijk onderdeel van van uw organisatie BCDR strategie is hoe bijhouden zakelijke werkbelasting en apps en worden uitgevoerd wanneer bijvoorbeeld geplande en ongeplande plaatsvinden. Azure Site herstel helpt goedkeuringen herhaling, failover en herstel van de werkbelasting en apps zodat ze beschikbaar vanaf een secundaire locatie zijn als uw hoofdlocatie bent uitvalt.

Herstel van de site:

- **Eenvoudiger uw strategie BCDR** , herstel van de Site kunt u heel gemakkelijk afhandelen herhaling, failover en herstel van meerdere business werkbelasting en apps vanaf één locatie. Site-herstel orchestrates herhaling en overname bij storing, maar niet snijpunt van de toepassingsgegevens van uw of relevante informatie hierover.
- **Hiermee beschikt u over flexibele herhaling** , Site-herstel gebruiken kunt u werkbelasting Hyper-V virtuele machines, VMware virtuele machines en Windows/Linux fysieke servers waarop repliceren.
- **Ondersteunt failover en herstel** , Site-herstel biedt test failovers voor noodgevallen herstel boren handhaven productieomgevingen. U kunt ook geplande failovers met een nul-gegevensverlies voor verwachte bijvoorbeeld of niet-geplande failover uitvoeren met minimale gegevensverlies (afhankelijk van de frequentie van replicatie) voor onverwachte systeemfouten. Na een failover kunt u failback aan uw primaire-sites. Site-herstel biedt herstel-abonnementen die opnemen kunnen scripts en Azure automatisering werkmappen, zodat u failover en herstel van toepassingen met meerdere niveaus aanpassen kunt.
- **Hiermee secundaire datacenter** , kunt u een secundaire on-premises-site of Azure repliceren. Met behulp van Azure als bestemming voor herstel, neemt de kosten en de complexiteit van een secundaire site onderhouden. Gerepliceerde gegevens worden opgeslagen in Azure opslag.
- **Integrates met bestaande BCDR technologieën** , Site-herstel samen met andere functies van de BCDR toepassing. Bijvoorbeeld, kunt u herstel van de Site te beveiligen van de SQL Server-back-end van zakelijke werkbelasting. Dit geldt ook voor ondersteuning voor SQL Server AlwaysOn voor het beheren van de overname van van de beschikbaarheidsgroepen.

Meer informatie:

- [Wat is herstel van Azure-Site?](../site-recovery/site-recovery-overview.md)
- [Hoe werkt herstel van Azure-Site?](../site-recovery/site-recovery-components.md)
- [Wat werkbelasting zijn beveiligd met DRM herstel van Azure-Site?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuele netwerken

Virtuele machines moet netwerkconnectiviteit. Als u wilt dat dit vereiste wordt ondersteund, vereist Azure virtuele machines kunnen niet worden verbonden met een Azure Virtual Network. Een Azure Virtual Network is een logische constructie gebouwd de fysieke Azure netwerk-structuur. Elke logische Azure Virtual Network is geïsoleerd van alle andere Azure virtuele netwerken. Deze moeten worden geïsoleerd helpt ervoor zorgen dat netwerkverkeer in uw implementaties niet toegankelijk zijn voor andere Microsoft Azure-klanten is.

Meer informatie:

- [Overzicht van de Azure netwerk-beveiliging](security-network-overview.md)
- [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
- [Netwerkfuncties en samenwerking voor Enterprise-scenario 's](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Beheer van beveiligingsbeleid en rapporteren

Azure Beveiligingscentrum kunt u voorkomen dat, detecteren en reageren op threats en vindt dat u meer inzicht in, en controle over, de beveiliging van uw Azure resources. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

Azure Beveiligingscentrum kunt u optimaliseren en VM beveiliging door te controleren:

- VM [aanbevelingen voor beveiliging](../security-center/security-center-recommendations.md) , zoals leveren systeemupdates toepassen, ACL's eindpunten configureren antimalware inschakelen, netwerk beveiligingsgroepen inschakelen en schijfversleuteling toepassen.
- De status van uw virtuele machines bewaken

Meer informatie:

- [Inleiding tot Azure Beveiligingscentrum](../security-center/security-center-intro.md)
- [Azure Beveiligingscentrum Veelgestelde vragen](../security-center/security-center-faq.md)
- [Azure Beveiligingscentrum Planning en bedrijfsactiviteiten](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Naleving

Azure virtuele Machines is gecertificeerd voor FISMA, FedRAMP HIPAA, PCI DSS niveau 1 en andere belangrijke naleving-programma's. Deze certificering gemakkelijker voor uw eigen Azure toepassingen moeten voldoen aan vereisten en voor uw bedrijf om tot een oplossing van een breed scala van nationale en internationale wettelijke vereisten.

Meer informatie:

- [Vertrouwenscentrum in Microsoft: naleving](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Vertrouwde Cloud: Microsoft Azure-beveiliging, Privacy en naleving](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
