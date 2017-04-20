<properties
    pageTitle="Maken en gebruiken van een SA's met blobopslag | Microsoft Azure"
    description="Deze zelfstudie wordt getoond hoe gedeelde toegang handtekeningen voor gebruik met blobopslag maakt en hoe u deze gebruiken vanaf uw clienttoepassingen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Gedeelde handtekeningen in Access, deel 2: Maken en gebruiken van een SA's met-blobopslag

## <a name="overview"></a>Overzicht

[Deel 1](storage-dotnet-shared-access-signature-part-1.md) van deze zelfstudie verkend gedeeld access handtekeningen (SA's) en uitleg over aanbevolen procedures voor het gebruik van deze. Deel 2 ziet u hoe u genereren en gebruikt u gedeelde toegang handtekeningen met blobopslag. De voorbeelden zijn geschreven in C# en gebruiken van de bibliotheek van Azure opslag-Client voor .NET. De scenario's waarvoor zijn deze aspecten van het werken met gedeelde toegang handtekeningen:

- Een gedeelde access-handtekening op een container te genereren
- Een gedeelde access-handtekening op een blob te genereren
- Een opgeslagen-beleid voor het beheren van handtekeningen in een container resources maken
- De gedeelde toegang handtekeningen van een clienttoepassing testen

## <a name="about-this-tutorial"></a>Over deze zelfstudie

In deze zelfstudie richten we ons op het maken van gedeelde toegang handtekeningen voor containers en BLOB's door twee consoletoepassingen te maken. De eerste consoletoepassing gegenereerd gedeelde toegang handtekeningen in een container en klik op een blob. Deze toepassing heeft kennis van de sleutels opslag-account. De tweede consoletoepassing en die als een clienttoepassing fungeert, toegang heeft tot de container en blob resources met de gedeelde toegang handtekeningen die zijn gemaakt met de eerste toepassing. Deze toepassing maakt gebruik van de gedeelde toegang handtekeningen alleen voor de verificatie van de toegang tot de container en resources blob - er geen kennis van de account te gebruiken.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Deel 1: Een consoletoepassing te genereren van gedeelde toegang handtekeningen maken

Eerst zorgt ervoor dat u de bibliotheek van Azure opslag-Client voor .NET is geïnstalleerd. U kunt het [pakket NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet-pakket") met de meest recente stroombaan voor de clientbibliotheek; installeren Dit is de aanbevolen methode om ervoor te zorgen dat u de meest recente oplossingen hebt. U kunt ook de clientbibliotheek als onderdeel van de meest recente versie van de [Azure SDK voor .NET](https://azure.microsoft.com/downloads/)downloaden.

Visual Studio, maak een nieuwe toepassing van de Windows-console en noem deze **GenerateSharedAccessSignatures**. Verwijzingen naar **Microsoft.WindowsAzure.Configuration.dll** en **Microsoft.WindowsAzure.Storage.dll**, gebruikmaakt van een van de volgende manieren van toevoegen:

-   Als u het pakket NuGet installeren wilt, moet u eerst de [NuGet-Client](https://docs.nuget.org/consume/installing-nuget)installeren. Selecteer in Visual Studio, **Project | NuGet-pakketten beheren**, online zoeken naar **Azure Storage**en volg de instructies voor het installeren.
-   U kunt ook de stroombaan Zoek in uw installatie van de Azure SDK en verwijzingen toevoegen aan deze.

Toevoegen aan de bovenkant van het bestand Program.cs, de volgende instructies voor het **gebruik van** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Bewerk het bestand app.config zodat deze een configuratie-instelling met een verbindingsreeks die naar uw account opslag verwijst bevat. Uw bestand app.config moet er ongeveer dit item zijn:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Een gedeelde Access-handtekening URI genereren voor een Container

Allereerst zullen we een methode voor het genereren van een gedeelde access-handtekening op een nieuwe container toevoegen. In dit geval de handtekening is gekoppeld aan een opgeslagen-beleid, zodat zij werkzaam de URI de informatie die aangeeft dat de verlooptijd en de machtigingen die deze worden toegewezen.

Code eerst toevoegen aan de methode **Main()** voor toegang tot uw account opslag verifiëren en een nieuwe container maken:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Voeg nu een nieuwe methode die de handtekening gedeelde toegang voor de container genereert en geeft als resultaat de handtekening URI:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Voeg de volgende regels onderaan in de methode **Main()** , voordat u de oproep door naar **Console.ReadLine()**, om te bellen **GetContainerSasUri()** en de handtekening URI naar het consolevenster schrijven:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Compileren en uitvoeren als u wilt uitvoeren van de gedeelde access-handtekening URI voor de nieuwe container. De URI is vergelijkbaar met de volgende URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Nadat u de code hebt uitgevoerd, zijn de gedeelde access-handtekening die u hebt gemaakt op de container zijn geldig voor het volgende 24 uur. De handtekening verleent een client-machtigingen voor lijst BLOB's in de container en een nieuwe blob schrijven naar de container.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Een gedeelde Access-handtekening URI voor een Blob genereren

Vervolgens wordt we vergelijkbare code voor het maken van een nieuwe blob binnen de container en een handtekening gedeelde toegang genereren voor het schrijven. Deze handtekening gedeelde toegang is niet gekoppeld aan een opgeslagen-beleid, zodat deze de begintijd, verlooptijd en machtigingsinformatie over de URI bevat.

Een nieuwe methode die Hiermee maakt u een nieuwe blob toevoegen en tekst, klikt u vervolgens genereert een handtekening gedeelde toegang en geeft als resultaat de handtekening URI schrijven:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Voeg de volgende regels om te bellen **GetBlobSasUri()**, voordat u de oproep door naar **Console.ReadLine()**, en de handtekening gedeelde toegang URI schrijven naar het consolevenster onderaan in de methode **Main()** :    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Compileren en uitvoeren als u wilt uitvoeren van de gedeelde access-handtekening URI voor de nieuwe blob. De URI is vergelijkbaar met de volgende URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Een opgeslagen-beleid maken op de Container

Nu een opgeslagen-beleid maken op de container, waarin de beperkingen voor een gedeelde toegang handtekeningen die gekoppeld zijn.

In de voorgaande voorbeelden we opgegeven de begintijd (impliciet of expliciet), de verlooptijd en de machtigingen voor de handtekening gedeelde toegang URI zelf. In de volgende voorbeelden wordt we deze op het opgeslagen-beleid en niet op de handtekening gedeelde toegang opgeven. Hierdoor kan wij deze beperkingen wijzigen zonder het opnieuw uitgeven de handtekening gedeelde toegang.

Het is mogelijk heeft een of meer van de beperkingen voor de handtekening gedeelde toegang en het restgetal op het opgeslagen-beleid. U kunt echter alleen opgeven de begintijd, verlooptijd en machtigingen in één plaats of de andere; u niet bijvoorbeeld Geef machtigingen op de handtekening gedeelde toegang en ook achter de komma opgeven in het opgeslagen-beleid.

Houd er rekening mee dat wanneer u een access-beleid aan een container toevoegt, u moet de bestaande machtigingen van de container, de nieuwe-beleid toevoegen, en vervolgens van de container machtigingen instellen.

Een nieuwe methode die Hiermee maakt u een nieuw opgeslagen-beleid aan een container en geeft als resultaat de naam van het beleid toevoegen:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

Voeg de volgende regels eerst een bestaande clienttoegangsbeleid onderaan in de methode **Main()** , voordat u de oproep door naar **Console.ReadLine()**, en vervolgens de methode **CreateSharedAccessPolicy()** aanroepen:    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Houd er rekening mee dat wanneer u het-beleid op een container uitschakelt, moet u eerst de bestaande machtigingen van de container, krijgen en vervolgens de machtigingen uitschakelen en vervolgens de machtigingen opnieuw instellen.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Een gedeelde Access-handtekening URI op de Container met een Access-beleid genereren

Vervolgens maken we een andere gedeelde toegang handtekening op de container die we eerder, maar klik nu die we de handtekening wordt koppelen aan het access-beleid die we hebben gemaakt in het vorige voorbeeld hebt gemaakt.

Een nieuwe methode voor het genereren van een andere gedeelde toegang handtekening op de container toevoegen:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Toevoegen aan de onderkant van de methode **Main()** , voordat u de oproep door naar **Console.ReadLine()**, de volgende regels om te bellen van de methode **GetContainerSasUriWithPolicy** :

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Een gedeelde Access-handtekening URI op de Blob die gebruikmaakt van een Access-beleid genereren

Ten slotte, zullen we een vergelijkbare manier voor het maken van een andere blob en genereren van een gedeelde access-handtekening die is gekoppeld aan een access-beleid toevoegen.

Een nieuwe methode voor het maken van een blob en genereren van een gedeelde access-handtekening toevoegen:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

Toevoegen aan de onderkant van de methode **Main()** , voordat u de oproep door naar **Console.ReadLine()**, de volgende regels om te bellen van de methode **GetBlobSasUriWithPolicy** :    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

De methode **Main()** ziet er nu als volgt in zijn geheel. Uitvoeren om de handtekening gedeelde toegang URI's schrijven naar het consolevenster vervolgens kopiëren en plak deze in een tekstbestand voor gebruik in het tweede deel van deze zelfstudie.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Wanneer u de GenerateSharedAccessSignatures console-toepassing uitvoert, ziet u de uitvoer is vergelijkbaar met de volgende handelingen uit in het consolevenster. Hierna ziet u de gedeelde toegang handtekeningen die u in deel 2 van de zelfstudie gebruikt.

![SA's-console-uitvoer-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Deel 2: Een consoletoepassing om te testen van de gedeelde toegang handtekeningen maken

Als u wilt testen van de gedeelde toegang handtekeningen hebt gemaakt in de voorgaande Column, maken we een tweede consoletoepassing waarin de handtekeningen voor bewerkingen uitvoeren op de container en klik op een blob wordt gebruikt.

> [AZURE.NOTE] Als u meer dan 24 uur is verstreken sinds u het eerste deel van de zelfstudie hebt voltooid, is de handtekeningen die u gegenereerd niet langer geldig. In dit geval moet u de code in de eerste consoletoepassing te genereren van vers gedeelde toegang handtekeningen voor gebruik in het tweede deel van de zelfstudie uitvoeren.

Visual Studio, maak een nieuwe toepassing van de Windows-console en noem deze **ConsumeSharedAccessSignatures**. Verwijzingen naar **Microsoft.WindowsAzure.Configuration.dll** en **Microsoft.WindowsAzure.Storage.dll**, toevoegen als u eerder hebt.

Toevoegen aan de bovenkant van het bestand Program.cs, de volgende instructies voor het **gebruik van** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

In de hoofdtekst van de methode **Main()** toevoegen de volgende constanten zijn en hun waarden bijwerken naar de gedeelde toegang handtekeningen die u in deel 1 van de zelfstudie hebt gegenereerd.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Een methode om te proberen Container bewerkingen met een gedeelde Access-handtekening toevoegen

Vervolgens zullen we een methode waarmee sommige vertegenwoordiger container bewerkingen met een gedeelde access-handtekening op de container kan worden getest toevoegen. Houd er rekening mee dat de handtekening gedeelde toegang als resultaat een verwijzing naar de container, toegang tot de container op basis van de handtekening alleen verificatie wordt gebruikt.

De volgende methode toevoegen aan Program.cs:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


De methode **Main()** om te bellen **UseContainerSAS()** met beide van de gedeelde toegang handtekeningen die u hebt gemaakt op de container bijwerken:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Een methode om te proberen Blob bewerkingen met een gedeelde Access-handtekening toevoegen

Ten slotte, zullen we een methode waarmee sommige vertegenwoordiger blob bewerkingen met een gedeelde access-handtekening op de blob kan worden getest toevoegen. In dit geval gebruiken we de constructor **CloudBlockBlob(String)**, in de handtekening gedeelde toegang geven als resultaat een verwijzing naar de blob. Geen andere verificatie is vereist. Dit gebaseerd op de handtekening alleen.

De volgende methode toevoegen aan Program.cs:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


De methode **Main()** om te bellen **UseBlobSAS()** met beide van de gedeelde toegang handtekeningen die u hebt gemaakt op de blob bijwerken:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Voer de consoletoepassing en de uitvoer om te zien welke bewerkingen zijn toegestaan voor welke handtekeningen toekijken. De uitvoer in het consolevenster ziet er ongeveer als volgt uit:

![SA's-console-uitvoer-2][sas-console-output-2]

## <a name="next-steps"></a>Volgende stappen

[Handtekeningen gedeelde Access, deel 1: Wat is het Model SA's?](storage-dotnet-shared-access-signature-part-1.md)

[Anonieme alleen de toegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md)

[Delegeren van Access met een gedeelde Access-handtekening (REST API)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Inleiding tot tabel en wachtrij SA 's](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
