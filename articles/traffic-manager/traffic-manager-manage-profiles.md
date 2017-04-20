<properties
    pageTitle="Azure verkeer Manager Profielbeheer | Microsoft Azure"
    description="In dit artikel helpt u bij het maken, uitschakelen, inschakelen, verwijderen en de geschiedenis van een profiel Azure verkeer Manager weergeven."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Een profiel Azure verkeer Manager beheren

Omleiding van verkeer methoden verkeer Manager profielen gebruiken om te bepalen de verdeling van verkeer naar uw cloudservices of de eindpunten van de website. In dit artikel wordt uitgelegd hoe maken en beheren van deze profielen.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Een snelle maken met verkeer Manager-profiel maken

U kunt snel een verkeer Manager-profiel maken met behulp van snelle maken in de portal van Azure klassieke. Snelle maken kunt u profielen maken met eenvoudige configuratie-instellingen. Echter, kunt u snel maken voor instellingen zoals de set eindpunten (cloudservices en websites), de volgorde failover voor de routering failover verkeer-methode of controle-instellingen. Nadat u uw profiel hebt, kunt u deze instellingen configureren in de portal van Azure klassieke. Verkeer Manager ondersteunt maximaal 200 eindpunten per profiel. Scenario's voor de meeste gebruik vereisen echter slechts een paar eindpunten.

### <a name="to-create-a-traffic-manager-profile"></a>Een verkeer Manager-profiel maken

