<properties
    pageTitle="Gedetailleerde SSH probleemoplossing voor een VM Azure | Microsoft Azure"
    description="Gedetailleerdere SSH probleemoplossing voor problemen die verbinding maken met een Azure virtuele machines"
    keywords="SSH verbinding geweigerd, ssh fout, azure ssh, SSH verbinding is mislukt"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Gedetailleerde stappen voor SSH voor probleemoplossing

Zijn er veel mogelijke oorzaken die de client SSH mogelijk niet kunnen bereiken van de service SSH op de VM. Als u hebt gevolgd door de meer [algemene stappen voor SSH](virtual-machines-linux-troubleshoot-ssh-connection.md), moet u verder oplossen probleem met de verbinding. In dit artikel begeleidt u bij gedetailleerde stappen voor probleemoplossing om te bepalen waar de SSH-verbinding is verbroken en hoe u deze kunt oplossen.

## <a name="take-preliminary-steps"></a>Voorbereidende stappen uitvoeren

In het volgende diagram ziet u de onderdelen die betrekking hebben op.

![Diagram waarin de onderdelen van SSH-service](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

De volgende stappen kunnen u de bron van de fout isoleren en duidelijk oplossingen of tijdelijke oplossingen.

Controleer eerst de status van de VM in de portal.

Klik in de [portal van Azure](https://portal.azure.com):

1. Selecteer voor VMs die zijn gemaakt met het implementatiemodel klassieke, **Blader** > **virtuele machines (klassieke)** > *VM naam*.

    -OF-

    Selecteer voor VMs die zijn gemaakt met het model resourcemanager, **Blader** > **virtuele machines** > *VM naam*.

    Het deelvenster status voor het VM moet worden **uitgevoerd**weergegeven. Schuif omlaag naar de recente activiteiten weergeven voor berekeningscluster, opslag en netwerk resources.

2. Selecteer **Instellingen** te onderzoeken eindpunten, IP-adressen en andere instellingen.

    Als u wilt identificeren eindpunten in VMs die zijn gemaakt met behulp van resourcemanager, controleert u of dat een [beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) is gedefinieerd. Ook controleren of dat de regels zijn toegepast op de beveiligingsgroep van netwerk en dat ze in het subnet verwezen bent.

Klik in de [portal van Azure klassieke](https://manage.windowsazure.com)voor VMs die zijn gemaakt met behulp van het implementatiemodel klassieke:

1. Selecteer **virtuele machines** > *VM naam*.
2. Selecteer van de VM **Dashboard** om de status ervan controleren.
3. Selecteer **Monitor** om recente activiteiten voor berekeningscluster, opslag en netwerk resources weer te geven.
4. Selecteer **eindpunten** om ervoor te zorgen dat er een eindpunt voor SSH-verkeer is toegestaan.

Om te verifiëren netwerkconnectiviteit, Controleer de geconfigureerde eindpunten en Zie als u de VM tot en met een ander protocol, zoals HTTP of een andere service kan bereiken.

Na deze stappen, probeert u opnieuw verbinding SSH.


## <a name="find-the-source-of-the-issue"></a>De bron van het probleem vinden

De SSH-client op uw computer kan mislukken bereiken van de service SSH op de VM Azure vanwege problemen of configuraties van het volgende:

- [De clientcomputer SSH](#source-1-ssh-client-computer)
- [Organisatie edge-apparaat](#source-2-organization-edge-device)
- [Service-eindpunt cloud en toegangsbeheerlijsten (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Beveiligingsgroepen netwerk](#source-4-network-security-groups)
- [Azure VM Linux gebaseerde](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Bron 1: SSH clientcomputer

Als u wilt uw computer verwijderen als de bron van de fout, controleert u of dat deze SSH verbindingen met een ander on-premises implementatie, op basis van Linux-computer kunt aanbrengen.

![Diagram met informatie over de onderdelen voor clientcomputers SSH](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Als de verbinding is mislukt, Controleer de volgende handelingen uit op uw computer:

- Binnenkomende of uitgaande SSH verkeer (TCP 22) wordt geblokkeerd door een lokale firewall die instellen
- Lokaal proxy clientsoftware waardoor SSH verbindingen hebt geïnstalleerd
- Lokaal netwerk monitoring software waardoor SSH verbindingen zijn geïnstalleerd
- Andere soorten beveiligingssoftware die om verkeer te controleren of specifieke soorten verkeer toestaan/weigeren

Als een van deze voorwaarden toepast, tijdelijk uitschakelen van de software en probeert u een SSH-verbinding met een lokale computer om vast te stellen de reden dat de verbinding wordt geblokkeerd op uw computer. Werkt u met uw netwerkbeheerder om te corrigeren van de software-instellingen wilt SSH verbindingen toestaan.

Als u verificatie via clientcertificaat gebruikt, controleert u of dat u naar de map .ssh in de basismap deze machtigingen hebben:

- Type chmod 700 ~/.ssh
- Type chmod 644 ~/.ssh/\*.pub
- Type chmod 600 ~/.ssh/id_rsa (of andere bestanden die uw persoonlijke sleutels die zijn opgeslagen in deze zijn)
- Type chmod 644 ~/.ssh/known_hosts (bevat hosts waarmee u via SSH verbonden bent)

## <a name="source-2-organization-edge-device"></a>Bron 2: Organisatie edge-apparaat

Als u wilt verwijderen edge-apparaat van uw organisatie als de bron van de fout, controleert u of dat een computer die rechtstreeks met Internet verbonden SSH verbindingen in uw VM Azure aanbrengen kunt. Als u toegang krijgt de VM via een VPN-verbinding voor de site-naar-site of een verbinding Azure ExpressRoute tot, gaat u verder met [bron 4: netwerk beveiligingsgroepen](#nsg).

![Diagram met informatie over de organisatie edge-apparaat](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Als u niet over een computer die rechtstreeks verbonden met Internet, een nieuwe Azure VM maken in een eigen resourcegroep of cloud service en deze gebruiken. Zie [een virtuele machine maken waarop Linux in Azure wordt aangegeven](virtual-machines-linux-quick-create-cli.md)voor meer informatie. De resourcegroep of VM en cloud-service verwijderen wanneer u klaar bent met het testen.

Als u een SSH-verbinding met een computer die rechtstreeks verbonden met Internet maken kunt, controleert u de edge-apparaat van uw organisatie voor:

- Een interne firewall die wordt geblokkeerd door SSH verkeer met Internet
- Een proxyserver waardoor SSH verbindingen
- Detectie van een geopende of netwerk software uitvoeren op apparaten in uw netwerk rand waardoor SSH verbindingen bewaken

Werken met uw netwerkbeheerder om te corrigeren van de instellingen van uw organisatie edge-apparaten zodat SSH verkeer met Internet.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Bron 3: De Cloud service-eindpunt en ACL

> [AZURE.NOTE] Deze bron geldt alleen voor VMs die zijn gemaakt met behulp van het implementatiemodel klassieke. Voor VMs die zijn gemaakt met behulp van resourcemanager, gaat u verder met [gegevensbron 4: netwerk beveiligingsgroepen](#nsg).

Als u wilt verwijderen de cloud service-eindpunt en ACL als de bron van de fout, controleert u of dat een andere Azure VM in hetzelfde virtuele netwerk SSH verbinding met uw VM kunt maken.

![Diagram met informatie over de service-eindpunt cloud en ACL](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Als u een andere VM geen in hetzelfde virtuele netwerk, kunt u eenvoudig een nieuwe record maken. Zie [maken een VM Linux op Azure met de CLI](virtual-machines-linux-quick-create-cli.md)voor meer informatie. Verwijder de extra VM wanneer u klaar bent met het testen.

Als u een SSH-verbinding met een VM in hetzelfde virtuele netwerk maken kunt, controleert u het volgende:

- **De eindpuntconfiguratie voor SSH-verkeer is toegestaan op het doel VM.** De privé TCP-poort van het eindpunt moet overeenkomen met de TCP-poorten waarop de service SSH op de VM luistert. (De standaardpoort is 22). Voor VMs die zijn gemaakt met het implementatiemodel resourcemanager, controleert u het poortnummer SSH TCP in de portal van Azure of door te **Bladeren**selecteren > **virtuele machines (v2)** > *VM naam* > **Instellingen** > **eindpunten**.

- **De ACL voor het eindpunt SSH-verkeer is toegestaan op de doelsite virtuele machine.** Een ACL kunt u opgeven toegestaan of een binnenkomend verkeer van Internet, op basis van het bron-IP-adres geweigerd. Zijn onjuist geconfigureerd ACL's kunnen voorkomen dat van binnenkomende SSH verkeer naar het eindpunt. Controleer uw ACL's om ervoor te zorgen dat binnenkomende verkeer uit het openbare IP-adressen van uw proxy of andere randserver is toegestaan. Zie [over netwerktoegang toegangsbeheerlijsten (ACL's)](../virtual-network/virtual-networks-acl.md)voor meer informatie.

U kunt het eindpunt verwijderen als een bron van het probleem, het huidige eindpunt verwijderen, een nieuw eindpunt maken en geef de naam SSH (TCP-poort 22 voor het poortnummer in openbare en persoonlijke). Zie de [eindpunten van een virtuele machine in Azure instellen](virtual-machines-windows-classic-setup-endpoints.md)voor meer informatie.

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Bron 4: Netwerk beveiligingsgroepen

Beveiligingsgroepen netwerk kunnen u nog meer gedetailleerde controle toegestane binnenkomende en uitgaande verkeer. U kunt regels die subnetten omvatten en cloud services in een Azure virtuele netwerk maken. Controleer uw netwerk groep beveiligingsregels om ervoor te zorgen dat SSH verkeer naar en vanuit Internet is toegestaan.
Zie voor meer informatie [over beveiligingsgroepen netwerk](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Bron 5: Linux gebaseerde Azure virtuele machines

De laatste bron over mogelijke problemen is de Azure virtuele machine zelf.

![Diagram met informatie over Linux gebaseerde Azure virtuele machines](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Als u dit nog niet hebt gedaan, volgt u de instructies [opnieuw een wachtwoord of SSH voor Linux gebaseerde virtuele machines in te stellen](virtual-machines-linux-classic-reset-access.md).

Probeer nogmaals verbinding te maken vanaf uw computer. Als het nog steeds niet, Hier volgen enkele van de mogelijke oorzaken:

- De SSH-service wordt niet uitgevoerd op de doelsite virtuele machine.
- De SSH-service is niet beschikbaar op TCP-poort 22. U kunt dit testen een Telnet-client installeren op uw lokale computer en uitvoeren "telnet *cloudServiceName*. cloudapp.net 22 '. Hiermee bepaalt u als de virtuele machine binnenkomende en uitgaande communicatie naar het eindpunt SSH kunnen.
- De lokale firewall op de doelsite virtuele machine heeft regels die verhinderen binnenkomende of uitgaande SSH-verkeer is toegestaan dat.
- Detectie van een geopende of netwerk software die wordt uitgevoerd op de Azure virtuele machine cmdlets voor controle is de SSH verbindingen te blokkeren.


## <a name="additional-resources"></a>Aanvullende informatie
Zie voor meer informatie over het oplossen van de toepassing toegang, [problemen oplossen met de toegang tot een toepassing wordt uitgevoerd op een Azure virtuele machines](virtual-machines-linux-troubleshoot-app-connection.md)