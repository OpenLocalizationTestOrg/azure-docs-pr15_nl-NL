<properties
    pageTitle="App-V apps gebruikt met Azure RemoteApp | Microsoft Azure"
    description="Leer hoe u de App-V-apps gebruiken in Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>App-V-apps gebruiken in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U kunt App-V toepassingen gebruiken in een verzameling Azure RemoteApp hybride, waarvoor aan domein toevoegen.

Voordat u begint, zorg er dan voor dat de App-V 5.1-client met de meest recente updates installeren. U moet maken van een [aangepaste afbeelding](remoteapp-create-custom-image.md) met de App-V-client.  

Het is eenvoudig gebruik van de infrastructuur van uw bestaande App-V met Azure RemoteApp. Aangezien een verzameling hybride wordt geïmplementeerd in een Azure-VNET die toegang tot uw domeincontroller heeft en de VMs domein toegevoegd zijn, kunt u gebruikmaken van uw bestaande App-v infrastructuur en implementatie methoden naar easyily hosttoepassing App-V in Azure RemoteApp. Hier volgen enkele manieren waarmee u rekening moet houden op basis van het type App-V implementatie die u momenteel hebt:

| Configuratieopties |                       | Positief                                                               | Negatief                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Afleveringsmethode       | Streaming (op aanvraag) | De App is altijd de meest recente en vers                                     | Eerste tijd latentie                                                                                    |
|                       | Gekoppeld               | Snelste; App is al aanwezig op de VM                              | Groter worden - inneemt afbeelding ruimte (limiet 127 GB)                                                           |
| App locatie-opslag  | Gedeelde inhoud        | App wordt uitgevoerd in het geheugen van Azure RemoteApp exemplaar                         | Eats geheugen en goede verbinding met streaming (bestandsserver) waarin de app zich bevindt                      |
|                       | Schijf (in cache)         | Snel worden uitgevoerd. De App is niet afhankelijk van de beschikbaarheid van inhoudsbron | Groter worden - inneemt afbeelding ruimte (limiet 127 GB)                                                           |
| Doelgroepen             | Gebruiker                  | Volledige zelfstandige App-V infrastructuur vereist                          |                                                                                                       |
|                       | Globale (machine)      |  Vooraf publiceren of afstemmen met de publicatie van server                         |  Moet bijwerken van uw Azure afbeelding als u wilt bijwerken van de app (enorme). Hiermee gaat vrij op afbeelding. |

 Nadat u uw aangepaste afbeelding en de verzameling hybride hebt gemaakt, uw toepassing publiceren gebruikers toe te wijzen en profiteren van uw bestaande App-V-toepassingen die worden gehost in Azure RemoteApp afgeleverd in een willekeurige plaats elk apparaat.
