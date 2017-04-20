
<properties
    pageTitle="Resources met de CLI Azure beheren | Microsoft Azure"
    description="Gebruik van de Azure-Interface met opdrachtregel (CLI) voor het beheren van Azure bronnen en groepen"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="danlep"/>

# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>De CLI Azure gebruiken voor het beheren van Azure resources en resourcegroepen


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST API](resource-manager-rest-api.md)


De opdrachtregel Azure (Azure CLI) is een van de verschillende hulpmiddelen die u gebruiken kunt om te implementeren en te beheren met resourcemanager resources. In dit artikel wordt kort beschreven algemene manieren voor het beheren van Azure resources en resourcegroepen met behulp van de Azure CLI in resourcemanager modus. Zie voor informatie over het gebruik van de CLI om te implementeren resources [Deploy resources met resourcemanager sjablonen en Azure CLI](resource-group-template-deploy-cli.md). Voor meer achtergrondinformatie over Azure resources en resourcemanager, gaat u naar het [Overzicht van Azure resourcemanager](azure-resource-manager/resource-group-overview.md).

>[AZURE.NOTE] Als u wilt beheren Azure resources met de CLI Azure, moet u [de CLI Azure installeren](xplat-cli-install.md), en [Meld u aan bij Azure](xplat-cli-connect.md) met behulp van de `azure login` opdracht. Controleer of de CLI is in de modus resourcemanager (uitvoeren `azure config mode arm`). Als u deze dingen hebt gedaan, bent u gereed om te gaan.



## <a name="get-resource-groups-and-resources"></a>Get-resourcegroepen en bronnen

### <a name="resource-groups"></a>Resourcegroepen

Als u een lijst met alle resourcegroepen in uw abonnement en de locaties, moet u deze opdracht uitvoeren.

    azure group list
    

### <a name="resources"></a>Resources
 U kunt alle resources in een groep, zoals een met naam *testRG*, de volgende opdracht uit te gebruiken.

    azure resource list testRG

Om weer te geven een individuele resource binnen de groep, zoals een VM benoemde *MyUbuntuVM*, gebruikt u een opdracht als volgt uit.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
U ziet u de parameter **Microsoft.Compute/virtualMachines** . Deze parameter geeft het type van de gewenste informatie op resource.
    
>[AZURE.NOTE]Wanneer u met de opdrachten **azure resource** dan de opdracht **lijst** , moet u de API-versie van de resource met de parameter **-o** . Als u niet zeker weet over de API-versie, raadpleegt u het sjabloonbestand en het veld apiVersion zoeken voor de resource. Zie voor meer informatie over API versies in resourcemanager [resourcemanager providers, regio's, API-versies en schema's](resource-manager-supported-services.md).

Bij het weergeven van gegevens op een resource, is het meestal nuttige informatie voor het gebruik van de `--json` parameter. Deze parameter zorgt ervoor dat de uitvoer beter leesbaar is, omdat sommige waarden geneste structuren of siteverzamelingen zijn. Het volgende voorbeeld bevat de resultaten van de opdracht **weergeven** als een document JSON retourneren.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Desgewenst kunt u de JSON gegevens opslaan in bestand met behulp van de &gt; teken de uitvoer naar een bestand. Bijvoorbeeld:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### <a name="tags"></a>Labels

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Resources beheren


Als u wilt een resource zoals een opslag-account toevoegen aan een resourcegroep, voert u een opdracht die vergelijkbaar is met:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"
    
Naast de API-versie van de resource opgeven met de parameter **-o** , met de parameter **-p** een tekenreeks JSON-indeling met alle vereiste of aanvullende eigenschappen worden doorgegeven.
    
    
Als u wilt verwijderen van een bestaande bron zoals de bron van een virtuele machine, gebruikt u een opdracht als volgt uit.

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Bestaande om bronnen te verplaatsen naar een ander resourcegroep of -abonnement, gebruikt u de opdracht **azure resource verplaatsen** . Het volgende voorbeeld ziet u hoe u een bestand Vgx. Cache verplaatsen naar een nieuwe resourcegroep. Met de parameter **-i** , vindt u dat een lijst door komma's gescheiden van de resource-id bevindt zich verplaatsen.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Toegangsbeheer voor resources

U kunt de CLI Azure maken en beheren van beleidsregels voor toegang tot Azure resources. Voor meer achtergrondinformatie over het beleidsdefinities en beleid toewijzen aan resources, raadpleegt u [beleid voor het beheren van resources en toegang gebruiken](resource-manager-policy.md).

