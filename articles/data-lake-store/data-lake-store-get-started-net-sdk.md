<properties
   pageTitle="Gegevens Lake Store .NET SDK gebruiken voor het ontwikkelen van toepassingen | Microsoft Azure"
   description="Azure Data Lake Store .NET SDK gebruiken voor het ontwikkelen van toepassingen"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-net-sdk"></a>Aan de slag met Azure Lake gegevensopslag met .NET SDK

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het gebruik van de [Azure Data Lake Store.NET SDK](https://msdn.microsoft.com/library/mt581387.aspx) bijvoorbeeld maakt mappen uploaden en downloaden gegevensbestanden, enzovoort basisbewerkingen wilt uitvoeren. Zie voor meer informatie over gegevens Lake, [Azure Lake gegevensopslag](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten voor

* **Visual Studio 2013 of 2015**. De onderstaande instructies Gebruik Visual Studio-2015.

* **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

* **De gegevensopslag Lake azure-account**. Zie voor instructies over het maken van een account [aan de slag met Azure Lake gegevensopslag](data-lake-store-get-started-portal.md)

* **Een Azure Active Directory-toepassing maken**. De Azure AD-toepassing kunt u de toepassing Lake gegevensopslag met Azure AD verifiëren. Er zijn verschillende manieren om te verifiëren met Azure AD, welke **eindgebruikers** of **service-naar-service-verificatie**. Zie voor instructies en meer informatie over het om te verifiëren, [verifiëren met Lake gegevensopslag Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-a-net-application"></a>Maak een .NET-toepassing

1. Open Visual Studio en maak een consoletoepassing.

2. Klik in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.

3. Uit **Nieuw Project**, typt of selecteert u de volgende waarden:

  	| Eigenschap | Waarde                       |
  	|----------|-----------------------------|
  	| Categorie | Sjablonen/Visual C# / Windows |
  	| Sjabloon | Consoletoepassing         |
  	| Naam     | CreateADLApplication        |

4. Klik op **OK** om het project te maken.

5. De Nuget-pakketten toevoegen aan uw project.

    1. Met de rechtermuisknop op de naam van het project in de Solution Explorer en klik op **NuGet-pakketten beheren**.
    2. Controleer of dat **pakket bron** is ingesteld op **nuget.org** en het selectievakje **opnemen voorlopige versie** is geselecteerd in het tabblad **Nuget Package Manager** .
    3. Zoeken naar en installeren van de volgende NuGet-pakketten:

        * `Microsoft.Azure.Management.DataLake.Store`-Deze zelfstudie gebruikt v0.12.5-preview.
        * `Microsoft.Azure.Management.DataLake.StoreUploader`-Deze zelfstudie gebruikt v0.10.6-preview.
        * `Microsoft.Rest.ClientRuntime.Azure.Authentication`-Deze zelfstudie gebruikt v2.2.8-preview.

        ![Een bron Nuget toevoegen] (./media/data-lake-store-get-started-net-sdk/ADL.Install.Nuget.Package.png "Een nieuwe Azure gegevens Lake-account maken")

    4. Sluit de **Nuget Package Manager**.

6. Open **Program.cs**, de bestaande code verwijderen en vervolgens de volgende beweringen om toe te voegen verwijzingen naar naamruimten opnemen.

        using System;
        using System.Threading;
        
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.StoreUploader;

7. De variabelen declareren, zoals hieronder wordt weergegeven en geef de waarden voor gegevensopslag Lake naam en de naam van de resource-groep die al aanwezig. Zorg er ook de lokale pad en de bestandsnaam die u hier opgeeft moet bestaan op de computer. Hiermee voegt u het volgende codefragment na de naamruimtedeclaraties.

        namespace SdkSample
        {
            class Program
            {
                private static DataLakeStoreAccountManagementClient _adlsClient;
                private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
                
                private static string _adlsAccountName;
                private static string _resourceGroupName;
                private static string _location;
                private static string _subId;

                
                private static void Main(string[] args)
                {
                    _adlsAccountName = "<DATA-LAKE-STORE-NAME>"; // TODO: Replace this value with the name of your existing Data Lake Store account.
                    _resourceGroupName = "<RESOURCE-GROUP-NAME>"; // TODO: Replace this value with the name of the resource group containing your Data Lake Store account.
                    _location = "East US 2";
                    _subId = "<SUBSCRIPTION-ID>";
                    
                    string localFolderPath = @"C:\local_path\"; // TODO: Make sure this exists and can be overwritten.
                    string localFilePath = localFolderPath + "file.txt"; // TODO: Make sure this exists and can be overwritten.
                    string remoteFolderPath = "/data_lake_path/";
                    string remoteFilePath = remoteFolderPath + "file.txt";
                }
            }
        }

In de resterende secties van het artikel ziet u hoe u met de beschikbare .NET-methoden bewerkingen zoals verificatie, bestandsupload, enzovoort.

## <a name="authentication"></a>Verificatie

### <a name="if-you-are-using-end-user-authentication-recommended-for-this-tutorial"></a>Als u van eindgebruikers-verificatie gebruikmaakt (aanbevolen voor deze zelfstudie)

Gebruik deze opdracht met een bestaande Azure AD "Native Client" toepassing niet omzeilen. Er is toegekend u hieronder. Als u deze zelfstudie sneller voltooid, wordt u aangeraden dat u deze methode gebruiken.

    // User login via interactive popup
    // Use the client ID of an existing AAD "Native Client" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.
    var nativeClientApp_clientId = "1950a258-227b-4e31-a9cf-717495945fc2";
    var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientApp_clientId, new Uri("urn:ietf:wg:oauth:2.0:oob"));
    var creds = UserTokenProvider.LoginWithPromptAsync(domain, activeDirectoryClientSettings).Result;

Het aantal wat u moet weten over deze codefragment van de bovenstaande.

* Als u de zelfstudie sneller voltooien, dit fragment gebruikt een een Azure AD-domein en client-ID die is standaard beschikbaar voor alle Azure abonnementen. Zo is, kunt u **gebruiken dit codefragment van de als-is in uw toepassing**.
* Echter als u uw eigen Azure AD-domein en toepassing client-ID gebruiken wilt, moet u maken een systeemeigen Azure AD-toepassing en klikt u vervolgens het Azure AD-domein, cliënt-ID, gebruiken en omleiden URI voor de toepassing die u hebt gemaakt. Zie [een Active Directory-toepassing maken](../resource-group-create-service-principal-portal.md#create-an-active-directory-application) voor instructies.

>[AZURE.NOTE] Er zijn de instructies in de bovenstaande koppelingen voor een Azure AD-webtoepassing. De stappen zijn echter precies hetzelfde zelfs als u ervoor kiest in plaats daarvan een systeemeigen clienttoepassing maken. 

### <a name="if-you-are-using-service-to-service-authentication-with-client-secret"></a>Als u met service-naar-service verificatie geheim client 

Het codefragment van de volgende kan worden gebruikt om te verifiëren van uw toepassing niet-interactief, met de client geheime / sleutel voor een toepassing / antwoordgroepservice hoofdsom. Gebruik deze met een bestaande [Azure AD "Web App" servicetoepassing](../resource-group-create-service-principal-portal.md).

    // Service principal / appplication authentication with client secret / key
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    var clientSecret = "<AAD-application-client-secret>";
    var clientCredential = new ClientCredential(webApp_clientId, clientSecret);
    var creds = ApplicationTokenProvider.LoginSilentAsync(domain, clientCredential).Result;

### <a name="if-you-are-using-service-to-service-authentication-with-certificate"></a>Als u van service-naar-service verificatie met certificaat gebruikmaakt

Een derde optie, kan het volgende fragment worden gebruikt om te verifiëren van uw toepassing niet-interactief, met behulp van het certificaat voor een toepassing / antwoordgroepservice hoofdsom. Gebruik deze met een bestaande [Azure AD "Web App" servicetoepassing](../resource-group-create-service-principal-portal.md).

    // Service principal / application authentication with certificate
    // Use the client ID and certificate of an existing AAD "Web App" application.
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    var domain = "<AAD-directory-domain>";
    var webApp_clientId = "<AAD-application-clientid>";
    System.Security.Cryptography.X509Certificates.X509Certificate2 clientCert = <AAD-application-client-certificate>
    var clientAssertionCertificate = new ClientAssertionCertificate(webApp_clientId, clientCert);
    var creds = ApplicationTokenProvider.LoginSilentWithCertificateAsync(domain, clientAssertionCertificate).Result;

## <a name="create-client-objects"></a>Client-objecten maken

Het volgende codefragment maakt de Lake gegevensopslag account en bestandssysteem client objecten, die worden gebruikt om te verzoeken verlenen aan de service.

    // Create client objects and set the subscription ID
    _adlsClient = new DataLakeStoreAccountManagementClient(creds);
    _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClient(creds);

    _adlsClient.SubscriptionId = _subId;

## <a name="list-all-data-lake-store-accounts-within-a-subscription"></a>Een lijst met alle Lake gegevensopslag mailaccounts binnen een abonnement

Het codefragment van de volgende bevat alle Lake gegevensopslag mailaccounts binnen een bepaald Azure abonnement.

    // List all ADLS accounts within the subscription
    public static List<DataLakeStoreAccount> ListAdlStoreAccounts()
    {
        var response = _adlsClient.Account.List();
        var accounts = new List<DataLakeStoreAccount>(response);
        
        while (response.NextPageLink != null)
        {
            response = _adlsClient.Account.ListNext(response.NextPageLink);
            accounts.AddRange(response);
        }
        
        return accounts;
    }

## <a name="create-a-directory"></a>Een map maken

Het volgende fragment een `CreateDirectory` methode die u gebruiken kunt om een map binnen een Lake gegevensopslag-account te maken.

    // Create a directory
    public static void CreateDirectory(string path)
    {
        _adlsFileSystemClient.FileSystem.Mkdirs(_adlsAccountName, path);
    }

## <a name="upload-a-file"></a>Een bestand uploaden

Het volgende fragment een `UploadFile` methode die u kunt bestanden uploaden naar een Lake gegevensopslag-account.

    // Upload a file
    public static void UploadFile(string srcFilePath, string destFilePath, bool force = true)
    {
        var parameters = new UploadParameters(srcFilePath, destFilePath, _adlsAccountName, isOverwrite: force);
        var frontend = new DataLakeStoreFrontEndAdapter(_adlsAccountName, _adlsFileSystemClient);
        var uploader = new DataLakeStoreUploader(parameters, frontend);
        uploader.Execute();
    }

`DataLakeStoreUploader`ondersteunt recursieve uploaden en downloaden tussen een lokaal bestandspad en een bestandspad Lake gegevensopslag.    

## <a name="get-file-or-directory-info"></a>Toon info bestand of map

Het volgende fragment een `GetItemInfo` methode die u gebruiken kunt om op te halen informatie over een bestand of map die beschikbaar zijn in Lake gegevensopslag. 

    // Get file or directory info
    public static FileStatusProperties GetItemInfo(string path)
    {
        return _adlsFileSystemClient.FileSystem.GetFileStatus(_adlsAccountName, path).FileStatus;
    }

## <a name="list-file-or-directories"></a>Lijst met bestanden of mappen

Het volgende fragment een `ListItem` methode die voor een overzicht van de bestanden en mappen in een account voor gegevensopslag Lake kunt gebruiken.

    // List files and directories
    public static List<FileStatusProperties> ListItems(string directoryPath)
    {
        return _adlsFileSystemClient.FileSystem.ListFileStatus(_adlsAccountName, directoryPath).FileStatuses.FileStatus.ToList();
    }

## <a name="concatenate-files"></a>Tekst.samenvoegen bestanden

Het volgende fragment een `ConcatenateFiles` methode die u gebruikt voor het samenvoegen van bestanden. 

    // Concatenate files
    public static void ConcatenateFiles(string[] srcFilePaths, string destFilePath)
    {
        _adlsFileSystemClient.FileSystem.Concat(_adlsAccountName, destFilePath, srcFilePaths);
    }

## <a name="append-to-a-file"></a>Toevoegen aan een bestand

Het volgende fragment een `AppendToFile` methode die u gebruikt de gegevens toevoegen aan een bestand dat al is opgeslagen in een account voor gegevensopslag Lake.

    // Append to file
    public static void AppendToFile(string path, string content)
    {
        var stream = new MemoryStream(Encoding.UTF8.GetBytes(content));
        
        _adlsFileSystemClient.FileSystem.Append(_adlsAccountName, path, stream);
    }

## <a name="download-a-file"></a>Een bestand downloaden

Het volgende fragment een `DownloadFile` methode die u kunt een bestand downloaden van een Lake gegevensopslag-account.

    // Download file
    public static void DownloadFile(string srcPath, string destPath)
    {
        var stream = _adlsFileSystemClient.FileSystem.Open(_adlsAccountName, srcPath);
        var fileStream = new FileStream(destPath, FileMode.Create);
        
        stream.CopyTo(fileStream);
        fileStream.Close();
        stream.Close();
    }

## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Gegevensopslag Lake .NET SDK verwijzing](https://msdn.microsoft.com/library/mt581387.aspx)
- [Gegevensopslag Lake REST verwijzing](https://msdn.microsoft.com/library/mt693424.aspx)
