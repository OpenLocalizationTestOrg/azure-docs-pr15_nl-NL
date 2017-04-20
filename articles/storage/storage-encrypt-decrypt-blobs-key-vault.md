<properties
    pageTitle="Zelfstudie: Versleutelen en ontsleutelen BLOB's in Microsoft Azure Storage met Azure-toets kluis | Microsoft Azure"
    description="Deze zelfstudie leert u hoe u versleutelen en een blob met aan de clientzijde codering voor Microsoft Azure Storage met Azure-toets kluis ontsleutelen."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="required"
    ms.date="10/18/2016"
    ms.author="lakasa;robinsh"/>

# <a name="tutorial-encrypt-and-decrypt-blobs-in-microsoft-azure-storage-using-azure-key-vault"></a>Zelfstudie: Versleutelen en ontsleutelen BLOB's in Microsoft Azure Storage met Azure-toets kluis

## <a name="introduction"></a>Inleiding

Deze zelfstudie wordt beschreven hoe u het gebruik van opslagruimte aan de clientzijde versleuteling met Azure-toets kluis. Deze begeleidt u bij het versleutelen en een blob in een consoletoepassing met behulp van deze technologieën ontsleutelen.

**Geschatte tijd in beslag:** 20 minuten

Zie voor informatie over Azure-toets kluis [Wat Azure-toets kluis is?](../key-vault/key-vault-whatis.md).

Zie [aan de clientzijde versleuteling en Azure toets kluis voor Microsoft Azure opslag](storage-client-side-encryption.md)voor informatie over aan de clientzijde versleuteling voor de opslag van Azure.


## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende:

- Een account Azure Storage
- Visual Studio 2013 of hoger
- Azure PowerShell


## <a name="overview-of-client-side-encryption"></a>Overzicht van aan de clientzijde versleuteling

Zie voor een overzicht van aan de clientzijde versleuteling voor Azure opslag, [aan de clientzijde versleuteling en Azure toets kluis voor Microsoft Azure Storage](storage-client-side-encryption.md)

Hier volgt een korte beschrijving van de werking van de client kant versleuteling:

1. De opslag van Azure-client SDK genereert een inhoud versleutelingssleutel (CEK), dat wil de symmetric sleutel van een een-permanente-gebruiken zeggen.
2. Gegevens van de klant is versleuteld met deze CEK.
3. De CEK wordt vervolgens verpakt (versleuteld) met de toets codering (KEK). De KEK kunt wordt aangegeven door een sleutel-id en worden van een combinatie van asymmetrische sleutel of een symmetric sleutel en kunnen worden beheerd lokaal of opgeslagen in Azure-toets kluis. De opslag-client zelf heeft nooit toegang tot de KEK. Deze roept alleen de algoritme van de belangrijkste tekstterugloop die is verstrekt door toets kluis. Klanten kunnen kiezen voor het gebruiken van aangepaste providers voor sleutel als ze willen tekstterugloop/uitpakken.
4. De versleutelde gegevens is vervolgens naar de opslag van Azure-service geüpload.


## <a name="set-up-your-azure-key-vault"></a>Uw Azure-toets kluis instellen
Om terug te gaan met deze zelfstudie, moet u de volgende stappen, die worden beschreven in deze zelfstudie [aan de slag met Azure toets kluis](../key-vault/key-vault-get-started.md):

- Maak een belangrijke kluis.
- Een toets of geheim toevoegen aan de belangrijkste kluis.
- Een toepassing met Azure Active Directory registreren.
- Autoriseer de toepassing toets of geheim te gebruiken.

Noteer de ClientID en ClientSecret die zijn gegenereerd tijdens de registratie van een toepassing met Azure Active Directory.

Beide sleutels maken in de belangrijkste kluis. We wordt ervan uitgegaan dat voor de rest van de zelfstudie dat u de volgende namen hebt gebruikt: ContosoKeyVault en TestRSAKey1.


## <a name="create-a-console-application-with-packages-and-appsettings"></a>Maak een consoletoepassing met pakketten en AppSettings

