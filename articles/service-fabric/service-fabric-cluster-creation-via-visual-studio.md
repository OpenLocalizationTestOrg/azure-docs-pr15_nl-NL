<properties
   pageTitle="Instellen van een Service stof cluster met Visual Studio | Microsoft Azure"
   description="Wordt uitgelegd hoe u voor het instellen van een Service stof cluster met behulp van Azure resourcemanager sjabloon gemaakt door een resourcegroep Azure-project in Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Een Service stof cluster instellen met behulp van Visual Studio
Dit artikel wordt beschreven hoe u een cluster Azure-Service stof instelt met behulp van Visual Studio en een resourcemanager Azure-sjabloon. We een groepsproject voor Visual Studio Azure resource gebruiken om de sjabloon te maken. Nadat de sjabloon is gemaakt, kan deze rechtstreeks naar Azure worden geïmplementeerd in Visual Studio. Dit kan ook worden gebruikt vanaf een script of als onderdeel van continue integratie (CI) faciliteit.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Een Service stof cluster-sjabloon maken met behulp van een groepsproject Azure resource
Als u wilt beginnen, opent u Visual Studio en maken van een project van Azure resource groep (deze beschikbaar is in de **Cloud** -map):

![Dialoogvenster voor de nieuwe Project met Azure resourcegroep project geselecteerd][1]

U kunt een nieuwe Visual Studio-oplossing voor dit project te maken of toevoegen aan een bestaande oplossing.

