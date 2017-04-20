<properties
   pageTitle="Verbinding maken met een beveiligde privé cluster | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe beveiligde communicatie binnen de zelfstandige of privé cluster ook tussen clients en de cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Een zelfstandige cluster op Windows met behulp van x.509-certificaten Secure

In dit artikel wordt beschreven hoe de communicatie tussen de verschillende knooppunten van uw cluster van zelfstandige versie van Windows, secure ook hoe clients verbinding maken met dit cluster met X.509 verificatie certificaten. Dit zorgt ervoor dat alleen bevoegde gebruikers kunnen toegang het cluster, de gebruikte toepassingen tot en beheertaken.  Certificaatbeveiliging moet zijn ingeschakeld voor het cluster wanneer het cluster wordt gemaakt.  

Zie voor meer informatie over de beveiliging cluster zoals naar knooppunten beveiliging, client tot knooppunt beveiliging en Rolgebaseerd toegangsbeheer, [Cluster beveiliging scenario's](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Welke certificaten hebt u nodig?

Om te beginnen met [de zelfstandige cluster downloaden](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) naar een van de knooppunten in uw cluster. In het gedownloade pakket vindt u een bestand **ClusterConfig.X509.MultiMachine.json** . Open het bestand en de sectie voor **beveiliging** onder de sectie **Eigenschappen** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Dit onderwerp vindt de certificaten die u nodig hebt voor het beveiligen van uw cluster van zelfstandige versie van Windows. Stel de waarden van **ClusterCredentialType** en **ServerCredentialType** naar *X509*om in te schakelen certificaten gebaseerde beveiliging.

>[AZURE.NOTE] Een [vingerafdruk](https://en.wikipedia.org/wiki/Public_key_fingerprint) is de primaire identiteit van een certificaat. Lees [hoe u kunt ophalen vingerafdruk van een certificaat](https://msdn.microsoft.com/library/ms734695.aspx) om vast te stellen de vingerafdruk van de certificaten die u maakt.

De volgende tabel bevat de certificaten die u nodig op uw cluster instellen hebt:

|**CertificateInformation-instelling**|**Beschrijving**|
|-----------------------|--------------------------|
|ClusterCertificate|Dit certificaat is vereist voor het beveiligen van de communicatie tussen de knooppunten op een cluster. U kunt twee verschillende certificaten, een primaire en een secundaire gebruiken voor de upgrade. Stel de vingerafdruk van het primaire certificaat in de sectie **vingerafdruk** en die van de secundaire in de variabelen **ThumbprintSecondary** .|
|ServerCertificate|Dit certificaat wordt geverifieerd op de client wanneer wordt geprobeerd te verbinden met dit cluster. Uitkomt kunt u het certificaat voor *ClusterCertificate* en *ServerCertificate*te gebruiken. U kunt twee verschillende certificaten, een primaire en een secundaire gebruiken voor de upgrade. Stel de vingerafdruk van het primaire certificaat in de sectie **vingerafdruk** en die van de secundaire in de variabelen **ThumbprintSecondary** . |
|ClientCertificateThumbprints|Dit is een verzameling certificaten die u wilt installeren op de geverifieerde clients. U kunt een aantal verschillende certificaten geïnstalleerd op de machines die u wilt toestaan van toegang tot het cluster hebben. Stel de vingerafdruk van elk certificaat in de variabele **CertificateThumbprint** . Als u de **IsAdmin** ingesteld op *waar*, kunt klikt u vervolgens de client met dit certificaat is geïnstalleerd doen beheerder management activiteiten op het cluster. Als de **IsAdmin** *Onwaar*is, kan de acties die is toegestaan voor de gebruikerstoegangsrechten, meestal alleen-lezen alleen uitvoeren in de client met dit certificaat. Lees voor meer informatie over rollen [Rolgebaseerd toegangsbeheer RBAC)](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|De algemene naam van het eerste clientcertificaat instellen voor de **CertificateCommonName**. De **CertificateIssuerThumbprint** is de vingerafdruk voor de uitgever van dit certificaat. Lees [werken met certificaten](https://msdn.microsoft.com/library/ms731899.aspx) meer informatie over algemene namen en de uitgever.|
|HttpApplicationGatewayCertificate|Dit is een optionele certificaat dat is opgegeven als u wilt beveiligen van uw Http-toepassingsgateway. Controleer of reverseProxyEndpointPort is ingesteld in nodeTypes als u dit certificaat gebruikt.|

Hier ziet u voorbeeld clusterconfiguratie waar de Cluster, Server en Client certificaten zijn verleend.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Krijgen de X.509 certificaten
Als u wilt beveiligen communicatie binnen het cluster, moet u eerst om x.509-certificaten voor uw knooppunten te verkrijgen. Ook als u wilt beperken verbinding met deze cluster aan geautoriseerde computers/gebruikers, moet u downloaden en installeren van certificaten voor de clientcomputers.

U moet een [Certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority) gebruiken voor kolomgroepen productie werkbelasting met X.509-certificaat voor het beveiligen van het cluster die zijn aangemeld. Voor meer informatie over het verkrijgen van deze certificaten, gaat u naar [hoe: een certificaat aanvragen](http://msdn.microsoft.com/library/aa702761.aspx).

U kunt voor kolomgroepen dat u voor testdoeleinden gebruikt kiezen voor het gebruik van een zelfondertekend certificaat.

## <a name="optional-create-a-self-signed-certificate"></a>Optioneel: Een zelfondertekend certificaat maken
Een manier om u te maken van een zelfondertekend certificaat dat goed kan worden beveiligd is via het script *CertSetup.ps1* in de map Service stof SDK in de map *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. In dit bestand bewerken en Hiermee kunt u een certificaat maken met een geschikte naam.

Nu het certificaat exporteren naar een PFX-bestand met een wachtwoord beveiligde. Eerst moet u de vingerafdruk van het certificaat. Voer de toepassing certmgr.exe. Ga naar de map **Local Computer\Personal** en zoek het certificaat dat u zojuist hebt gemaakt. Dubbelklik op het certificaat te openen, schakelt u het tabblad *Details* en schuif omlaag naar het veld *vingerafdruk* . Kopieer de vingerafdrukwaarde in de PowerShell-opdracht onder, waarbij de spaties worden verwijderd.  Wijzig de waarde *$pswd* in een beveiligd-wachtwoordverificatie geschiktheid voor het beveiligen en uitvoeren van de PowerShell:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

U kunt de volgende PowerShell-opdracht uitvoeren als u wilt zien van de details van een certificaat op de computer zijn geïnstalleerd:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

U kunt ook als u een Azure-abonnement hebt, volgt u de sectie [certificaten toevoegen aan de sleutel kluis](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>De certificaten installeren
Nadat u de certificaten hebt, kunt u deze knooppunten installeren. Uw knooppunten moeten beschikken over de meest recente Windows PowerShell 3.x op deze is geïnstalleerd. U moet Herhaal deze stappen op elk knooppunt voor zowel de Cluster als de Server en een secundaire certificaten.

1. De .pfx-bestanden worden gekopieerd naar het knooppunt.
2. Open een PowerShell-venster als een beheerder en voer de volgende opdrachten. Vervang de *$pswd* door het wachtwoord dat u gebruikt om te dit certificaat maken. Vervang de *$PfxFilePath* door het volledige pad van de .pfx gekopieerd naar dit knooppunt.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Vervolgens moet u het besturingselement voor access op dit certificaat instellen zodat de Service stof-proces, die compatibel is met de Netwerkservice-account, kan worden gebruikt door het volgende script uit te voeren. Geef de vingerafdruk van het certificaat en "Netwerkservice" voor de serviceaccount. U kunt controleren of de ACL's op het certificaat juist het hulpprogramma voor het certmgr.exe en bekijken via de persoonlijke sleutels voor beheren op het certificaat zijn.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Verbinding maken met ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 - X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - winkelnaam mijn
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Verbinding maken met ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```
