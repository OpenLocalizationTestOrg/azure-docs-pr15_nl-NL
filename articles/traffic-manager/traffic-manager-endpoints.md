<properties
   pageTitle="Eindpunten in Azure verkeer Manager beheren | Microsoft Azure"
   description="In dit artikel kunt u toevoegen, verwijderen, inschakelen en eindpunten van Azure verkeer Manager uitschakelen."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Toevoegen, uitschakelen, inschakelen of eindpunten verwijderen

De functie Web Apps in Azure App-Service biedt al failover en round robin verkeer routeren functionaliteit voor websites binnen een datacenter, ongeacht de website-modus. Azure verkeer Manager kunt u opgeven failover en round robin verkeer omleiding voor websites en cloud services in verschillende datacenters. De eerste stap moet u deze functionaliteit is aan de verkeer-beheer van het eindpunt van service of de website van cloud toevoegen.

>[AZURE.NOTE] U kunt externe locaties of verkeer Manager profielen toevoegen als eindpunten met behulp van de Azure klassieke portal. U moet de REST API [Definitie maken](http://go.microsoft.com/fwlink/p/?LinkId=400772) of de Windows PowerShell [Toevoegen-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774)gebruiken.

U kunt ook afzonderlijke eindpunten die deel van een profiel verkeer Manager uitmaken uitschakelen. Eindpunten bevatten cloudservices en websites. Een eindpunt uitschakelen als onderdeel van het profiel verlaat, maar het profiel worden verwerkt als wanneer het eindpunt niet in deze inbegrepen is. Deze actie is bijzonder nuttig zijn voor het tijdelijk verwijderen van een eindpunt dat in de onderhoudsmodus of opnieuw worden geïmplementeerd. Als u het eindpunt opnieuw gebruiksklaar hebt, kunt u deze kunt inschakelen.

>[AZURE.NOTE] Een eindpunt uitschakelen heeft niets te maken met de implementatie staat in Azure wordt aangegeven. Een eindpunt orde blijft omhoog en ontvangen van verkeer zelfs wanneer in verkeer Manager uitgeschakeld. Daarnaast een eindpunt in één profiel uitschakelen heeft geen invloed op de status ervan in een ander profiel.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Een cloud-service of de website van eindpunt toevoegen


1. Naar het deelvenster verkeer Manager in de portal van Azure klassieke en zoek het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die al deel uitmaken van uw configuratie.
3. Onderaan op de pagina, klikt u op **toevoegen** voor toegang tot de pagina **Service eindpunten toevoegen** . De pagina worden standaard de cloudservices onder **Service-eindpunten**.
4. Voor cloudservices, selecteert u de cloudservices in de lijst pstn_conferencing wilt inschakelen als eindpunten voor dit profiel. Als u de naam van de cloud-service uitschakelt, wordt deze verwijderd uit de lijst met eindpunten.
5. Klik op de vervolgkeuzelijst **Servicetype** voor websites, en selecteer **Web-app**.
6. Selecteer de websites in de lijst naar deze als eindpunten voor dit profiel toevoegen. Als u de websitenaam van de uitschakelt, wordt deze verwijderd uit de lijst met eindpunten. Houd er rekening mee dat u slechts één website per Azure datacenter (ook wel bekend als een regio) kunt selecteren. Als u een website in een datacenter waarop meerdere websites selecteren, wanneer u de eerste website selecteert, worden de andere in het dezelfde datacenter niet beschikbaar voor selectie. Bedenk ook dat alleen standaard websites worden vermeld.
7. Nadat u de eindpunten voor dit profiel selecteert, klikt u op het vinkje op de rechterbenedenhoek de wijzigingen wilt opslaan.

>[AZURE.NOTE] Als u de *Failover* verkeer routeren methode, nadat u toevoegen of verwijderen van een eindpunt gebruikt, moet u de lijst Failover prioriteit op de pagina configuratie zodat de failover volgorde die u wilt gebruiken voor uw configuratie aanpassen. Zie de [Routering van failover-configureren-verkeer](traffic-manager-configure-failover-routing-method.md)voor meer informatie.

## <a name="to-disable-an-endpoint"></a>Een eindpunt uitschakelen

1. Naar het deelvenster verkeer Manager in de portal van Azure klassieke en zoek het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die van uw configuratie uitmaken deel.
3. Klik op het eindpunt dat u wilt uitschakelen en klik vervolgens op **uitschakelen** onder aan de pagina.
4. Verkeer wordt gestopt die doorloopt naar het eindpunt op basis van de DNS-Time to Live (TTL) geconfigureerd voor de naam van het verkeer Manager-domein. U kunt de TTL wijzigen van de pagina configuratie van het profiel verkeer Manager.

## <a name="to-enable-an-endpoint"></a>Een eindpunt inschakelen

1. Naar het deelvenster verkeer Manager in de portal van Azure klassieke en zoek het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die van uw configuratie uitmaken deel.
3. Klik op het eindpunt dat u wilt inschakelen en klik vervolgens op **inschakelen** onder aan de pagina.
4. Verkeer wordt gestart die doorloopt naar de service opnieuw als gedicteerde door het profiel.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Een cloud-service of de website van een eindpunt verwijderen


1. Naar het deelvenster verkeer Manager in de portal van Azure klassieke en zoek het verkeer Manager-profiel waarvan de instellingen van het eindpunt dat u wilt wijzigen en klik vervolgens op de pijl rechts van de naam van een profiel. Hiermee wordt de instellingenpagina voor het profiel geopend.
2. Aan de bovenkant van de pagina, klikt u op **eindpunten** om weer te geven van de eindpunten die al deel uitmaken van uw configuratie.
3. Klik op de naam van het eindpunt dat u wilt verwijderen uit het profiel op de pagina-eindpunten.
4. Onderaan op de pagina, klikt u op **verwijderen**.

>[AZURE.NOTE] U kunt externe locaties of verkeer Manager profielen niet verwijderen als eindpunten met behulp van de Azure klassieke portal. Windows PowerShell, moet u gebruiken. Zie [Verwijderen-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- [Failover-mailroutering methode configureren](traffic-manager-configure-failover-routing-method.md)
- [Round robin routeren methode configureren](traffic-manager-configure-round-robin-routing-method.md)
- [Prestaties routeren methode configureren](traffic-manager-configure-performance-routing-method.md)
- [Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)
- [Bewerkingen op verkeer Manager (REST API overzicht)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