1. **Uw cloudservices en websites dashboard implementeren naar uw productieomgeving.** Zie voor meer informatie over cloudservices, [Cloudservices](http://go.microsoft.com/fwlink/p/?LinkId=314074). Zie voor meer informatie over websites, [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Meld u aan bij de portal van Azure klassieke.** Klik op **Nieuw** in de linkerbenedenhoek van de portal, klikt u op **Netwerkservices > verkeer Manager**, en klik vervolgens op **Snelle maken** om te beginnen met het configureren van uw profiel.
3. **Configureer de DNS-voorvoegsel.** Een unieke DNS voor uw profiel van de manager verkeer overgeven voorvoegselnaam. U kunt alleen het voorvoegsel voor een domeinnaam verkeer Manager opgeven.
4. **Selecteer het abonnement.** Selecteer het juiste Azure abonnement. Elk profiel is gekoppeld aan een één abonnement. Als u slechts één abonnement hebt, deze optie niet wordt weergegeven.
5. **Selecteer de verkeer routeren methode.** Selecteer de methode verkeer rondsturen in **verkeer routering van beleid**. Zie voor meer informatie over methoden voor het doorsturen van verkeer, [over verkeer Manager verkeer routeren methoden](traffic-manager-routing-methods.md).
6. **Klikt u op 'Maken' om te maken van het profiel**. Wanneer de profielconfiguratie is voltooid, kunt u uw profiel kunt vinden in het deelvenster verkeer Manager in de portal van Azure klassieke.
7. **Eindpunten, bewaken en aanvullende instellingen configureren in de portal van Azure klassieke.** Gebruik van de snelle maken configureert basisinstellingen alleen. Het is nodig om aanvullende instellingen zoals de lijst met eindpunten en de volgorde van de failover-eindpunt te configureren.


## <a name="disable-enable-or-delete-a-profile"></a>Uitschakelen, inschakelen of verwijderen van een profiel

U kunt een bestaand profiel kunt uitschakelen, zodat deze Manager verkeer aanvragen van gebruikers niet naar de geconfigureerde eindpunten verwijst. Wanneer u een profiel verkeer Manager uitschakelt, worden het profiel en de gegevens in het profiel behouden blijven en kunnen worden bewerkt in de interface verkeer Manager.  Verwijzingen cv wanneer u het profiel opnieuw inschakelen. Wanneer u een profiel verkeer Manager in de portal van Azure klassieke maakt, wordt deze automatisch ingeschakeld. Als u dat een profiel is niet meer nodig, kunt u deze verwijderen.

### <a name="to-disable-a-profile"></a>Een profiel uitschakelen

1. Als u een aangepaste domeinnaam gebruikt, wijzig de CNAME-record op uw Internet DNS-server zodat deze niet meer aan uw profiel verkeer Manager verwijst.
2. Verkeer drukt, stopt wordt doorgestuurd naar de eindpunten tot en met de profielinstellingen verkeer Manager.
3. Selecteer het profiel dat u wilt uitschakelen. Markeer het profiel door te klikken op de kolom naast de naam van een profiel op de pagina voor het verkeer beheren. Opmerking, klikt u op de naam van het profiel of de pijl naast de naam wordt geopend de instellingenpagina voor het profiel.
4. Selecteer het profiel en klik op **uitschakelen** onder aan de pagina.

### <a name="to-enable-a-profile"></a>Een profiel inschakelen

1. Selecteer het profiel dat u wilt uitschakelen. Markeer het profiel door te klikken op de kolom naast de naam van een profiel op de pagina voor het verkeer beheren. Opmerking, klikt u op de naam van het profiel of de pijl naast de naam wordt geopend de instellingenpagina voor het profiel.
2. Selecteer het profiel en klik op **inschakelen** onder aan de pagina.
3. Als u een aangepaste domeinnaam gebruikt, maakt u een resource CNAME-record op de Internet DNS-server te laten verwijzen naar de naam van het domein van uw profiel verkeer Manager.
4. Verkeer wordt doorgestuurd naar de eindpunten opnieuw.

### <a name="to-delete-a-profile"></a>Een profiel te verwijderen

1. Zorg ervoor dat de DNS-resource record op uw Internet DNS-server niet langer een CNAME-record voor resource die naar de naam van het domein van uw profiel verkeer Manager verwijst gebruikt.
2. Selecteer het profiel dat u wilt uitschakelen. Markeer het profiel door te klikken op de kolom naast de naam van een profiel op de pagina voor het verkeer beheren. Opmerking, klikt u op de naam van het profiel of de pijl naast de naam wordt geopend de instellingenpagina voor het profiel.
3. Selecteer het profiel en klik op **verwijderen** onder aan de pagina.

## <a name="view-traffic-manager-profile-change-history"></a>Weergave verkeer Manager profiel wijzigingsoverzicht

U kunt het wijzigingsoverzicht weergeven voor uw profiel verkeer Manager in de Azure klassieke portal in Management-Services.

### <a name="to-view-your-traffic-manager-change-history"></a>Als u wilt bekijken uw Manager verkeer wijzigingsoverzicht

1. Klik in het linkerdeelvenster van de Azure klassieke portal op **Management Services**.
2. Klik op **Bewerking logboeken**op de pagina Management-Services.
3. Klik op de pagina bewerking Logboeken kunt u filteren om het wijzigingsoverzicht voor uw profiel verkeer Manager weer te geven. Selecteer uw filteropties en klik op het vinkje om de resultaten weer te geven.

   - De wijzigingen wilt bekijken voor al uw profielen, selecteert u uw abonnement en tijdsbereik en selecteer vervolgens **Verkeer Manager** in het snelmenu **Type** .
   - Als u wilt filteren op de naam van een profiel, typ de naam van het profiel in het veld **Naam van de Service** of selecteert u deze in het snelmenu te openen.
   - Als u wilt weergeven voor elke afzonderlijke wijziging, selecteert u de rij met de wijziging die u wilt weergeven en klik vervolgens op **Details** onder aan de pagina. U kunt de XML-weergave van het object API die is gemaakt of bijgewerkt als onderdeel van de bewerking weergeven in het venster **Bewerkingsdetails** .

## <a name="next-steps"></a>Volgende stappen

- [Een eindpunt toevoegen](traffic-manager-endpoints.md)
- [Failover-mailroutering methode configureren](traffic-manager-configure-failover-routing-method.md)
- [Round robin routeren methode configureren](traffic-manager-configure-round-robin-routing-method.md)
- [Prestaties routeren methode configureren](traffic-manager-configure-performance-routing-method.md)
- [Een bedrijfsdomein Internet wijst u een domeinnaam verkeer Manager](traffic-manager-point-internet-domain.md)
- [Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)