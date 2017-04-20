<properties 
    pageTitle="Gebruik van Azure opslag met een oplossing van de integratie van continue Jenkins | Microsoft Azure" 
    description="Deze zelfstudie laten zien hoe de service Azure blob gebruiken, zoals een opslagplaats voor onderdelen die zijn gemaakt door een oplossing van de integratie van continue Jenkins maken." 
    services="storage" 
    documentationCenter="java" 
    authors="dineshmurthy" 
    manager="jahogg" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Gebruik van Azure-opslag met een oplossing continue Jenkins-integratie

## <a name="overview"></a>Overzicht

De volgende informatie wordt uitgelegd hoe u Blob storage als een opslagplaats voor opbouwen onderdelen die zijn gemaakt door een oplossing Jenkins continue integratie (CI) of een bron van downloadbare bestanden moet worden gebruikt in een proces opbouwen. Een scenario's beschreven waar u dit interessant is wanneer u in een flexibele ontwikkelomgeving coderen bent (via Java of andere talen), versies worden uitgevoerd op basis van continue integratie en moet u een bibliotheek voor de onderdelen van uw opbouwen, zodat u, bijvoorbeeld deze met andere organisatieleden van uw, uw klanten delen, of voor het behoud van een archief. Een ander scenario is wanneer u uw werk opbouwen zelf andere bestanden, bijvoorbeeld afhankelijkheden downloaden als onderdeel van de build input is vereist.

In deze zelfstudie gaat u de invoegtoepassing voor de opslag Azure gebruiken voor Jenkins CI ter beschikking gesteld door Microsoft.

## <a name="overview-of-jenkins"></a>Overzicht van Jenkins ##

Jenkins schakelt op doorlopend integratie van een project software doordat ontwikkelaars eenvoudig integreren hun codewijzigingen en hebt builds geproduceerd automatisch en regelmatig, waardoor de productiviteit van de ontwikkelaars vergroten. Versies zijn versienummer en opbouwen onderdelen kunnen worden geüpload naar verschillende opslagplaatsen. In dit onderwerp wordt uitgelegd hoe Azure-blobopslag gebruiken als opslagplaats van de onderdelen opbouwen. Het downloaden van afhankelijkheden van Azure-blobopslag wordt dit ook weergegeven.

