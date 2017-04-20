<properties
    pageTitle="Voorbeeld van MyDriving Azure IoT: snel aan de slag | Microsoft Azure"
    description="Aan de slag met een app die is een volledig demonstratie van hoe u een systeem IoT ontwerpen met behulp van Microsoft Azure, met inbegrip van de Stream analyses en Machine Learning gebeurtenis Hubs."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT systeem: snel aan de slag

MyDriving is een systeem waarop u het ontwerp en de uitvoering van een typisch [Internet van dingen](iot-suite-overview.md) (IoT)-oplossing die worden verzameld telemetrielogboek op apparaten, die gegevens in de cloud worden verwerkt en is van toepassing machine learning-te leveren een geavanceerde antwoord wordt verzonden. Gegevens over uw reizen auto het tekstvenster geregistreerd met behulp van gegevens uit uw mobiele telefoon en een adapter die gegevens van uw auto control-mailsysteem verzamelt. Deze worden deze gegevens gebruikt om feedback te geven op uw groot stijl ten opzichte van andere gebruikers.

De reële doel van MyDriving is om u te helpen bij het maken van uw eigen IoT-oplossing. Maar daarvoor, laten we u aan de slag kan gaan met de MyDriving-app--als lid van het team van de gebruiker test. Hiermee geeft u een ervaring van de app en het systeem erachter als consument, voordat u de architectuur dieper. Deze ook maakt u kennis met HockeyApp, een leuke manier voor het beheren van de alfa- en bèta-verdeling van uw apps om te testen van gebruikers.

## <a name="use-the-mobile-experience"></a>Gebruik de mobiele ervaring

Als u een Android-, iOS- of Windows 10-apparaat hebt, kunt u de app MyDriving gebruiken.

### <a name="android-and-windows-10-mobile-installation"></a>Android en Windows 10 Mobile installatie

Op uw apparaat:

1.  Toestaan dat apps ontwikkeling:

    -   Android: Klik In het dialoogvenster **Instellingen** > **beveiliging**en apps toestaan van **onbekende bronnen**.

    -   Windows 10: Klik In het dialoogvenster **Instellingen** > **Updates** > **Voor ontwikkelaars**, **modus voor ontwikkelaars**instellen.

