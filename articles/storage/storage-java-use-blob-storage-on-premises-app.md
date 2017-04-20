<properties
    pageTitle="Lokale toepassing met blobopslag (Java) | Microsoft Azure"
    description="Informatie over het maken van een consoletoepassing die een afbeelding uploadt naar Azure en geeft de afbeelding in uw browser. Voorbeelden van de code in Java."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="on-premises-application-with-blob-storage"></a>Lokale toepassing met-blobopslag

## <a name="overview"></a>Overzicht

Het volgende voorbeeld ziet u hoe u Azure opslag kunt gebruiken voor de opslag van afbeeldingen in Azure wordt aangegeven. De code in dit artikel is voor een consoletoepassing met een afbeelding uploadt naar Azure en maakt vervolgens een HTML-bestand dat wordt weergegeven van de afbeelding in uw browser.

## <a name="prerequisites"></a>Vereisten voor

- Een Java ontwikkelaars Kit (JDK), 1,6 of hoger is geïnstalleerd.
- De SDK Azure is geïnstalleerd.
- Het oppervlak voor de bibliotheken Azure voor Java en eventuele potten afhankelijkheid van toepassing zijn geïnstalleerd en in het opbouwen pad die worden gebruikt door uw compileerprogramma Java. Zie [de SDK Azure for Java downloaden](java-download-azure-sdk.md)voor informatie over het installeren van de Azure-bibliotheken voor Java.
- Een Azure opslag-account is ingesteld. De naam van het account en accountsleutel voor het account opslag worden gebruikt door de code in dit artikel. Lees [hoe u een Account opslag maken](storage-create-storage-account.md#create-a-storage-account) voor informatie over het maken van een opslag-account en [weergave en kopie opslag toegangstoetsen](storage-create-storage-account.md#view-and-copy-storage-access-keys) voor informatie over het ophalen van de accountsleutel.

- U kunt een van de lokale-afbeeldingsbestand met de naam hebt gemaakt die zijn opgeslagen op het pad c:\\myimages\\image1.jpg. Ook wijzigen de constructor   **FileInputStream** in het voorbeeld naar een andere afbeelding pad en de bestandsnaam gebruikt.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Azure-blobopslag gebruiken om een bestand te uploaden

Een stapsgewijze procedure wordt hier weergegeven. Als u gaan wilt, wordt de hele code wordt gepresenteerd verderop in dit artikel.

De code beginnen door invoer voor de Azure core Opslagklassen, de Azure blob client klassen, de klassen Java IO en de klas **URISyntaxException** op te nemen.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Een klasse met de naam **StorageSample**declareren en de vierkante haak openen, **{**opnemen.

    public class StorageSample {

In de klas **StorageSample** declareren een tekenreeksvariabele waarin u de standaard-eindpunt protocol, de naam van uw opslag-account en uw opslagruimte sneltoets, zoals opgegeven in uw account Azure opslag. De waarden van de tijdelijke aanduiding vervangen **uw\_account\_naam** en **uw\_account\_sleutel** account met uw eigen naam en account-toets, respectievelijk.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Toevoegen in uw declaratie voor **belangrijkste**, een blok **probeert** opnemen en de benodigde openen haken bevinden, **{**opnemen.

    public static void main(String[] args)
    {
        try
        {

Variabelen van het volgende type (de beschrijvingen zijn voor hoe ze worden gebruikt in dit voorbeeld) declareren:

-   **CloudStorageAccount**: gebruikt om het accountobject met uw Azure-opslagaccountnaam en de toets geïnitialiseerd, en om de blob client-object te maken.
-   **CloudBlobClient**: gebruikt voor toegang tot de service blob.
-   **CloudBlobContainer**: waarmee u een container blob hebt gemaakt, de BLOB's in de container lijst en de container verwijderen.
-   **CloudBlockBlob**: kon u een van de lokale-afbeeldingsbestand uploaden naar de container.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Een waarde aan de variabele **account** toewijzen.

    account = CloudStorageAccount.parse(storageConnectionString);

Een waarde aan de variabele **serviceClient** toewijzen.

    serviceClient = account.createCloudBlobClient();

Een waarde aan de **container** variabele toewijzen. We krijgt een verwijzing naar een container met de naam **gettingstarted**.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Maak de container. Deze methode maakt de container als deze niet bestaat (en u hiernaar **waar terugkeert**). Als de container bestaat, als het resultaat **Onwaar**. Een alternatief voor het **createIfNotExists** is de methode voor **maken** (die een fout zullen retourneren als de container al bestaat).

    container.createIfNotExists();

Anonieme toegang instellen voor de container.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Een verwijzing naar de blob blokkeren, waarin de blob in Azure opslag vertegenwoordigt krijgen.

    blob = container.getBlockBlobReference("image1.jpg");

Gebruik de constructor **bestand** verwijst naar het lokaal gemaakte bestand dat u uploadt. Controleer of dat u kunt dit bestand hebt gemaakt voordat u de code uitvoert.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Upload het lokale bestand via een oproep met de methode **CloudBlockBlob.upload** . De eerste parameter voor de methode **CloudBlockBlob.upload** is een **FileInputStream** -object met het lokale bestand die worden geüpload naar Azure opslag. De tweede parameter is de grootte, in bytes, van het bestand.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Bellen van een Help-functie met de naam **MakeHTMLPage**, zodat u een eenvoudige HTML-pagina met een ** &lt;afbeelding&gt; ** element met de bron is ingesteld op de blob die bevindt zich nu in uw account Azure opslag. De code voor **MakeHTMLPage** wordt verderop in dit artikel worden besproken.

    MakeHTMLPage(container);

Een statusbericht en informatie over de gemaakte HTML-pagina afgedrukt.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

De blokkering **probeert** sluiten door in te voegen een vierkante haak sluiten: **}**

Verwerken van de volgende uitzonderingen:

-   **FileNotFoundException**: door de constructors **FileInputStream** of **FileOutputStream** kan worden gegenereerd.
-   **StorageException**: door de bibliotheek van de opslagruimte Azure-client kan worden gegenereerd.
-   **URISyntaxException**: door de methode **ListBlobItem.getUri** kan worden gegenereerd.
-   **Uitzondering**: afhandeling van algemene uitzonderingen.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

**Belangrijkste** sluiten door in te voegen een vierkante haak sluiten: **}**

Maak een **MakeHTMLPage** om een eenvoudige HTML-pagina te maken met de naam. Deze methode heeft een parameter van het type **CloudBlobContainer**, die worden gebruikt om te doorlopen in de lijst met geüploade BLOB's. Deze methode in de weergave van uitzonderingen van het type **FileNotFoundException**, die kan worden gegenereerd door het **FileOutputStream** constructor en **URISyntaxException**, die door de methode **ListBlobItem.getUri** kan worden gegenereerd. Het haakje openen, **{**opnemen.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Maak een lokaal bestand met de naam **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Schrijven naar het lokale bestand, toe te voegen in de ** &lt;html&gt;**, ** &lt;koptekst&gt;**, en ** &lt;hoofdtekst&gt; ** elementen.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

De lijst met geüploade BLOB's doorlopen. Voor elke blob, in de HTML-pagina maken een ** &lt;img&gt; ** element met het kenmerk **src** is verzonden naar de URI van de blob doordat deze in uw account Azure opslag bestaat.
Hoewel u slechts één afbeelding in dit voorbeeld wordt toegevoegd als u meer hebt toegevoegd, zou dit code al deze sequentieel.

Voor eenvoudig, in dit voorbeeld wordt ervan uitgegaan dat elke geüploade blob is een afbeelding. Als u hebt bijgewerkt BLOB's dan afbeeldingen of BLOB van pagina's in plaats van blok BLOB's, de code naar wens aanpassen.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Sluiten de ** &lt;hoofdtekst&gt; ** element en de ** &lt;html&gt; ** element.

    stream.println("</body>");
    stream.println("</html>");

Sluit het lokale bestand.

    stream.close();

**MakeHTMLPage** sluiten door in te voegen een vierkante haak sluiten: **}**

**StorageSample** sluiten door in te voegen een vierkante haak sluiten: **}**

Hieronder volgt de volledige code voor dit voorbeeld. Vergeet niet te wijzigen van de tijdelijke aanduiding voor waarden **uw\_account\_naam** en **uw\_account\_toets** uw accountnaam en accountsleutel, respectievelijk gebruiken.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Naast het uploaden van uw lokale afbeeldingsbestand met Azure storage, maakt de voorbeeldcode u een lokaal bestand namedindex.html, die u kunt openen in uw browser om uw geüploade afbeelding te bekijken.

Omdat de code uw accountnaam en accountsleutel bevat, moet u ervoor zorgen dat uw broncode veilig is.

## <a name="to-delete-a-container"></a>Een container

Omdat wordt geheven voor opslag, wilt u mogelijk de container **gettingstarted** verwijderen nadat u klaar bent met experimenteren met dit voorbeeld. Als u wilt een container verwijderen, gebruikt u de methode **CloudBlobContainer.delete** .

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Als u wilt bellen de methode **CloudBlobContainer.delete** , is het proces van het **CloudStorageAccount**, **ClodBlobClient**en **CloudBlobContainer** objecten worden geïnitialiseerd hetzelfde zoals voor de methode **createIfNotExist** . Hierna volgt een voorbeeld van een voltooid verwijderen van de container met de naam **gettingstarted**.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Zie voor een overzicht van andere blob storage klassen en methoden, [het gebruik van Java-blobopslag](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over opslagtaken voor meer complexe.

- [Azure opslag SDK for Java](https://github.com/azure/azure-storage-java)
- [Azure opslag Client SDK verwijzing](http://dl.windowsazure.com/storage/javadoc/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
