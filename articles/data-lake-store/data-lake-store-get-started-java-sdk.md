<properties
   pageTitle="Data Lake Store Java SDK gebruiken voor het ontwikkelen van toepassingen | Microsoft Azure"
   description="Azure Data Lake Store Java SDK gebruiken voor het ontwikkelen van toepassingen"
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
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Aan de slag met Azure Lake gegevensopslag Java gebruiken

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het gebruik van de Azure Data Lake Store Java SDK bijvoorbeeld maakt mappen uploaden en downloaden gegevensbestanden, enzovoort basisbewerkingen wilt uitvoeren. Zie voor meer informatie over gegevens Lake, [Azure Lake gegevensopslag](data-lake-store-overview.md).

U kunt de documenten die Java SDK-API voor gegevensopslag met Lake van Azure op [Azure gegevens Lake Store Java API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/)kunt openen.

## <a name="prerequisites"></a>Vereisten voor

* Java Development Kit (JDK 7 of hoger, gebruikt Java versie 1.7 of hoger)
* De gegevensopslag Lake Azure-account. Volg de instructies bij [aan de slag met Azure Lake gegevensopslag met behulp van de Azure-Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Deze zelfstudie Maven voor opbouwen en project afhankelijkheden wordt gebruikt. Hoewel het is mogelijk te maken zonder een systeem opbouwen zoals Maven of Gradle, is deze systemen tabelmaakquery veel gemakkelijker voor het beheren van afhankelijkheden.
* (Optioneel) En IDE zoals [IntelliJ IDEE](https://www.jetbrains.com/idea/download/) of [Eclips](https://www.eclipse.org/downloads/) of soortgelijke.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe ik verifiëren met behulp van Azure Active Directory?

In deze zelfstudie we een geheim Azure AD-toepassing client gebruiken om op te halen een Azure Active Directory-token (service-naar-service-verificatie). We gebruiken deze token om u te maken van een object Lake gegevensopslag client om bewerkingen bestands- en -bewerkingen uitvoeren. Voor instructies over het om te verifiëren met Azure Lake gegevensopslag met de geheim client, voert u de volgende algemene stappen uit:

1. Een webtoepassing Azure AD maken
2. De client-ID, geheim client en token eindpunt voor de webtoepassing Azure AD ophalen.
3. Toegang voor de Azure AD-webtoepassing configureren op de Lake gegevensopslag bestand/tijdelijke map die u wilt openen vanaf de Java-toepassing die u maakt.

Zie voor instructies over hoe u deze stappen uitvoert, [een Active Directory-toepassing maken](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory biedt andere opties als u ook naar een token ophalen. U kunt kiezen uit een aantal verschillende verificatiemethoden aanpassen aan uw scenario, bijvoorbeeld een toepassing wordt uitgevoerd in een browser, een toepassing gedistribueerd als een bureaubladtoepassing of een servertoepassing on-premises implementatie of in een Azure virtuele machines. U kunt er ook voor kiezen uit verschillende soorten referenties zoals wachtwoorden, certificaten, 2-factor verificatie, enzovoort. Azure Active Directory Daarnaast kunt u uw on-premises Active Directory-gebruikers synchroniseren met de cloud. Zie voor meer informatie [Verificatie-scenario's voor Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Een Java-toepassing maken

Voorbeeld van de code beschikbaar [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) begeleidt u door het proces voor het maken van bestanden in de store, het samenvoegen van bestanden, een bestand downloaden en sommige bestanden in de winkel verwijderen. Deze sectie in dit artikel leest u de belangrijkste onderdelen van de code.

1. Maak een project Maven [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) vanaf de opdrachtregel of een IDE gebruiken. Zie voor instructies over het maken van een Java-project met IntelliJ [hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Zie voor instructies over het maken van een project met Eclips [hier](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. De volgende afhankelijkheden toevoegen aan uw Maven **pom.xml** -bestand. Toevoegen van de volgende fragment van de tekst tussen de ** \</version >** tag en de ** \</project >** tag:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    De eerste afhankelijkheid is het gebruik van de gegevens Lake Store SDK (`azure-datalake-store`) uit de bibliotheek maven. De tweede afhankelijkheid (`slf4j-nop`) is om op te geven welke framework logboekregistratie wilt gebruiken voor deze toepassing. De gegevens Lake Store SDK [slf4j](http://www.slf4j.org/) logboekregistratie gevel, waarin u kiezen uit een aantal populaire logboekregistratie kaders, zoals log4j, Java logboekregistratie kunt, logback, enzovoort, gebruikt of niet vastleggen. In dit voorbeeld wordt logboekregistratie uitgeschakeld, zodat we de binding **slf4j-nop** gebruiken. Zie voor informatie over het gebruik van andere opties voor logboekregistratie in uw app [hier](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>De toepassingscode hebt toegevoegd

Er zijn drie hoofdonderdelen naar de code.

1. De Azure Active Directory-token verkrijgen

2. Gebruik het token maken van een client Lake gegevensopslag.

3. Gebruik de client Lake gegevensopslag bewerkingen uit te voeren.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Stap 1: Een Azure Active Directory-token verkrijgen.

De gegevens Lake Store SDK biedt handige methoden waarmee u de beveiligingstokens die nodig zijn om te communiceren met het account voor gegevensopslag Lake verkrijgen. De SDK schrijft echter niet dat alleen deze methoden worden gebruikt. U kunt een andere manier voor het verkrijgen van token ook, zoals met behulp van de [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)of uw eigen aangepaste code gebruiken.

Als u de gegevens Lake Store SDK wilt verkrijgen token voor de Active Directory-webtoepassing u eerder hebt gemaakt, gebruikt u de statische methoden in `AzureADAuthenticator` class. **Aanvullen-hier** vervangen door de werkelijke waarden voor de Azure Active Directory-webtoepassing.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Stap 2: Maak een object Azure Lake gegevensopslag-client (ADLStoreClient)

Een object [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) maken, moet u de naam van de gegevensopslag Lake-account en de Azure Active Directory-token die u hebt gegenereerd in de laatste stap opgeven. Houd er rekening mee dat de accountnaam Lake gegevensopslag moet een volledig gekwalificeerde domeinnaam. Vervang **Aanvullen-hier** bijvoorbeeld met een ander nummer zoals **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Stap 3: Gebruik de ADLStoreClient bestands- en -bewerkingen uit te voeren

De onderstaande code bevat fragmenten voorbeeld van enkele veelgebruikte bewerkingen. U kunt de volledige [gegevens Lake Store Java SDK-API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/) van het object **ADLStoreClient** om te zien van andere bewerkingen bekijken.
 
Houd er rekening mee dat bestanden lezen en geschreven naar met behulp van standaard Java-streams. Dit betekent dat u een van de Java-streams boven aan de streams Lake gegevensopslag om te profiteren van standaardfuncties van Java (bijvoorbeeld afdrukken streams voor opgemaakte uitvoer, of vanaf een van de compressie of codering streams voor extra functionaliteit op boven, enz.) kunt lagen.

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Stap 4: Maken en uitvoeren van de toepassing

1. Als u wilt starten vanuit een IDE, zoeken en druk op de knop **uitvoeren** . Als u wilt uitvoeren vanaf Maven, gebruikt u [Raad: Raad](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. Als u wilt produceren maken op een zelfstandige oppervlak die u kunt uitvoeren vanaf de opdrachtregel het oppervlak met alle afhankelijkheden opgenomen, met de [constructie-invoegtoepassing voor Maven](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). De pom.xml in de [voorbeeld-broncode op github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) bevat een voorbeeld van hoe u dit doet.


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