2.  Deelnemen aan het testteam bèta-zich registreert met of aanmelden bij [HockeyApp](https://rink.hockeyapp.net). HockeyApp kunt u gemakkelijk distributie van eerdere versies van uw app om te testen van gebruikers.

    Als u Windows 10 gebruikt, gebruikt u de browser Edge.

    Als u een deelnemer opbouwen 2016, meld u aan met het hetzelfde Microsoft-account e-mailbericht dat u geregistreerd voor de vergadering, met behulp van een van de Microsoft-knoppen. U bent al hebben geregistreerd met HockeyApp.

    ![Aanmeldingsscherm van HockeyApp](./media/iot-solution-get-started/image1.png)

3.  Download en installeer de app van hier:

    -   [Android-apparaat](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    Er zijn twee items. Installeer het certificaat in **Vertrouwde personen**. Installeer de app.

*Eventuele problemen starten van de app voor Windows 10 Mobile?* Uw telefoon mogelijk een update of twee achter. Controleer of dat u de meest recente updates hebt gebruikt, of installeren:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>iOS-installatie

Als u opbouwen 2016 hebt gevolgd, kunt u de app downloaden als lid van het testteam op HockeyApp:

1.  Klik op uw iOS-apparaat, moet u zich aanmelden bij [HockeyApp](https://rink.hockeyapp.net).
    Gebruik een van de Microsoft-aanmelding knoppen en meld u aan met het hetzelfde Microsoft-account e-mailbericht dat u met de vergadering hebt geregistreerd. (Gebruik niet de velden e-mailadres en wachtwoord).

    ![Aanmeldingsscherm van HockeyApp](./media/iot-solution-get-started/image1.png)

2.  In het dashboard HockeyApp MyDriving selecteren en deze downloaden.

3.  Autoriseer de bètaversie van HockeyApp:

    een. Ga naar **Instellingen** > **algemene** > **profielen en Apparaatbeheer.**

    b. Het certificaat **Bits Stadium GmbH** vertrouwt.

Als u niet deelneemt aan opbouwen 2016, kunt u bouwen en implementeren van de app zelf:

1.   Downloaden van de code [uit GitHub].

2.   Bouw en implementeer met [behulp van Xamarin].

In de [Naslaggids voor het MyDriving](http://aka.ms/mydrivingdocs)vindt u meer informatie.

## <a name="get-an-obd-adapter-optional"></a>Krijg een OBD-adapter (optioneel)

Dit is het gedeelte dat dit een reële Internet van zaken-systeem is! Kunt u de app zonder een, maar ze geen dure spelen met de echte is.

Ingebouwde diagnostische gegevens (OBD) is de functie Auto waarmee de garage worden afstemmen van uw auto en diagnose stellen bij oneven geluiden en waarschuwing lichten. Tenzij uw auto van geweldige antiquity, vindt u een socket ergens in de stuurcabine, meestal achter een flap onder het dashboard. Met de juiste verbindingslijn, kunt u de doelstellingen van de prestaties van de engine en bepaalde wijzigingen aanbrengen. Een verbindingslijn OBD-kan goedkoop worden aangeschaft op de gebruikelijke plaatsen. Deze verbinding maakt via Bluetooth of Wi-Fi aan een app op uw telefoon.

In dit geval gaan we uw auto verbinden met de cloud. Directe verbindingen vanuit de OBD is op uw telefoon, maar onze app werkt doorsturen. Uw auto telemetrielogboek wordt verzonden rechtstreeks naar de hub MyDriving IoT waar deze aan te melden uw weg reizen en beoordeel uw groot stijl wordt verwerkt.

Verbinding met het maken van een OBD-apparaat:

1.  Controleer of uw auto een socket OBD-heeft.

2.  Een OBD-adapter verkrijgen:

    -   Als u een Android- of Windows-telefoon gebruikt, moet u een OBD-II voor Bluetooth-adapter. We [BAFX producten 34t5 Bluetooth OBDII Scan Tool]gebruikt.

    -   Als u een iOS-telefoon gebruikt, moet u een Wi-Fi-ingeschakeld OBD-adapter. We gebruikt [ScanTool OBDLink MX Wi-Fi: OBD-Adapter/Diagnostic Scanner].

3.  Volg de instructies die worden meegeleverd bij uw OBD-adapter Maak verbinding met uw telefoon. Houd het volgende in gedachten:

    -   Een Bluetooth-adapter moet worden gekoppeld aan de telefoon, klikt u op de pagina **Instellingen** .

    -   Een Wi-Fi-adapter moet er een adres in het bereik 192.168.xxx.xxx.

4.  Als u verschillende auto's hebt, kunt u afzonderlijke adapter verkrijgen voor elke (maximaal drie).

Als u een OBD-adapter niet hebt, wordt de app nog steeds stuurt gegevens locatie en de snelheid van GPS-ontvanger van de telefoon naar de back-end en wordt u gevraagd of u wilt een OBD simuleren.

U vindt meer informatie over het gebruik van gegevens uit de OBD-adapter in de app en opties voor het maken van uw eigen OBD-apparaat in de sectie 2.1, "IoT apparaten', in de [Naslaggids voor het MyDriving](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>De app gebruiken

Start de app. Er is een eerste Quickstart u stapsgewijs door hoe dit werkt.

### <a name="track-your-trips"></a>Uw reizen bijhouden

Tik op de knop opnemen (groot rode cirkel onderaan in het scherm) als u wilt een reis starten en tik nogmaals om te beëindigen.

![Afbeelding van de opnameknop voor reis bijhouden](./media/iot-solution-get-started/image2.png)

Telkens wanneer die u een reis start als er geen OBD-apparaat, wordt u gevraagd of u wilt gebruiken de simulator.

Aan het einde van een reis, tikt u op de knop stoppen en krijgt u een overzicht.

![Voorbeeld van een samenvatting reis](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Controleer uw reizen

![Voorbeeld van een eerdere reis](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Uw profiel bekijken

![Voorbeeld van een profiel sturende-stijl](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Stuur ons uw feedback testen

Omdat we hebben gemaakt MyDriving om u te helpen aan de slag met uw eigen IoT-systemen, horen we zeker graag van u hoe u ook de werkstroom werkt. Laat het ons weten als:

- U ondervindt problemen of uitdagingen.

- Er is een extensiepunt dat u meer geschikt is voor uw scenario.

- U vinden een efficiënte manier voor het uitvoeren van bepaalde behoeften.

- U hebt andere suggesties voor het verbeteren van MyDriving of deze documentatie.

Binnen de app MyDriving zelf, kunt u de methode voor feedback van ingebouwde HockeyApp: op iOS en Android, net Geef uw telefoon een schudden of gebruikt u de opdracht **Feedback** -menu. Dit koppelen een schermafbeelding, zodat we wat u belt weet over automatisch. En als er een ongelukkige loopt, HockeyApp de logboeken vastlopen om u te laat het ons weten ze verzamelt. U kunt ook feedback via de [portal HockeyApp].

U kunt ook een [probleem op GitHub]bestand of kan het beter hieronder (en-us edition).

We hoor graag van u!

## <a name="next-steps"></a>Volgende stappen

-   Verken de [Naslaggids voor het MyDriving](http://aka.ms/mydrivingdocs) als u wilt weten hoe wij hebt ontworpen en het hele MyDriving systeem ingebouwd.

-   [Maken en implementeren van een systeem van uw eigen](iot-solution-build-system.md) met behulp van onze resourcemanager Azure-scripts. De [Naslaggids voor het MyDriving](http://aka.ms/mydrivingdocs) ook u wordt begeleid gebieden waar u de meeste aanpassingen kunt aanbrengen.

  [uit GitHub]: https://github.com/Azure-Samples/MyDriving
  [Xamarin gebruiken]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX producten 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS
  [ScanTool OBDLink MX Wi-Fi: OBD-Adapter/Diagnostic Scanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp-portal]: https://rink.hockeyapp.org
  [actie op GitHub]: https://github.com/Azure-Samples/MyDriving/issues
