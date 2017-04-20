<properties 
    pageTitle="Een verbinding uit een indexering Azure zoeken met SQL Server configureren op een Azure virtuele machine | Microsoft Azure | Indexeerfuncties" 
    description="Versleutelde verbindingen inschakelen en configureren van de firewall voor verbindingen met SQL Server op een Azure virtuele machine (VM) uit een indexering op Azure zoeken." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Een verbinding uit een indexering Azure zoeken met SQL Server configureren op een Azure-VM

Zoals beschreven in [Azure SQL-Database verbinding naar Azure zoekprogramma's via indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions), indexeerfuncties ten opzichte van **SQL Server Azure VMs** (of **SQL Azure VMs** voor korte) maken door Azure zoeken wordt ondersteund, maar er zijn een paar-beveiliging vereisten voor het eerste kunt verrichten. 

**Duur van taak:** Ongeveer 30 minuten, ervan uitgaande dat u al een certificaat op de VM geïnstalleerd.

## <a name="enable-encrypted-connections"></a>Versleutelde verbindingen hebt ingeschakeld

Azure zoeken is een versleuteld kanaal vereist voor alle indexering aanvragen via een openbare internetverbinding. In dit gedeelte vindt u de stappen om dit te activeren.

1. Controleer de eigenschappen van het certificaat om te controleren of dat de onderwerpnaam is de FQDN-naam (Fully Qualified Domain Name) van Azure VM. U kunt een hulpmiddel zoals CertUtils of de module Certificaten gebruiken om de eigenschappen te bekijken. U kunt de FQDN-naam krijgen van het VM service-blad Essentials sectie in het veld **openbare IP-adres/DNS-label een naam** in de [portal van Azure](https://portal.azure.com/).

    - Voor VMs gemaakt met de nieuwere **Resourcemanager** -sjabloon, de FQDN-naam is opgemaakt als `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Voor oudere VMs gemaakt als een **klassieke** VM, de FQDN-naam is opgemaakt als `<your-cloud-service-name.cloudapp.net>`. 

2. SQL Server als u wilt gebruiken het certificaat dat met de Register-Editor (regedit) configureren. 

    Hoewel SQL Server Configuration Manager vaak voor deze taak gebruikt wordt, kunt u deze niet gebruiken voor dit scenario. Dit niet het geïmporteerde certificaat gevonden omdat de FQDN-naam van de VM op Azure niet overeenkomen met de FQDN-naam volgens de VM (verwijst naar het domein als de lokale computer of het netwerkdomein waaraan deze is toegevoegd). Wanneer namen niet overeenkomen, gebruikt u regedit om op te geven van het certificaat.

    - In regedit, bladert u naar deze registersleutel: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    De `[MSSQL13.MSSQLSERVER]` deel hangt af van de versie en de exemplaarnaam. 

    - Stel de waarde van de sleutel **certificaat** op de **vingerafdruk** van het SSL-certificaat dat u hebt geïmporteerd voor VM.

    Er zijn verschillende manieren om de vingerafdruk, bepaalde beter dan de andere. Als u deze uit de module **certificaten** in MMC kopieert, wordt u waarschijnlijk een onzichtbaar voorloop teken [zoals is beschreven in dit ondersteuningsartikel](https://support.microsoft.com/kb/2023869/), waardoor een fout wanneer u verbinding probeert te kiezen. Er bestaan verschillende tijdelijke oplossingen voor het corrigeren van dit probleem. De eenvoudigste manier is om te verwijderen met backspace via en typ het eerste teken van de vingerafdruk het eerste teken in het veld sleutelwaarde in regedit verwijderen opnieuw. U kunt ook een ander hulpmiddel kunt u de vingerafdruk kopiëren.

3. Machtigingen verlenen voor de serviceaccount. 

    Zorg ervoor dat de SQL Server-account de juiste machtigingen van de persoonlijke sleutel van de SSL-certificaat wordt verleend. Als u deze stap laten corrigeren, SQL Server niet gestart. U kunt de module **certificaten** of **CertUtils** gebruiken voor deze taak.

4. De SQL Server-service opnieuw starten.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>SQL Server-connectiviteit configureren in VM

Nadat u de versleutelde verbinding vereist Azure zoeken hebt ingesteld, zijn er extra configuratiestappen ingebouwd in SQL Server Azure VMs. Als u dit nog niet hebt gedaan, wordt de volgende stap is voltooid configuratie met behulp van een van deze artikelen:

- Zie voor een **Resource Manager** VM, [verbinden aan een virtuele Machine op Azure met Resource Manager van SQL Server](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- Zie voor een **Klassiek** VM, [verbinden aan een virtuele Machine op Azure klassieke van SQL Server](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Met name Raadpleeg de sectie in elk artikel om 'verbinding te maken via internet'.

## <a name="configure-the-network-security-group-nsg"></a>De beveiligingsgroep van netwerk (NSG) configureren

Het is niet ongebruikelijk voor het configureren van de NSG en bijbehorende Azure eindpunt of Access (Toegangsbeheerlijst) uw VM Azure als toegankelijk wilt maken naar andere partijen verzonden. De kans groot dat u dit nog toe te staan dat uw eigen toepassingslogica verbinding maken met uw SQL Azure-VM hebt gedaan. Het is niet verschillen voor een Azure zoeken-verbinding met uw SQL Azure-VM. 

De onderstaande koppelingen bieden instructies van NSG configuratie voor VM implementaties. Gebruik deze instructies om te ACL een Azure zoeken eindpunt op basis van het IP-adres.

> [AZURE.NOTE] Achtergrondinformatie [Wat is een beveiligingsgroep netwerk?](../virtual-network/virtual-networks-nsg.md)

- Zie voor een **Resource Manager** VM, [het maken van NSGs voor ARM implementaties](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- Zie voor een **Klassiek** VM, [het maken van NSGs voor klassieke implementaties](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-adressen, kan een paar problemen die u eenvoudig worden verholpen als u zich bewust bent van het probleem en mogelijke oplossingen opleveren. De resterende secties vindt aanbevelingen voor het afhandelen van problemen met het IP-adressen in de ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Toegang tot het IP-adres van de zoekopdracht beperken

Het is raadzaam dat u de toegang tot het IP-adres van uw zoekservice in de ACL in plaats van uw SQL Azure-VMs breed openen om te verbindingsverzoeken beperken. U kunt het IP-adres eenvoudig bepalen door het pingen de FQDN-naam (bijvoorbeeld `<your-search-service-name>.search.windows.net`) van uw search-service.

#### <a name="managing-ip-address-fluctuations"></a>IP-adres schommelingen beheren

Als uw zoekservice heeft slechts één eenheid zoeken (dat wil zeggen één replica en één partition), wordt het IP-adres wijzigen tijdens het periodiek onderhoud opstarten wordt daardoor van een bestaande ACL met uw zoekservice IP-adres.

Er is een manier om te voorkomen dat de volgende connectivity-fout gebruik van meer dan één replica en één partition in Azure zoeken. Dit doet, waardoor de kosten toenemen, maar deze ook is het IP-adres-probleem opgelost. Niet in Azure zoeken wijzigen IP-adressen als er meer dan één zoeken eenheid.

Een tweede manier is om de verbinding met mislukt en klik vervolgens opnieuw configureren de ACL's in de NSG. U kunt gemiddeld IP-adressen wijzigen om de paar weken verwachten. Voor klanten die beheerde indexeren op incidentele basis, mogelijk deze methode goede.

Een derde goede (maar niet met name secure) benadering is het opgeven van het bereik van de IP-adres van de Azure regio waar uw search-service is ingericht. De lijst met IP-bereiken waaruit openbare IP-adressen zijn toegewezen aan Azure resources wordt gepubliceerd op [Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Opnemen van de portal IP-adressen van Azure zoeken

Als u de portal van Azure gebruikt om te maken van een indexering, moet de portal logica Azure zoeken ook toegang tot uw SQL Azure-VM tijdens de aanmaaktijd van een. Azure zoeken portal IP-adressen kunnen u vinden door het pingen `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Volgende stappen

Met de configuratie uit de weg, kunt u nu een SQL Server op Azure VM opgeven als de gegevensbron voor een indexering Azure zoeken. Zie [Azure SQL-Database verbinding naar Azure zoekprogramma's via indexeerfuncties](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) voor meer informatie.
