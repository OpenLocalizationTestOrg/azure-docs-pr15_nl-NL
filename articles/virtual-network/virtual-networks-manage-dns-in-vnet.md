<properties 
   pageTitle="Beheren van DNS-servers die worden gebruikt door een virtueel netwerk (VNet)"
   description="Meer informatie over het toevoegen en verwijderen van DNS-servers in een virtueel netwerk (vnet)"
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

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Beheren van DNS-servers die worden gebruikt door een virtueel netwerk (VNet)

U kunt de lijst met DNS-servers gebruikt in een VNet in de beheerportal of in het configuratiebestand netwerk kunt beheren. U kunt maximaal 12 DNS-servers toevoegen voor elke VNet. Wanneer u DNS-servers opgeeft, is het is belangrijk om te bevestigen dat u een lijst met uw DNS-servers in de juiste volgorde voor uw omgeving. DNS-serverlijsten werken niet round robin. Deze worden gebruikt in de volgorde waarin ze zijn opgegeven. Als de eerste DNS-server in de lijst kan worden bereikt, wordt de client die DNS-server, ongeacht of de DNS-server goed al dan niet werkt gebruiken. U wijzigt de volgorde van de server DNS voor uw virtuele netwerk, de DNS-servers verwijderen uit de lijst en deze weer in de volgorde waarin u wilt toevoegen.

>[AZURE.WARNING] Nadat de DNS-lijst is bijgewerkt, moet u de virtuele machines bevinden in uw virtuele netwerk, zodat ze de nieuwe DNS-serverinstellingen verdergaan opnieuw starten. Virtuele machines blijven gebruiken de huidige configuratie totdat ze opnieuw worden gestart.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Een lijst van de DNS-server voor een virtueel netwerk met behulp van de Portal Management bewerken

1. Meld u aan bij de **beheerportal**.

1. Klik op **netwerken te gebruiken**in het navigatiedeelvenster en klik vervolgens op de naam van uw virtual netwerk in de kolom **naam** .

1. Klik op **configureren**.

1. In **DNS-Servers**, kunt u het volgende:

    - **(Toevoegen) van een nieuwe DNS-server – registreren** Alleen de naam en het IP-adres in de vakken typen. Hiermee wordt een DNS-server toegevoegd aan uw virtuele netwerk DNS-Servers lijst en ook de DNS-server registreert bij Azure.

    - **Een DNS-server die eerder is geregistreerd – toevoegen** Als u al een DNS-server met Azure geregistreerd, kunt u deze selecteren in de vooraf gevulde lijst.

    - **Een DNS-server uit het virtuele netwerk – verwijderen** Klik op de X naast de server die u wilt verwijderen. Houd er rekening mee dat dit alleen de server verwijderd uit deze lijst virtueel netwerk. De DNS-server blijft geregistreerde in Azure wordt aangegeven voor uw andere virtuele netwerken te gebruiken. Als een DNS-server uit uw abonnement wilt verwijderen, gaat u naar de pagina **DNS-Servers-netwerken >** .

    - **Naar de volgorde van DNS-servers:** Verwijder alle DNS-servers die worden vermeld, en voeg deze toe in de volgorde waarin u de gewenste weer in. Vergeet niet dat dit niet een round robin DNS-lijst is.

    - **De naam van een DNS-server – wijzigen** Markeer de DNS-server in de lijst en typ vervolgens de nieuwe naam. Hiermee wordt een nieuwe DNS-server registreren in Azure wordt aangegeven, evenals toe te voegen aan de lijst DNS-Servers voor uw virtuele netwerk. De oude DNS-server en het IP-adres blijven geregistreerde met Azure. U kunt verwijderen op de pagina **DNS-Servers** als u geen van deze voor andere virtuele netwerken gebruikmaakt.

1. Klik op **Opslaan** onder aan de pagina om op te slaan van uw nieuwe configuratie van DNS-servers.

1. Start opnieuw op de virtuele machines bevinden in het virtuele netwerk toe te staan dat ze in het bezit van de nieuwe DNS-instellingen.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Een lijst van DNS-servers met een netwerk configuratie-bestand bewerken

Als u wilt bewerken een lijst van de DNS-servers met behulp van een configuratiebestand netwerk, moet u eerst uw configuratie-instellingen in de beheerportal exporteren. U vervolgens het netwerk configuratie-bestand bewerken en achterwaarts door de beheerportal te importeren. Hieronder vindt u een uitgebreide lijst met stappen voor het voltooien van dit proces.

1. Netwerkinstellingen is virtual exporteren naar een configuratiebestand netwerk. Zie voor meer informatie en stappen voor het exporteren van uw netwerkinstellingen voor de configuratie, [Virtuele netwerkinstellingen exporteren naar een bestand van de configuratie netwerk](virtual-networks-using-network-configuration-file.md).

1. Geef de DNS-server-gegevens voor uw virtuele netwerk. Zie [een DNS-Server in een virtuele netwerk configuratiebestand opgeven](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)voor meer informatie over het opgeven van een DNS-server. Zie voor meer informatie over netwerk configuratiebestanden, [Azure virtuele netwerk configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx) en [configureren van een virtuele netwerk met een netwerk configuratiebestand](virtual-networks-using-network-configuration-file.md).

1. Het netwerk configuratie-bestand importeren. Zie [een configuratiebestand netwerk importeren](virtual-networks-using-network-configuration-file.md)voor meer informatie en stappen voor het importeren van het configuratiebestand netwerk.

1. Start opnieuw op de virtuele machines bevinden in het virtuele netwerk toe te staan dat ze in het bezit van de nieuwe DNS-instellingen.
