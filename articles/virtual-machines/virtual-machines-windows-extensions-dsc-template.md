<properties
   pageTitle="Gewenste provinciale configuratie Resource Manager sjabloon | Microsoft Azure"
   description="Resourcemanager Gegevenssjabloon definiëren voor gewenst staat configuratie in Azure met voorbeelden en probleemoplossing"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows-VMSS en gewenst staat configuratie met Azure resourcemanager sjablonen
In dit artikel worden de resourcemanager-sjabloon voor de [gewenste staat configuratie extensie-handler](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Voorbeeld van de sjabloon voor een Windows-VM

Het codefragment van de volgende Hiermee gaat u naar de sectie bron van de sjabloon.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Voorbeeld van de sjabloon voor Windows VMSS

Een knooppunt VMSS heeft een sectie 'Eigenschappen' met de "VirtualMachineProfile", "extensionProfile"-kenmerk. DSC wordt onder "extensies" toegevoegd. 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="detailed-settings-information"></a>Instellingen voor gedetailleerde informatie

Het volgende schema is voor het gedeelte instellingen van de extensie Azure DSC in een resourcemanager Azure-sjabloon.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Meer informatie
| Naam van eigenschap | Type | Beschrijving |
| --- | --- | --- |
| settings.wmfVersion | tekenreeks | Geeft de versie van Windows Management Framework die moeten worden geïnstalleerd op uw VM. Deze eigenschap instelt op 'meest recente' installaties de meest recente versie van WMF. De alleen huidige mogelijke waarden voor deze eigenschap zijn **'4.0', '5.0', ' 5.0PP' en 'laatste'**. Deze mogelijke waarden zijn onderhevig aan updates. De standaardwaarde is 'meest recente'.|
| Settings.Configuration.URL | tekenreeks | Hiermee geeft u de URL-locatie van waaruit u kunt uw DSC configuratie zip-bestand downloaden. Als de URL die vereist is voor een token SA's voor access, moet u de eigenschap protectedSettings.configurationUrlSasToken ingesteld op de waarde van uw token SA's. Deze eigenschap is vereist als settings.configuration.script en/of settings.configuration.function zijn gedefinieerd. |
| Settings.Configuration.script | tekenreeks | Hiermee geeft u de bestandsnaam van het script dat de definitie van uw configuratie DSC bevat. Dit script moet zich in de hoofdmap van het zip-bestand dat is gedownload van de URL die is opgegeven door de eigenschap configuration.url. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.script zijn gedefinieerd. |
| Settings.Configuration.Function | tekenreeks | Hiermee geeft u de naam van uw configuratie DSC. De configuratie met de naam moet worden opgenomen in het script gedefinieerd door configuration.script. Deze eigenschap is vereist als settings.configuration.url en/of settings.configuration.function zijn gedefinieerd. |
| settings.configurationArguments | Siteverzameling | Hiermee definieert u eventuele parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap is niet versleuteld. |
| settings.configurationData.url | tekenreeks | Hiermee geeft u de URL van waaruit u kunt het downloaden van uw configuratie-gegevensbestand (.pds1) wilt gebruiken als invoer voor uw DSC-configuratie. Als de URL die vereist is voor een token SA's voor access, moet u de eigenschap protectedSettings.configurationDataUrlSasToken ingesteld op de waarde van uw token SA's.|
| settings.privacy.dataEnabled | tekenreeks | Hiermee schakelt telemetrielogboek siteverzameling of. De enige mogelijke waarden voor deze eigenschap zijn **'Inschakelen', 'Uit te schakelen' '', of $null**. Deze eigenschap verlaten leeg of null, kunt telemetrielogboek. De standaardwaarde is ''. [Meer informatie](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Siteverzameling | Hiermee definieert u alternatieve locaties van waaruit u kunt de WMF downloaden. [Meer informatie](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Siteverzameling | Hiermee definieert u eventuele parameters die u wilt doorgeven aan uw DSC-configuratie. Deze eigenschap is versleuteld. |
| protectedSettings.configurationUrlSasToken | tekenreeks | Hiermee geeft u het token SA's voor toegang tot de URL die is gedefinieerd door configuration.url. Deze eigenschap is versleuteld. |
| protectedSettings.configurationDataUrlSasToken | tekenreeks | Hiermee geeft u het token SA's voor toegang tot de URL die is gedefinieerd door configurationData.url. Deze eigenschap is versleuteld. |

## <a name="settings-vs-protectedsettings"></a>Instellingen versus ProtectedSettings
Alle instellingen worden opgeslagen in een tekstbestand instellingen op de VM.
Klik onder 'instellingen' zijn openbare eigenschappen omdat ze niet zijn gecodeerd in het instellingenbestand van de tekst.
Eigenschappen onder 'protectedSettings' zijn versleuteld met een certificaat en worden niet weergegeven in tekst zonder opmaak in dit bestand dan op de VM.

Als de configuratie referenties moet, kan ze kunnen worden opgenomen in protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt afgeleid van de sectie 'Aan de slag' van de [pagina DSC extensie Handler overzicht](virtual-machines-windows-extensions-dsc-overview.md).
Dit voorbeeld wordt resourcemanager sjablonen in plaats van cmdlets om te implementeren van de extensie. Sla de configuratie "IisInstall.ps1", plaatst u deze in een. ZIP-bestand en upload het bestand in een toegankelijke-URL. In dit voorbeeld wordt Azure-blobopslag, maar het is mogelijk om te downloaden. ZIP-bestanden vanaf elke willekeurige locatie.

De volgende code wordt in de sjabloon Azure resourcemanager de VM downloaden van het juiste bestand en de juiste PowerShell-functie uitvoeren:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Bijwerken vanuit de oudere bestandsindeling
De instellingen in de vorige-indeling (met de openbare eigenschappen ModulesUrl, ConfigurationFunction, SasToken of eigenschappen) automatisch aanpassen aan de huidige indeling en voer net zoals ze gedaan.

Het volgende schema is wat het vorige instellingenschema vroeger:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Hier ziet u hoe de eerdere indeling past voor de huidige indeling:

| Naam van eigenschap | Vorige Schema Equivalent |
| --- | --- |
| settings.wmfVersion | Instellingen. WMFVersion |
| Settings.Configuration.URL | Instellingen. ModulesUrl |
| Settings.Configuration.script | Eerste deel van de instellingen. ConfigurationFunction (voordat u '\\\\') |
| Settings.Configuration.Function | Tweede deel van de instellingen. ConfigurationFunction (nadat '\\\\') |
| settings.configurationArguments | Instellingen. Eigenschappen |
| settings.configurationData.url | protectedSettings.DataBlobUri (zonder SA's token) |
| settings.privacy.dataEnabled | Instellingen. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | Instellingen. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | Instellingen. SasToken |
| protectedSettings.configurationDataUrlSasToken | SA's token uit protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Probleemoplossing - foutcode 1100
Foutcode 1100 geeft aan dat er een probleem met de invoer van de gebruiker de DSC-extensie.
De tekst van deze fouten variabele en kan veranderen.
Hier volgen enkele van de fouten die u mogelijk optreden en hoe u ze kunt oplossen.

### <a name="invalid-values"></a>Ongeldige waarden
"Privacy.dataCollection is '{0}'. De enige mogelijke waarden zijn '', 'Inschakelen' en 'Uitschakelen' ""WmfVersion is '{0}'. Alleen mogelijke waarden zijn... en 'meest recente' "

Probleem: Een gegeven waarde is niet toegestaan.

Oplossing: Wijzig de ongeldige waarde in een ongeldige waarde. Zie de tabel in de sectie Details.

### <a name="invalid-url"></a>Ongeldige URL
"ConfigurationData.url is '{0}'. Dit is geen geldige URL""DataBlobUri is '{0}'. Dit is geen geldige URL""Configuration.url is '{0}'. Dit is geen geldige URL'

Probleem: A opgegeven dat URL is niet geldig.

Oplossing: Controleer uw meegeleverde URL's. Zorg ervoor dat alle URL's oplossen naar geldige locaties dat de extensie toegang hebben tot op de externe computer.

### <a name="invalid-configurationargument-type"></a>Ongeldige ConfigurationArgument Type
"Ongeldige configurationArguments Typ {0}"

Probleem: De eigenschap ConfigurationArguments kan omzetten in een Hashtable-object. 

Oplossing: Controleer uw eigendom ConfigurationArguments Hashtable. Volg de opmaak van het voorgaande voorbeeld. Let voor offertes, komma's en accolades geplaatst.

### <a name="duplicate-configurationarguments"></a>Dubbele ConfigurationArguments
"Dubbele argumenten '{0}' gevonden in openbare en beveiligde configurationArguments"

Probleem: De ConfigurationArguments in openbare instellingen en de ConfigurationArguments in instellingen voor de beveiligde bevatten eigenschappen met dezelfde naam.

Oplossing: Verwijder een van de dubbele eigenschappen.

### <a name="missing-properties"></a>Ontbrekende eigenschappen
"Configuration.function moet configuration.url of configuration.module worden opgegeven"

"Configuration.url vereist dat configuration.script is opgegeven"

"Configuration.script vereist dat configuration.url is opgegeven"

"Configuration.url vereist dat configuration.function is opgegeven"

"ConfigurationUrlSasToken vereist dat configuration.url is opgegeven"

"ConfigurationDataUrlSasToken vereist dat configurationData.url is opgegeven"

Probleem: Een gedefinieerde eigenschap moet nog een eigenschap die ontbreekt.

Oplossingen: 
- Geef de ontbrekende eigenschap.
- De eigenschap die nog moet worden de ontbrekende eigenschap verwijderen.


## <a name="next-steps"></a>Volgende stappen
Meer informatie over DSC en VM schaal Hiermee stelt u in de [Virtuele Machine schaal wordt ingesteld met de extensie van Azure DSC](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Meer informatie zoeken op [van DSC secure Referentiebeheer](virtual-machines-windows-extensions-dsc-credentials.md). 

Zie [Inleiding tot de configuratie van Azure staat gewenst extensie handler](virtual-machines-windows-extensions-dsc-overview.md)voor meer informatie over de Azure DSC extensie handler. 

Voor meer informatie over PowerShell DSC, [gaat u naar het midden van de documentatie PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 
