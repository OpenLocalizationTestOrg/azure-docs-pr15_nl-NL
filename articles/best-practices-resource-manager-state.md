<properties
    pageTitle="Afhandeling staat in resourcemanager sjablonen | Microsoft Azure"
    description="Toont aanbevolen methoden voor het gebruik van complexe objecten staat gegevens delen met Azure resourcemanager sjablonen en gekoppelde sjablonen"
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>De staat in Azure resourcemanager sjablonen delen

Dit onderwerp bevat aanbevolen procedures voor het beheren en delen van staat op sjablonen. De parameters en variabelen die worden weergegeven in dit onderwerp ziet u voorbeelden van het type van objecten u kunt gemakkelijk ordenen van uw implementatievereisten voor. In deze voorbeelden, kunt u uw eigen objecten met eigenschapswaarden zinvolle voor uw omgeving implementeren.

In dit onderwerp maakt deel uit van een grotere whitepaper. Als u wilt lezen het volledige papier, downloadt u [Aandachtspunten voor de hele wereld Class Resource Manager-sjablonen en bewezen procedures](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Geef standaard configuratieinstellingen

In plaats van een sjabloon die beschikbaar totale flexibiliteit en talloze variaties zijn bieden, is een algemene patroon op te geven van een selectie van bekende configuraties. Gebruikers kunnen in feite standaard t-shirt formaten zoals sandbox, kleine, middelgrote en grote selecteren. Andere voorbeelden van t-shirt grootten zijn productaanbod, zoals community edition of enterprise edition. In andere gevallen kan het zijn werkbelasting / regiospecifieke configuraties van een technologie – zoals kaart verkleinen of geen sql.

U kunt met complexe objecten, variabelen met verzamelingen van gegevens, ook wel 'Eigenschappenverzamelingen' genoemd maken en deze gegevens gebruiken om de declaratie van de resource station in uw sjabloon. Deze methode biedt goede, bekende configuraties van verschillende grootten die vooraf zijn geconfigureerd voor klanten. Zonder bekende configuraties, moeten gebruikers van de sjabloon bepalen cluster hun eigen aanpassen, factor platform resourcebeperkingen en berekeningen voor de resulterende partitioneren van opslag accounts en andere resources (vanwege cluster grootte- en resourcekalenders beperkingen). Naast een betere ervaring voor de klant aanbrengt, zijn een paar bekende configuraties gemakkelijker te ondersteunen en kunt u een hoger niveau van de dichtheid geven.

Het volgende voorbeeld ziet u het definiëren van variabelen die complexe objecten voor het weergeven van verzamelingen gegevens bevatten. De verzamelingen definiëren waarden die worden gebruikt voor VM grootte, netwerkinstellingen, -instellingen en instellingen voor de beschikbaarheid.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Zoals u ziet dat de variabele **tshirtSize** worden samengevoegd voor de grootte van de t-shirt die u door een parameter (**klein**, **Gemiddeld**, **groot verstrekt**) naar de tekst **tshirtSize**. U gebruikt deze variabele om op te halen, de bijbehorende complexe objectvariabele dat t-shirt-formaat.

Vervolgens kunt u deze variabelen verderop in de sjabloon raadplegen. De mogelijkheid om te verwijzen naar benoemde variabelen en hun eigenschappen de sjabloonsyntaxis van de vereenvoudigd en gemakkelijker om context te begrijpen. Het volgende voorbeeld wordt een resource om te implementeren met behulp van de objecten die eerder weergegeven waarden instellen gedefinieerd. Bijvoorbeeld, de grootte VM is ingesteld door het ophalen van de waarde voor `variables('tshirtSize').vmSize` wanneer de waarde voor de grootte van de schijf wordt opgehaald uit `variables('tshirtSize').diskSize`. Bovendien de URI voor een gekoppelde sjabloon is geconfigureerd met de waarde voor `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>De staat doorgeven aan een sjabloon

U delen staat in een sjabloon tot en met parameters die u tijdens de implementatie opgeeft.

De volgende tabel bevat veelgebruikte parameters in sjablonen.

Naam | Waarde | Beschrijving
---- | ----- | -----------
locatie    | Tekenreeks uit een beperkte lijst met Azure regio 's   | De locatie waar de resources zijn geïmplementeerd.
storageAccountNamePrefix    | Tekenreeks    | Unieke DNS-naam voor de opslag-Account waar van de VM schijven worden geplaatst
Domeinnaam  | Tekenreeks    | De naam van het domein van het publiek toegankelijke jumpbox VM in de indeling: **{domeinnaam}. { locatie}.cloudapp.com** bijvoorbeeld: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Tekenreeks    | Gebruikersnaam voor de VMs
Beheerderswachtwoord   | Tekenreeks    | Wachtwoord voor de VMs
tshirtSize  | Tekenreeks uit een beperkte lijst met aangeboden t-shirt grootte   | De grootte van de eenheid benoemde schaal inrichten. Bijvoorbeeld "Klein", "Medium", "Groot"
virtualNetworkName  | Tekenreeks    | Naam van het virtuele netwerk waarmee de consument wil gebruiken.
enableJumpbox   | Tekenreeks uit een beperkte lijst (ingeschakeld/uitgeschakeld) | Parameter die aangeeft of een jumpbox voor de omgeving wordt ingeschakeld. Waarden: 'ingeschakeld","uitgeschakeld"

De **tshirtSize** -parameter wordt gebruikt in de vorige sectie luidt als volgt:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>De staat doorgeven aan gekoppelde sjablonen

Wanneer u verbinding maakt met gekoppelde sjablonen, vaak gebruik een combinatie van statische en variabelen gegenereerd.

### <a name="static-variables"></a>Statische variabelen

Statische variabelen vaak gebruikt, zodat grondtal waarden, zoals URL's, die overal in een sjabloon worden gebruikt.

In het volgende sjabloon fragment, `templateBaseUrl` geeft de hoofdmap locatie voor de sjabloon in GitHub. De volgende regel genereert een nieuwe variabele `sharedTemplateUrl` die de basis-URL met de naam van de bekende van de sjabloon gedeelde resources worden samengevoegd. Onder deze regel een complexe objectvariabele wordt gebruikt voor de opslag van de grootte van een t-shirt, waarbij de basis-URL naar de locatie van de sjabloon bekende configuratie samengevoegd en die zijn opgeslagen in de `vmTemplate` eigenschap.

Het voordeel van deze methode is dat als de sjabloonlocatie wijzigt, u alleen hoeft de statische variabele op één plaats beschikbaar, die wordt doorgegeven overal in de gekoppelde sjablonen te wijzigen.

    "variables": {
      "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
      "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "slaveCount": 1,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      }
    }

### <a name="generated-variables"></a>Gegenereerde variabelen

Naast het statische variabelen, worden meerdere variabelen bevatten dynamisch gegenereerd. In dit gedeelte vindt u enkele van de algemene typen gegenereerde variabelen.

#### <a name="tshirtsize"></a>tshirtSize

U bent bekend met deze gegenereerde variabele van de voorbeelden hierboven.

#### <a name="networksettings"></a>networkSettings

In een capaciteit, de mogelijkheid of end-to-end bereik oplossing sjabloon maakt de gekoppelde sjablonen gewoonlijk resources die aanwezig in een netwerk. Eén ingewikkelde manier is om met een complexe object op te slaan netwerkinstellingen en aan de gekoppelde sjablonen door te geven.

Hieronder ziet u een voorbeeld van netwerkinstellingen communiceren.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Resources die zijn gemaakt met gekoppelde sjablonen zijn vaak in een set beschikbaarheid opgenomen. In het volgende voorbeeld wordt de naam van de beschikbaarheid van de set wordt opgegeven en ook de foutenstructuuranalyse-domein en een update domain tellen als u wilt gebruiken.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Als u meerdere beschikbaarheid sets (bijvoorbeeld één basispagina knooppunten) en een andere voor gegevensknooppunten, kunt u een naam gebruiken als een voorvoegsel nodig, meerdere beschikbaarheid sets opgeven of het model die eerder weergegeven voor het maken van een variabele voor een specifieke t-shirt grootte volgen.

#### <a name="storagesettings"></a>storageSettings

Opslag details worden vaak met gekoppelde sjablonen gedeeld. In het onderstaande voorbeeld bevat een object *storageSettings* informatie over de opslag-account en de container.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Met gekoppelde sjablonen moet u mogelijk besturingssysteeminstellingen doorgeven aan verschillende typen van knooppunten op verschillende bekende configuratie typen. Een complexe object is een eenvoudige manier opslaan en delen van informatie over het besturingssysteem en ook eenvoudiger ter ondersteuning van meerdere besturingssystemen voor implementatie.

Het volgende voorbeeld ziet een object voor *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Een gegenereerde variabele, *machineSettings* is een complexe-object met een combinatie van core variabelen voor het maken van een VM. De variabelen bevatten beheerder-gebruikersnaam en wachtwoord, een voorvoegsel voor de VM namen en de verwijzing naar een besturingssysteem-afbeelding.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Houd er rekening mee dat *osImageReference* haalt de waarden uit de variabele *osSettings* is gedefinieerd in de belangrijkste sjabloon. Dit betekent dat u kunt het besturingssysteem gemakkelijk wijzigen voor een VM, geheel of op basis van de voorkeur van een sjabloon voor consumenten.

#### <a name="vmscripts"></a>vmScripts

Het object *vmScripts* bevat informatie over de scripts downloaden en uitvoeren van een exemplaar van VM, inclusief buiten en binnen verwijzingen. Verwijzingen bevatten buiten de infrastructuur. Binnenkant verwijzingen zijn de geïnstalleerde software is geïnstalleerd en de configuratie.

U kunt de eigenschap *scriptsToDownload* gebruiken voor een overzicht van de scripts om naar de VM te downloaden. Dit object bevat ook verwijzingen naar opdrachtregelargumenten voor verschillende soorten acties. Deze acties bevatten de standaardinstallatie voor elk afzonderlijk knooppunt, een installatie die wordt uitgevoerd nadat alle knooppunten zijn geïmplementeerd en extra scripts die specifiek zijn voor een bepaalde sjabloon wordt uitgevoerd.

In dit voorbeeld is van een sjabloon gebruikt om te implementeren MongoDB, waarvoor een instellingen voor het leveren van beschikbaarheid. De *arbiterNodeInstallCommand* is toegevoegd aan *vmScripts* voor het installeren van de instellingen.

De sectie variabelen is waar u de variabelen die de specifieke tekst voor het uitvoeren van het script met de juiste waarden definiëren vinden.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Status retourneren van een sjabloon

Niet alleen kunt u gegevens doorgeven in een sjabloon gebruiken, kunt u ook gegevens terug naar de bellen sjabloon delen. Klik in de sectie **uitvoer** van een gekoppelde sjabloon kunt u sleutel/waardeparen die kunnen worden gebruikt door de bronsjabloon opgeven.

Het volgende voorbeeld ziet u hoe u kunt het privé IP-adres dat is gegenereerd in een gekoppelde sjabloon doorgeven.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

Binnen de belangrijkste sjabloon, kunt u die gegevens met de volgende syntaxis:

    "[reference('master-node').outputs.masterip.value]"

U kunt deze expressie in de sectie uitvoer of in de sectie bronnen van de belangrijkste sjabloon gebruiken. U kunt de expressie niet gebruiken in de sectie variabelen, omdat dit is afhankelijk van de runtime-status. Gebruik het volgende om deze waarde te retourneren van de belangrijkste sjabloon:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Zie [meerdere gegevensschijven voor een virtuele Machine maken](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine)voor een voorbeeld van het gebruik van de sectie uitvoer van een gekoppelde sjabloon om terug te keren gegevensschijven voor een virtuele machine.

## <a name="define-authentication-settings-for-virtual-machine"></a>Verificatie-instellingen voor virtuele machine definiëren

U kunt hetzelfde patroon eerder weergegeven voor configuratie-instellingen gebruiken om op te geven van de verificatie-instellingen voor een virtuele machine. U kunt een parameter voor doorgeven maken in het type verificatie.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

U variabelen voor de verschillende verificatietypen toevoegen en een variabele om op te slaan welk type voor deze installatie op basis van de waarde van de parameter wordt gebruikt.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Als u definieert de virtuele machine, stelt u de **osProfile** naar de variabele die u hebt gemaakt.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de secties van de sjabloon, raadpleegt u [Authoring Azure resourcemanager sjablonen](resource-group-authoring-templates.md)
- De functies die beschikbaar in een sjabloon zijn, raadpleegt u [Azure resourcemanager sjabloon functies](resource-group-template-functions.md)