Bijvoorbeeld het volgende beleid als u wilt weigeren van alle aanvragen waar locatie niet West Amerikaans of Noord centraal ons is definiëren en sla deze op de policy.json beleid definitie bestand:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Voer de opdracht **beleid definitie maken** :

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Deze opdracht ziet u de volgende strekking uitvoer.

    + Beleid definitie MyPolicy gegevens maken: PolicyName: MyPolicy gegevens: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    gegevens: PolicyType: aangepaste gegevens: weergavenaam: niet gedefinieerd van gegevens: Beschrijving: niet gedefinieerd van gegevens: PolicyRule: veld locatie =, in [westus, northcentralus], effect = weigeren

 Als u wilt een beleid voor het bereik dat u wilt toewijzen, gebruikt u de **PolicyDefinitionId** geretourneerd uit de vorige opdracht. In dit bereik is het abonnement in het volgende voorbeeld, maar u kunt beperken resourcegroepen of afzonderlijke resources:

    Azure beleidstoewijzing maken MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

U kunt ophalen, wijzigen of verwijderen van beleidsdefinities met behulp van de opdrachten **beleid definitie weergeven**, **beleid definitie**en **beleid definitie verwijderen** .

U kunt op dezelfde manier ophalen, wijzigen of verwijderen van toewijzingen van beleid met behulp van de opdrachten **beleidstoewijzing weergeven**, **beleidstoewijzing instellen**en **beleidstoewijzing verwijderen** .


## <a name="export-a-resource-group-as-a-template"></a>Een resourcegroep exporteren als een sjabloon

Voor een bestaande resourcegroep, kunt u de sjabloon resourcemanager voor de resourcegroep weergeven. De sjabloon exporteren biedt twee voordelen:

1. Omdat alle de infrastructuur is gedefinieerd in de sjabloon, kunt u eenvoudig toekomstige implementaties van de oplossing automatiseren.

2. U kunt vertrouwd te raken met de sjabloonsyntaxis van de door te kijken de JSON die uw oplossing vertegenwoordigt.

De CLI Azure gebruikt, kunt u exporteren van een sjabloon die de huidige status van uw resourcegroep vertegenwoordigt, of de sjabloon die is gebruikt voor een bepaalde implementatie downloaden.

* **De sjabloon voor een resourcegroep exporteren** : dit is handig als u in een resourcegroep wijzigingen, en moet de JSON-weergave van de huidige status ophalen. De gegenereerde sjabloon bevat echter alleen een minimum aantal parameters en er geen variabelen. De meeste van de waarden in de sjabloon zijn hard gecodeerde. Voordat de gegenereerde sjabloon wordt geïmplementeerd, wilt u mogelijk meer van de waarden converteren naar parameters, zodat u de implementatie van verschillende omgevingen kunt aanpassen.

    Als u wilt exporteren de sjabloon voor een resourcegroep naar een lokale map, voer de `azure group export` opdracht, zoals wordt weergegeven in het volgende voorbeeld. (Vervangt door een lokale map voor uw besturingssysteem-omgeving.)

        azure group export testRG ~/azure/templates/

* **Download de sjabloon voor een bepaalde implementatie** --dit is handig wanneer u de werkelijke sjabloon die is gebruikt moet voor het implementeren van resources weergeven. De sjabloon bevat alle parameters en variabelen die is gedefinieerd voor de oorspronkelijke implementatie. Als iemand in uw organisatie aan de resourcegroep buiten de definitie in de sjabloon wijzigingen hebt aangebracht, niet met deze sjabloon echter aangeduid in de huidige status van het resourceveld groep.

    Als u wilt downloaden van de gebruikte sjabloon voor een bepaalde implementatie naar een lokale map, uitvoeren de `azure group deployment template download` opdracht. Bijvoorbeeld:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Sjabloon exporteren in de Preview-versie wordt weergegeven en niet alle typen momenteel ondersteund exporteren van een sjabloon. Tijdens een poging om het exporteren van een sjabloon, ziet u mogelijk een fout die aangeeft dat enkele bronnen zijn niet geëxporteerd. Indien nodig, definiëren handmatig voor deze resources in uw sjabloon na downloaden.



## <a name="next-steps"></a>Volgende stappen

* Zie om details van implementatie bewerkingen, en implementatie oplossen met de CLI Azure, [implementatie-bewerkingen met Azure CLI weergeven](resource-manager-troubleshoot-deployments-cli.md).
* Als u de CLI gebruiken wilt voor het instellen van een toepassing of script voor toegang tot resources, raadpleegt u [Azure CLI gebruiken om een service hoofdsom voor toegang tot resources te maken](resource-group-authenticate-service-principal-cli.md).


