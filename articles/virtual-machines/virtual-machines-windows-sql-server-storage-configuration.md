<properties
    pageTitle="Configuratie van de opslagruimte voor SQL Server VMs | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe Azure opslag configureert voor SQL Server VMs tijdens inrichtingsmodel (resourcemanager implementatie). Ook wordt uitgelegd hoe u opslagruimte voor uw bestaande SQL Server-VMs kunt configureren."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>Configuratie van de opslagruimte voor SQL Server VMs

Als u de afbeelding van een SQL Server-VM in Azure wordt aangegeven configureert, helpt de Portal om de configuratie van uw opslagruimte te automatiseren. Dit geldt ook voor opslag koppelen aan de VM, die opslag toegankelijk zijn voor SQL Server bellen, en deze configureren om te optimaliseren voor uw specifieke vereisten.

In dit onderwerp wordt uitgelegd hoe Azure configureert opslagruimte voor uw SQL Server-VMs tijdens het inrichten zowel voor bestaande VMs. Deze configuratie is gebaseerd op de [prestaties aanbevolen procedures](virtual-machines-windows-sql-performance.md) voor Azure VMs met SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

## <a name="prerequisites"></a>Vereisten voor
Als u wilt de geautomatiseerde opslag configuratie-instellingen gebruiken, moet uw VM de volgende kenmerken:

- Deze is ingericht met een [SQL Server-galerij](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing).
- Het [resourcemanager implementatiemodel](../resource-manager-deployment-model.md)gebruikt.
- Gebruikt [Premium opslag](../storage/storage-premium-storage.md).

## <a name="new-vms"></a>Nieuwe VMs
De volgende secties wordt beschreven hoe u opslag voor nieuwe SQL Server virtuele machines configureert.

### <a name="azure-portal"></a>Azure-Portal
Wanneer een Azure VM door een afbeelding van de galerie met SQL Server is geïnstalleerd, kunt u kiezen voor het automatisch configureren van de opslagruimte voor uw nieuwe VM. U opgeven de opslaggrootte, prestatielimieten en type werkbelasting. De volgende schermafbeelding ziet u het opslag configuratie blad gebruikt tijdens SQL VM inrichten.

