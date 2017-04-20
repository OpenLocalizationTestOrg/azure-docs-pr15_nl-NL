<properties
   pageTitle="(Windows PowerShell-script) publiceren-WebApplicationWebSite | Microsoft Azure"
   description="Leer hoe u een webproject publiceren naar een Azure-website. Dit script maakt de vereiste bronnen in uw Azure-abonnement als deze nog niet bestaan."
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

# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Publiceren-WebApplicationWebSite (Windows PowerShell-script)

##<a name="syntax"></a>Syntaxis

Een webproject worden gepubliceerd naar een Azure-website. Het script maakt de vereiste bronnen in uw Azure-abonnement als deze nog niet bestaan.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Configuratie

Het pad naar de JSON-configuratiebestand waarmee de details van de implementatie beschreven.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|waar|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

## <a name="subscriptionname"></a>SubscriptionName

De naam van de Azure-abonnement dat u wilt maken van de website in.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

## <a name="webdeploypackage"></a>WebDeployPackage

Het pad naar de web-implementatiepakket publiceren naar de website. U kunt dit pakket maken met behulp van de wizard Publiceren in Visual Studio. Zie [aan de slag met Azure-Cloudservices en ASP.NET](http://go.microsoft.com/fwlink/p/?LinkID=623089)voor meer informatie.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

## <a name="databaseserverpassword"></a>DatabaseServerPassword

De gebruikersnaam en wachtwoord voor de SQL-database in Azure wordt aangegeven.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|geen|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput

Als de waarde true, berichten afdrukken vanuit het script naar de uitvoerstroom.

|Parameter|Standaardwaarde|
|---|---|
|Aliassen|geen|
|Vereist?|ONWAAR|
|Positie|met de naam|
|Standaardwaarde|ONWAAR|
|Invoer pijplijn accepteren?|ONWAAR|
|Jokertekens accepteren?|ONWAAR|

## <a name="remarks"></a>Opmerkingen

Ontwikkelaar en Test omgevingen, Zie voor een volledige uitleg over het gebruik van het script maken [Windows PowerShell-Scripts gebruiken om te publiceren op ontwikkelaar en Test-omgevingen](vs-azure-tools-publishing-using-powershell-scripts.md).

Het configuratiebestand JSON Hiermee geeft u de details van wat is worden geïmplementeerd. Het bevat de informatie die u hebt opgegeven tijdens het maken van het project, zoals de naam en de gebruikersnaam voor de website. Het bevat ook de database inrichten, indien van toepassing. De volgende code toont een voorbeeld JSON-configuratiebestand:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

U kunt het configuratiebestand JSON als u wilt wijzigen wat wordt geïmplementeerd bewerken. Een sectie webSite is vereist, maar het gedeelte van de database is optioneel.

## <a name="next-steps"></a>Volgende stappen

Zie [Publiceren-WebApplicationVM (Windows PowerShell-script)](vs-azure-tools-publish-webapplicationvm.md) voor meer informatie.