Meer informatie over Jenkins kan worden gevonden op [Jenkins vergaderen](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Voordelen van het gebruik van de Blob-service ##

Voordelen van het gebruik van de Blob-service voor het hosten van uw agile ontwikkeling opbouwen onderdelen zijn onder andere:

- Betere beschikbaarheid van uw opbouwen onderdelen en/of downloadbare afhankelijkheden.
- Prestaties wanneer uw oplossing Jenkins CI uploads uw onderdelen opbouwen.
- Prestaties wanneer uw klanten en partners downloadt de onderdelen van uw opbouwen.
- Controle over access gebruikersbeleid, met een keuze tussen anonieme toegang, verlooptijd gebaseerde gedeelde toegang handtekening access, privé toegang, enzovoort.

## <a name="prerequisites"></a>Vereisten voor ##

U moet de volgende handelingen uit voor het gebruik van de service Blob met uw oplossing Jenkins CI:

- Een oplossing Jenkins continue integratie.

    Als u een oplossing Jenkins CI momenteel geen hebt, kunt u een Jenkins CI-oplossing met de volgende techniek uitvoeren:

    1. Download jenkins.war van <http://jenkins-ci.org>op een computer Java is ingeschakeld.
    2. Bij een opdrachtprompt die wordt geopend naar de map waarin jenkins.war uitvoeren:

        `java -jar jenkins.war`

    3. Open in uw browser `http://localhost:8080/`. Hiermee opent u het dashboard Jenkins, die u gebruiken wilt voor het installeren en configureren van de invoegtoepassing voor de opslag van Azure.

        Terwijl een typisch Jenkins CI-oplossing zou worden ingesteld om uit te voeren als een service, de war Jenkins uitgevoerd op de opdrachtregel worden voldoende zijn voor deze zelfstudie.

- Een Azure-account. U kunt zich aanmelden voor een Azure-account aan <http://www.azure.com>.

- Een account Azure opslag. Als u niet al een opslag-account hebt, kunt u één behulp van de stappen in [een opslag-Account maken](storage-create-storage-account.md#create-a-storage-account).

- Bekend zijn met de oplossing Jenkins CI wordt aanbevolen maar niet verplicht, omdat voor de volgende inhoud wordt een voorbeeld van de eenvoudige worden gebruikt om aan te geven de stappen die nodig zijn bij het gebruik van de service Blob als opslagplaats voor Jenkins CI bouwen onderdelen.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Het gebruik van de service Blob met Jenkins CI ##

Als u wilt de service Blob Jenkins gebruikt, moet u de opslag van Azure-invoegtoepassing te installeren, configureren van de invoegtoepassing voor gebruik van uw account opslag en maakt u een actie na het samenstellen die uw onderdelen opbouwen naar uw account opslag uploadt. Volgende stappen uit worden in de volgende secties beschreven.

## <a name="how-to-install-the-azure-storage-plugin"></a>Het installeren van de invoegtoepassing voor de opslag van Azure ##

1. Klik in het dashboard Jenkins op **Jenkins beheren**.
2. Klik op de pagina **Jenkins beheren** op **Invoegtoepassingen beheren**.
3. Klik op het tabblad **beschikbaar** .
4. Schakel in de sectie **Onderdeel Uploaders** **-invoegtoepassing van Microsoft Azure Storage**.
5. Klik op **installeren zonder opnieuw starten** of moet **nu downloaden en installeren na opnieuw starten**.
6. Start opnieuw Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Het configureren van de Azure Storage-invoegtoepassing voor gebruik van uw account opslag ##

1. Klik in het dashboard Jenkins op **Jenkins beheren**.
2. Klik op **Systeem configureren**op de pagina **Jenkins beheren** .
3. In de sectie **Configuratie van Microsoft Azure opslag-Account** :
    1. Voer uw opslagaccountnaam, die u van de [Azure-Portal ontvangt kunt](https://portal.azure.com).
    2. Voer uw accountsleutel opslag, dat u ook kan worden verkregen uit de [Azure-Portal](https://portal.azure.com).
    3. Gebruik de standaardwaarde voor **Blob Service eindpunt URL** als u de openbare Azure cloud gebruikt. Als u een ander Azure cloud gebruikt, gebruikt u het eindpunt als opgegeven in de [Portal van Azure](https://portal.azure.com) voor uw account opslag. 
    4. Klik op **valideren opslag referenties** valideren van uw account opslag. 
    5. [Optionele] Als u extra opslagruimte accounts die u wilt dat aan uw CI Jenkins ter beschikking gesteld hebt, klikt u op **meer opslagruimte Accounts toevoegen**.
    6. Klik op **Opslaan** als uw instellingen wilt opslaan.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Het maken van een na het samenstellen actie die de onderdelen van uw opbouwen naar uw account opslag uploadt ##

Voor instructieset doeleinden, eerst moet we maken van een taak die meerdere bestanden kunt maken, en vervolgens in de actie na het samenstellen de bestanden uploaden naar uw opslag-account toevoegen.

1. Klik op **Nieuw Item**binnen het dashboard Jenkins.
2. De taak **MyJob**een naam, klikt u op **een vrije stijl van een software-project bouwen**en klik vervolgens op **OK**.
3. Klik in de sectie **maken** van de taakconfiguratie op **toevoegen bouwen stap** en kies **uitvoeren Windows batch opdracht**.
4. In **de opdracht**, gebruikt u de volgende opdrachten:

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. In de sectie **Acties na het samenstellen** van de taakconfiguratie en klikt u op **toevoegen na het samenstellen actie** en kies **onderdelen met Azure-blobopslag uploaden**.
6. Voor **opslagaccountnaam**, selecteert u de rekening opslagruimte wilt gebruiken.
7. Voor **de naam van de Container**, geef de containernaam van de. (De container wordt gemaakt als deze niet bestaat nog wanneer de onderdelen van de build worden geüpload.) U kunt omgevingsvariabelen gebruiken, zodat dit voorbeeld **${JOB_NAME}** opgeven als de containernaam van de.

    **Tip**
    
    Onder de **opdracht** is sectie waar u een script hebt ingevoerd voor **Windows uitvoeren batch opdracht** een koppeling naar de omgevingsvariabelen herkend door Jenkins. Klik op deze koppeling voor meer informatie over de omgeving variabele namen en beschrijvingen. Houd er rekening mee dat omgevingsvariabelen met speciale tekens, zoals de omgevingsvariabele **BUILD_URL** niet als een containernaam of het algemene virtuele pad zijn toegestaan.

8. Klik op **nieuwe container standaard openbaar maken** voor dit voorbeeld. (Als u een privé-container gebruikt wilt, u moet een handtekening gedeelde toegang om toegang te verlenen maken. Die valt buiten het bereik van dit onderwerp. U kunt meer informatie over gedeelde toegang handtekeningen bij [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Optionele] Klik op **wissen.Control container voordat u uploadt** als u wilt dat de container inhoudsopgave worden gewist voordat opbouwen onderdelen worden geüpload (laat het uitgeschakeld als u niet op te schonen van de inhoud van de container wilt).
10. Voer voor een **Lijst van onderdelen uploaden**, * *tekst /*.txt**.
11. Voor **algemene virtuele pad voor geüploade onderdelen**, voor toepassing van deze zelfstudie, voert u **${bouwen\_ID} / ${bouwen\_getal}**.
12. Klik op **Opslaan** als uw instellingen wilt opslaan.
13. In het dashboard Jenkins, klik op **Nu maken** om uit te voeren **MyJob**. Bekijk de console-uitvoer voor status. Statusberichten voor Azure opslag worden opgenomen in de console-uitvoer zodra de actie na het samenstellen begint te uploaden opbouwen onderdelen.
14. Nadat de taak is voltooid, kunt u de onderdelen van de build bestuderen met het openen van de openbare blob.
    1. Meld u bij de [Portal van Azure](https://portal.azure.com).
    2. Klik op **opslag**.
    3. Klik op de naam van het opslag-account die u voor Jenkins gebruikt.
    4. Klik op **Containers**.
    5. Klik op de container met de naam **myjob**, namelijk de kleine letters versie van de naam van de taak die aan u toegewezen wanneer u de taak Jenkins gemaakt. Namen van de container en blob zijn kleine letters (en hoofdlettergevoelig) in Azure opslag. In de lijst met BLOB's voor de container met de naam **myjob** ziet u **hello.txt** en **date.txt**. Kopieer de URL voor een van deze items en deze in uw browser openen. Als een onderdeel opbouwen ziet u het tekstbestand dat is geüpload.

Slechts één na het samenstellen actie die onderdelen naar Azure-blobopslag uploadt kan worden gemaakt per taak. Houd er rekening mee dat de actie na het samenstellen onderdelen uploaden naar Azure-blobopslag opgeven kunt verschillende bestanden (inclusief jokertekens) en paden naar bestanden in de **Lijst van onderdelen te uploaden** met een puntkomma als scheidingsteken. Bijvoorbeeld, als uw opbouwen Jenkins oppervlak-bestanden en TXT-bestanden in map die van uw werkruimte **maken krijgt** en u wilt uploaden op Azure-blobopslag, gebruikt u de volgende voor de **Lijst van onderdelen uploaden** -waarde: **bouwen /\*.jar; opbouwen /\*.txt**. U kunt ook de syntaxis van de dubbele gebruiken om een pad vanuit de naam van de blob gebruiken. Bijvoorbeeld desgewenst kunt u de potten om toegang te krijgen geüpload met **binaire bestanden** in het blob-pad en de TXT-bestanden ophalen die zijn geüpload, **Aankondigingen** gebruiken in het pad blob gebruiken in de volgende handelingen uit voor de **Lijst van onderdelen uploaden** -waarde: **bouwen /\*. jar::binaries; opbouwen /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Het maken van een opbouwen-stap die u van Azure-blobopslag downloadt ##

De volgende stappen hoe een stap opbouwen items downloaden van Azure-blobopslag configureren. Dit is handig als u opnemen van items in uw opbouwen wilt, bijvoorbeeld potten die u in Azure bijhoudt blob storage.

1. Klik in de sectie **maken** van de taakconfiguratie op **toevoegen bouwen stap** en kies **downloaden van Azure-blobopslag**.
2. Voor **opslagaccountnaam**, selecteert u de rekening opslagruimte wilt gebruiken.
3. Voor **de naam van de Container**, geef de naam van de container met de BLOB's die u wilt downloaden. U kunt omgevingsvariabelen gebruiken.
4. Geef de blob-naam voor **de naam van de Blob**. U kunt omgevingsvariabelen gebruiken. U kunt ook een sterretje, het als een jokerteken nadat u de eerste letter (s) van de naam van de blob hebt opgegeven. Bijvoorbeeld **project\* ** alle BLOB's waarvan de naam met **project begint**wilt opgeven.
5. [Optionele] Voor het **downloaden van pad**, geef het pad op de computer van de Jenkins waar u wilt downloaden van bestanden van Azure-blobopslag. Omgevingsvariabelen kunnen ook worden gebruikt. (Als u een waarde voor het **downloaden van pad**niet opgeeft, de bestanden van Azure-blobopslag gedownload naar de werkruimte van de taak.)

Als u andere items die u wilt downloaden van Azure-blobopslag hebt, kunt u aanvullende opbouwen stappen.

Nadat u een opbouwen hebt uitgevoerd, kunt u neemt de opbouwen geschiedenis console-uitvoer of kijkt u naar uw downloadlocatie om te zien of de BLOB's die u denkt dat zijn gedownload.  

## <a name="components-used-by-the-blob-service"></a>Onderdelen die worden gebruikt door de service Blob ##

Hieronder vindt u een overzicht van de onderdelen van de Blob-service.

- **Opslag-Account**: alle toegang tot Azure Storage vindt plaats via een opslag-account. Dit is het hoogste niveau van de naamruimte voor toegang tot BLOB's. Een account kan een onbeperkt aantal containers, bevatten, zolang de totale grootte onder 100TB.
- **Container**: een container biedt een groepering van een set BLOB's. Alle BLOB's moeten zich in een container. Een account kan een onbeperkt aantal containers bevatten. Een onbeperkt aantal BLOB's kan worden opgeslagen in een container.
- **BLOB**: een bestand van elk type en het formaat. Er zijn twee soorten BLOB's die kunnen worden opgeslagen in Azure Storage: BLOB van blokkeren en pagina's. De meeste bestanden zijn blok BLOB's. Een blob één blok mag maximaal 200GB groot zijn. In deze zelfstudie wordt blok BLOB's. Pagina BLOB's, een ander type blob, mag maximaal 1TB in de grootte en efficiënter zijn als bereiken van bytes in een bestand vaak worden gewijzigd. Zie voor meer informatie over BLOB's [lidmaatschap blok BLOB's, BLOB's toevoegen en BLOB van pagina's](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **URL-indeling**: BLOB's worden gebruikt met de volgende URL-notatie:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    (De bovenstaande notatie geldt voor de openbare Azure cloud. Als u een ander Azure cloud gebruikt, gebruikt het eindpunt van de [Azure-Portal](https://portal.azure.com) om te bepalen de URL-eindpunt.)

    In de bovenstaande, indeling `storageaccount` staat voor de naam van uw account opslag, `container_name` staat voor de naam van de container, en `blob_name` respectievelijk de naam van uw blob, vertegenwoordigt. U kunt meerdere paden, gescheiden door een slash, hebben binnen de containernaam van de **/**. De naam van de container voorbeeld in deze zelfstudie is **MyJob**, en **${bouwen\_ID} / ${bouwen\_getal}** is gebruikt voor het algemene virtuele pad, resulteert in de blob ondervindt met de volgende URL:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Volgende stappen

- [Jenkins vergaderen](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
- [Azure opslag SDK for Java](https://github.com/azure/azure-storage-java)
- [Azure opslag Client SDK verwijzing](http://dl.windowsazure.com/storage/javadoc/)
- [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

Zie ook [Java Developer Center](https://azure.microsoft.com/develop/java/)voor meer informatie.