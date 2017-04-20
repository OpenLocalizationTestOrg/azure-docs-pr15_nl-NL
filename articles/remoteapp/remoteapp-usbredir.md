<properties 
    pageTitle="Hoe leid u USB-apparaten in Azure RemoteApp om? | Microsoft Azure" 
    description="Informatie over het gebruik van omleiding voor USB-apparaten in Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Hoe leid u USB-apparaten in Azure RemoteApp om?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Omleiding-apparaten kan gebruikers de USB-apparaten die zijn bijgevoegd bij hun computer of tablet met de apps in Azure RemoteApp gebruiken. Bijvoorbeeld als u Skype via Azure RemoteApp gedeeld, moeten uw gebruikers kunnen hun apparaat camera's gebruiken.

Controleer voordat u verder gaat, of dat u de gegevens van de USB-omleiding in [werken met omleiding in Azure RemoteApp](remoteapp-redirection.md)lezen. Echter de aanbevolen nusbdevicestoredirect:s: * werkt niet voor USB-web-camera's en werkt mogelijk niet voor sommige USB-printers of multifunctionele USB-apparaten. Inherent aan het ontwerp en vanwege de beveiliging, wordt de beheerder van het Azure RemoteApp heeft omleiding inschakelen door te apparaatklasse-GUID of apparaat exemplaar-ID voordat uw gebruikers die apparaten kunnen gebruiken.

Hoewel in dit artikel moment over de omleiding van de camera web spreekt, kunt u een soortgelijke benadering doorsturen USB-printers en andere multifunctionele USB-apparaten die niet worden doorgestuurd door de **nusbdevicestoredirect:s:*** opdracht.

## <a name="redirection-options-for-usb-devices"></a>Omleiding van opties voor het USB-apparaten
Azure RemoteApp gebruikt vergelijkbaar regelingen voor USB-apparaten omleiden als die beschikbaar voor Remote Desktop Services. De onderliggende technologie kunt u de juiste omleidingsmethode kiezen voor een bepaald, om de beste van beide op hoog niveau en RemoteFX USB apparaat omleiding gebruiken de **usbdevicestoredirect:s:** opdracht. Er zijn vier elementen aan deze opdracht:

| Verwerkingsvolgorde | Parameter           | Beschrijving                                                                                                                |
|------------------|---------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1                | *                   | Hiermee selecteert u alle apparaten die niet worden opgehaald door op hoog niveau omleiding. Opmerking: standaard zo, * werkt niet voor USB-web-camera's.  |
|                  | {Apparaatklasse-GUID} | Hiermee selecteert u alle apparaten die overeenkomen met de opgegeven apparaat setup-klasse.                                                           |
|                  | USB\InstanceID      | Hiermee selecteert u een USB-apparaat dat is opgegeven voor de opgegeven exemplaar-ID.                                                                  |
| 2                | -USB\Instance-ID    | Hiermee verwijdert u de van omleidingsinstellingen voor het opgegeven apparaat.                                                                 |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Een USB-apparaat omleiden met behulp van het apparaatklasse-GUID
Er zijn twee manieren om te vinden van de apparaatklasse-GUID die kan worden gebruikt voor omleiding van het. 

De eerste optie is via de [System-Defined apparaat Setup klassen beschikbaar voor leveranciers](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Kies de klas die het meest overeenkomt met het apparaat is aangesloten op de lokale computer. Dit kan een beeldapparaat class of vastleggen videoapparaat class zijn voor digitale camera's. U moet doen sommige experimenten met de apparaatklassen om te vinden van de juiste klasse-GUID die met het lokaal aangesloten USB-apparaat (in onze geval de webcamera van uw werkt).

Een betere manier of de tweede optie, wordt als volgt te werk als u wilt zoeken de klasse-GUID van het apparaat:

1. Open de Apparaatbeheer, zoek het apparaat dat wordt omgeleid en met de rechtermuisknop en open vervolgens de eigenschappen.
![Open de Apparaatbeheer](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Kies op het tabblad **Details** , de eigenschap **Klasse-Guid**. De waarde die wordt weergegeven, is de klasse-GUID voor dat type apparaat.
![Camera-eigenschappen](./media/remoteapp-usbredir/ra-classguid.png)
3. Gebruik de waarde van de klasse-Guid apparaten die overeenkomen met deze doorsturen.

Bijvoorbeeld:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

U kunt meerdere apparaat omleidingen in de dezelfde cmdlet combineren. Bijvoorbeeld: als u wilt omleiden lokale opslag en een USB-web-camera, cmdlet ziet er zo uit:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Wanneer u apparaatomleiding door klasse GUID instellen worden alle apparaten die overeenkomen met die klasse GUID in de opgegeven collectie omgeleid. Als er meerdere computers in het lokale netwerk waarop de dezelfde USB-web camera's, kunt u bijvoorbeeld een enkel cmdlet om te leiden alle van de web-camera's uitvoeren.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Een USB-apparaat omleiden met behulp van de ID van het apparaat exemplaar

Als u meer fijnmazige controle wilt en om te bepalen omleiding per apparaat, kunt u de parameter van de omleiding **USB\InstanceID** .

Het lastigste onderdeel van deze methode het vinden van de USB-apparaat-ID. U hebt toegang tot de computer en de specifieke USB-apparaat nodig. Volg deze stappen:

1. Het apparaatomleiding in externe-bureaubladsessie inschakelen, zoals wordt beschreven in [hoe kan ik mijn apparaten en bronnen gebruiken in een extern bureaublad-sessie?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Open een verbinding met extern bureaublad en klik op **Opties weergeven**.
3. Klik op **Opslaan als** u de huidige verbindingsinstellingen op een RDP-bestand opslaan.  
    ![De instellingen opslaan als een RDP-bestand](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Kies een bestandsnaam en een locatie, bijvoorbeeld "MyConnection.rdp" en 'Dit PC\Documents', en sla het bestand.
5. Open het MyConnection.rdp-bestand met een teksteditor en zoek de exemplaar-ID van het apparaat dat u wilt omleiden.

Nu de exemplaar-ID in de volgende cmdlet gebruiken:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Help ons te helpen u 
Wist u naast de beoordeling van dit artikel en het toevoegen van opmerkingen omlaag onder, kunt u wijzigingen aanbrengen in het artikel zelf? Iets ontbreken? Iets mis? Ik iets dat is alleen verwarrend schrijven? Omhoog schuiven en klik op **bewerken op GitHub** als u wijzigingen wilt - die wordt verzonden naar ons voor revisie en vervolgens zodra we zich op deze afmelden, ziet u uw wijzigingen en verbeteringen hier.