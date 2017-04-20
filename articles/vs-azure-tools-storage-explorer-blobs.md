<properties
    pageTitle="Azure-blobopslag resources met opslag Explorer (Preview) beheren | Microsoft Azure"
    description="Azure Blob Containers en BLOB's met opslag Explorer (Preview) beheren"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Azure-blobopslag resources met opslag Explorer (Preview) beheren

## <a name="overview"></a>Overzicht

[Azure-blobopslag](./storage/storage-dotnet-how-to-use-blobs.md) is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die toegankelijk is vanuit een willekeurige plaats in de wereld via HTTP of HTTPS.
U kunt blobopslag gebruiken om weer te geven van gegevens over de hele wereld openbaar of privé toepassingsgegevens opgeslagen. In dit artikel leert u hoe u opslag Explorer (Preview) om te werken met blob containers en BLOB's gebruiken.

## <a name="prerequisites"></a>Vereisten voor

U voltooit de stappen in dit artikel, hebt u het volgende nodig:

- [Download en installeer opslag Explorer (preview)](http://www.storageexplorer.com)
- [Verbinding maken met een Azure opslag-account of service](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Een container blob maken

Alle BLOB's moeten zich bevinden in een container blob, dat wil gewoon een logische groepering van BLOB's zeggen. Een account kan bevatten een onbeperkt aantal containers en elke container kan een onbeperkt aantal BLOB's opslaan.

De volgende stappen illustreren hoe u een container blob in opslag Explorer (Preview) maakt.

1.  Open Opslagverkenner (Preview).
1.  Vouw in het linkerdeelvenster uit het opslag-account waarin u wilt maken van de container blob.
1.  Met de rechtermuisknop op **Blob Containers**en - selecteer in het contextmenu - **Blob Container maken**.

    ![Contextmenu voor blob containers maken][0]

1.  Een tekstvak wordt weergegeven onder de map **Containers Blob** . Voer de naam voor de container blob. Zie het gedeelte van de [Container naming regels](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) voor een lijst van regels en beperkingen voor aangepaste blob containers.

    ![Blob Containers tekstvak maken][1]

1.  Druk op **Enter** wanneer u klaar bent om te maken van de container blob, of op **Esc** om te annuleren. Zodra de container blob is gemaakt, wordt deze weergegeven onder de map **Containers Blob** voor het geselecteerde opslag-account.

    ![BLOB Container gemaakt][2]

## <a name="view-a-blob-containers-contents"></a>Een container blob inhoud bekijken

BLOB containers bevatten BLOB's en mappen (die u kunnen ook BLOB's bevatten).

De volgende stappen wordt uitgelegd hoe de inhoud van een container blob in opslag Explorer (Preview) bekijken:

1.  Open Opslagverkenner (Preview).
1.  Vouw in het linkerdeelvenster uit de opslag-rekening met de container blob dat u wilt bekijken.
1.  De opslag van de account **Blob Containers**uitvouwen.
1.  Met de rechtermuisknop op de blob container die u wilt bekijken en - selecteer in het contextmenu - **Editor van de Container openen Blob**.
U kunt ook dubbelklikken op de blob container die u wilt bekijken.

    ![Open blob container Access-lint][19]

1.  Het hoofdvenster wordt de inhoud van de container blob weergegeven.

    ![BLOB container-editor][3]

## <a name="delete-a-blob-container"></a>Een container blob verwijderen

BLOB containers kunnen eenvoudig worden gemaakt en zo nodig verwijderd. (Om te zien hoe afzonderlijke BLOB's verwijderen, raadpleegt u de sectie [beheren BLOB's in een container blob](./#managing-blobs-in-a-blob-container).)

De volgende stappen illustreren hoe u een container blob in opslag Explorer (Preview) verwijderen:

1.  Open Opslagverkenner (Preview).
1.  Vouw in het linkerdeelvenster uit de opslag-rekening met de container blob dat u wilt bekijken.
1.  De opslag van de account **Blob Containers**uitvouwen.
1.  Met de rechtermuisknop op de blob container die u wilt verwijderen en - in het contextmenu - Selecteer **verwijderen**.
U kunt ook drukt u op **verwijderen** om te verwijderen van de momenteel geselecteerde blob container.

    ![Contextmenu voor blob container verwijderen][4]

1.  Selecteer **Ja** in het bevestigingsvenster.

    ![Blob Container bevestiging verwijderen][5]

## <a name="copy-a-blob-container"></a>Een container blob kopiëren

Opslag Explorer (Preview) kunt u een container blob naar het Klembord kopiëren en plak dat onderdeel blob in een ander opslag-account. (Om te zien hoe afzonderlijke BLOB's kopiëren, raadpleegt u de sectie [beheren BLOB's in een container blob](./#managing-blobs-in-a-blob-container).)

De volgende stappen illustreren hoe u een container blob van één opslag-account naar een andere kopiëren.

1.  Open Opslagverkenner (Preview).
1.  Vouw in het linkerdeelvenster uit de opslag-rekening met de container blob dat u wilt kopiëren.
1.  De opslag van de account **Blob Containers**uitvouwen.
1.  Met de rechtermuisknop op de blob container die u wilt kopiëren en - selecteer in het contextmenu - **Kopie Blob Container**.

    ![Contextmenu voor blob container kopiëren][6]

1.  Met de rechtermuisknop op het gewenste "doel" opslag account waarin u wilt plakken van de container blob en - selecteer in het contextmenu - **Blob Container plakken**.

    ![Contextmenu voor plakken blob container][7]

## <a name="get-the-sas-for-a-blob-container"></a>De SA's voor een container blob ophalen

Een [gedeelde toegang handtekening (SA's)](./storage/storage-dotnet-shared-access-signature-part-1.md) gedelegeerde toegang geeft tot bronnen in uw account opslag.
Dit betekent dat u een client beperkte machtigingen aan objecten in uw account opslagruimte voor een bepaalde termijn van tijd en met een opgegeven reeks machtigingen, zonder dat u moet het delen van uw account toegangstoetsen kunt verlenen.

De volgende stappen illustreren hoe u een SA's voor een container blob maakt:

1.  Open Opslagverkenner (Preview).
1.  Vouw in het linkerdeelvenster uit de opslagruimte rekening met de blob container waarvoor u een SA's wilt.
1.  De opslag van de account **Blob Containers**uitvouwen.
1.  Met de rechtermuisknop op de gewenste blob container en - selecteer in het contextmenu - **Handtekening met gedeelde toegang krijgen**.

    ![Contextmenu voor SA's ophalen][8]

1.  Klik in het dialoogvenster **Gedeeld Access handtekening** Geef het beleid, de begin-en vervaldatum, de tijdzone en toegangsniveaus die u wilt gebruiken voor de resource.

    ![Opties voor SA 's][9]

1.  Wanneer u klaar bent met de opties SA's, selecteert u **maken**.

1.  Een tweede dialoogvenster van de **Access-handtekening gedeeld** vervolgens waarin de container blob samen met de URL en QueryStrings die u gebruiken kunt voor toegang tot de resource opslag worden weergegeven.
Selecteer **kopiëren** naast de URL die u wilt kopiëren naar het Klembord.

    ![Kopieer de URL's SA 's][10]

1.  Wanneer u klaar bent, selecteert u als volgt te **sluiten**.

## <a name="manage-access-policies-for-a-blob-container"></a>Access-beleid voor een container blob beheren

De volgende stappen wordt uitgelegd hoe beheren (toevoegen en verwijderen) toegang tot beleidsregels voor een container blob:

1.  Open Opslagverkenner (Preview).
1.  Vouw de opslag-account de container blob waarvan u wilt beheren clienttoegangsbeleid met in het linkerdeelvenster.
1.  De opslag van de account **Blob Containers**uitvouwen.
1.  Selecteer de gewenste blob container en - selecteer in het contextmenu - **Clienttoegangsbeleid beheren**.

    ![Contextmenu voor access-beleid beheren][11]

1.  Het dialoogvenster **Clienttoegangsbeleid** wordt een lijst met alle clienttoegangsbeleid al hebt gemaakt voor de geselecteerde blob container.

    ![Opties voor het beleid van Access][12]        

1.  Volg deze stappen afhankelijk van de access-beleid management taak:

    - **Een nieuw-beleid toevoegen** - Selecteer **toevoegen**. Zodra gegenereerd, wordt het dialoogvenster **Clienttoegangsbeleid** het nieuw toegevoegde-beleid (met de standaardinstellingen) weergegeven.
    - **Een access-beleid bewerken** - eventueel wijzigingen doorvoeren, en selecteer **Opslaan**.
    - **Verwijderen van een access-beleid** - Select **verwijderen** naast het access-beleid dat u wilt verwijderen.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Het toegangsniveau van openbare instellen voor een container blob

Elke container blob is standaard ingesteld op "Geen openbare toegang".

De volgende stappen illustreren hoe u een openbare toegangsniveau voor een container blob opgeven.

1.  Open Opslagverkenner (Preview).
1.  Vouw de opslag-account de container blob waarvan u wilt beheren clienttoegangsbeleid met in het linkerdeelvenster.
1.  De opslag van de account **Blob Containers**uitvouwen.
1.  Selecteer de gewenste blob container en - selecteer in het contextmenu - **Instellen openbare toegangsniveau**.

    ![Contextmenu voor openbare access niveau instellen][13]

1.  Geef in het dialoogvenster **Instellen Container openbare toegangsniveau** op het gewenste toegangsniveau.

    ![Openbare niveau toegangsopties instellen][14]

1.  Selecteer **toepassen**.

## <a name="managing-blobs-in-a-blob-container"></a>BLOB's in een container blob beheren

Als u een container blob hebt gemaakt, kunt u een blob uploaden naar dat onderdeel blob, kunt u een blob downloaden naar uw lokale computer, een blob openen op uw lokale computer, en nog veel meer.

De volgende stappen illustreren hoe u beheert de BLOB's (en mappen) in een container blob.

1.  Open Opslagverkenner (Preview).
1.  Vouw in het linkerdeelvenster uit de opslag-rekening met de container blob dat u wilt beheren.
1.  De opslag van de account **Blob Containers**uitvouwen.
1.  Dubbelklik op de blob container die u wilt bekijken.
1.  Het hoofdvenster wordt de inhoud van de container blob weergegeven.

    ![Weergave blob container][3]

1.  Het hoofdvenster wordt de inhoud van de container blob weergegeven.

1.  Volg deze stappen afhankelijk van de taak die u wilt uitvoeren:

    - **Bestanden uploaden naar een container blob**

        1.  Selecteer op de werkbalk van het hoofdvenster **uploaden**en klik vervolgens op **Bestanden uploaden** in het vervolgkeuzemenu.

            ![Menu bestanden uploaden][15]

        1.  Selecteer in het dialoogvenster **bestanden uploaden** , de knop weglatingsteken (**...**) aan de rechterkant van het tekstvak **bestanden** om te selecteren van de bestanden die u wilt uploaden.

            ![Opties voor bestanden uploaden][16]

        1.  Geef het type van **Blob**. [Aan de slag met Azure-blobopslag met .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) beschreven de verschillen tussen de verschillende blob typen.

        1.  Geef desgewenst een doelmap waarin de geselecteerde bestanden worden geüpload. Als de doelmap niet bestaat, kunt u het wordt gemaakt.

        1.  Selecteer **uploaden**.

    - **Een map uploaden naar een container blob**

        1.  Selecteer op de werkbalk van het hoofdvenster **uploaden**en klik vervolgens op **Map uploaden** in het vervolgkeuzemenu.

            ![Map menu uploaden][17]

        1.  Selecteer de knop weglatingsteken (**...**) aan de rechterkant van het tekstvak **map** in de map waarvan u wilt uploaden de inhoud te selecteren in het dialoogvenster **map uploaden** .

            ![Het dialoogvenster Mapopties uploaden][18]

        1.  Geef het type van **Blob**. [Aan de slag met Azure-blobopslag met .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) beschreven de verschillen tussen de verschillende blob typen.

        1.  Geef desgewenst een doelmap waarin de inhoud van de geselecteerde map worden geüpload. Als de doelmap niet bestaat, kunt u het wordt gemaakt.

        1.  Selecteer **uploaden**.

    - **Een blob downloaden naar uw lokale computer**

        1.  Selecteer de blob die u wilt downloaden.

        1.  Selecteer op de werkbalk van het hoofdvenster, **te downloaden**.

        1.  Geef de locatie waar u de blob gedownload en de naam die u wilt geven in het dialoogvenster **opgeven waar de gedownloade blob opslaan** .  

        1.  Selecteer **Opslaan**.

    - **Open een blob op uw lokale computer**

        1.  Selecteer de blob die u wilt openen.

        1.  Klik op de werkbalk van het hoofdvenster, selecteert u **openen**.

        1.  De blob wordt gedownload en geopend met de toepassing die is gekoppeld aan de onderliggende bestandstype van de blob.

    - **Een blob naar het Klembord kopiëren**

        1.  Selecteer de blob die u wilt kopiëren.

        1.  Klik op de werkbalk van het hoofdvenster, selecteer **kopiëren**.

        1.  Navigeer naar een andere blob container in het linkerdeelvenster en dubbelklik erop om deze te bekijken in het hoofdvenster.

        1.  Klik op de werkbalk van het hoofdvenster, selecteer **Plakken** om een kopie van de blob te maken.

    - **Een blob verwijderen**

        1.  Selecteer de blob die u wilt verwijderen.

        1.  Klik op de werkbalk van het hoofdvenster, selecteert u **verwijderen**.

        1.  Selecteer **Ja** in het bevestigingsvenster.

## <a name="next-steps"></a>Volgende stappen

- Bekijk de [meest recente opslag Explorer (Preview) releaseopmerkingen en video's](http://www.storageexplorer.com).
- Informatie over het [maken van toepassingen met Azure BLOB's, tabellen, wachtrijen, en bestanden](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png