Visual Studio, een nieuwe consoletoepassing te maken.

Nodig nuget pakketten toevoegen in de Package Manager-Console.

    Install-Package WindowsAzure.Storage

    // This is the latest stable release for ADAL.
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault
    Install-Package Microsoft.Azure.KeyVault.Extensions


AppSettings toevoegen aan de App.Config.

    <appSettings>
        <add key="accountName" value="myaccount"/>
        <add key="accountKey" value="theaccountkey"/>
        <add key="clientId" value="theclientid"/>
        <add key="clientSecret" value="theclientsecret"/>
        <add key="container" value="stuff"/>
    </appSettings>

Voeg de volgende `using` instructies en zorg ervoor dat u een verwijzing naar System.Configuration toevoegen aan het project.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.Azure.KeyVault;
    using System.Threading;     
    using System.IO;


## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Een methode als u een token in uw consoletoepassing toevoegen

De volgende methode wordt gebruikt door de sleutel kluis klassen die nodig hebt om te verifiëren voor toegang tot uw belangrijkste kluis.

    private async static Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(
            ConfigurationManager.AppSettings["clientId"],
            ConfigurationManager.AppSettings["clientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

## <a name="access-storage-and-key-vault-in-your-program"></a>Toegang tot opslagcapaciteit en de toets kluis in het programma

Voeg de volgende code in de functie Hoofdgegeven.

    // This is standard code to interact with Blob storage.
    StorageCredentials creds = new StorageCredentials(
        ConfigurationManager.AppSettings["accountName"],
        ConfigurationManager.AppSettings["accountKey"]);
    CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
    CloudBlobClient client = account.CreateCloudBlobClient();
    CloudBlobContainer contain = client.GetContainerReference(ConfigurationManager.AppSettings["container"]);
    contain.CreateIfNotExists();

    // The Resolver object is used to interact with Key Vault for Azure Storage.
    // This is where the GetToken method from above is used.
    KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);


> [AZURE.NOTE] Belangrijke kluis objectmodellen
>
>Het is belangrijk om te begrijpen dat er werkelijk twee toets kluis objectmodellen letten: een is gebaseerd op de REST API (KeyVault naamruimte) en de andere is een uitbreiding van aan de clientzijde versleuteling.

> De toets kluis-Client samenwerkt met de REST API en kunt u JSON Web toetsen en geheimen voor de twee soorten zaken aan bod die zijn opgenomen in de sleutel kluis.

> De toets kluis extensies zijn klassen die lijken, specifiek worden gemaakt voor aan de clientzijde versleuteling in Azure opslag. Ze bevatten een interface voor toetsen (IKey) en op basis van het concept van een sleutel-resolvercache klassen. Er zijn twee implementaties van IKey die u moet weten: RSAKey en SymmetricKey. Nu deze overeenkomen met de zaken aan bod die zijn opgenomen in een kluis sleutel worden aangebracht, maar nu zijn onafhankelijke klassen (zodat de sleutel en geheim opgehaald door de sleutel kluis-Client Implementeer IKey).


## <a name="encrypt-blob-and-upload"></a>Versleutelen blob en uploaden
De volgende code als u wilt versleutelen een blob en uploadt dit naar uw Azure opslag-account hebt toegevoegd. De methode **ResolveKeyAsync** die wordt gebruikt als resultaat een IKey.


    // Retrieve the key that you created previously.
    // The IKey that is returned here is an RsaKey.
    // Remember that we used the names contosokeyvault and testrsakey1.
    var rsa = cloudResolver.ResolveKeyAsync("https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", CancellationToken.None).GetAwaiter().GetResult();


    // Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    // Reference a block blob.
    CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

    // Upload using the UploadFromStream method.
    using (var stream = System.IO.File.OpenRead(@"C:\data\MyFile.txt"))
        blob.UploadFromStream(stream, stream.Length, null, options, null);


Hier volgt een schermafbeelding van de [Azure klassieke Portal](https://manage.windowsazure.com) voor een blob die zijn versleuteld met behulp van aan de clientzijde versleuteling met een sleutel die zijn opgeslagen in de sleutel kluis. De eigenschap **KeyId** is de URI voor de sleutel in de sleutel kluis die als de KEK fungeert. De eigenschap **EncryptedKey** bevat de versleutelde versie van de CEK.

![Schermafbeelding van Blob metagegevens die versleuteling metagegevens bevat][1]

> [AZURE.NOTE] Als u de constructor BlobEncryptionPolicy bekijkt, ziet u dat het een sleutel en/of een resolvercache kunt accepteren. Let op die op dit moment kunt u een resolvercache voor versleuteling omdat dit het geval momenteel niet is ondersteund een standaard-sleutel.



## <a name="decrypt-blob-and-download"></a>Blob ontsleutelen en downloaden
Decoderen is in feite wanneer het gebruik van de klassen resolvercache logisch. De ID van de sleutel voor versleuteling is gekoppeld aan de label in de metagegevens, dus is er geen reden voor het ophalen van de sleutel en de koppeling tussen de sleutel en blob onthouden. U hoeft om ervoor te zorgen dat deze toets in sleutel kluis blijft.   

De persoonlijke sleutel van een sleutel RSA blijft aanwezig in sleutel kluis, zodat voor decoderen voordoen, de sleutel versleuteld uit de metagegevens blob waarin de CEK wordt verzonden naar toets kluis voor ontsleutelen.

Voeg het volgende als u wilt versleutelen de blob die u zojuist hebt geüpload.

    // In this case, we will not pass a key and only pass the resolver because
    // this policy will only be used for downloading / decrypting.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
    BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

    using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
        blob.DownloadToStream(np, null, options, null);


> [AZURE.NOTE] Er zijn een paar andere soorten resolvers key management om gemakkelijker te maken, met inbegrip van: AggregateKeyResolver en CachingKeyResolver.


## <a name="use-key-vault-secrets"></a>Toets kluis geheimen gebruiken
De manier waarop een geheim gebruiken met aan de clientzijde versleuteling is via de klasse SymmetricKey omdat een geheim in principe een symmetric sleutel is. Maar, zoals hierboven, een geheim in sleutel kluis niet is toegewezen aan een SymmetricKey precies. Er zijn een paar dingen om te begrijpen:


- De sleutel in een SymmetricKey heeft een vaste lang: 128, 192, 256, 384 of 512 bits.
- De sleutel in een SymmetricKey moet Base64 codering.
- Een toets kluis geheim dat wordt gebruikt als een SymmetricKey moet een inhoudstype van "application/octet-stream" in sleutel kluis.

Hier volgt een voorbeeld in PowerShell van het maken van een geheim in sleutel kluis die kunnen worden gebruikt als een SymmetricKey.
Opmerking: De vastgelegde waarde, $key, is bedoeld voor alleen demonstratie doel. In uw eigen code wilt u deze toets genereren.

    // Here we are making a 128-bit key so we have 16 characters.
    //  The characters are in the ASCII range of UTF8 so they are
    //  each 1 byte. 16 x 8 = 128.
    $key = "qwertyuiopasdfgh"
    $b = [System.Text.Encoding]::UTF8.GetBytes($key)
    $enc = [System.Convert]::ToBase64String($b)
    $secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

    // Substitute the VaultName and Name in this command.
    $secret = Set-AzureKeyVaultSecret -VaultName 'ContoseKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"

In uw consoletoepassing, kunt u dezelfde aanroep als voordat u deze geheim als een SymmetricKey ophalen.

    SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
        "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
        CancellationToken.None).GetAwaiter().GetResult();

Dat is. Te rekenen op!

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Microsoft Azure Storage met C#, [Microsoft Azure opslag clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Zie voor meer informatie over de Blob REST API, [Blob Service REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx).

Ga naar de [Microsoft Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)voor de meest recente informatie over Microsoft Azure opslag.


<!--Image references-->
[1]: ./media/storage-encrypt-decrypt-blobs-key-vault/blobmetadata.png
