<properties
    pageTitle="Integratie van Azure belangrijke kluis voor SQL Server configureren op Azure VMs (resourcemanager)"
    description="Leer hoe u de configuratie van SQL Server-versleuteling voor gebruik met Azure-toets kluis automatiseren. Dit onderwerp wordt uitgelegd hoe u Azure-toets kluis integratie met SQL Server virtuele machines met resourcemanager gemaakt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>Integratie van Azure belangrijke kluis voor SQL Server configureren op Azure VMs (resourcemanager)

> [AZURE.SELECTOR]
- [Resourcemanager](virtual-machines-windows-ps-sql-keyvault.md)
- [Klassieke](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Overzicht
Er zijn meerdere SQL Server-versleuteling functies, zoals [transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [kolom niveau codering (wissen)](https://msdn.microsoft.com/library/ms173744.aspx)en [back-codering](https://msdn.microsoft.com/library/dn449489.aspx). Deze vormen van versleuteling moeten u voor het beheren en bewaren van de cryptografische sleutels die u voor versleuteling gebruikt. De service Azure toets kluis (AKV) is bedoeld om de beveiliging en beheer van deze toetsen op een locatie beveiligde en ten zeerste beschikbaar te verbeteren. De [SQL Server-Connector](http://www.microsoft.com/download/details.aspx?id=45344) zorgt voor SQL Server om deze toets uit kluis van Azure-toets te gebruiken.

Als u met SQL Server met zowel on-premises machines er stappen die [u kunt volgen om toegang tot Azure toets kluis van uw lokale SQL Server-computer](https://msdn.microsoft.com/library/dn198405.aspx). Maar voor SQL Server in Azure VMs, kunt u tijd besparen met behulp van de functie *Integratie met Azure toets kluis* .

Wanneer deze functie is ingeschakeld, deze automatisch de SQL Server-Connector installeert, configureert de EKM-provider voor toegang tot Azure-toets kluis en maakt een de referentie zodat u toegang tot uw kluis. Als u de stappen in de documentatie van de hierboven genoemde on-premises implementatie bekeken, kunt u zien dat deze functie stappen 2 en 3 automatiseert. Het enige wat dat u nog steeds moet handmatig doen is het opzetten van de belangrijkste kluis en toetsen. Hierin wordt automatisch in de instellingen van uw SQL-VM. Wanneer deze functie is dit setup voltooid, kunt u de T-SQL-instructies om te beginnen met het coderen van uw databases of back-ups, zoals u gewend bent kunt uitvoeren.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Inschakelen en configureren van AKV-integratie
U kunt tijdens het inrichten AKV-integratie inschakelen of configureert voor bestaande VMs.

### <a name="new-vms"></a>Nieuwe VMs
Als u een nieuwe SQL Server virtuele machine met resourcemanager zijn geïnstalleerd, biedt de Azure-portal een stap Azure-toets kluis-integratie inschakelen. De functie Azure-toets kluis is alleen beschikbaar voor de onderneming, ontwikkelaars en evaluatie-edities van SQL Server.

![Integratie van SQL Azure belangrijke kluis](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Zie voor een gedetailleerd overzicht van de inrichting van [inrichten SQL Server in de Portal Azure virtuele machines](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Bestaande VMs
Voor bestaande SQL Server virtuele machines, selecteert u uw VM SQL Server. Selecteer het gedeelte **SQL Server-configuratie** van het blad **Instellingen** .

![Integratie van de SQL-AKV voor bestaande VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Klik in het blad **SQL Server configuration** op de knop **bewerken** in de sectie van de integratie van automatische toets kluis.

![Integratie van de SQL-AKV configureren voor bestaande VMs](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Wanneer u klaar bent, klikt u op de knop **OK** vanaf de onderkant van het blad **SQL Server configuration** uw wijzigingen op te slaan.

>[AZURE.NOTE] U kunt ook AKV-integratie met een sjabloon. Zie [Azure quickstart sjabloon voor Azure toets kluis integratie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update)voor meer informatie.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
