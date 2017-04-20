<properties
 pageTitle="Beheerde apparaten achter een gateway IoT inschakelen | Microsoft Azure"
 description="Richtlijnen onderwerp via een IoT Gateway gemaakt met de SDK van de Gateway IoT samen met apparaten die worden beheerd door IoT Hub."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Beheerde apparaten achter een gateway IoT inschakelen

## <a name="basic-device-isolation"></a>Eenvoudige apparaat moeten worden geïsoleerd

IoT gateways organisaties vaak gebruiken om te verhogen van de algehele beveiliging van hun IoT-oplossingen. Sommige apparaten moeten gegevens verzenden naar de cloud, maar kunnen zelf beveiligen tegen threats op internet. U kunt deze apparaten uit externe threads beschermen door ze communiceren met externe wereld via een gateway.

De gateway bevindt zich op de rand tussen een beveiligde omgeving en de geopende internet. Apparaten Neem contact op met de gateway en de gateway de berichten langs doorstuurt naar de bestemming van de juiste cloud. De gateway is harde ten opzichte van externe threads, blokkeert onbevoegde verzoeken, staat geautoriseerde in gebonden verkeer en dat in gebonden verkeer naar het juiste apparaat doorstuurt.

![][1]

U kunt ook apparaten die zichzelf achter een gateway voor een toegevoegde laag voor waardepapier beschermen kunnen plaatsen. In dit scenario moet u alleen de gateway OS hersteld ten opzichte van de meest recente problemen, in plaats van het besturingssysteem op elk apparaat bijwerken behouden.

## <a name="isolation-plus-intelligence"></a>Moeten worden geïsoleerd plus bedrijfsinformatie

Een harde router is een voldoende gateway te isoleren gewoon apparaten. Echter vereisen IoT oplossingen vaak dat een gateway vindt u meer intelligence dan gewoon isoleren apparaten. U wilt bijvoorbeeld uw apparaten beheren vanuit de cloud. U zijn kunt gebruiken [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), een standaardapparaat management protocol, voor het deel van de cloud beheer van de oplossing. Maar verzenden de apparaten met een niet TCP/IP telemetrielogboek ingeschakeld protocol. Bovendien de apparaten produceren grote hoeveelheden gegevens en u alleen wilt uploaden van een gefilterde subset van de telemetrielogboek. U kunt een oplossing waarin een gateway IoT kunnen omgaan met twee afzonderlijke streams van gegevens maken. De gateway moet uitvoeren:

-   Meer informatie over het **Telemetrielogboek**, filteren en uploadt dit naar de cloud via de gateway. De gateway is niet langer een eenvoudige router die stuurt alleen maar gegevens tussen het apparaat en de cloud.

-   Gewoon exchange de **LWM2M apparaat management gegevens** tussen de apparaten en de cloud. De gateway niet hoeft te begrijpen van de gegevens die afkomstig zijn erin en hoeft alleen om ervoor te zorgen dat de gegevens heen en weer wordt doorgegeven tussen de apparaten en de cloud.

De volgende afbeelding ziet u dit scenario:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>De oplossing: Azure IoT Apparaatbeheer en de SDK van de Gateway IoT 

Voorbeeld van het publiek versie van [Azure IoT Apparaatbeheer] [ lnk-device-management] en bètaversie van de [Azure IoT Gateway SDK] dit scenario inschakelen. De gateway omgaat met elke gegevensstroom als volgt:

-   **Telemetrielogboek**: U kunt de SDK van de Gateway IoT gebruiken om te maken van een pijplijn die begrijpt, filters en verzendt telemetriegegevens in de cloud. De SDK van de Gateway IoT biedt code die wordt geïmplementeerd onderdelen van deze verkooppijplijn namens de ontwikkelaar. U vindt meer informatie over de architectuur van de SDK in de [IoT Gateway SDK - aan de slag] [ lnk-gateway-get-started] zelfstudie.

-   **Apparaatbeheer**: Azure Apparaatbeheer biedt een LWM2M-client die wordt uitgevoerd op het apparaat, evenals een cloud-interface voor het verlenen van opdrachten voor het beheer aan op het apparaat.
    
    U kunt een speciale logica op de gateway geen nodig, omdat deze niet hoeft te verwerken de LWM2M gegevens uitwisselen tussen het apparaat en uw hub IoT. Hier kunt u Internetverbinding delen, een functie van veel moderne besturingssystemen, gaat u op de gateway bij naar de uitwisseling van gegevens LWM2M inschakelen. U kunt een geschikt besturingssysteem wordt uitgevoerd voor dit scenario omdat de SDK van de Gateway IoT tal van besturingssystemen worden ondersteund. Hier vindt u instructies voor het inschakelen van internetverbinding delen op [Windows 10] en [Ubuntu], twee van de vele ondersteunde besturingssystemen.

De volgende afbeelding ziet u de hoogste niveau architectuur gebruikt voor het inschakelen van dit scenario met [Azure IoT Apparaatbeheer] [ lnk-device-management] en de [SDK van Azure IoT Gateway].

![][3]

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het gebruik van de SDK van de Gateway IoT, raadpleegt u de volgende zelfstudies:

- [IoT Gateway SDK - aan de slag met Linux][lnk-gateway-get-started]
- [IoT Gateway SDK – apparaat naar cloud-berichten verzenden met een gesimuleerd apparaat gebruik Linux][lnk-gateway-simulated]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure IoT Gateway SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md