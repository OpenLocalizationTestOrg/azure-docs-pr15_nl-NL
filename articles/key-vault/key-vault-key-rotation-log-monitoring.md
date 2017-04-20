<properties
    pageTitle="Toets kluis instellen met complete belangrijke draaiing en controle | Microsoft Azure"
    description="Gebruik deze procedures om het afspraken beter kunt instellen met belangrijke draaiing en het monitoren van belangrijke kluis Logboeken"
    services="key-vault"
    documentationCenter=""
    authors="swgriffith"
    manager="mbaldwin"
    tags=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="jodehavi;stgriffi"/>
#<a name="how-to-setup-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Toets kluis met complete belangrijke draaiing en controle instellen

##<a name="introduction"></a>Inleiding

Na het maken van uw Azure-toets kluis, is mogelijk om te beginnen gebruikmaken van deze kluis om op te slaan uw toetsen en geheimen. Uw toepassingen niet meer nodig hebt om uw toetsen of geheimen, maar eerder vraagt om ze uit de belangrijkste kluis naar wens. Hiermee kunt u toetsen en geheimen bijwerken zonder die invloed hebben op het gedrag van de toepassing, waarmee u een groot aantal mogelijkheden rond uw sleutel worden geopend en de werking van geheime management.

In dit artikel worden doorlopen een voorbeeld van gebruikmaken van Azure-toets kluis om op te slaan een geheim, in dit geval een Azure opslag-accountsleutel die door een toepassing wordt geopend. Deze demonstreert implementatie van een geplande draaiing van die accountsleutel opslag ook. Ten slotte doorlopen deze een demonstratie van het controleren van de belangrijkste kluis controlelogboeken bijhouden en waarschuwingen verhogen indien de onverwachte aanvragen.

> \[AZURE. Opmerking\] deze zelfstudie is niet bedoeld om uit te leggen in detail de eerste set up van uw Azure-toets kluis. Zie [aan de slag met Azure toets kluis](key-vault-get-started.md)voor deze informatie. Of voor de opdrachtregel platforms instructies, raadpleegt u [deze gelijkwaardige zelfstudie](key-vault-manage-with-cli.md).

##<a name="setting-up-keyvault"></a>Bij het instellen van KeyVault

Om een toepassing een geheim ophalen van Azure-toets kluis, moet u eerst het geheim maken en uploaden naar uw kluis. Dit kunt doen eenvoudig via PowerShell zoals hieronder wordt weergegeven.

Een Azure PowerShell-sessie starten en meld u aan bij uw Azure-account met de volgende opdracht uit:

```powershell
Login-AzureRmAccount
```

Voer uw gebruikersnaam in te voeren Azure-account en wachtwoord in het pop-browservenster. Azure PowerShell krijgt alle abonnementen die zijn gekoppeld aan dit account en al dan niet standaard, gebruikt de eerste fase.

Als u meerdere abonnementen hebt, is het wellicht geeft u aan een specifieke een die is gebruikt voor het maken van uw Azure-toets kluis. Typ het volgende als u wilt zien van de abonnementen voor uw account:

```powershell
Get-AzureRmSubscription
```

