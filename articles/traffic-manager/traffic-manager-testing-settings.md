<properties
    pageTitle="Testen verkeer Manager instellingen | Microsoft Azure"
    description="In dit artikel kunt u test verkeer Manager-instellingen"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="test-your-traffic-manager-settings"></a>Test uw instellingen voor verkeer Manager

Als u wilt test uw instellingen voor verkeer Manager, moet u beschikken over meerdere klanten, op verschillende locaties, van waaruit u kunt uw tests uitvoeren. Brengt u de eindpunten in uw profiel verkeer Manager omlaag één voor één.

* De DNS-TTL-waarde laag ingesteld zodat wijzigingen snel (bijvoorbeeld 30 seconden doorgeven).
* Weet het IP-adressen van uw Azure cloudservices en websites in het profiel dat u wilt testen.
* Gebruik hulpmiddelen waarmee u een DNS-naam omzetten in een IP-adres en dat adres weer te geven.

Als u wilt zien dat de DNS-namen omzetten in IP-adressen van de eindpunten in uw profiel dat u controleert. De namen moeten oplossen op een wijze die consistent is met de verkeer routeren methode gedefinieerd in het profiel verkeer Manager. U kunt de hulpprogramma's zoals **nslookup** of **graven** gebruiken voor het oplossen van DNS-namen.

De volgende voorbeelden kunnen u uw profiel verkeer Manager testen.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Controleer verkeer Manager profiel nslookup en ipconfig gebruiken in Windows

1. Open een opdracht of Windows PowerShell-prompt als beheerder.
2. Type `ipconfig /flushdns` naar de DNS-resolvercache leegmaken.
3. Type `nslookup <your Traffic Manager domain name>`. De volgende opdracht controleert bijvoorbeeld de naam van het domein met het voorvoegsel *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Het resultaat van een typisch bevat de volgende informatie:

    * De naam van de DNS- en IP-adres van de DNS-server om op te lossen deze domeinnaam verkeer Manager wordt geopend.
    * De domeinnaam verkeer Manager u hebt getypt op de opdrachtregel na 'nslookup' en het IP-adres waarnaar lost u het domein verkeer Manager. Het tweede IP-adres is het belangrijk om te controleren. Dit moet overeenkomen met een openbaar virtuele IP-(VIP) adres voor een van de cloudservices of websites in het verkeer Manager profiel dat u wilt testen.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Het testen van de failover-verkeer routeren methode

1. Laat alle eindpunten omhoog.
2. Een enkele client gebruikt, het omzetten van DNS voor uw bedrijf domeinnaam met nslookup of een vergelijkbare hulpprogramma aanvragen
3. Zorg ervoor dat het opgelost IP-adres overeenkomt met het primaire-eindpunt.
4. Buiten uw primaire eindpunt aan of verwijderen van de controle bestand zodat deze Manager verkeer denkt dat de toepassing is niet beschikbaar.
5. Wacht totdat de DNS-Time to Live (TTL) van het profiel verkeer Manager plus twee minuten extra. Bijvoorbeeld: als uw DNS-TTL is 300 seconden (5 minuten), u moet wachten zeven minuten.
6. Uw DNS-cache en verzoek om DNS-resolutie van client met nslookup leegmaken. In Windows, kunt u uw DNS-cache met de opdracht ipconfig/flushdns leegmaken.
7. Zorg ervoor dat het opgelost IP-adres overeenkomt met uw secundaire eindpunt.
8. Herhaal het proces, beurtelings te brengen naar beneden elk eindpunt. Controleer of de DNS-records geeft als resultaat het IP-adres van de volgende eindpunt in de lijst. Wanneer alle eindpunten niet beschikbaar zijn, moet u het IP-adres van het primaire eindpunt opnieuw ontvangen.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Hoe u kunt de methode gewogen verkeer rondsturen testen

1. Laat alle eindpunten omhoog.
2. Een enkele client gebruikt, het omzetten van DNS voor uw bedrijf domeinnaam met nslookup of een vergelijkbare hulpprogramma aanvragen
3. Zorg ervoor dat het opgelost IP-adres overeenkomt met een van de eindpunten.
4. Uw DNS-client-cache leegmaken en herhaalt u stappen 2 en 3 voor elk eindpunt. Hier ziet u verschillende IP-adressen voor elk van uw eindpunten geretourneerd.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Het testen van de prestaties verkeer routeren methode

Als u wilt testen effectief een prestaties verkeer routeren methode, moet u clients die zich bevinden in verschillende gedeelten van de wereld hebben. U kunt clients maken in verschillende Azure regio's die kunnen worden gebruikt om te testen van uw services. Als u een globale-netwerk hebt, kunt u op afstand Meld u aan bij-clients in andere delen van de wereld en voert u uw tests daarvandaan.

U kunt ook zijn er vrij te geven op het web DNS-zoekopdracht en graven services beschikbaar. Sommige hulpprogramma's kunt u de mogelijkheid om te controleren van DNS-naamresolutie vanuit verschillende locaties overal ter wereld. Voer een zoekopdracht op 'Zoeken DNS' voor voorbeelden. Externe services zoals Gomez of Keynote kunnen worden gebruikt om te bevestigen dat uw profielen verkeer distribueren zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

* [Over verkeer Manager verkeer routeren methoden](traffic-manager-routing-methods.md)
* [Prestatieoverwegingen verkeer Manager](traffic-manager-performance-considerations.md)
* [Probleemoplossing verkeer Manager niet beschikbaar is weergegeven staat](traffic-manager-troubleshooting-degraded.md)




