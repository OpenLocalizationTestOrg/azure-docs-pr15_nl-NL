
<properties
    pageTitle="Valideer de VNET Azure voor gebruik met Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over het om ervoor te zorgen dat uw VNET Azure is klaar voor gebruik met Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Valideer de VNET Azure voor gebruik met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Voordat u een VNET Azure met Azure RemoteApp gebruikt, wilt u mogelijk de VNET valideren. Hiermee voorkomt u problemen met connectivity.

Ga als volgt te werk om te valideren uw VNET Azure:

1. Maak een Azure virtuele machines binnen het subnet van de Azure VNET die u wilt gebruiken met Azure RemoteApp.

2. Maak verbinding met die VM met behulp van de optie **verbinding maken** in de beheerportal.
3. Deelnemen aan de virtuele machine naar het domein dat u wilt gebruiken met Azure RemoteApp. Als u een verzameling hybride die is verbonden met uw on-premises netwerk maakt, moet u de virtuele machine toevoegen aan uw lokale domein.

Als dit lukt, is de VNET Azure klaar voor gebruik met RemoteApp.

Zie de volgende artikelen voor meer informatie over de werkstroom van de siteverzameling end-to-end hybride:

- [Het plannen van uw virtuele netwerk van Azure RemoteApp](remoteapp-planvnet.md)
- [Een hybride-verzameling maken](remoteapp-create-hybrid-deployment.md)
- [Azure RemoteApp siteverzameling implementeren naar uw Azure Virtual Network (met de ondersteuning voor ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
