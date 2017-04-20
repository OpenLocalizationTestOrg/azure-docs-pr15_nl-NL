<properties
   pageTitle="Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren | Microsoft Azure"
   description="In dit document leert u hoe aanbevelingen in Azure Beveiligingscentrum kunnen beveiligen van uw Azure resources en blijf overeenkomstig de beleidsregels van beveiligingsbeleid voor apparaten."
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

# <a name="managing-security-recommendations-in-azure-security-center"></a>Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren

In dit document begeleidt u bij het gebruik van aanbevelingen in Azure Beveiligingscentrum kunt u uw Azure resources beveiligen.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.  Dit is niet een stapsgewijze handleiding.

## <a name="what-are-security-recommendations"></a>Wat zijn aanbevelingen voor beveiliging?
Beveiligingscentrum analyseert regelmatig de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden mogelijke beveiligingsrisico's aangegeven, wordt deze aanbevelingen gemaakt. De aanbevelingen helpt u bij het proces van het configureren van de benodigde besturingselementen.

## <a name="implementing-security-recommendations"></a>Aanbevelingen voor beveiliging implementeren

### <a name="set-recommendations"></a>Set aanbevelingen

In de [instelling beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum](security-center-policies.md)leert u naar:

- Beveiligingsbeleid voor apparaten configureren.
- Gegevens verzamelen inschakelen.
- Kies welke aanbevelingen om te zien als onderdeel van uw beveiligingsbeleid.

Huidige beleid aanbevelingen center rond systeemupdates, volgens de basislijn regels, antimalware-programma's, [netwerk-beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) op subnetten en netwerkinterfaces, controle van de SQL-database SQL database-transparante gegevensversleuteling en web application firewalls.  [Beveiligingsbeleid voor apparaten instellen](security-center-policies.md) , vindt u een beschrijving van elke optie aanbeveling.

### <a name="monitor-recommendations"></a>Monitor aanbevelingen
Na het instellen van een beveiligingsbeleid, analyseert Beveiligingscentrum de beveiligingsstatus van uw resources identificeren mogelijke problemen. De tegel **aanbevelingen** op het blad **Beveiligingscentrum** kunt u het totale aantal aanbevelingen die wordt aangeduid met Beveiligingscentrum weten.

![Aanbevelingen-tegel][1]

Bekijk de details van elke aanbeveling:

1. Klik op de **tegel aanbevelingen** op het blad **Beveiligingscentrum** . Hiermee opent u het blad **aanbevelingen** .

De aanbevelingen worden weergegeven in een tabel waarin elke regel één bepaalde aanbeveling vertegenwoordigt. De kolommen van deze tabel zijn:

- **Beschrijving**: dit artikel wordt uitgelegd de aanbeveling en wat u moet worden uitgevoerd om deze op te lossen.
- **RESOURCE**: ziet u de bronnen waarop deze aanbeveling van toepassing is.
- **De staat**: Beschrijving van de huidige status van de aanbeveling:
    - **Open**: de aanbeveling nog niet is verholpen.
    - **In uitvoering**: de aanbeveling momenteel wordt toegepast op de resources en geen actie is vereist voor u.
    - **Opgelost**: de aanbeveling is al voltooid (in dit geval de regel grijs weergegeven).
- **ERNST**: Beschrijving van de ernst van die bepaalde aanbeveling:
    - **Hoog**: een beveiligingsprobleem bestaat met een zinvolle resource (zoals een toepassing, een VM of een beveiligingsgroep netwerk) en aandacht vereist.
    - **Gemiddeld**: een beveiligingsprobleem bestaat en niet-kritieke of extra stappen zijn vereist om deze te of om een proces te voltooien.
    - **Laag**: een beveiligingsprobleem bestaat die moeten worden behandeld, maar geen onmiddellijke aandacht vereist. (Standaard lage aanbevelingen worden niet weergegeven, maar u kunt filteren op lage aanbevelingen als u wilt zien ze.)

De volgende tabel als een overzicht gebruiken om u te helpen u meer informatie over de beschikbare aanbevelingen en wat elke doen als u deze hebt toegepast.

