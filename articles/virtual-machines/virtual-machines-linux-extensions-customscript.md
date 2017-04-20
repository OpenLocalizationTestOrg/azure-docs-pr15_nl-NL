<properties
   pageTitle="Aangepaste scripts op Linux VMs | Microsoft Azure"
   description="Linux VM configuratietaken automatiseren met behulp van de aangepast Script-extensie"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Gebruik van de extensie Azure aangepast Script met Linux virtuele Machines

De aangepast Script extensie downloads en scripts op Azure virtuele machines wordt uitgevoerd. Dit toestel is handig voor het bericht implementatieconfiguratie, software installeren of een andere configuratie / management taak. Scripts kunnen worden gedownload van Azure opslag of andere toegankelijke internetlocatie of voor de extensie runtime beschikbaar. De extensie aangepast Script werkt naadloos samen met Azure resourcemanager sjablonen en kan ook worden uitgevoerd met de CLI Azure, PowerShell, Azure-portal of de Azure virtuele machines REST API.

Dit document details van het gebruik van de extensie aangepast Script van de Azure CLI en een resourcemanager Azure-sjabloon en de stappen voor probleemoplossing op Linux-systemen ook details.

## <a name="extension-configuration"></a>Configuratie van de extensie

De configuratie aangepast Script extensie Hiermee geeft u items, zoals scriptlocatie en de opdracht om te worden uitgevoerd. Deze configuratie kan worden opgeslagen in de configuratiebestanden op de opdrachtregel of in een sjabloon Azure resourcemanager opgegeven. Gevoelige gegevens kunnen worden opgeslagen in een beveiligd configuratie, die is versleuteld en alleen ontsleuteld in de virtuele machine. De beveiligde configuratie is handig als de opdracht execution geheimen zoals een wachtwoord bevat.

### <a name="public-configuration"></a>Openbare configuratie

Schema:

- **commandToExecute**: (vereist, tekenreeksexpressie) het fragment punt script uitvoeren
- **fileUris**: (optioneel, tekenreeksmatrix) de URL's voor bestanden moeten worden gedownload.
- **tijdstempel** (optioneel, geheel getal) Gebruik dit veld alleen voor een opnieuw uitvoeren van het script activeren door de waarde van dit veld te wijzigen.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Beveiligde configuratie

Schema:

- **commandToExecute**: (optioneel, tekenreeks) het fragment punt script uitvoeren. Gebruik dit veld in plaats daarvan als de opdracht geheimen zoals wachtwoorden bevat.
- **storageAccountName**: (optioneel, tekenreeks) de naam van opslag-account. Als u opslag referenties opgeven, moeten alle fileUris URL's voor Azure BLOB's zijn.
- **storageAccountKey**: (optioneel, tekenreeks) de toegangstoets van opslag-account.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI

Wanneer u de CLI Azure met de extensie van aangepast Script uitvoeren, maak een configuratiebestand of bestanden met ten minste de bestands-uri en de scriptopdracht worden uitgevoerd.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

(Optioneel) de opdracht kan worden uitgevoerd met de `--public-config` en `--private-config` optie, kunt de configuratie te worden opgegeven tijdens de uitvoering en zonder een afzonderlijke configuratiebestand.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Voorbeelden van Azure CLI

**Voorbeeld 1** : openbare configuratie met script-bestand.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure CLI-opdracht:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Voorbeeld 2** : openbare configuratie met geen script-bestand.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-opdracht:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Voorbeeld 3** : een openbare configuratiebestand wordt gebruikt om op te geven van de script-bestand URI en een beveiligde configuratiebestand wordt gebruikt om op te geven van de opdracht om te worden uitgevoerd.

Openbare configuratiebestand:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Beveiligde configuratiebestand:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI-opdracht:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Resourcemanager-sjabloon

De extensie Azure aangepast Script kan worden uitgevoerd op VM implementatie moment met een sjabloon resourcemanager. Hiervoor toevoegen correct opgemaakte JSON aan de implementatiesjabloon.

### <a name="resource-manager-examples"></a>Resourcemanager voorbeelden

**Voorbeeld 1** : openbare configuratie.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Voorbeeld 2** : de opdracht kan worden uitgevoerd in de beveiligde configuratie.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Zie de .net Core muziek Store Demo voor een volledig voorbeeld - [Muziek Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer de aangepast Script-extensie wordt uitgevoerd, kan het script is gemaakt of gedownload in een map die vergelijkbaar is met het volgende voorbeeld. Uitvoer van de opdracht wordt ook opgeslagen in deze map in `stdout` en `stderr` bestand.

```none
/var/lib/azure/custom-script/download/0/
```

De extensie Azure-Script levert een logboek, u hier vindt.

```none
/var/log/azure/customscript/handler.log
```

De status van de extensie van aangepast Script kan ook worden opgehaald met de CLI Azure.

```none
azure vm extension get <resource-group> <vm-name>
```

De uitvoer ziet er als de volgende tekst uit:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Volgende stappen

Zie [overzicht van de Azure Script-extensie voor Linux](./virtual-machines-linux-extensions-features.md)voor informatie over andere VM scriptextensies.