![Configuratie van SQL Server VM opslag tijdens het inrichten](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Op basis van uw keuzen, Azure configuratietaken worden uitgevoerd door de volgende opslag na het maken van de VM:

- Hiermee maakt u en schijven premium gegevens koppelt aan de virtuele machine.
- Hiermee configureert u de gegevensschijven toegankelijk voor SQL Server.
- Hiermee configureert u de gegevensschijven in een opslaggroep op basis van de opgegeven grootte en prestaties van (IO's / s en doorvoer) vereisten.
- De opslag van toepassingen koppelt aan een nieuw station op de virtuele machine.
- Optimaliseert deze nieuwe station op basis van uw type opgegeven werkbelasting (gegevensopslag, afhandeling processing of algemeen).

Zie voor meer informatie over hoe Azure Opslaginstellingen configureert, de [configuratie van opslag](#storage-configuration). Voor een volledige stapsgewijze instructies voor het maken van een SQL Server-VM in de Portal Azure, raadpleegt u [de zelfstudie inrichten](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Sjablonen voor het beheren van resource
Als u de volgende resourcemanager sjablonen gebruikt, zijn al dan niet standaard, met de configuratie van de groep geen opslag twee premium gegevensschijven gekoppeld. U kunt echter deze sjablonen voor als u wilt wijzigen van het aantal premium gegevensschijven die zijn gekoppeld aan de virtuele machine aanpassen.

- [VM met automatische back-up maken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [VM maken met het geautomatiseerde herstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [VM maken met AKV-integratie](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Bestaande VMs
Voor bestaande SQL Server-VMs, kunt u sommige Opslaginstellingen in de portal van Azure wijzigen. Selecteer uw VM, gaat u naar het gedeelte instellingen en selecteer vervolgens SQL Server Configuration. Het blad SQL Server Configuration ziet u de huidige gebruik van de opslag van uw VM. Alle stations die aanwezig op uw VM worden weergegeven in deze grafiek. De opslagruimte in beslag wordt voor elk station weergegeven in vier secties:

- SQL-gegevens
- SQL-logboek
- Overige (niet-SQL-opslag)
- Beschikbaar

![Opslagruimte voor bestaande SQL Server VM configureren](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Als u wilt configureren de opslag voor het toevoegen van een nieuw station of een bestaand station uitbreiden, klikt u op de koppeling bewerken boven de grafiek.

De configuratieopties die u ziet varieert afhankelijk van of u hebt u deze functie voordat u gebruikt. Wanneer u voor de eerste keer gebruikt, kunt u uw opslagvereisten voor een nieuw station opgeven. Als u deze functie eerder hebt gebruikt om een station te maken, kunt u kiezen om uit te breiden van dat station opslag.

### <a name="use-for-the-first-time"></a>Wordt gebruikt voor de eerste keer
Als dit de eerste keer met deze functie is, kunt u de opslaglimieten voor grootte en prestaties van voor een nieuw station opgeven. Dit probleem is vergelijkbaar met wat u bij de inrichting van de tijd wilt zien. Het belangrijkste verschil is dat u niet is toegestaan om op te geven van het type werkbelasting. Deze beperking voorkomt dat storend voor eventuele bestaande SQL Server-configuraties op de virtuele machine.

![SQL Server-opslag schuifregelaars configureren](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure Hiermee maakt u een nieuw station op basis van uw specificaties. Dit scenario Azure worden uitgevoerd in de volgende opslag configuratietaken uit te voeren:

- Hiermee maakt u en schijven premium gegevens koppelt aan de virtuele machine.
- Hiermee configureert u de gegevensschijven toegankelijk voor SQL Server.
- Hiermee configureert u de gegevensschijven in een opslaggroep op basis van de opgegeven grootte en prestaties van (IO's / s en doorvoer) vereisten.
- De opslag van toepassingen koppelt aan een nieuw station op de virtuele machine.

Zie voor meer informatie over hoe Azure Opslaginstellingen configureert, de [configuratie van opslag](#storage-configuration).

### <a name="add-a-new-drive"></a>Een nieuw station toevoegen
Als u opslag op uw SQL Server-VM al hebt ingesteld, Hiermee gegevensniveaus uitvouwen opslag worden twee nieuwe opties. De eerste optie is om toe te voegen een nieuwe schijf, waarvoor het prestatieniveau van uw VM kunt vergroten.

![Een nieuw station toevoegen aan een VM SQL](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Na het toevoegen van het station, moet u echter enkele extra handmatige configuratie als u wilt bereiken van de toename van de prestaties uitvoeren.

### <a name="extend-the-drive"></a>De schijf uitbreiden
De andere methode voor het uitvouwen van opslag is voor het uitbreiden van het bestaande station. Deze optie vergroot de beschikbare opslagruimte voor uw harde schijf, maar niet wordt verhoogd prestaties. Niet met de opslag van toepassingen wijzigen het aantal kolommen u nadat de opslag van toepassingen is gemaakt. Het aantal kolommen bepaalt het aantal parallelle schrijft, die kan worden gestreept op de gegevensschijven. Alle toegevoegde gegevensschijven kunnen geen daarom prestaties verbeteren. Ze kunnen alleen meer opslagruimte bieden voor de gegevens worden geschreven. Deze beperking wordt ook betekent dat, wanneer u het station uitbreidt, het aantal kolommen bepaalt het minimum aantal gegevensschijven die u kunt toevoegen. Als u een groep opslagruimte met vier gegevensschijven maakt, dus het aantal kolommen ook vier. Elk gewenst moment die u de opslag uitbreiden, moet u ten minste vier gegevensschijven toevoegen.

![Een station voor een SQL-VM uitbreiden](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Opslagconfiguratie
Deze sectie bevat een overzicht voor de opslag configuratiewijzigingen waarmee Azure automatisch tijdens SQL VM inrichting of configuratie in de Portal Azure.

- Als u minder dan twee TBs van opslagruimte voor uw VM hebt geselecteerd, is Azure maakt geen een resourcegroep die opslag.
- Als u ten minste twee TBs van opslagruimte voor uw VM hebt geselecteerd, configureert Azure een resourcegroep die opslag. Het volgende gedeelte van dit onderwerp vindt u de details van de configuratie van de groep opslag.
- Automatische opslagconfiguratie altijd worden gebruikt in [premium opslag](../storage/storage-premium-storage.md) P30 gegevensschijven. Er is dus een toewijzing 1:1 tussen uw geselecteerde aantal TB en het aantal gegevensschijven die zijn bijgevoegd bij uw VM.

Zie de [opslagruimte prijzen](https://azure.microsoft.com/pricing/details/storage) pagina op het tabblad **Schijfopruiming opslag** voor het prijsinformatie.

### <a name="creation-of-the-storage-pool"></a>Het maken van de opslag van toepassingen
Azure gebruikt de volgende instellingen maken van de opslag van toepassingen op SQL Server VMs.

| Instelling | Waarde |
|-----|-----|
| Streepgrootte  | 256 KB (gegevensopslag); 64 KB (transacties) |
| Schijfgrootte | 1 TB |
| Cache | Lezen |
| Toewijzingsgrootte | Grootte van de toewijzing NTFS 64 KB |
| Direct bestand initialisatie | Ingeschakeld |
| Pagina's vergrendelen in het geheugen | Ingeschakeld |
| Herstel | Eenvoudig herstel (geen tolerantie) |
| Aantal kolommen | Aantal gegevens schijven<sup>1</sup> |
| TempDB locatie | Die zijn opgeslagen op gegevens schijven<sup>2</sup> |

<sup>1</sup> nadat de opslag van toepassingen is gemaakt, kunt u het aantal kolommen in de opslag van toepassingen niet wijzigen.

<sup>2</sup> deze instelling is alleen van toepassing op de eerste station dat u maakt met de functie van de configuratie opslag.

## <a name="workload-optimization-settings"></a>Instellingen voor het optimaliseren van werkbelasting
De volgende tabel beschrijft de tekstopties drie werkbelasting en hun bijbehorende optimalisaties toe:

| Type werkbelasting | Beschrijving | Optimalisaties |
|-----|-----|-----|
| **Algemene** | De standaardinstelling van die ondersteuning biedt voor de meeste werkbelasting | Geen |
| **Verwerking van transacties** | De opslagcapaciteit voor traditionele database OLTP werkbelasting wordt geoptimaliseerd | Vlag 1117 aanwijzen<br/>Vlag 1118 aanwijzen |
| **Gegevensopslag** | De opslagcapaciteit voor analytische en rapportage werkbelasting wordt geoptimaliseerd | Vlag 610 aanwijzen<br/>Vlag 1117 aanwijzen |

>[AZURE.NOTE] Wanneer u een SQL-VM inrichten door deze te selecteren in de stap van de configuratie opslag, kunt u alleen het type werkbelasting opgeven.

## <a name="next-steps"></a>Volgende stappen
Zie voor andere onderwerpen over met SQL Server in Azure VMs, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
