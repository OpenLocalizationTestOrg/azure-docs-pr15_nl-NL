<properties
    pageTitle="Hadoop, HBase of Storm clusters maken op Linux in met krul en de REST API van Azure HDInsight | Microsoft Azure"
    description="Leer hoe u met krul, Azure resourcemanager sjablonen en de REST API van Azure HDInsight Linux gebaseerde-clusters maken. U kunt het clustertype (Hadoop, HBase of Storm) opgeven of scripts gebruiken om aangepaste onderdelen te installeren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Linux gebaseerde clusters maken in met krul en de REST API van Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

De Azure REST API kunt u management bewerkingen uitvoeren op de services van de Azure platform, inclusief het maken van nieuwe resources zoals HDInsight Linux gebaseerde clusters. In dit document leert u hoe u Azure resourcemanager sjablonen een HDInsight cluster en de bijbehorende opslag configureren en het gebruik van krul om de sjabloon implementeren naar de Azure REST API maken een nieuw HDInsight cluster maken.

> [AZURE.IMPORTANT] De stappen in dit document gebruiken het standaardaantal werknemer knooppunten (4) voor een cluster HDInsight. Als u van plan bent om meer dan 32 werknemer knooppunten, bij het maken van het cluster of door het cluster schaalbaarheid na het maken, selecteert u de grootte van een hoofd knooppunt met ten minste 8 cores en 14GB ram.
>
> Zie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/)voor meer informatie over het knooppunt grootte en de bijbehorende kosten.

##<a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure CLI__. De CLI Azure gebruikt voor het maken van een service principal, die vervolgens wordt gebruikt om te genereren verificatietokens voor aanvragen voor de REST API van Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __omslaan__. Dit hulpprogramma beschikbaar is via het pakket management-mailsysteem of kan worden gedownload van [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Als u de opdrachten uitvoeren in dit document PowerShell gebruikt, moet u eerst verwijderen de `curl` alias die dat standaard wordt gemaakt. Deze alias Roep-WebRequest, een PowerShell-cmdlet gebruikt in plaats van krul wanneer u gebruikt het `curl` opdracht vanuit een PowerShell-prompt en fouten voor veel van de opdrachten in dit document zullen retourneren.
    > 
    > Als u wilt deze alias verwijderen, gebruikt u de volgende handelingen uit de PowerShell-prompt:
    >
    > `Remove-item alias:curl`
    >
    > Zodra de alias is verwijderd, moet u mogelijk zijn de versie van krul die u hebt geïnstalleerd op uw computer wilt gebruiken.

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Een sjabloon maken

Azure resourcebeheer sjablonen zijn JSON-documenten die beschrijven van een __resourcegroep__ en alle resources in deze (zoals HDInsight.) Deze benadering op basis van sjabloon kunt u alle bronnen die u nodig voor HDInsight in één sjabloon hebt definiëren en voor het beheren van wijzigingen in de groep als geheel tot en met __implementaties__ die wijzigingen toepassen op de groep.

Sjablonen zijn meestal die beschikbaar zijn in twee gedeelten; de sjabloon zelf, en een parameterbestand dat u vullen met specifieke waarden in de configuratie. Voor voorbeeld, de naam van cluster, beheerdersnaam en wachtwoord. Wanneer u rechtstreeks met de REST API, moet u deze in één bestand combineren. De indeling van dit document JSON is:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

De volgende is bijvoorbeeld een fusie van de sjabloon en parameters-bestanden vanuit [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), die een Linux gebaseerde cluster met een wachtwoord om het gebruikersaccount SSH secure maakt.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

In dit voorbeeld wordt gebruikt in de stappen in dit document. U moet de tijdelijke aanduiding voor _waarden_ in de sectie __Parameters__ aan het einde van het document vervangen door de waarden die u wilt gebruiken voor uw cluster.

##<a name="login-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

Volg de stappen beschreven in [verbinding maken met een Azure abonnement vanaf de opdrachtregel van Azure (Azure CLI)](../xplat-cli-connect.md) en verbinden met uw abonnement via de `azure login` opdracht.

##<a name="create-a-service-principal"></a>Een service principal maken

> [AZURE.NOTE] Deze stappen zijn een verkorte versie van de informatie in de sectie _verifiëren service met een wachtwoord - Azure CLI hoofdsom_ van het document [een service principal met Azure resourcemanager verifiëren](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) . Deze stappen Maak een nieuwe service principal die kunnen worden gebruikt voor verificatie van de REST API aanvragen gebruikt om te maken van Azure resources zoals een cluster HDInsight.

1. Vanuit de opdrachtprompt een sessie of shell, gebruikt u de volgende opdracht uit voor een overzicht van uw Azure-abonnementen.

        azure account list
        
    Selecteer het abonnement dat u wilt gebruiken en noteer de __Id__ -kolom in de lijst. Dit is de __abonnements-ID__ en wordt gebruikt in de meeste van de stappen in dit document.

