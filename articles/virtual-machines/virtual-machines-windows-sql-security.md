<properties
    pageTitle="Beveiligingsoverwegingen voor SQL Server in Azure | Microsoft Azure"
    description="In dit onderwerp verwijst naar de resources die zijn gemaakt met het implementatiemodel klassieke en algemene richtlijnen voor het beveiligen van SQL Server wordt uitgevoerd in een Azure Virtual Machine bevat."
    services="virtual-machines-windows"
    documentationCenter="na"
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
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Beveiligingsoverwegingen voor SQL Server in Azure virtuele Machines
 
In dit onderwerp bevat algemene richtlijnen voor beveiliging waarmee beveiligde toegang tot SQL Server-exemplaren in een VM Azure vast te stellen. Echter om te garanderen betere beveiliging in uw SQL Server-database-exemplaren in Azure wordt aangegeven, wordt aangeraden om de procedures voor de beveiliging van traditionele on-premises implementatie naast de aanbevolen procedures voor beveiliging te voor Azure implementeren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Zie voor meer informatie over de procedures voor beveiliging van SQL Server, [SQL Server 2008 R2 aanbevolen procedures voor beveiliging - operationele en beheertaken](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure voldoet aan verschillende industriële voorschriften en standaarden die kunnen u een compatibele oplossing maken met SQL Server wordt uitgevoerd in een virtuele Machine inschakelen. Zie voor informatie over naleving van de regelgeving met Azure [Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/).

Hier volgt een lijst met beveiligingstips die moeten worden beschouwd bij het configureren en verbinding maken met het exemplaar van SQL Server in een VM Azure.

## <a name="considerations-for-managing-accounts"></a>Overwegingen voor het beheren van accounts:

- Maak een unieke lokale beheerdersaccount niet met de **beheerder van**naam.

- Complexe sterke wachtwoorden gebruiken voor alle accounts. Voor meer informatie over het maken van een sterk wachtwoord, Zie [Tips voor het maken van een sterke wachtwoorden](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) artikel.

- Standaard geselecteerd Azure Windows-verificatie tijdens de installatie van SQL Server-VM. Daarom de login **SA** is uitgeschakeld en een wachtwoord is ingesteld door setup. We aanbevolen die de aanmelding **SA** moet worden niet gebruikt of ingeschakeld. Hier volgen alternatieve strategieën een SQL-aanmelding desgewenst:
    - Maak een SQL-account dat lid van de systeembeheerder is.
    - Als u een aanmelding **SA** gebruiken moet, de aanmelding inschakelen en wijzig de naam en een nieuw wachtwoord toewijzen.
    - De opties die eerder zijn vermeld, moeten een wijziging de verificatiemodus voor **SQL Server en Windows-verificatiemodus**. Zie [Wijzigen Server-verificatiemodus](https://msdn.microsoft.com/library/ms188670.aspx)voor meer informatie.

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Overwegingen voor het beveiligen van verbindingen met Azure virtuele machines:

- Houd rekening met [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) gebruiken voor het beheer van de virtuele machines in plaats van openbare RDP-poorten.

- Gebruik een [Beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) (NSG) toestaan of weigeren netwerkverkeer naar uw VM. Als u wilt gebruiken een NSG en een eindpunt ACL al op hun plaats staan, moet u eerst het eindpunt ACL verwijderen. Voor informatie over hoe u dit doet, Zie [Beheren toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Als u eindpunten gebruikt, verwijdert u elke eindpunten van de virtuele machine als u deze niet gebruiken. Zie voor instructies over het gebruik van ACL's met eindpunten [beheren de ACL op een eindpunt](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Een versleutelde verbinding de optie inschakelen voor een exemplaar van de SQL Server-Database Engine in Azure virtuele Machines. SQL server-instantie met een ondertekend certificaat configureren. Zie [Versleutelde verbindingen inschakelen in de Database Engine](https://msdn.microsoft.com/library/ms191192.aspx) en [Syntaxis verbindingsreeks](https://msdn.microsoft.com/library/ms254500.aspx)voor meer informatie.

- Als uw virtuele machines moet alleen worden geopend via een netwerk met een specifieke, gebruikt u Windows Firewall toegang beperken tot bepaalde IP-adressen of subnetten.

## <a name="next-steps"></a>Volgende stappen

Als u ook geïnteresseerd in aanbevelingen rond prestaties bent, raadpleegt u [Prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-performance.md).

Zie voor andere onderwerpen over met SQL Server in Azure VMs, [SQL Server Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md).
