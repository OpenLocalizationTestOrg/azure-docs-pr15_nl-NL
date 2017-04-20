<properties
   pageTitle="Wat is een netwerk Access (Toegangsbeheerlijst)?"
   description="Meer informatie over ACL 's"
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

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Wat is een eindpunt toegangsbeheerlijst (ACL's)?

Een eindpunt Access (Toegangsbeheerlijst) is een beveiligingsuitbreiding die beschikbaar zijn voor uw Azure-implementatie. Een ACL biedt de mogelijkheid om te selectief toestaan of weigeren van verkeer naar een virtuele machine-eindpunt. Deze pakket filteren functionaliteit biedt een extra beveiliging. Netwerk ACL's voor alleen eindpunten, kunt u opgeven. U kunt geen een ACL voor een virtueel netwerk of een specifiek subnet opgenomen in een virtueel netwerk opgeven.

> [AZURE.IMPORTANT] Het wordt aanbevolen netwerk beveiligingsgroepen (NSGs) gebruiken in plaats van ACL's indien mogelijk. Zie voor meer informatie over NSGs [Wat is een beveiligingsgroep netwerk?](virtual-networks-nsg.md).

ACL's kunnen worden geconfigureerd met behulp van PowerShell of de beheerportal. Zie voor meer informatie over het configureren van een netwerk ACL via PowerShell [Beheren toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell](virtual-networks-acl-powershell.md). Zie [hoe u omhoog eindpunten instellen met een virtuele Machine](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)configureren van een netwerk ACL met behulp van de beheerportal.

Netwerk ACL's gebruikt, kunt u het volgende doen:

- Selectief toestaan of weigeren van binnenkomende verkeer op basis van externe subnet IPv4-adresbereiken naar een virtuele machine invoer eindpunt.

- Zwarte lijst IP-adressen

- Meerdere regels per VM eindpunt maken

- Maximaal 50 ACL regels per VM eindpunt opgeven

- Gebruik regel ordening om ervoor te zorgen de juiste set regels worden toegepast op een bepaald VM eindpunt (laag naar hoog)

- Geef een ACL voor een specifieke externe subnet IPv4-adres.

## <a name="how-acls-work"></a>De werking van ACL 's

Een ACL is een object met een lijst met regels. Wanneer u een ACL maken en deze op een eindpunt VM toepassen, pakket filteren van plaats op het hostknooppunt van uw VM. Dit betekent dat het verkeer van externe IP-adressen is gefilterd op het hostknooppunt voor overeenkomende ACL regels in plaats van op uw VM. Hiermee voorkomt u dat uw VM uitgaven het belangrijk CPU-bewerkingen voor het pakket filteren.

Wanneer een virtuele machine is gemaakt, worden standaard ACL geplaatst op hun plaats staan om alle binnenkomende verkeer te blokkeren. Echter als een eindpunt is gemaakt voor (poort 3389), klikt u vervolgens de standaard ACL is gewijzigd als u wilt dat alle inkomende verkeer is toegestaan voor dat eindpunt. Binnenkomende verkeer uit een externe subnet vervolgens mag dat eindpunt en de inrichting van geen firewall is vereist. Alle andere poorten zijn geblokkeerd voor binnenkomende verkeer, tenzij de eindpunten worden gemaakt voor deze poorten. Uitgaand verkeer is toegestaan al dan niet standaard.

**Standaard ACL voorbeeldtabel**

| **Regel #** | **Externe Subnet** | **Eindpunt** | **Vergunning aan te vragen/weigeren** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Toestaan      |

## <a name="permit-and-deny"></a>Toestaan of weigeren

U kunt selectief toestaan of weigeren voor een virtuele machine invoer eindpunt verkeer door te maken van regels die opgeven "toestaan" of 'weigeren'. Het is belangrijk te weten dat al dan niet standaard, wanneer een eindpunt is gemaakt, al het verkeer is toegestaan naar het eindpunt. Om die reden is het is belangrijk is voor meer informatie over het maken van regels vergunning aan te vragen/weigeren en op de juiste volgorde plaatsen als u gedetailleerde controle wilt over het netwerkverkeer dat u wilt dat het eindpunt VM te bereiken.

Points u rekening moet houden:

1. **Geen ACL –** Standaard wanneer een eindpunt is gemaakt, we toestaan dat alle voor het eindpunt.

1. **Toestaan dat-** Wanneer u een of meer "toestaan" bereiken hebt toegevoegd, kunt u alle andere bereiken zijn weigert al dan niet standaard. Alleen pakketten van het toegestane IP-bereik kunnen communiceren met het eindpunt VM.

1. **Weigeren-** Wanneer u een of meer "weigeren" bereiken hebt toegevoegd, verleent u alle andere bereiken van verkeer al dan niet standaard.

1. **Combinatie van toestaan of weigeren-** U kunt een combinatie van "toestaan" en "weigeren' als u wilt kunnen fungeren out een specifieke IP-bereik worden toegestaan of geweigerd.

## <a name="rules-and-rule-precedence"></a>Regels en de prioriteit van regels

Netwerk ACL's kunnen worden ingesteld op specifieke virtuele machine eindpunten. U kunt bijvoorbeeld een netwerk ACL voor een RDP-eindpunt dat is gemaakt op een virtuele machine welke vergrendelingen omlaag toegang voor bepaalde IP-adressen opgeven. De volgende tabel ziet een manier om toegang te verlenen aan openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen zijn geweigerd. We staan in een regel *laagste voorrang* volgorde.

### <a name="multiple-rules"></a>Meerdere regels

In het onderstaande voorbeeld als u toegang wilt geven naar het eindpunt RDP alleen uit twee openbare IPv4-adresbereiken (65.0.0.0/8 en 159.0.0.0/8), kunt dit u bereiken door het opgeven van twee regels *toestaan* . In dit geval aangezien RDP al dan niet standaard voor een virtuele machine is gemaakt, kunt u toegang tot de RDP-poort op basis van een extern subnet vergrendelen. Het volgende voorbeeld ziet u een manier om toegang te verlenen aan openbare virtuele IP-adressen (VIP's) van een bepaald bereik om toegang te verlenen voor RDP. Alle andere externe IP-adressen zijn geweigerd. Dit werkt omdat netwerk ACL's kan worden ingesteld voor een specifieke virtuele machine-eindpunt en toegang al dan niet standaard.

**Voorbeeld: meerdere regels**

| **Regel #** | **Externe Subnet** | **Eindpunt** | **Vergunning aan te vragen/weigeren** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Toestaan      |
| 200    | 159.0.0.0/8   | 3389     | Toestaan      |

### <a name="rule-order"></a>Regelvolgorde

Omdat meerdere regels kunnen worden opgegeven voor een eindpunt, moet er een manier om te ordenen van regels om te bepalen welke regels voorrang. De regelvolgorde Hiermee geeft u de prioriteit van regels. Netwerk ACL's Volg een regel *laagste voorrang* volgorde. Klik in het onderstaande voorbeeld is het eindpunt op poort 80 selectief krijgen voor toegang tot alleen bepaalde IP-adresbereiken. Als u wilt dit configureren, zijn er een regel voor weigeren (regel \# 100) voor de adressen in de ruimte 175.1.0.1/24. Een tweede regel is vervolgens met de prioriteit van regels 200 waarmee toegang tot alle andere adressen onder 175.0.0.0/8 opgegeven.

**Voorbeeld: de prioriteit van regels**

| **Regel #** | **Externe Subnet** | **Eindpunt** | **Vergunning aan te vragen/weigeren** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Weigeren        |
| 200    | 175.0.0.0/8   | 80       | Toestaan      |

## <a name="network-acls-and-load-balanced-sets"></a>Netwerk ACL's en gebalanceerde sets laden

Netwerk ACL's kunnen worden opgegeven in een eindpunt van taakverdeling instellen (kg ingesteld). Als een ACL is opgegeven voor het instellen van een kg, wordt de ACL netwerk wordt toegepast op alle virtuele Machines in die kg instellen. Bijvoorbeeld als een kg instellen wordt gemaakt met "Poort 80" en 3 VMs de kg instellen bevat, de ACL netwerk gemaakt op eindpunt "Poort 80" van een die VM automatisch op de andere VMs toegepast wordt.

![Netwerk ACL's en gebalanceerde sets laden](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Volgende stappen

[Het beheren van toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell](virtual-networks-acl-powershell.md)