>[AZURE.NOTE] Als u het project Azure resource groep onder het knooppunt Cloud niet ziet, hoeft u niet de Azure SDK is geïnstalleerd. Web Platform Installer ([deze nu installeren](http://www.microsoft.com/web/downloads/platform.aspx) als u nog niet hebt), starten en zoek vervolgens naar 'Azure SDK voor .NET' en de versie die compatibel is met uw versie van Visual Studio installeren.

Nadat u de knop OK raakt, Visual Studio wordt u gevraagd om te selecteren van de Resource Manager-sjabloon die u wilt maken:

![Dialoogvenster Azure sjabloon selecteren met het geselecteerde Service stof Cluster-sjabloon][2]

Selecteer de sjabloon Service stof Cluster en druk op de knop OK opnieuw. Het project en de sjabloon resourcemanager zijn nu gemaakt.

## <a name="prepare-the-template-for-deployment"></a>De sjabloon voorbereiden voor implementatie
Voordat u de sjabloon wordt geïmplementeerd als de cluster wilt maken, moet u waarden opgeven voor de vereiste Sjabloonparameters. Deze parameterwaarden worden opgelezen uit de `ServiceFabricCluster.parameters.json` bestand, dat in de `Templates` map van het project van de groep resource. Open het bestand en geef de volgende waarden:

|Parameternaam           |Beschrijving|
|-----------------------  |--------------------------|
|adminUserName            |De naam van het beheerdersaccount voor Service stof machines (knooppunten).|
|certificateThumbprint    |De vingerafdruk van het certificaat waarmee het cluster beveiligt.|
|sourceVaultResourceId    |De *resource-ID* van de belangrijkste kluis waarin het certificaat dat het cluster beveiligt is opgeslagen.|
|certificateUrlValue      |De URL van het beveiligingscertificaat cluster.|

De sjabloon stof resourcemanager voor Visual Studio Service wordt gemaakt van een beveiligde cluster dat is beveiligd met een certificaat. Dit certificaat wordt aangegeven door de laatste drie sjabloonparameters (`certificateThumbprint`, `sourceVaultValue`, en `certificateUrlValue`), en deze in een **Azure toets kluis**moet bestaan. Voor meer informatie over het maken van het beveiligingscertificaat cluster, Zie [Service stof cluster beveiliging scenario's](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) artikel.

## <a name="optional-change-the-cluster-name"></a>Optioneel: de clusternaam van de wijzigen
Elke Service stof cluster heeft een naam. Wanneer een cluster stof is gemaakt in Azure wordt aangegeven, bepaalt de naam van cluster (samen met de Azure regio) de naam van de Domain Name System (DNS) voor het cluster. Als u naam toewijzen aan uw cluster bijvoorbeeld `myBigCluster`, en de locatie (Azure regio) van de resourcegroep die wordt gehost in het nieuwe cluster Oost VS, is de DNS-naam van het cluster `myBigCluster.eastus.cloudapp.azure.com`.

Standaard is de naam van de cluster automatisch gegenereerd en die uniek zijn aangebracht door een willekeurig achtervoegsel koppelen aan een voorvoegsel 'cluster'. Dit kunt u heel gemakkelijk de sjabloon wilt gebruiken als onderdeel van een systeem **continue integratie** (CI). Als u wilt gebruiken van een specifieke naam voor uw cluster, een duidelijke voor u instellen dat de waarde van de `clusterName` variabele in het sjabloonbestand resourcemanager (`ServiceFabricCluster.json`) naar de naam van de door u gekozen. Dit is de eerste variabele gedefinieerd in dat bestand.

## <a name="optional-add-public-application-ports"></a>Optioneel: openbare toepassing poorten toevoegen
U kunt ook de poorten openbare toepassing voor de cluster wijzigen voordat u deze implementeert. Standaard is de sjabloon wordt geopend van slechts twee openbare TCP-poorten (80 en 8081). Als u meer voor uw toepassingen nodig hebt, wijzigt u de definitie van taakverdeling Azure in de sjabloon. De definitie is opgeslagen in de belangrijkste sjabloonbestand (`ServiceFabricCluster.json`). Open dat bestand en zoek naar `loadBalancedAppPort`. Elke poort is gekoppeld aan drie onderdelen:

1. Een sjabloonvariabele dat de TCP-poorten-waarde voor de poort definieert:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. Een *test* waarmee wordt gedefinieerd hoe vaak en hoe lang de Azure laden verdeling probeert te gebruiken van een specifiek Service stof knooppunt voordat deze is mislukt over naar een andere. De sondes maken deel uit van de resource van taakverdeling. Hier ziet u de definitie test voor de eerste standaardpoort van toepassing:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. Een *regel taakverdeling* die gekoppeld aan de poorten en de test, waarmee van taakverdeling voor een reeks knooppunten Service stof:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Als de toepassingen die u wilt implementeren naar het cluster nodig meer poorten hebt, kunt u deze toevoegen door te maken, extra test en regeldefinities van taakverdeling. Zie [aan de slag een interne taakverdeling met een sjabloon maken](../load-balancer/load-balancer-get-started-ilb-arm-template.md)voor meer informatie over het werken met Azure taakverdeling via resourcemanager sjablonen.

## <a name="deploy-the-template-by-using-visual-studio"></a>De sjabloon met behulp van Visual Studio implementeren
Nadat u hebt opgeslagen dat de vereiste parameterwaarden in de`ServiceFabricCluster.param.dev.json` bestand, u bent klaar om de sjabloon te implementeren en uw cluster Service stof te maken. Met de rechtermuisknop op de resource groepsproject in Visual Studio Solution Explorer en kies **Deploy | Nieuwe implementatie...**. Zo nodig wordt Visual Studio weergegeven in het dialoogvenster **Deploy aan resourcegroep** waarin u te verifiëren bij Azure:

![Dialoogvenster resourcegroep te implementeren][3]

Het dialoogvenster u kunt kiezen uit een bestaande resourcegroep resourcemanager voor het cluster en geeft u de optie voor het maken van een nieuwe record. Gewoonlijk is het handig voor het gebruik van een afzonderlijke resourcegroep voor een cluster Service stof.

Nadat u de knop Deploy raakt, vraagt Visual Studio u om te bevestigen van de sjabloon parameterwaarden. Druk op de knop **Opslaan** . Een parameter heeft geen een permanente waarde: het beheerdersaccount-wachtwoord voor het cluster. U moet een wachtwoordwaarde opgeven als Visual Studio wordt u gevraagd om een.

>[AZURE.NOTE] Beginnen met Azure SDK 2,9, ondersteunt Visual Studio lezen wachtwoorden uit **Kluis van Azure-toets** tijdens de implementatie. Klik in het dialoogvenster van de parameters sjabloon zoals u ziet dat de `adminPassword` parameter tekstvak heeft een pictogram voor een klein "sleutel" aan de rechterkant. Dit pictogram kunt u een bestaande belangrijke kluis geheim selecteert als het beheerderswachtwoord voor het cluster. Zorg ervoor dat voor het inschakelen van Azure resourcemanager toegang voor sjabloonimplementatie in de geavanceerde-beleid van uw belangrijkste kluis. 

U kunt de voortgang van het implementatieproces in het uitvoervenster Visual Studio controleren. Wanneer de sjabloonimplementatie is voltooid, wordt het nieuwe cluster is klaar voor gebruik!

>[AZURE.NOTE] Als u PowerShell is nooit gebruikt voor het beheren van Azure vanaf de computer waarop u nu gebruikt, moet u doen met een huishoudelijke.
>1. Inschakelen PowerShell-scripts door te voeren de [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) opdracht. Voor de ontwikkeling machines is "niet-beperkte" beleid meestal acceptabel.
>2. Beslissen of u wilt toestaan dat diagnostische gegevens verzamelen via Azure PowerShell-opdrachten en uitvoeren [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) of [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) zo nodig. Dit vermijdt onnodige aanwijzingen tijdens de sjabloonimplementatie.

Als er fouten zijn, gaat u naar de [Azure-portal](https://portal.azure.com/) en open de resourcegroep die u geïmplementeerd in. Klik op **alle instellingen**en klik op **implementaties** op het blad instellingen. Een mislukte resourcegroep implementatie verlaat er gedetailleerde diagnostische informatie.

>[AZURE.NOTE] Service stof clusters vereisen een bepaald aantal knooppunten moeten omhoog voor het behoud van beschikbaarheid en behouden state - genoemd "onderhouden quorum." Daarom niet veilige af te sluiten alle computers in het cluster tenzij u eerst een [volledige back-up van uw status](service-fabric-reliable-services-backup-restore.md)hebt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over het instellen van Service stof cluster met behulp van de Azure portal](service-fabric-cluster-creation-via-portal.md)
- [Meer informatie over het beheren en implementeren van Service stof toepassingen gebruik van Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