> [AZURE.NOTE] U wilt meer informatie over de [klassieke en resourcemanager implementatiemodellen](../azure-classic-rm.md) voor Azure resources.

|Aanbeveling|Beschrijving|
|-----|-----|
|[Gegevens verzamelen voor abonnementen inschakelen](security-center-enable-data-collection.md)|Wordt aanbevolen dat u hebt ingeschakeld gegevensverzameling in het beveiligingsbeleid voor elk van uw abonnementen en alle virtuele machines (VMs) in uw abonnementen.|
|[Een OS problemen verhelpen](security-center-remediate-os-vulnerabilities.md)|Aanbevolen dat u uw configuraties OS met regels voor de aanbevolen configuratie uitlijnen bijvoorbeeld toegestaan niet wachtwoorden worden opgeslagen.|
|[Systeemupdates toepassen](security-center-apply-system-updates.md)|Raadt ontbrekende systeem beveiligingsupdates en essentiële updates naar VMs.|
|[Start opnieuw op na het systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates)|Wordt aanbevolen een VM om het proces van het toepassen van de systeemupdates te voltooien opnieuw te starten.|
|[Een web application-firewall toevoegen](security-center-add-web-application-firewall.md)|Raadt een firewall-toepassing (WAF) voor web eindpunten. U kunt meerdere webtoepassingen in Beveiligingscentrum beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. WAF toestellen (gemaakt met het implementatiemodel resourcemanager-) moeten worden geïmplementeerd op een aparte virtueel netwerk. WAF toestellen (gemaakt met het klassieke implementatiemodel) zijn beperkt tot het gebruik van de beveiligingsgroep van een netwerk. In de toekomst wordt deze ondersteuning uitgebreid voor een volledig aangepast distributie van een toestel WAF (klassieke). Beveiligingscentrum wordt aangeraden het inrichten van een WAF om te beschermen tegen aanvallen hebt samengesteld uw webtoepassingen op VMs en klik op App-Service-omgeving (ASE). Meer informatie over ASE, raadpleegt u de [App omgeving servicedocumentatie](../app-service/app-service-app-service-environments-readme.md). |
|[Beveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection)|U voltooit de configuratie van een WAF, moet verkeer worden gerouteerd naar het toestel WAF. Deze aanbeveling volgen, wordt de wijzigingen nodig setup voltooid.|
|[Een volgende generatie Firewall toevoegen](security-center-add-next-generation-firewall.md)|Wordt aanbevolen dat u een volgende generatie Firewall (NGFW) uit een Microsoft-partner toevoegen om te kunnen vergroten uw beveiligingsinstellingen.|
|[Route verkeer via NGFW alleen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Wordt aanbevolen dat u netwerk groep (NSG) regels voor binnenkomende verkeer naar uw VM via uw NGFW afdwingen configureren.|
|[Endpoint Protection installeren](security-center-install-endpoint-protection.md)|Wordt aanbevolen dat u antimalware-programma's voor VMs (alleen voor Windows-VMs) inrichten.|
|[Endpoint Protection systeemstatus waarschuwingen oplossen](security-center-resolve-endpoint-protection-health-alerts.md)|Wordt aanbevolen dat u endpoint protection fouten oplossen.|
|[Beveiligingsgroepen netwerk inschakelen voor subnetten of een virtuele machines](security-center-enable-network-security-groups.md)|Aanbevolen NSGs op subnetten of VMs in te schakelen.|
|[Toegang via Internet die tegenover elkaar liggen eindpunt beperken](security-center-restrict-access-through-internet-facing-endpoints.md)|Wordt aanbevolen dat u regels voor binnenkomende verkeer voor NSGs configureren.|
|[Controle van SQL server inschakelen](security-center-enable-auditing-on-sql-servers.md)|Wordt aanbevolen dat u hebt ingeschakeld controle voor Azure SQL-servers (Azure SQL-service alleen; niet zijn opgenomen in SQL waarop uw virtuele machines).|
|[Controle van de SQL-database activeert](security-center-enable-auditing-on-sql-databases.md)|Wordt aanbevolen dat u hebt ingeschakeld controle voor Azure SQL-databases (Azure SQL-service alleen; niet zijn opgenomen in SQL waarop uw virtuele machines).|
|[Transparante gegevensversleuteling inschakelen op SQL-databases](security-center-enable-transparent-data-encryption.md)|Aanbevolen versleuteling voor SQL-databases (alleen voor SQL Azure-service) in te schakelen.|
|[VM Agent inschakelen](security-center-enable-vm-agent.md)|Kunt u zien welke VMs vereisen de VM-Agent. De VM-Agent moeten worden geïnstalleerd op VMs om te kunnen inrichten patch scannen, volgens de basislijn scannen en antimalware-programma's. De VM-Agent is al dan niet standaard geïnstalleerd voor VMs die zijn geïmplementeerd van Azure Marketplace. Het artikel [VM Agent en uitbreidingen – deel 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) vindt u informatie over het installeren van de VM-Agent.|
| [Schijfversleuteling toepassen](security-center-apply-disk-encryption.md) |Wordt aanbevolen dat u uw VM schijven Azure-schijf versleuteling gebruikt (Windows en Linux VMs) versleutelen. Versleuteling geschikt is voor de OS en de gegevens volumes op uw VM.|
|[Contactgegevens beveiliging bieden](security-center-provide-security-contact-details.md) | Aanbevolen dat u beveiliging van de contactgegevens voor elk van uw abonnementen. Contactgegevens is een e-mailadres en telefoonnummer nummer. De gegevens worden gebruikt om contact met u opnemen als ons beveiligingsteam vindt dat uw resources zijn is gehackt. |
| [Versie van het besturingssysteem bijwerken](security-center-update-os-version.md) | Wordt aanbevolen dat u de versie van het besturingssysteem (OS) voor uw Cloudservice naar de meest recente versie beschikbaar voor uw gezin OS bijwerkt.  Zie voor meer informatie over Cloud Services, de [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md). |
| [Beveiligingsprobleem beoordeling is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) | Wordt aanbevolen dat u een oplossing van een beveiligingsprobleem assessment op uw VM installeren. |
| [Problemen verhelpen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Kunt u zien van de systeem- en problemen gedetecteerd door de beveiligingsprobleem assessment oplossing geïnstalleerd op uw VM. |

U kunt filteren en aanbevelingen negeren.

1. Klik op **Filter** op het blad **aanbevelingen** . Het blad **Filter** wordt geopend en selecteert u de ernst en staat waarden die u wilt zien.

    ![Aanbevelingen filteren][2]

2. Als u constateert dat een aanbeveling niet van toepassing is, kunt u de aanbeveling negeren en klikt u vervolgens afmelden bij uw weergave filteren. Er zijn twee manieren om te sluiten van een aanbeveling. Eén manier is een item met de rechtermuisknop op en selecteer vervolgens **verwijderen**. De andere is plaats de muisaanwijzer op een item, klik op de drie puntjes die worden weergegeven aan de rechterkant en selecteer vervolgens **verwijderen**. U kunt ontslagen aanbevelingen weergeven door te klikken op **Filter**en vervolgens **Dismissed**te selecteren.

    ![Aanbeveling sluiten][3]

### <a name="apply-recommendations"></a>Aanbevelingen toepassen
Nadat u hebt bekeken bepalen alle aanbevelingen, welke thema dat u eerst wilt gebruiken. Het is raadzaam dat u het prioriteitsniveau gebruiken, zoals de belangrijkste parameter welke aanbevelingen wordt berekend eerst moet worden toegepast.

In de tabel met de bovenstaande aanbevelingen, selecteer een aanbeveling en doorlopen als een voorbeeld van hoe u een aanbeveling toepast.

## <a name="see-also"></a>Zie ook
In dit document, zijn u kennis met aanbevelingen voor de beveiliging in Beveiligingscentrum. Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) , informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md) , informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingsmeldingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) , informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen met Azure Beveiligingscentrum Monitoring](security-center-partner-solutions.md) , informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over Azure beveiliging beheercentrum](security-center-faq.md) , zoeken Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) , weblogberichten over Azure beveiliging en naleving zoeken.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
