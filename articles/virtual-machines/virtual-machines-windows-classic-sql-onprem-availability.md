<properties
    pageTitle="On-premises altijd op beschikbaarheidsgroepen uitbreiden naar Azure | Microsoft Azure"
    description="Deze zelfstudie resources die zijn gemaakt met het implementatiemodel klassieke gebruikt en wordt beschreven hoe u met de wizard Replica toevoegen in SQL Server Management Studio (SSMS) een replica altijd op beschikbaarheid van de groep toevoegen in Azure wordt aangegeven."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>On-premises altijd op beschikbaarheidsgroepen uitbreiden naar Azure

Altijd op beschikbaarheid-groepen bieden beschikbaarheid voor groepen van database door toe te voegen secundaire replica's. Deze replica's toestaan via databases voor het geval een fout is verbroken. Ze kunnen ook worden gebruikt te dragen gelezen werkbelasting of back-taken.

U kunt on-premises implementatie beschikbaarheidsgroepen naar Microsoft Azure uitbreiden door de inrichting van een of meer Azure VMs met SQL Server en deze vervolgens als replica's toevoegen aan uw on-premises implementatie beschikbaarheid van de groepen.

Deze zelfstudie wordt ervan uitgegaan dat u hebt het volgende:

- Een actieve Azure-abonnement. Kunt u [zich registreren voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- Een bestaande altijd op beschikbaarheid van de groep on-premises implementatie. Zie voor meer informatie over de beschikbaarheid van de groepen, [Altijd op beschikbaarheid van de groepen](https://msdn.microsoft.com/library/hh510230.aspx).

- Connectiviteit tussen de on-premises netwerk en uw Azure virtuele netwerk. Zie voor meer informatie over het maken van deze virtuele netwerk [configureren een VPN-verbinding van het Site-naar-Site in de portal van Azure klassieke](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Wizard Azure Replica toevoegen

In dit gedeelte ziet u hoe u de **Wizard voor Azure Replica toevoegen** voor het uitbreiden van uw oplossing altijd op beschikbaarheid van de groep als u wilt opnemen Azure replica's gebruiken.

1. Vanuit SQL Server Management Studio uitvouwen **Altijd op beschikbaarheid** > **Beschikbaarheidsgroepen** > **[naam van uw groep beschikbaarheid]**.

1. Met de rechtermuisknop op **Beschikbaarheid replica's**en klik vervolgens op **Replica toevoegen**.

1. De **Replica toevoegen aan de beschikbaarheid van de groep Wizard** wordt standaard weergegeven. Klik op **volgende**.  Als u **niet op deze pagina opnieuw weergeven** tijdens de optie onder aan de pagina een vorige starten van de wizard hebt geselecteerd, wordt dit scherm wordt niet weergegeven.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. U moet verbinding maken met alle bestaande secundaire replica's. U kunt klikken op **verbinding maken...** naast elke replica of u kunt klikken op **Verbinding maken met alle...** onder aan het scherm. Klik op **volgende** om naar het volgende scherm te gaan na verificatie.

1. Klik op de pagina **Opgeven replica's** meerdere tabbladen worden weergegeven aan de bovenkant: **replica's**, **eindpunten** **Back-up voorkeuren**en **luisteraar ervan af**. Klik in het tabblad **Replicas** op **Toevoegen Azure Replica...** aan de Wizard Azure Replica toevoegen starten.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Selecteer een bestaand Azure Management certificaat in het lokale archief van de Windows-certificaat als u een voordat u hebt geïnstalleerd. Selecteer of Voer de id van een Azure-abonnement als u een voordat u hebt gebruikt. U kunt klikken op downloaden om te downloaden en installeren van een Azure-certificaat voor Management en downloaden van de lijst met abonnementen Azure-account gebruikt.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. U vult elke veld op de pagina met waarden die wordt gebruikt voor het maken van de Azure Virtual Machine (VM) die als host de replica fungeert.

  	|Instelling|Beschrijving|
|---|---|
|**Afbeelding**|Selecteer de gewenste combinatie van besturingssysteem en SQL Server|
|**VM grootte**|Selecteer de grootte van VM die het beste aan uw bedrijfsbehoeften|
|**VM naam**|Geef een unieke naam voor de nieuwe VM. De naam moet tussen 3 en 15 tekens bevatten, kan alleen letters, cijfers en afbreekstreepjes bevatten en moeten beginnen met een letter en eindigen met een letter of een getal.|
|**VM gebruikersnaam**|Geef de gebruikersnaam van een die het beheerdersaccount op de VM|
|**VM beheerderswachtwoord**|Een wachtwoord opgeven voor het nieuwe account|
|**Bevestig het wachtwoord**|Bevestig het wachtwoord van het nieuwe account|
|**Virtual Network**|Geef het Azure virtuele netwerk waarmee de nieuwe VM moet gebruiken. Zie voor meer informatie over virtuele netwerken, [Virtuele netwerk-overzicht](../virtual-network/virtual-networks-overview.md).|
|**Virtual Network Subnet**|Het virtuele netwerk subnet die moet worden gebruikt door de nieuwe VM opgeven|
|**Domein**|Bevestig de vooraf ingestelde waarde voor het domein klopt.|
|**Domeinnaam**|Een account in de lokale beheerdersgroep lokale knooppunten opgeven|
|**Wachtwoord**|Geef het wachtwoord voor de domeinnaam voor de gebruiker|

1. Klik op **OK** om te valideren van de implementatie-instellingen.

1. Juridische voorwaarden worden vervolgens weergegeven. Lees en klik op **OK** als u akkoord met deze termen gaat.

1. De pagina **Opgeven replica's** wordt opnieuw weergegeven. Controleer de instellingen voor de nieuwe Azure replica op de tabbladen **replica's**, **eindpunten**en **Back-up-voorkeuren** . Instellingen wijzigen om te voldoen aan de bedrijfsvereisten van uw.  Zie voor meer informatie over de parameters zijn in de volgende tabbladen, [Replica's pagina opgeven (beschikbaarheid van de Wizard Nieuwe groep Wizard/toevoegen Replica)](https://msdn.microsoft.com/library/hh213088.aspx). Houd er rekening mee dat listeners kunnen niet worden gemaakt met behulp van het tabblad luisteraar ervan af voor beschikbaarheid van de groepen die Azure replica's bevatten. Bovendien als een luisteraar ervan af al vóór het starten van de Wizard is gemaakt, ontvangt u een bericht aangegeven dat deze niet wordt ondersteund in Azure wordt aangegeven. We bekijkt listeners maken in de sectie **maken luisteraar ervan af van een groep beschikbaarheid** .

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Klik op **volgende**.

1. Selecteer de gegevens synchronisatie-methode die u wilt gebruiken op de pagina **Selecteer eerste synchronisatie voor gegevens** en klik op **volgende**. Selecteer voor de meeste scenario's, **Volledige gegevenssynchronisatie**. Zie voor meer informatie over methoden voor adreslijstsynchronisatie van gegevens, [Selecteert u gegevens synchronisatie beginpagina (altijd op beschikbaarheid van de groep Wizards)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Bekijk de resultaten op de pagina **Validation** . Resterende problemen worden gecorrigeerd en de validatie opnieuw uitvoeren indien nodig. Klik op **volgende**.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. De instellingen op de pagina **Summary** te controleren en klik op **Voltooien**.

1. Het inrichten begint. Wanneer de wizard voltooid is, klikt u op **sluiten** om terug te de wizard afsluit.

>[AZURE.NOTE] De Wizard Azure Replica toevoegen wordt een logboekbestand gemaakt in Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Dit bestand kan worden gebruikt om op te lossen mislukte Azure replica implementaties. Als de Wizard geen actie wordt uitgevoerd mislukt, worden alle eerdere bewerkingen hersteld, inclusief de ingerichte VM verwijderen.

## <a name="create-an-availability-group-listener"></a>Een luisteraar ervan af van beschikbaarheid van de groep maken

Nadat u de van de beschikbaarheidsgroep hebt gemaakt, moet u een luisteraar ervan af voor clients verbinding maken met de replica's maken. Listeners rechtstreekse binnenkomende verbindingen met de primaire of een secundaire replica alleen-lezen. Zie voor meer informatie over listeners, [configureren een ILB luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure wordt aangegeven](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Volgende stappen

Naast de **Wizard voor Azure Replica toevoegen** aan uw groep altijd-Klik op van beschikbaarheid uitbreiden naar Azure, u mogelijk ook enkele SQL Server-werkbelastingen volledig naar verplaatsen Azure. Zie de [inrichting van een SQL Server virtuele Machine op Azure](virtual-machines-windows-portal-sql-server-provision.md)om te beginnen.

Zie voor andere onderwerpen over met SQL Server in Azure VMs, [SQL Server op Azure virtuele Machines](virtual-machines-windows-sql-server-iaas-overview.md).