Als u wilt opgeven van het abonnement dat is gekoppeld aan uw belangrijkste kluis die u meldt zich, typ:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID> 
```

Zoals in dit artikel wordt beschreven opslaan van een sleutel opslag-account als een geheim, moet u die accountsleutel opslag ophalen.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Na uw geheim ophalen, in dit geval uw accountsleutel opslag, moet u naar die omzetten in een tekenreeks met secure en maak een geheim met deze waarde in uw belangrijkste kluis.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
De volgende u zult om de URI voor het geheim die u zojuist hebt gemaakt. Hiermee wordt gebruikt in een latere stap wanneer u de toets kluis om op te halen uw geheim belt. Voer de volgende PowerShell-opdracht en noteer de waarde 'Id', dat wil de geheime URI zeggen.

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

##<a name="setting-up-application"></a>Bij het instellen van toepassing

Nu dat u een geheime opgeslagen hebt wordt u wilt ophalen die geheim en het gebruik van de code. Er zijn een paar stappen vereist om dit de eerste en de belangrijkste van uw toepassing registreren met Azure Active Directory en vervolgens vertellen Azure-toets kluis uw toepassingsgegevens zodat deze aanmeldingsaanvragen van uw toepassing.

> \[AZURE. Opmerking\] uw toepassing moet op de dezelfde Azure Active Directory-tenant als uw sleutel kluis worden gemaakt. 

Het tabblad toepassingen van Azure Active Directory voor het eerst openen

![Geopende toepassingen in Azure AD](./media/keyvault-keyrotation/AzureAD_Header.png)

Kies 'Toevoegen' als u een nieuwe toepassing toevoegen aan uw Azure AD

![Kies toepassing toevoegen](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Laat het toepassingstype als Web-toepassing en/of WEB API en geef een naam op voor uw toepassing.

![De naam van de toepassing](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Geef uw toepassing een 'aanmelding URL' en een '-App-ID URI'. Deze kunnen worden alles wat die u wilt gebruiken voor deze demo en later kunnen worden gewijzigd indien nodig.

![Vereiste URI's bieden](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Zodra de toepassing wordt toegevoegd aan Azure AD, wordt u worden overgebracht naar de pagina. Klik op het tabblad 'Configureren' en zoek vanaf dat punt en de waarde 'Client-ID' kopiëren. Noteer de client-ID voor de volgende stappen.

De volgende u een sleutel voor uw toepassing moet kunnen zijn om te communiceren met uw Azure AD genereren. U kunt dit maken onder de sectie 'Toetsen' op het tabblad 'Configuratie'. Noteer de de nieuwe sleutel uit uw Azure AD-toepassing voor gebruik in een latere stap.

![Toetsen van Azure AD-App](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Voordat er een oproepen vanuit uw toepassing in sleutel kluis moet u de toets kluis informeren over uw toepassing en de bijbehorende ' machtigingen. De volgende opdracht wordt op de naam van de kluis en de client-ID uit uw Azure AD-app en verleent 'Get-toegang tot uw belangrijkste kluis voor de toepassing.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

U bent nu klaar om te beginnen met het samenstellen van uw toepassing-oproepen. In uw toepassing moet u eerst de NuGet-pakketten nodig is om te communiceren met Azure-toets kluis en Azure Active Directory installeren. Voer de volgende opdrachten uit de Visual Studio Package Manager-console. Houd er rekening mee dat bij het schrijven van dit artikel de huidige versie van het pakket voor ActiveDirectory 3.10.305231913, is zodat u kunt de meest recente versie bevestigen en hiervan te werken.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

In uw toepassingscode, maakt u een klasse waarin de methode voor de verificatie van uw Active Directory. In dit voorbeeld class heet 'Utils'. Vervolgens moet u de volgende via toevoegen.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Voeg nu de volgende methode voor het ophalen van de token JWT van Azure AD. Gecodeerd tekenreekswaarden in uw web- of -configuratie voor onderhoud kunt u de harde verplaatsen.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Tot slot kunt u de benodigde code als u wilt bellen toets kluis en uw geheime waarde op te halen toevoegen. Eerst moet u het volgende toevoegen met de instructie.

```csharp
using Microsoft.Azure.KeyVault;
```

Vervolgens voegt u de methode oproepen naar roepen toets kluis en uw geheim op te halen. In deze methode krijgt u het geheim URI die u hebt opgeslagen in een vorige stap. Houd rekening met het gebruik van de methode GetToken van de Utils klasse hiervoor hebt gemaakt.
    
```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Wanneer u uw toepassing uitvoert, u moet nu geverifieerd bij Azure Active Directory en vervolgens uw geheime waarde ophalen uit uw Azure-toets kluis.

##<a name="key-rotation-using-azure-automation"></a>Toets draaiing met Azure automatisering

