<properties 
   pageTitle="Het migreren van affiniteit groepen met een regionale virtuele netwerk (VNet)"
   description="Meer informatie over het migreren van affiniteit groepen naar regionale vnets"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-migrate-from-affinity-groups-to-a-regional-virtual-network-vnet"></a>Het migreren van affiniteit groepen met een regionale virtuele netwerk (VNet)

U kunt een groep affiniteit gebruiken om ervoor te zorgen dat resources die zijn gemaakt in dezelfde groep affiniteit fysiek door de servers die dicht bij elkaar zijn, zodat deze resources om te communiceren sneller worden gehost. In het verleden zijn groepen affiniteit een vereiste voor het maken van virtuele netwerken (VNets). De manager-service voor het netwerk dat beheerd VNets kan alleen op dat moment werken in een set van fysieke servers of eenheid voor tijdschaal. Verbeteringen in de architectuur hebben de reikwijdte van netwerkbeheer naar een gebied verhoogd.

Grond van deze architectuur verbeteringen, affiniteit groepen zijn niet langer aanbevolen of vereist voor virtuele netwerken. Het gebruik van affiniteit groepen voor VNets wordt vervangen door de regio's. VNets die zijn gekoppeld aan regio's worden regionale VNets genoemd.

Bovendien is het raadzaam dat u affiniteit groepen in het algemeen niet gebruikt. Naast de vereiste VNet zijn affiniteit groepen ook belangrijk om te gebruiken om ervoor te zorgen resources, zoals berekeningscluster en opslag, dicht bij elkaar zijn geplaatst. Met de huidige Azure netwerkarchitectuur zijn deze vereisten plaatsing echter niet meer nodig. Zie [affiniteit groepen en VMs](#Affinity-groups-and-VMs) voor het aantal resterende bepaalde omstandigheden waarin u wilt mogelijk een groep affiniteit gebruiken.

## <a name="creating-and-migrating-to-regional-vnets"></a>Maken en deze te migreren naar regionale VNets

Gaan, bij het maken van nieuwe VNets, gebruikt u *regio*. U ziet het volgende als een optie in de beheerportal. Houd er rekening mee dat in het configuratiebestand netwerk Hiermee als *locatie worden*.

>[AZURE.IMPORTANT] Hoewel deze nog steeds technisch mogelijk is om te maken van een virtueel netwerk dat is gekoppeld aan een groep affiniteit, is er geen dwingende redenen dat te doen. Vele nieuwe functies, zoals netwerk beveiligingsgroepen, zijn alleen beschikbaar wanneer u een regionale VNet gebruikt en zijn niet beschikbaar voor virtuele netwerken die zijn gekoppeld aan affiniteit groepen.

### <a name="about-vnets-currently-associated-with-affinity-groups"></a>Over VNets die momenteel zijn verbonden met affiniteit groepen

VNets die momenteel zijn gekoppeld aan affiniteit groepen zijn ingeschakeld voor de migratie naar regionale VNets. Als u wilt migreren naar een regionale VNet, als volgt te werk:

1. Exporteer het configuratiebestand netwerk. U kunt PowerShell of de beheerportal gebruiken. Zie voor instructies voor het gebruik van de beheerportal [configureren uw VNet met een netwerk configuratie-bestand](virtual-networks-using-network-configuration-file.md).

1. Het configuratiebestand netwerk, de oude waarden vervangen door de nieuwe waarden bewerken. 

    > [AZURE.NOTE] De **locatie** is het gebied dat u hebt opgegeven voor de groep affiniteit die is gekoppeld aan uw VNet. Als uw VNet gekoppeld aan een groep affiniteit die in West VS is, bevindt zich wanneer u migreert, kunt u voor uw locatie, moet verwijzen naar West VS. 
    
    De volgende regels in uw netwerk configuratiebestand, de waarden vervangen door uw eigen bewerken: 

    **Oude waarde:** \<VirtualNetworkSitename = "VNetUSWest" AffinityGroup = "VNetDemoAG"\> 

    **Nieuwe waarde:** \<VirtualNetworkSitename = "VNetUSWest" locatie = "West US"\>

1. Sla uw wijzigingen en [Importeer](virtual-networks-using-network-configuration-file.md) de netwerkconfiguratie op Azure.

>[AZURE.NOTE] Deze migratie wordt niet elke uitvaltijd naar uw services.

## <a name="affinity-groups-and-vms"></a>Affiniteit groepen en VMs

Zoals eerder is vermeld, worden niet meer over het algemeen affiniteit groepen voor VMs aanbevolen. U kunt een groep affiniteit moet gebruiken, alleen wanneer u een reeks VMs de absolute laagste netwerklatentie tussen de VMs moet hebben. VMs plaatst in een groep affiniteit, worden de VMs alle geplaatst in dezelfde berekeningscluster cluster of schaal eenheid.

Houd er rekening mee dat met een groep affiniteit kunt twee, mogelijk negatief is, gevolgen is:

- De set VM tekengrootten worden beperkt tot het instellen van VM grootten door de eenheid voor tijdschaal van berekeningscluster worden aangeboden.

- Er is een hogere kans dat er een nieuwe VM toekennen. Dit gebeurt wanneer de eenheid voor specifieke tijdschaal voor de groep affiniteit buiten capaciteit is.

### <a name="what-to-do-if-you-have-a-vm-in-an-affinity-group"></a>Wat u moet doen als er een VM in een groep affiniteit

VMs die momenteel beschikbaar in een groep affiniteit zijn hoeft niet te worden verwijderd uit de groep affiniteit.

Nadat een VM wordt geïmplementeerd, is het naar een eenheid voor tijdschaal één geïmplementeerd. Affiniteit groepen kunt Beperk het aantal beschikbare VM formaten voor een nieuwe VM-implementatie, maar alle bestaande VM die wordt geïmplementeerd nog is beperkt tot het instellen van VM formaten beschikbaar in de eenheid voor tijdschaal waarin de VM wordt geïmplementeerd. Reden heeft een VM verwijderen uit de groep affiniteit geen effect.
 
