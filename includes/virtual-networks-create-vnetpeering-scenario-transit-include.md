## <a name="service-chaining---transit-through-peered-vnet"></a>Service Chaining - overdracht door peered VNet

Hoewel het gebruik van systeem routes verkeer automatisch voor de implementatie vergemakkelijkt, zijn er omstandigheden waarin u wilt bepalen de routering van pakketten door een virtueel toestel.
In dit scenario zijn er twee VNets in een abonnement, HubVNet en VNet1 zoals is beschreven in het onderstaande diagram. U implementeren netwerk virtuele Appliance(NVA) in VNet HubVNet. Na het instellen van VNet peering tussen HubVNet en VNet1, kunt u instellen van de gebruiker gedefinieerde Routes en geeft u de volgende hop naar NVA in de HubVNet.

![NVA overdracht](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] De volgt de procedure waarmee wordt ervan uitgegaan dat alle VNets hier worden in hetzelfde abonnement. Maar deze werkt ook scenario's met een cross-abonnement.

De belangrijke eigenschap overdracht routering inschakelen is de parameter "Toestaan doorgestuurd verkeer". Hiermee kunt accepteren en het verkeer van/naar de NVA de peered VNet verzenden.  
