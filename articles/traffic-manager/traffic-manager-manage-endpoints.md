<properties
    pageTitle="Eindpunten in Azure verkeer Manager beheren | Microsoft Azure"
    description="In dit artikel kunt u toevoegen, verwijderen, inschakelen en eindpunten van Azure verkeer Manager uitschakelen."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Toevoegen, uitschakelen, inschakelen of eindpunten verwijderen

De functie Web Apps in Azure App-Service biedt al failover en round robin verkeer routeren functionaliteit voor websites binnen een datacenter, ongeacht de website-modus. Azure verkeer Manager kunt u opgeven failover en round robin verkeer omleiding voor websites en cloud services in verschillende datacenters. De eerste stap moet u deze functionaliteit is aan de verkeer-beheer van het eindpunt van service of de website van cloud toevoegen.

>[AZURE.NOTE]  In dit artikel wordt uitgelegd hoe u de klassieke portal. De portal van Azure klassieke biedt alleen ondersteuning voor het maken en de toewijzing van cloudservices en WebApps als eindpunten. De nieuwe [Azure-portal](https://portal.azure.com) is de voorkeur interface.

U kunt ook afzonderlijke eindpunten die deel van een profiel verkeer Manager uitmaken uitschakelen. Een eindpunt uitschakelen als onderdeel van het profiel verlaat, maar het profiel worden verwerkt als wanneer het eindpunt niet in deze inbegrepen is. Deze actie is handig voor het tijdelijk verwijderen van een eindpunt dat in de onderhoudsmodus of opnieuw worden geïmplementeerd. Als u het eindpunt opnieuw gebruiksklaar hebt, kunt u deze kunt inschakelen.

>[AZURE.NOTE] Een eindpunt uitschakelen heeft niets te maken met de implementatie staat in Azure wordt aangegeven. Een correct eindpunt blijft omhoog en ontvangen van verkeer zelfs wanneer uitgeschakeld in verkeer Manager. Daarnaast een eindpunt in één profiel uitschakelen heeft geen invloed op de status ervan in een ander profiel.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Een cloud-service of de website van eindpunt toevoegen

1. Zoek in het deelvenster verkeer Manager in de portal van Azure klassieke het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen. Open de instellingenpagina, klikt u op de pijl rechts van de naam van een profiel.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die al deel uitmaken van uw configuratie.
3. Onderaan op de pagina, klikt u op **toevoegen** voor toegang tot de pagina **Service eindpunten toevoegen** . De pagina worden standaard de cloudservices onder **Service-eindpunten**.
4. Voor cloudservices, selecteert u de cloudservices in de lijst ze toevoegt als eindpunten voor dit profiel. Als u de naam van de cloud-service uitschakelt, wordt deze verwijderd uit de lijst met eindpunten.
5. Klik op de vervolgkeuzelijst **Servicetype** voor websites, en selecteer **Web-app**.
6. Selecteer de websites in de lijst naar deze als eindpunten voor dit profiel toevoegen. Als u de websitenaam van de uitschakelt, wordt deze verwijderd uit de lijst met eindpunten. U kunt slechts één website per Azure datacenter (ook wel bekend als een regio) selecteren. Wanneer u de eerste website selecteert, worden de andere websites in het dezelfde datacenter niet beschikbaar voor selectie. Bedenk ook dat alleen standaard websites worden vermeld.
7. Nadat u de eindpunten voor dit profiel selecteert, klikt u op het vinkje op de rechterbenedenhoek de wijzigingen wilt opslaan.

>[AZURE.NOTE] Nadat u toevoegt of een eindpunt verwijderen uit een profiel met behulp van de *Failover* verkeer routeren methode, de failover prioriteitenlijst kan niet worden gesorteerd ze door u gewenste manier. U kunt de volgorde van de lijst Failover prioriteit op de pagina configuratie aanpassen. Zie de [Routering van failover-configureren-verkeer](traffic-manager-configure-failover-routing-method.md)voor meer informatie.

## <a name="to-disable-an-endpoint"></a>Een eindpunt uitschakelen

1. Zoek in het deelvenster verkeer Manager in de portal van Azure klassieke het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen. Open de instellingenpagina, klikt u op de pijl rechts van de naam van een profiel.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die van uw configuratie uitmaken deel.
3. Klik op het eindpunt dat u wilt uitschakelen en klik vervolgens op **uitschakelen** onder aan de pagina.
4. Clients gaat u verder met het verzenden van verkeer naar het eindpunt voor de duur van Time to Live (TTL). U kunt de TTL-waarde op de pagina configuratie van het profiel verkeer Manager wijzigen.

## <a name="to-enable-an-endpoint"></a>Een eindpunt inschakelen

1. Zoek in het deelvenster verkeer Manager in de portal van Azure klassieke het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen. Open de instellingenpagina, klikt u op de pijl rechts van de naam van een profiel.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die van uw configuratie uitmaken deel.
3. Klik op het eindpunt dat u wilt inschakelen en klik vervolgens op **inschakelen** onder aan de pagina.
4. Clients worden doorgestuurd naar het eindpunt ingeschakeld, zoals dit door het profiel.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Een cloud-service of de website van een eindpunt verwijderen

1. Zoek in het deelvenster verkeer Manager in de portal van Azure klassieke het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen. Open de instellingenpagina, klikt u op de pijl rechts van de naam van een profiel.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die al deel uitmaken van uw configuratie.
3. Klik op de naam van het eindpunt dat u wilt verwijderen uit het profiel op de pagina-eindpunten.
4. Onderaan op de pagina, klikt u op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* [Verkeer Manager profielen beheren](traffic-manager-manage-profiles.md)
* [Methoden voor het doorsturen configureren](traffic-manager-configure-routing-method.md)
* [Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)
* [Prestatieoverwegingen verkeer Manager](traffic-manager-performance-considerations.md)
* [Bewerkingen op verkeer Manager (REST API overzicht)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
