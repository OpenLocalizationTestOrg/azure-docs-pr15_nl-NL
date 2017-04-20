<properties
   pageTitle="Aan de slag met Azure Batch CLI | Microsoft Azure"
   description="Een snelle introductie over de Batch-opdrachten in Azure CLI ophalen voor het beheren van Azure Batch serviceresources"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Aan de slag met Azure Batch CLI

De Interface met meerdere platforms Azure opdrachtregel (Azure CLI) kunt u uw Batchaccounts en resources zoals groepen, taken en taken in Linux, Mac en Windows opdracht houders beheren. U kunt met de CLI Azure uitvoeren en scripts veel van dezelfde taken die is uitgevoerd met de Batch API's, Azure-portal en Batch PowerShell-cmdlets.

In dit artikel is gebaseerd op Azure CLI versie 0.10.5.

## <a name="prerequisites"></a>Vereisten voor

* [Installeren van de Azure CLI](../xplat-cli-install.md)

* [De CLI Azure verbinden met uw Azure-abonnement](../xplat-cli-connect.md)

* Overschakelen naar **resourcemanager modus**:`azure config mode arm`

>[AZURE.TIP] Het is raadzaam dat u uw installatie Azure CLI regelmatig om te profiteren van de service-updates en verbeteringen bijwerkt.

## <a name="command-help"></a>Opdracht help

U kunt help-informatie voor elke opdracht weergeven in de CLI Azure door toe te voegen `-h` als de enige optie na de opdracht. Bijvoorbeeld:

* Voor Help-informatie voor de `azure` opdracht, invoeren:`azure -h`
* Als u een lijst met alle Batch-opdrachten in de CLI, gebruikt u:`azure batch -h`
* Als u informatie over het maken van een Batch-account, invoeren:`azure batch account create -h`

Als u twijfelt gebruiken de `-h` opdrachtregel optie Hulp eventuele Azure CLI weer te geven.

## <a name="create-a-batch-account"></a>Een Batch-account maken

Syntaxis:

    azure batch account create [options] <name>

Voorbeeld:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Hiermee maakt een nieuw account voor de Batch met de opgegeven parameters. U moet ten minste een locatie, de resourcegroep en de accountnaam opgeven. Als u een resourcegroep nog geen hebt, maakt u een door te voeren `azure group create`, en geeft u een van de Azure regio's (zoals "West ons") voor de `--location` optie. Bijvoorbeeld:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] De naam van het Batch-account moet uniek zijn binnen de Azure regio die het account is gemaakt. Er kan alleen kleine letters alfanumerieke tekens bevatten en moet 3-24 tekens lang zijn. U kunt geen gebruikmaken van speciale tekens zoals `-` of `_` in Batch accountnamen.

### <a name="linked-storage-account-autostorage"></a>Gekoppelde opslag-account (autostorage)

(Optioneel) kunt u een **algemene** opslag-account koppelen aan uw account Batch wanneer u deze hebt gemaakt. De functie [toepassingspakketten](batch-application-packages.md) van Batch gebruikt blobopslag in een gekoppelde algemene opslag-account, de [Batch bestand conventies .NET](batch-task-output.md) -bibliotheek. Deze optioneel functies helpen u bij het distribueren van de toepassingen die uw batchtaken uitvoeren en persistent maken van de gegevens die ze produceren.

Als u wilt wanneer u deze hebt gemaakt, kunt u een bestaande opslag van Azure-account koppelen aan een nieuw account voor de Batch, geef de `--autostorage-account-id` optie. Deze optie moet de volledig gekwalificeerde resource-ID van het account opslag.

Eerst uw opslag-account details weergeven:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Gebruik de waarde van de **Url** voor de `--autostorage-account-id` optie. De Url-waarde begint met '/ abonnementen /' en bevat uw abonnement-ID- en resourcekalenders pad naar het opslag-account:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Een account Batch verwijderen

Syntaxis:

    azure batch account delete [options] <name>