Er zijn verschillende opties voor de uitvoering van een strategie draaiing voor waarden die u als Azure-toets kluis geheimen opslaat. Geheimen kunnen worden gedraaid als onderdeel van een handmatig proces, ze via programmacode kunnen worden gedraaid door gebruikmaken van API-oproepen of ze in een automatiseringsscript mogelijk worden gedraaid. Voor de toepassing van dit artikel wordt we optimaal benutten Azure PowerShell gecombineerd met Azure automatisering toegangstoets Azure opslag-Account wijzigen en vervolgens een belangrijke kluis geheim wordt bijgewerkt met de nieuwe sleutel. 

Zodat Azure automatisering geheime waarden instellen in uw belangrijkste kluis moet u om de client-ID voor de verbinding met de naam 'AzureRunAsConnection' die is gemaakt wanneer u uw exemplaar van de Azure automatisering tot stand gebracht. U kunt deze-ID openen door te kiezen "Activa" vanuit uw exemplaar van de Azure automatisering. Daarvandaan u kiest u 'Verbindingen' en selecteer vervolgens het principe van de service 'AzureRunAsConnection'. U wilt moet u rekening houden met de 'toepassings-ID'. 

![Azure automatisering cliënt-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

Terwijl u nog steeds in het venster activa wilt u ook kiest u 'Modules'. Uit modules u Selecteer 'Galerie' en zoek vervolgens naar en 'Importeren' bijgewerkte versies van elk van de volgende modules.

    Azure
    Azure.Storage   
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage
    
> \[AZURE. Opmerking\] bij het schrijven van dit artikel alleen het bovenstaande genoteerd modules die nodig zijn voor het script gedeeld hieronder worden bijgewerkt. Als u vindt dat uw taak automatisering is verbroken, kunt u te bevestigen dat u hebt alle benodigde modules en bijbehorende afhankelijkheden die zijn geïmporteerd.

Nadat u hebt de toepassings-ID voor de verbinding Azure automatisering opgehaald, moet u uw Azure-toets kluis informeren dat deze toepassing toegang heeft tot geheimen in uw kluis bijwerken. Dit kunt u doen met de volgende PowerShell-opdracht.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Vervolgens selecteert u de resource 'Runbooks' onder uw exemplaar van de Azure automatisering en selecteer 'toevoegen een Runbook'. Selecteer 'Snel Maak'. Naam van uw runbook en selecteer 'PowerShell' als het type runbook. (Optioneel) een beschrijving toevoegen. Klik tot slot op 'Maken'.

![Runbook maken](./media/keyvault-keyrotation/Create_Runbook.png)

Klik in het deelvenster editor voor uw nieuwe runbook wordt u het volgende PowerShell-script plakken.

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -StorageAccountName $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Vanuit het deelvenster editor kunt u 'Test deelvenster' Test het script. Zodra het script wordt uitgevoerd zonder fouten kunt u de optie 'Publiceren' en vervolgens kunt u een schema voor het runbook weer op het deelvenster van de configuratie runbook toepassen.

##<a name="key-vault-auditing-pipeline"></a>Toets kluis controle verkooppijplijn

Als u een Azure-toets kluis kunt u controle logboeken verzamelen op toegangsaanvragen voor de sleutel kluis inschakelen. Deze logboeken worden opgeslagen in een aangewezen opslag van Azure-account en klik vervolgens kunnen worden opgevraagd, gecontroleerd en geanalyseerd. Onder begeleidt bij een scenario waarbij de maakt gebruik van Azure-functies, controlelogboeken Azure logica Apps en -toets kluis als u wilt maken van een pijplijn een e-mailbericht verzenden wanneer geheimen uit de kluis worden opgehaald door een app die overeenkomen met de app-id van de web-app.

U moet eerst logboekregistratie inschakelen op de toets kluis. U kunt dit doen via de volgende PowerShell-opdrachten (is zichtbaar voor uitgebreide informatie [hier](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>' 
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Als dit is ingeschakeld, wordt waarna controlelogboeken bijhouden gestart verzamelen in de aangewezen opslag-account. Deze logboeken bevat gebeurtenissen op hoe en wanneer uw sleutel kluizen worden geopend en door wie. 

> \[AZURE. Opmerking\] u kunt uw logboekinformatie maximaal openen, 10 minuten na de toets kluis bewerking. In de meeste gevallen wordt het sneller dan deze zijn.

De volgende stap is het [opzetten van een wachtrij Azure Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Dit is waar belangrijke kluis controlelogboeken bijhouden worden geplaatst. Eenmaal in de wachtrij, de logica-App wordt ophalen en ze handelen. Maken van een Service-Bus is relatief uitgestreken doorsturen en hieronder vindt u de hoogste niveau stappen:

1. Maak een naamruimte Service Bus (als u al een die u wilt gebruiken voor deze vervolgens gaat u verder met stap 2 hebt).
2. Blader naar de Service-Bus in de portal en selecteer de naamruimte die u wilt maken van de wachtrij in.
3. Selecteer Nieuw en kies Service Bus -> wachtrij en voer de vereiste gegevens.
4. Pak de verbindingsgegevens Service Bus door de naamruimte te kiezen en te klikken op _Verbindingsgegevens_. Moet u deze informatie voor het volgende gedeelte.

Vervolgens wordt u [een functie Azure maken](../azure-functions/functions-create-first-azure-function.md) om te controleren van de sleutel kluis logboeken binnen het opslag-account en ga verder nieuwe gebeurtenissen. Dit is een functie die volgens een schema wordt geactiveerd.

Maak een Azure-functie (Kies de optie Nieuw functie App in de portal ->). U kunt tijdens het maken van een bestaand hostingprovider schema gebruiken of een nieuw account te maken. U kunt ook kiezen voor het dynamische hostingprovider. Meer informatie over de functie hostingprovider opties vindt u [hier](../azure-functions/functions-scale.md).

Wanneer de functie Azure is gemaakt, gaat u naar deze en kiest u een timer, functie en C\# klik vervolgens op **maken** vanuit het startscherm.

![Azure functies Blade starten](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Het tabblad _ontwikkelen_ door de run.csx-code te vervangen door het volgende:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging; 
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log) 
{ 
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob) 
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```
> \[AZURE. Opmerking\] Zorg ervoor dat voor het vervangen van de variabelen in de bovenstaande zodat deze verwijzen bij uw opslagruimte account waar de sleutel kluis Logboeken geschreven, de Service-Bus die u eerder hebt gemaakt en het specifieke pad naar de belangrijkste kluis opslag logboeken code.

De functie opgehaald het meest recente logboekbestand van het account opslagruimte waar de sleutel kluis Logboeken geschreven, de meest recente gebeurtenissen van dat bestand pakt en verplaatst deze naar een Service Bus wachtrij. Aangezien één bestand meerdere gebeurtenissen hebben kan, bijvoorbeeld via een volledige uur, klikt u vervolgens we een _sync.txt_ bestand maken die de functie lijkt op om te bepalen de tijdstempel van de laatste gebeurtenis die is opgehaald. Dit zorgt ervoor dat dat we niet push dezelfde gebeurtenis meerdere keren. Dit bestand _sync.txt_ bevat gewoon een tijdstempel voor de laatste aangetroffen gebeurtenis. De logboeken, wanneer geladen, moeten worden gesorteerd op basis van de tijdstempel om ervoor te zorgen dat ze correct worden geordend.

Voor deze functie we verwijzingen maken naar een aantal extra bibliotheken die niet nog beschikbaar zijn van het vak in Azure-functies. Als u wilt opnemen deze, moeten we Azure functies om op te halen ze nuget gebruiken. Kies de optie _Bestanden weergeven_ 

![De optie bestanden weergeven](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

en toevoegen van een nieuw bestand met de naam van _project.json_ met de volgende inhoud:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Na het _Opslaan_ , wordt dit Azure functies om te downloaden van de vereiste binaire bestanden activeren. 

Ga naar het tabblad **integreren** en geef de parameter timer een beschrijvende naam voor het gebruik van binnen de functie. In de code wordt ervan uitgegaan dat de timer _myTimer_genoemd. Een [expressie CRON](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) als volgt opgeven: 0 \* \* \* \* \* voor de timer, waardoor de functie minuut uitvoeren. 

In het tabblad dezelfde **integreren** toevoegen een invoer die van het type _Azure-blobopslag_. Deze verwijst naar het bestand _sync.txt_ met het tijdstempel van de laatste gebeurtenis die door de functie hebt bekeken. Dit komen beschikbaar binnen de functie door de parameternaam. De invoer van de Azure-blobopslag verwacht in de code wordt de naam van de parameter moeten _inputBlob_. Kies het opslag-account waar het bestand _sync.txt_ zich bevinden (het kan zijn hetzelfde of een ander opslag-account) en geef in het padveld het pad waar het bestand zich bevindt in, in de notatie van {container-name}/path/to/sync.txt.

Een uitvoer die van het type _Azure-blobopslag_ uitvoer toevoegen. Dit wordt ook verwijzen naar het _sync.txt_ -bestand dat u zojuist hebt gedefinieerd in het vak invoerbereik. Hiermee worden gebruikt door de functie om te schrijven van de tijdstempel van de laatste gebeurtenis hebt bekeken. De bovenstaande code verwacht deze parameter _outputBlob_worden aangeroepen.

De functie is nu klaar. Zorg ervoor dat Ga terug naar het tabblad **ontwikkelen** en _Sla_ de code. Het uitvoervenster voor gecompileerd fouten controleren en corrigeren van de dienovereenkomstig gewijzigd. Als deze gecompileerd, klikt u vervolgens de code moet nu worden uitgevoerd en elke minuut wordt de logboeken aan de sleutel kluis controleren en alle nieuwe gebeurtenissen naar de gedefinieerde Service Bus wachtrij push. Hier ziet u logboekinformatie wegschrijven naar de telkens van het logboek venster wanneer die de functie wordt geactiveerd.

###<a name="azure-logic-app"></a>Azure logica-App

Volgende moeten we een Azure logica App maakt die de gebeurtenissen die de functie is om naar de wachtrij Service Bus verdergaan, parseert de inhoud en verzend een e-mailbericht op basis van een voorwaarde wordt aangepast.

[Een App logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md) door te gaan naar nieuw -> logica-App. 

Nadat de logica-App is gemaakt, Ga naar deze en kiest u _bewerken_. Binnen de logica App-editor, kies beheerde api van de _Service Bus wachtrij_ en voer uw referenties Service Bus als u wilt deze verbinden met de wachtrij.

![Azure logica App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Kies volgende om toe te _voegen een voorwaarde_. Schakel in de voorwaarde, naar de _Geavanceerde editor_ en voer de volgende gegevens, de APP_ID vervangen door de werkelijke APP_ID van uw web-app:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Deze expressie in principe retourneert **Onwaar** als de eigenschap **toepassings-id** van de binnenkomende gebeurtenis (dit is de hoofdtekst van het bericht Service Bus) niet de **toepassings-id** van de app is. 

Maak nu een actie onder de optie _Nee, als er niets..._ .

![Azure logica App kiezen actie](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Voor de actie, kiest u _Office 365 - e-mailbericht verzenden_. Vul de velden aan het maken van een e-mailbericht wilt verzenden als de voorwaarde de gedefinieerde onwaar retourneert. Als u nog geen Office 365 kan u alternatieven voor bereiken hetzelfde bekijken.

U hebt nu een complete verkooppijplijn die, minuut, wordt gezocht naar de nieuwe sleutel kluis controlelogboeken bijhouden. Alle nieuwe logboeken vindt, deze wordt push ze naar een Service Bus wachtrij. De logica-App wordt geactiveerd zodra u een nieuw bericht in de wachtrij terechtkomt en als de toepassings-id binnen de gebeurtenis niet overeenkomen met de app-id van de bellen-toepassing en vervolgens een e-mailbericht verzenden. 