2. Maak een nieuwe toepassing in Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Vervang de waarden voor de `--name`, `--home-page`, en `--identifier-uris` met uw eigen waarden. Een wachtwoord opgeven voor het nieuwe Active Directory-fragment.
    
    > [AZURE.NOTE] Aangezien u deze toepassing voor verificatie via een service-principal, maakt het `--home-page` en `--identifier-uris` waarden niet nodig hebt om te verwijzen naar een webpagina die worden gehost op internet. ze moeten alleen unieke URI's.
    
    Sla de waarde van __toepassings-id__ van de gegevens als resultaat gegeven.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Maak een service die eerder principal basis van de __toepassings-id__ -waarde geretourneerd.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Sla de __Object-Id__ -waarde van de gegevens als resultaat gegeven.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. De __eigenaar__ rol toewijzen aan de service die eerder principal basis van de __Object-ID__ -waarde geretourneerd. De __abonnements-ID__ die u eerder hebt aangeschaft, moet u ook gebruiken.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Zodra deze opdracht is voltooid, de service principal nu eigenaar toegang heeft tot de opgegeven abonnement-ID.

##<a name="get-an-authentication-token"></a>Een verificatietoken ophalen

1. Gebruik de volgende handelingen uit om de __Tenant-ID__ voor uw abonnement.

        azure account show -s <subscription ID>
        
    Zoek de __Tenant-ID__van de gegevens als resultaat gegeven.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Genereer een nieuwe token de Azure REST API gebruiken.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    __TenantID__, __toepassings-id__en __wachtwoord__ vervangen door de waarden verkregen of eerder hebt gebruikt.

    Als deze aanvraag geslaagd is, ontvangt u een reactie 200 reeks en de hoofdtekst van het antwoord bevat een JSON-document.

    De JSON-document die het resultaat van deze aanvraag bevat een element met de naam __access_token__; de waarde van dit element is het toegangstoken die u om de aanvragen die in de volgende secties van dit document worden gebruikt voor verificatie gebruiken moet.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Een resourcegroep maken

Gebruik de volgende handelingen uit om een nieuwe resourcegroep te maken. Voordat u de bronnen zoals het cluster HDInsight kunt maken, moet u de groep eerst maken. 

* __SubscriptionID__ vervangen door de ontvangen tijdens het maken van de service-hoofdsom abonnements-ID.
* __AccessToken__ vervangen door het toegangstoken in de vorige stap hebt ontvangen.
* __DataCenterLocation__ vervangen door het datacenter dat u maken van de resourcegroep en resources wilt, in. Bijvoorbeeld 'Zuid centraal ons op'. 
* __ResourceGroupName__ vervangen door de naam die u wilt gebruiken voor deze groep:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Als deze aanvraag geslaagd is, ontvangt u een reactie 200 reeks en de hoofdtekst van het antwoord bevat een JSON-document met informatie over de groep. De `"provisioningState"` element bevat een waarde van `"Succeeded"`.

##<a name="create-a-deployment"></a>Een implementatie maken

Gebruik het volgende om te implementeren van de configuratie van het cluster (door de sjabloon en parameter waarden), aan de resourcegroep.

* __SubscriptionID__ en __AccessToken__ vervangen door de waarden die u eerder hebt gebruikt. 
* __ResourceGroupName__ vervangen door de naam van de resource-groep die u in de vorige sectie hebt gemaakt.
* __DeploymentName__ vervangen door de naam die u wilt gebruiken voor deze installatie.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Als u de JSON-document met de sjabloon en de parameters naar een bestand hebt opgeslagen, kunt u de volgende handelingen uit in plaats van `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Als deze aanvraag geslaagd is, ontvangt u een reactie 200 reeks en de hoofdtekst van het antwoord bevat een JSON-document met informatie over de implementatie-bewerking.

> [AZURE.IMPORTANT] Houd er rekening mee dat de implementatie is verzonden, maar niet op dit moment is voltooid. Het kan enkele minuten duren, meestal ongeveer 15, voor de implementatie om te voltooien.

##<a name="check-the-status-of-a-deployment"></a>Controleer de status van een implementatie

Als u wilt controleren van de status van de implementatie, gebruikt u de volgende:

* __SubscriptionID__ en __AccessToken__ vervangen door de waarden die u eerder hebt gebruikt. 
* __ResourceGroupName__ vervangen door de naam van de resource-groep die u in de vorige sectie hebt gemaakt.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Hiermee retourneert informatie een JSON-document met informatie over de implementatie-bewerking. De `"provisioningState"` element bevat de status van de implementatie; Als dit document een waarde van bevat `"Succeeded"`, en vervolgens de implementatie is voltooid. Uw cluster moet nu beschikbaar maken voor gebruik.

##<a name="next-steps"></a>Volgende stappen

Nu die u hebt gemaakt met een cluster HDInsight, gebruikt u de volgende manieren te werk voor meer informatie over het werken met uw cluster. 

###<a name="hadoop-clusters"></a>Hadoop clusters

* [Component gebruiken met HDInsight](hdinsight-use-hive.md)
* [Varken met HDInsight gebruiken](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase clusters

* [Aan de slag met HBase op HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-toepassingen voor HBase op HDInsight ontwikkelen](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm clusters

* [Ontwikkel Java topologieën voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md)
* [Gebruik Python onderdelen in Storm op HDInsight](hdinsight-storm-develop-python-topology.md)
* [Implementeren en topologieën met Storm op HDInsight controleren](hdinsight-storm-deploy-monitor-topology-linux.md)
