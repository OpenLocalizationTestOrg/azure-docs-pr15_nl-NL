<properties
   pageTitle="Publiceren WebApplicationVM | Microsoft Azure"
   description="Leer hoe u een webtoepassing met een virtuele machine implementeren. Dit script maakt de vereiste bronnen in uw Azure-abonnement als deze nog niet bestaan."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-webapplicationvm-windows-powershell-script"></a>Publiceren-WebApplicationVM (Windows PowerShell-script)

Een webtoepassing met een virtuele machine implementeert. Het script maakt de vereiste bronnen in uw Azure-abonnement als deze nog niet bestaan.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Configuratie

Het pad naar de JSON-configuratiebestand waarmee de details van de implementatie beschreven.

|Aliassen|geen|
|---|---|
|Vereist?|waar|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

### <a name="subscriptionname"></a>SubscriptionName

De naam van de Azure abonnement waarin u wilt maken van de virtuele machine.

|Aliassen|geen|
|---|---|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|Het eerste abonnement gebruikt in het abonnementsbestand|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

### <a name="webdeploypackage"></a>WebDeployPackage

Het pad naar de web-implementatiepakket publiceren naar de virtuele machine. U kunt dit pakket maken met behulp van de wizard Publiceren in Visual Studio. Zie [hoe: Web implementatiepakket maken in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx).

|Aliassen|geen|
|---|---|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

### <a name="allowuntrusted"></a>AllowUntrusted

Als dit waar is, kunt u het gebruik van certificaten die niet zijn ondertekend door een vertrouwde basiscertificeringsinstantie.

|Aliassen|geen|
|---|---|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|ONWAAR|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

### <a name="vmpassword"></a>VMPassword

De referenties voor de VM-account. Voorbeeld: - VMPassword @{Name = "admin"; Wachtwoord = "wachtwoord"}

|Aliassen|geen|
|---|---|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

### <a name="databaseserverpassword"></a>DatabaseServerPassword

De referenties voor de SQL-database in Azure wordt aangegeven. Voorbeeld: - DatabaseServerPassword @{Name = "admin"; Wachtwoord = "wachtwoord"}

|Aliassen|geen|
|---|---|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Als de waarde true, berichten afdrukken vanuit het script naar de uitvoerstroom.

|Aliassen|geen|
|---|---|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|ONWAAR|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

## <a name="remarks"></a>Opmerkingen

Ontwikkelaar en Test omgevingen, Zie voor een volledige uitleg over het gebruik van het script maken [Windows PowerShell-Scripts gebruiken om te publiceren op ontwikkelaar en Test-omgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

Het configuratiebestand JSON Hiermee geeft u de details van wat is worden geïmplementeerd. Het bevat de informatie die u hebt opgegeven toen u het project, zoals de naam, de affiniteit groep, de VHD afbeelding en de grootte van de virtuele machine hebt gemaakt. Ook bevat de eindpunten op de virtuele machine, de databases inrichten, indien van toepassing en implementatie parameters met webonderdelen. De volgende code toont een voorbeeld JSON-configuratiebestand:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

U kunt het configuratiebestand JSON als u wilt wijzigen wat is ingericht bewerken. Een virtuele machine en een cloudservice zijn vereist, maar het gedeelte van de database is optioneel.
