<properties
   pageTitle="Een eindpunt verkeer Manager in- of uitschakelen | Microsoft Azure"
   description="In dit artikel helpen uw Manager verkeer profiel eindpunten in- of uitschakelen."
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

# <a name="disable-or-enable-a-traffic-manager-endpoint"></a>Een eindpunt verkeer Manager in- of uitschakelen

U kunt ook afzonderlijke eindpunten die deel van een profiel verkeer Manager uitmaken uitschakelen. Eindpunten bevatten cloudservices en websites. Een eindpunt uitschakelen als onderdeel van het profiel verlaat, maar het profiel worden verwerkt als wanneer het eindpunt niet in deze inbegrepen is. Deze actie is bijzonder nuttig zijn voor het tijdelijk verwijderen van een eindpunt dat in de onderhoudsmodus of opnieuw worden geïmplementeerd. Zodra het eindpunt opnieuw gebruiksklaar is, kan worden ingeschakeld

>[AZURE.NOTE] **Een eindpunt uitschakelen heeft niets te maken met de implementatie staat in Azure wordt aangegeven. Een eindpunt orde blijft omhoog en ontvangen van verkeer zelfs wanneer in verkeer Manager uitgeschakeld. Daarnaast een eindpunt in één profiel uitschakelen heeft geen invloed op de status ervan in een ander profiel.**

## <a name="to-disable-an-endpoint"></a>Een eindpunt uitschakelen

1. Naar het deelvenster verkeer Manager in de portal van Azure klassieke en zoek het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
1. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die van uw configuratie uitmaken deel.
1. Klik op het eindpunt dat u wilt uitschakelen en klik vervolgens op **uitschakelen** onder aan de pagina.
1. Verkeer wordt gestopt die doorloopt naar het eindpunt op basis van de DNS-Time to Live (TTL) geconfigureerd voor de naam van het verkeer Manager-domein. U kunt de TTL wijzigen van de pagina configuratie van het profiel verkeer Manager.

## <a name="to-enable-an-endpoint"></a>Een eindpunt inschakelen


1. Naar het deelvenster verkeer Manager in de portal van Azure klassieke en zoek het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
1. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die van uw configuratie uitmaken deel.
1. Klik op het eindpunt dat u wilt inschakelen en klik vervolgens op **inschakelen** onder aan de pagina.
1. Verkeer wordt gestart die doorloopt naar de service opnieuw als gedicteerde door het profiel.

## <a name="next-steps"></a>Volgende stappen

[Verkeer Manager - uitschakelen, inschakelen of verwijderen van een profiel](disable-enable-or-delete-a-profile.md)

[Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)

[Prestatieoverwegingen verkeer Manager](traffic-manager-performance-considerations.md)