Voorbeeld:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Hiermee verwijdert u het opgegeven Batch-account. Wanneer u wordt gevraagd, bevestig u wilt verwijderen van het account (account verwijderen kan enige tijd duren).

## <a name="manage-account-access-keys"></a>Toegangstoetsen account beheren

Moet u een toegangstoets [maken](#create-and-modify-batch-resources) en wijzigen van resources in uw account Batch.

### <a name="list-access-keys"></a>Lijst-toegangstoetsen

Syntaxis:

    azure batch account keys list [options] <name>

Voorbeeld:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Hier worden de toetsen account voor het opgegeven Batch-account.

### <a name="generate-a-new-access-key"></a>Een nieuwe access-code genereren

Syntaxis:

    azure batch account keys renew [options] --<primary|secondary> <name>

Voorbeeld:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Genereert u deze opnieuw de opgegeven account-toets voor de opgegeven Batch-account.

## <a name="create-and-modify-batch-resources"></a>Maken en wijzigen van de Batch resources

U kunt de CLI Azure maken, lezen, bijwerken, en verwijderen (CRUD) Batch resources zoals groepen, knooppunten, taken en taken te berekenen. Deze CRUD-bewerkingen is vereist voor uw accountnaam Batch, toegangstoets en eindpunt. Kunt u deze met de `-a`, `-k`, en `-u` opties of set- [omgevingsvariabelen](#credential-environment-variables) die de CLI gebruikt automatisch (indien ingevuld).

### <a name="credential-environment-variables"></a>Referentie-omgevingsvariabelen

U kunt instellen dat `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, en `AZURE_BATCH_ENDPOINT` omgevingsvariabelen in plaats van de precisie `-a`, `-k`, en `-u` opties op de opdrachtregel voor elke opdracht die u uitvoert. De Batch CLI gebruikt deze variabelen (als instellen) zodat u kunt weglaten de `-a`, `-k`, en `-u` opties. De rest van dit artikel wordt ervan uitgegaan gebruik van deze omgevingsvariabelen.

>[AZURE.TIP] Uw sleutels met een lijst met `azure batch account keys list`, en weer te geven van de account eindpunt met `azure batch account show`.

### <a name="json-files"></a>JSON-bestanden

Wanneer u Batch resources zoals pools en taken maakt, kunt u een JSON-bestand met de configuratie van de nieuwe resource in plaats van de parameters doorgeven als opdrachtregelopties opgeven. Bijvoorbeeld:

`azure batch pool create my_batch_pool.json`

Terwijl u veel bewerkingen van resources maken met alleen-opdrachtregelopties uitvoert kunt, wordt in sommige functies een bestand in JSON-indeling met de resourcedetails vereist. U kunt bijvoorbeeld een JSON-bestand moet gebruiken als u wilt opgeven bronbestanden voor een starttaak.

Als u wilt de JSON vereist voor het maken van een resource hebt gevonden, raadpleegt u de [Batch REST API verwijzing] [ rest_api] documentatie op MSDN. Voorbeeld JSON bevat elk onderwerp "Toevoegen *resourcetype"*voor het maken van de resource, die u als sjablonen voor uw JSON-bestanden gebruiken kunt. Bijvoorbeeld JSON voor het maken van de groep vindt u in [een resourcegroep die bij een account toevoegen][rest_add_pool].

>[AZURE.NOTE] Als u een bestand JSON opgeeft wanneer u een resource maakt, worden alle andere parameters die u op de opdrachtregel voor de desbetreffende resource opgeeft genegeerd.

## <a name="create-a-pool"></a>Een groep maken

Syntaxis:

    azure batch pool create [options] [json-file]

Voorbeeld (Virtuele machineconfiguratie):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Voorbeeld (Cloud Services-configuratie):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Hiermee maakt u een groep berekeningscluster knooppunten in de Batch-service.

Zoals vermeld in het [overzicht van de functie Batch](batch-api-basics.md#pool), hebt u wanneer u een besturingssysteem voor de knooppunten in uw groep selecteert twee opties: **VM configuratie** en **Configuratie van de Cloud-Services**. Gebruik de `--image-*` opties voor het maken van virtuele machineconfiguratie van toepassingen, en `--os-family` Cloud Services-configuratie van toepassingen maken. U kunt geen beide opgeven `--os-family` en `--image-*` opties.

U kunt opgeven van toepassingen [toepassingspakketten](batch-application-packages.md) en de opdrachtregel voor een [taak starten](batch-api-basics.md#start-task). U echter als u wilt opgeven bronbestanden voor de starttaak, moet in plaats daarvan gebruiken een [JSON-bestand](#json-files).

Een groep met verwijderen:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Controleren of de [lijst met afbeeldingen VM](batch-linux-nodes.md#list-of-virtual-machine-images) waarden geschikt is voor de `--image-*` opties.

## <a name="create-a-job"></a>Een taak maken

Syntaxis:

    azure batch job create [options] [json-file]

Voorbeeld:

    azure batch job create --id "job001" --pool-id "pool001"

Een taak toevoegt aan de Batch-account en de groep waarop de taken uitvoeren.

Verwijderen van een project met:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Lijst met groepen, taken, taken en andere resources

Elke Batch brontype ondersteunt een `list` opdracht waarmee query's van uw account Batch en bronnen van dat type lijsten. U kunt bijvoorbeeld de groepen weergeven in uw account en de taken in een taak:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Efficiënt vermelding van resources

Voor snellere query wordt uitgevoerd, kunt u **selecteren**, **filteren**en **uitvouwen** component opties voor `list` bewerkingen. Gebruik deze opties om de hoeveelheid gegevens die worden geretourneerd door de Batch-service te beperken. Omdat alle filters serverzijde plaatsvindt, kruist alleen de gegevens die u geïnteresseerd bent in het netwerk. Gebruik deze componenten op te slaan bandbreedte (en dus afstemmen) wanneer u een lijst met bewerkingen uitvoert.

Hiermee herstelt u bijvoorbeeld alleen de groepen waarvan id's met 'renderTask beginnen':

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

De Batch CLI ondersteuning biedt voor alle drie componenten: ondersteund door de service Batch:

* `--select-clause [select-clause]`Een subset van eigenschappen voor elke entiteit retourneren
* `--filter-clause [filter-clause]`Alleen entiteiten die overeenkomen met de opgegeven OData-expressie retourneren
* `--expand-clause [expand-clause]`De entiteitsgegevens in één onderliggende REST aanroep aanvragen. De component uitvouwen ondersteunt alleen het `stats` eigenschap op dit moment.

Zie [de Batch Azure-service efficiënt Query](batch-efficient-list-queries.md)voor meer informatie over de drie componenten: en uitvoeren lijst query's met hen.

## <a name="application-package-management"></a>Pakket Toepassingsbeheer

Toepassingspakketten om er zelf een vereenvoudigd toe om te implementeren toepassingen naar de knooppunten berekenen in uw toepassingen. Met de CLI Azure, kunt u uploaden van toepassingspakketten, pakket versies beheren en pakketten verwijderen.

Een nieuwe toepassing maken en toevoegen van een Pakketversie:

**Maken** van een toepassing:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Toevoegen** het toepassingspakket van een:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Activeren** het pakket:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Stel de **standaardversie** voor de toepassing:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Het toepassingspakket van een implementeren

Wanneer u een nieuwe groep maakt, kunt u een of meer toepassingspakketten voor implementatie opgeven. Wanneer u een pakket bij het maken van toepassingen opgeeft, wordt deze geïmplementeerd op elk knooppunt als het knooppunt joins van toepassingen. Pakketten zijn ook geïmplementeerd wanneer een knooppunt wordt opnieuw gestart of reimaged.

Geef de `--app-package-ref` option bij het maken van een groep om het toepassingspakket van een implementeren naar van de groep knooppunten, terwijl ze deelnemen aan de groep. De `--app-package-ref` optie kunt u een lijst met puntkomma als scheidingsteken van toepassings-id's om te implementeren naar de knooppunten berekeningscluster.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Als u een groep maken met behulp van opdrachtregelopties, kunt u *welke* versie van de toepassing pakket om te implementeren voor de berekeningscluster knooppunten, bijvoorbeeld "1.10-Bèta3" geen momenteel opgeven. Daarom moet u eerst een standaardversie voor de toepassing met opgeven `azure batch application set [options] --default-version <version-id>` voordat u de toepassingen maken (Zie de vorige sectie). U kunt echter de pakketversie van een voor de groep opgeven als u een [JSON-bestand](#json-files) in plaats van opdrachtregelopties gebruiken wanneer u de groep maakt.

U vindt meer informatie over de toepassingspakketten in [toepassing-implementatie waarbij Azure Batch toepassing-pakketten](batch-application-packages.md).

>[AZURE.IMPORTANT] [Koppeling een Azure Storage-account](#linked-storage-account-autostorage) moet u bij uw account Batch toepassingspakketten gebruiken.

### <a name="update-a-pools-application-packages"></a>Een resourcegroep die van toepassing-pakketten bijwerken

Als u de toepassingen die zijn toegewezen aan een bestaande toepassingen bijwerken, actie-de `azure batch pool set` command met de `--app-package-ref` optie:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Als u wilt het nieuwe toepassingspakket om te berekenen van knooppunten al in een bestaande toepassingen implementeren, moet u opnieuw opstart of reimage die knooppunten:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] U kunt een lijst van de knooppunten in een groep, samen met hun knooppunt-id's aanvragen met `azure batch node list`.

Houd er rekening mee dat u moet al de toepassing hebt geconfigureerd met een standaardversie vóór implementatie (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing

In dit gedeelte is bedoeld om u te geven met resources wilt gebruiken wanneer u problemen met Azure CLI oplossen. Deze won't per se oplossing voor alle problemen, maar kunt u de oorzaak verfijnen en wijst u waarmee resources.

* Gebruik `-h` om **help-tekst** voor een opdracht op de CLI

* Gebruik `-v` en `-vv` om weer te geven van de **uitgebreide** opdrachtuitvoer; `-vv` "extra" uitgebreid en wordt de werkelijke REST vergaderverzoeken en antwoorden. Deze schakelopties zijn handig voor het weergeven van de volledige fout uitvoer.

* U kunt **de uitvoer van de opdracht als JSON** met weergeven de `--json` optie. Bijvoorbeeld `azure batch pool show "pool001" --json` pool001 van eigenschappen worden weergegeven in de indeling van JSON. U kunt vervolgens kopiëren en wijzigen van deze uitvoer gebruiken in een `--json-file` (Zie [JSON bestanden](#json-files) eerder in dit artikel).

* De [Batch-forum op MSDN] [ batch_forum] is een uitstekende help resource en sterk wordt gecontroleerd door teamleden Batch. Zorg ervoor dat uw vragen er posten als u problemen ondervindt of hulp nodig hebt met een specifieke bewerking.

* Niet elke Batch resource-bewerking is momenteel ondersteund door de CLI Azure. Bijvoorbeeld niet kunt u momenteel een toepassing pakket *versie* voor een groep, alleen de id van het pakket opgeven In dat geval moet u mogelijk opgeven een `--json-file` voor uw opdracht in plaats van de beschikbare opties. Zorg ervoor dat u de hoogte blijven van de meest recente versie van de CLI volgt te werk om toekomstige verbeteringen.

## <a name="next-steps"></a>Volgende stappen

*  Zie de [toepassing-implementatie waarbij Azure Batch toepassing-pakketten](batch-application-packages.md) voor meer informatie over deze functie gebruiken om te beheren en implementeren van de toepassingen die u op Batch berekeningscluster knooppunten uitvoert.

* Zie [de Batch-service efficiënt Query](batch-efficient-list-queries.md) voor meer informatie over het aantal items en het type informatie die wordt geretourneerd voor query's naar Batch wordt afgetrokken.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx