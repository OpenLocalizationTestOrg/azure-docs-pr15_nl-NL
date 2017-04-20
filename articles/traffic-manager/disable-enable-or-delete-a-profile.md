<properties
   pageTitle="Uitschakelen, inschakelen of verwijderen van een profiel verkeer Manager | Microsoft Azure"
   description="In dit artikel kunt u werken met uw profielen verkeer Manager."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Uitschakelen, inschakelen of verwijderen van een profiel


U kunt een bestaand verkeer Manager-profiel uitschakelen zodat deze niet aanvragen van gebruikers naar de geconfigureerde eindpunten verwijst wordt. Als u een profiel verkeer Manager, het zelf profiel en de gegevens in het profiel uitschakelt, blijft intact en kunnen worden bewerkt in de interface verkeer Manager. Als u het profiel opnieuw in te schakelen wilt, kunt u eenvoudig zodat in de Azure-portal en -verwijzingen wordt hervat doen. Wanneer u een profiel verkeer Manager in de portal van Azure maakt, wordt deze automatisch ingeschakeld.

## <a name="to-disable-a-profile"></a>Een profiel uitschakelen

1. Wijzig de resource DNS-record op de server Internet DNS-gebruik van de juiste recordtype en de verwijzing naar een andere naam of het IP-adres van een specifieke locatie op Internet. Met andere woorden, de resource DNS-record op uw Internet DNS-server wijzigen zodat deze geen gebruik maakt van een resource CNAME-record, die naar de naam van het domein van uw profiel verkeer Manager verwijst.
1. Verkeer, niet meer wordt doorgestuurd naar de eindpunten via de instellingen van het verkeer Manager profiel.
1. Selecteer het profiel dat u wilt uitschakelen. Selecteer het profiel, klik op de pagina verkeer Manager Markeer het profiel door te klikken op de kolom naast de naam van een profiel. Klik niet op de naam van het profiel of de pijl naast de naam, zoals dit u naar de instellingenpagina voor het profiel gaat.
1. Selecteer het profiel en klik op uitschakelen onder aan de pagina.

## <a name="to-enable-a-profile"></a>Een profiel inschakelen

1. Selecteer het profiel dat u wilt inschakelen. Selecteer het profiel, klik op de pagina verkeer Manager Markeer het profiel door te klikken op de kolom naast de naam van een profiel. Klik niet op de naam van het profiel of de pijl naast de naam, zoals dit u naar de instellingenpagina voor het profiel gaat.
1. Selecteer het profiel en klik op inschakelen onder aan de pagina.
1. De DNS resource record op de server Internet DNS-gebruik van de CNAME-recordtype dat de naam van uw bedrijf domein met de naam van het domein van uw profiel verkeer Manager overeenkomt wijzigen. Zie [punt een bedrijfsdomein Internet naar een domein verkeer Manager](traffic-manager-point-internet-domain.md)voor meer informatie.
1. Verkeer zijn verzonden, wordt doorgestuurd naar de eindpunten opnieuw.

## <a name="delete-a-profile"></a>Verwijderen van een profiel


1. Zorg ervoor dat de DNS-resource record op uw Internet DNS-server niet langer een CNAME-record voor resource die naar de naam van het domein van uw profiel verkeer Manager verwijst gebruikt.
1. Selecteer het profiel dat u wilt verwijderen. Selecteer het profiel, klik op de pagina verkeer Manager Markeer het profiel door
1. Klik op de kolom naast het profiel. Klik niet op de naam van het profiel of de pijl naast de naam, zoals dit u naar de instellingenpagina voor het profiel gaat.
1. Selecteer het profiel en klik op verwijderen onder aan de pagina.

## <a name="next-steps"></a>Volgende stappen

[Een eindpunt in- of uitschakelen voor Manager - verkeer](disable-or-enable-an-endpoint.md)

[Failover-mailroutering methode configureren](traffic-manager-configure-failover-routing-method.md)

[Round robin routeren methode configureren](traffic-manager-configure-round-robin-routing-method.md)

[Prestaties routeren methode configureren](traffic-manager-configure-performance-routing-method.md)

[Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)

