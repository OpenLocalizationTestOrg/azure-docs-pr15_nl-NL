<properties
   pageTitle="Bladeren en opslagbronnen beheren met Server Explorer | Microsoft Azure"
   description="Bladeren en opslagbronnen beheren met Server Explorer"
   services="visual-studio-online"
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
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Bladeren en beheren van opslag Resources met Server Verkenner

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht
Als u de hulpmiddelen Azure voor Microsoft Visual Studio hebt geïnstalleerd, kunt u blob, wachtrij en gegevens in een tabel uit uw opslag-accounts weergeven voor Azure. Het knooppunt Azure-opslag in Server Explorer ziet u gegevens in uw lokale opslag emulator-account en uw andere Azure opslag-accounts.

Als u wilt bekijken Server Explorer Visual Studio, op de menubalk, kiest u **weergave**, **Server Explorer**. Het Opslagknooppunt ziet u alle van de opslag-accounts die bestaan onder elk Azure abonnement/certificaat waarmee u bent verbonden. Als uw account opslag niet wordt weergegeven, kunt u deze toevoegen volgens de instructies [verderop in dit onderwerp](#add-storage-accounts-by-using-server-explorer).

Begin in Azure SDK 2.7 en kunt u ook gebruiken het nieuwe Cloud Explorer weergeven en beheren van uw Azure resources. Zie [Azure-bronnen beheren met Cloud Explorer](./vs-azure-tools-resources-managing-with-cloud-explorer.md) voor meer informatie.


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Weergeven en beheren van opslagbronnen in Visual Studio

Een lijst met BLOB's, wachtrijen en tabellen weergegeven Server Explorer automatisch in uw opslagruimte emulator-account. De opslagruimte emulator-account wordt vermeld in Server Explorer onder het knooppunt opslag als het knooppunt **ontwikkeling** .

Vouw de opslagruimte emulator-account als resources wilt weergeven, de **ontwikkeling** uit. Als de opslagruimte emulator nog niet is gestart wanneer u het knooppunt **ontwikkeling** uitvouwen, wordt deze automatisch gestart. Dit kan enkele seconden duren. U kunt blijven voor gebruik in andere gebieden van Visual Studio, terwijl de emulator opslag wordt gestart.

Als u wilt bekijken resources in een opslag-account, vouw van de opslag-account in Server Explorer. De volgende onderliggende knooppunten worden weergegeven:

- BLOB 's

- Wachtrijen

- Tabellen

## <a name="work-with-blob-resources"></a>Werken met Blob Resources

Het knooppunt BLOB's bevat een overzicht van containers voor het geselecteerde opslag-account. BLOB containers blob-bestanden bevatten en u kunt deze BLOB's indelen in mappen en submappen. Lees [hoe u Blob Storage van .NET gebruiken](./storage/storage-dotnet-how-to-use-blobs.md) voor meer informatie.

### <a name="to-create-a-blob-container"></a>Een container blob maken

1. Open het snelmenu voor het knooppunt **BLOB's** , en kies vervolgens **Blob Container maken**.

1. Voer de naam van de nieuwe container in het dialoogvenster **Blob Container maken** en kies vervolgens **Ok**

    >[AZURE.NOTE] De naam van de container blob moet beginnen met een cijfer (0-9) of kleine letter (a-z).

### <a name="to-delete-a-blob-container"></a>Een container blob

- Open het snelmenu voor de blob container die u wilt verwijderen en kies vervolgens **verwijderen**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Een lijst van de items in een container blob weergeven

- Open het menu bewerken voor een naam van de container blob in de lijst en kies vervolgens **Weergave Blob Container**.

    Wanneer u de inhoud van een container blob bekijkt, ziet u op een tabblad bekend als de weergave van de container blob.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    U kunt de volgende bewerkingen uit op BLOB's uitvoeren met behulp van de knoppen in de rechterbovenhoek van de weergave van de container blob:

    - Voer een filterwaarde en toepassen

    - De lijst met BLOB's in de container vernieuwen

    - Een bestand uploaden

    - Een blob verwijderen

      >[AZURE.NOTE] Een bestand verwijderen uit een container blob verwijderen niet als het onderliggende bestand. deze alleen verwijderd uit de container blob.

    - Open een blob

    - Een blob opslaan in de lokale computer

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Een map of submap maken in een container blob

1. Kies de container blob in Server Explorer. Kies de knop **Uploaden Blob** in het venster container.

    ![Een bestand uploaden naar een map blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. Kies de knop **Bladeren** om op te geven van het bestand dat u wilt uploaden en klik vervolgens in het vak **map (optioneel)** Voer de naam van een map in het dialoogvenster **Nieuwe bestand uploaden** .

    U kunt submappen in de container mappen toevoegen door dezelfde procedure te volgen. Als u de naam van een map niet opgeeft, wordt het bestand worden geüpload naar het bovenste niveau van de container blob. Het bestand wordt weergegeven in de opgegeven map in de container.

    ![Map toevoegen aan een container blob](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Dubbelklik op de map of druk op ENTER om de inhoud van de map weer te geven. Wanneer u aan de map van de container deelneemt, kunt u Ga terug naar de hoofdsite van de container met de knop **Openen bovenliggende map** (pijl-omhoog).

### <a name="to-delete-a-container-folder"></a>Een container-map verwijderen

 - Alle bestanden in de map verwijderen

    >[AZURE.NOTE] Omdat mappen in blob containers virtuele mappen, niet kunt u een lege map maken of kunt u een map als de bestandsinhoud wilt verwijderen verwijdert. U moet de volledige inhoud van een map voor het verwijderen van de map verwijderen.

### <a name="to-filter-blobs-in-a-container"></a>Om te filteren BLOB's in een container

U kunt de BLOB's die worden weergegeven door op te geven van een algemeen voorvoegsel filteren.

Als u het voorvoegsel invoeren bijvoorbeeld `hello` in de tekst van het filter en kies de **uitvoeren** (**!**) knop, alleen de BLOB's die met 'Hallo beginnen' weergegeven.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Het filterveld is hoofdlettergevoelig en biedt geen ondersteuning voor filteren met jokertekens. BLOB's kunnen alleen worden gefilterd op voorvoegsel. Het voorvoegsel mogelijk een scheidingsteken opnemen als u een scheidingsteken ordenen BLOB's in een virtuele hiërarchie gebruikt. Bijvoorbeeld filteren op het voorvoegsel HelloFabric / geeft als resultaat alle BLOB's beginnen met die tekenreeks.

### <a name="to-download-blob-data"></a>Blobgegevens moeten worden gedownload

- In **Server Explorer**opent u het snelmenu voor een of meer BLOB's en kiest u **openen**, of kies de naam blob en vervolgens klikt u op de knop **openen** , of dubbelklik op de naam blob.

    De voortgang van het downloaden van een blob wordt weergegeven in het venster **Azure activiteitenlogboek** .

    De blob wordt geopend in de standaardeditor voor dat bestandstype. Als het besturingssysteem het bestandstype herkent, het bestand is geopend in een lokaal geïnstalleerde toepassing niet omzeilen. anders wordt u gevraagd om te kiezen van een toepassing die geschikt is voor het bestandstype van de blob. Het lokale bestand dat wordt gemaakt wanneer u een blob downloaden wordt gemarkeerd als alleen-lezen.

    BLOB-gegevens is lokaal opgeslagen in de cache en gecontroleerd aan de hand van de blob laatst gewijzigd in de Blob-service. Als de blob is bijgewerkt nadat het laatst is gedownload, wordt het gedownload opnieuw; anders worden de blob vanaf de lokale schijf geladen. Standaard wordt een blob gedownload naar een tijdelijke map. Als u wilt downloaden BLOB's naar een bepaalde map, open het snelmenu voor de geselecteerde blob namen en kies **OpslaanAls**. Als u een blob op deze manier opslaat, het blob-bestand niet is geopend en het lokale bestand wordt gemaakt met de kenmerken van de alleen-lezen-schrijven.

### <a name="to-upload-blobs"></a>BLOB's uploaden

- Kies de knop **Uploaden Blob** als de container geopend voor weergave in de weergave van de container blob is.

    U kunt een of meer bestanden uploaden en u kunt bestanden van elk type uploaden. Het **Gebeurtenissenlogboek van Azure** ziet u de voortgang van het uploaden. Zie voor meer informatie over het werken met blob-gegevens, [het gebruik van de Azure Blob Storage-Service in .NET](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Logboeken overgebracht naar BLOB's weergeven

- Als u diagnostische gegevens van Azure aan te melden gegevens uit uw Azure toepassing gebruikt en u de logboeken naar uw opslag-account hebt overgebracht, ziet u containers die zijn gemaakt door Azure voor deze logboeken. Deze logboeken weergeven in een Server Explorer is een eenvoudige manier om aan te geven van problemen met uw-toepassing, met name als deze wordt is geïmplementeerd op Azure. Zie voor meer informatie over Azure diagnostische gegevens [Vastleggen gegevens door gebruik van Azure diagnostische gegevens verzamelen](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Om de URL voor een blob

- Snelmenu van de blob openen en kies vervolgens **De URL kopiëren**.

### <a name="to-edit-a-blob"></a>Een blob bewerken

- Selecteer de label en kies vervolgens de knop **Openen Blob** .

    Het bestand is gedownload naar een tijdelijke locatie en geopend op de lokale computer. Nadat u wijzigingen hebt gemaakt, moet u de blob opnieuw uploaden.

## <a name="work-with-queue-resources"></a>Werken met wachtrij Resources

Opslag services wachtrijen worden gehost in een Azure opslag-account en kunt u ze toe te staan dat uw cloud service rollen om te communiceren met elkaar en met andere services door een bericht om doorgeven. U kunt de wachtrij via programmacode openen via een cloudservice en via een webservice voor externe klanten. U kunt ook toegang tot de wachtrij rechtstreeks via Server Explorer in Visual Studio.

Wanneer u een cloudservice die wordt gebruikt wachtrijen ontwikkelt, wilt u mogelijk het gebruik van Visual Studio wachtrijen maken en werken met hen interactief terwijl u ontwikkelen en testen van uw code.

In Server Explorer kunt u de wachtrijen weergeven in een opslag-account, maken en wachtrijen verwijderen, opent u een wachtrij als de berichten wilt weergeven en berichten toevoegen aan een wachtrij. Wanneer u een wachtrij voor weergave opent, kunt u de afzonderlijke berichten weergeven en u kunt de volgende acties uitvoeren in de wachtrij met behulp van de knoppen in de linkerbovenhoek:

- De weergave van de wachtrij vernieuwen

- Een bericht toevoegen aan de wachtrij

- Het bovenste bericht in wachtrij.

- De hele wachtrij wissen

De volgende afbeelding ziet u een rij met twee berichten.

![Een wachtrij weergeven](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Zie voor meer informatie over opslag services wachtrijen, [hoe: Gebruik de wachtrij Storage-Service](http://go.microsoft.com/fwlink/?LinkID=264702). Zie voor informatie over de webservice voor wachtrijen voor opslag-services, [Wachtrij Service concepten](http://go.microsoft.com/fwlink/?LinkId=264788). Zie voor informatie over het verzenden van berichten naar een opslag services wachtrij met behulp van Visual Studio, [Berichten verzenden naar een wachtrij opslag-Services](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Opslag services wachtrijen zijn service bus wachtrijen. Zie voor meer informatie over de service bus wachtrijen, Service Bus wachtrijen, onderwerpen en abonnementen.

## <a name="work-with-table-resources"></a>Werken met tabel-Resources

Er worden grote hoeveelheden gestructureerde gegevens opgeslagen in de tabel Azure storage-service. De service is een NoSQL gegevensopslag die accepteert geverifieerd oproepen van binnen en buiten de Azure cloud. Azure tabellen zijn geschikt om gestructureerde, niet-relationele gegevens op te slaan.

### <a name="to-create-a-table"></a>Een tabel maken

1. Selecteer het knooppunt **tabellen** van het account opslagruimte in Server Explorer en kies op **Tabel maken**.

1. Voer een naam voor de tabel in het dialoogvenster **Tabel maken** .

### <a name="to-view-table-data"></a>Tabelgegevens moeten worden weergegeven

1. Open het knooppunt **Azure** in Server Explorer en open vervolgens het knooppunt **opslag** .

1. Open het knooppunt voor het account van opslag waarin u geïnteresseerd bent en open vervolgens het knooppunt **tabellen** om een lijst met tabellen voor de opslag-account.

1. Open het snelmenu voor een tabel en kies vervolgens **View-tabel**.

    ![Een Azure-tabel in Solution Explorer](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

De tabel is gerangschikt op entiteiten (weergegeven in rijen) en eigenschappen (weergegeven in kolommen). De volgende afbeelding ziet bijvoorbeeld entiteiten in de **Ontwerpfunctie voor tabellen**weergegeven:

### <a name="to-edit-table-data"></a>Gegevens in een tabel bewerken

1. In de **Ontwerpfunctie voor tabellen**, opent u het snelmenu voor een entiteit (één rij) of een eigenschap (één cel) en kies vervolgens **bewerken**.

    ![Toevoegen of bewerken van een Tabelentiteit](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Entiteiten in één tabel zijn niet verplicht voor dezelfde set met eigenschappen (kolommen). Houd rekening met de volgende beperkingen voor het weergeven en bewerken van gegevens in een tabel.
    - U kunt geen bekijken of bewerken van binaire gegevens (type byte[]), maar u kunt opslaan in een tabel.

    - U kunt de waarden van **PartitionKey** of **RowKey** , niet bewerken, omdat in Azure-tabelopslag die bewerkingen niet ondersteund.

    - U kunt niet een eigenschap met de naam van de tijdstempel maken, Azure Storage services gebruiken een eigenschap met die naam.

    - Als u een DateTime-waarde invoert, moet u een indeling die geschikt zijn voor de instellingen van het land en taal van uw computer is volgen (bijvoorbeeld DD-MM-jjjj: mm: ss [AM | PM] voor Amerikaans Engels).

### <a name="to-add-entities"></a>Entiteiten toevoegen

1. Kies in de **Ontwerpfunctie voor tabellen**, de knop **Toevoegen entiteit** , waarop zich dicht bij de rechterbovenhoek van de tabelweergave.

    ![Entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. Voer in het dialoogvenster **Entiteit toevoegen** de waarden van de eigenschappen **PartitionKey** en **RowKey** .

    ![Dialoogvenster entiteit toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Voer de waarden zorgvuldig omdat u deze niet meer wijzigen nadat u het dialoogvenster sluiten, tenzij u de entiteit verwijderen en opnieuw toe te voegen.

### <a name="to-filter-entities"></a>Om te filteren entiteiten

De set entiteiten die worden weergegeven in een tabel als u de opbouwfunctie voor query's gebruikt, kunt u aanpassen.

1. U opent de opbouwfunctie voor query's door een tabel voor weergave te openen.

1. Kies de meest rechtse knop op de werkbalk van de tabelweergave.

    Het dialoogvenster **Opbouwfunctie voor Query's** wordt weergegeven. De volgende afbeelding ziet een query die wordt samengesteld in de opbouwfunctie voor query's.

    ![Opbouwfunctie voor query 's](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Als u klaar bent met het samenstellen van de query, sluit het dialoogvenster. De resulterende tekstvorm van de query wordt weergegeven in een tekstvak als filter WCF Data Services.

1. Als u wilt de query uitvoert, kiest u het groene driehoekje.

    U kunt ook entiteitsgegevens die wordt weergegeven in de **Ontwerpfunctie voor tabellen** als u een tekenreeks WCF Data Services filter rechtstreeks in het filterveld filteren. Dit soort tekenreeks is vergelijkbaar met een SQL WHERE-component, maar wordt verzonden naar de server als een HTTP-aanvraag. Zie voor informatie over het maken van tekenreeksen [Het bouwen van tekenreeksen voor de ontwerpfunctie voor tabellen](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    De volgende afbeelding ziet u een voorbeeld van een tekenreeks geldig filter:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Opslaggegevens vernieuwen

Wanneer Server Explorer verbinding maakt of formule gegevens worden opgehaald uit een opslag-account, kan het duren minuten voor de bewerking is voltooid. Als deze geen verbinding kunt maken, kan de bewerking time-out. Terwijl de gegevens zijn opgehaald, kunt u eraan werkt in andere delen van Visual Studio. Als u wilt de bewerking te annuleren als het te lang duurt, kies de knop **Vernieuwen stoppen** op de werkbalk van de Server Explorer.

#### <a name="to-refresh-blob-container-data"></a>Blob container gegevens vernieuwen

- Selecteer het knooppunt **BLOB's** onder **opslag** en klikt u op de knop **vernieuwen** op de werkbalk Server Explorer.

- Als u wilt vernieuwen van de lijst met BLOB's dat wordt weergegeven, klikt u op de knop **uitvoeren** .

#### <a name="to-refresh-table-data"></a>Vernieuwen van gegevens in een tabel

- Selecteer het knooppunt **tabellen** onder **opslag** en klikt u op de knop **vernieuwen** .

- Als u wilt vernieuwen van de lijst met entiteiten die wordt weergegeven in de **Ontwerpfunctie voor tabellen**, kies de knop **uitvoeren** op de **Ontwerpfunctie voor tabellen**.

#### <a name="to-refresh-queue-data"></a>Wachtrijgegevens vernieuwen

- Selecteer het knooppunt **wachtrijen** en kies vervolgens de knop **vernieuwen** .

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Alle items in een account opslag vernieuwen

- Kies de naam van het account en kies vervolgens de knop **vernieuwen** op de werkbalk voor de Server Explorer.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Opslag-accounts toevoegen met behulp van de Server Explorer

Er zijn twee manieren opslag accounts toevoegen met behulp van de Server Explorer. U kunt een nieuw account voor de opslag maken in uw Azure-abonnement of u kunt een bestaand opslag-account koppelen.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Een nieuw opslag-account maken met behulp van Server Explorer

1. In Server Explorer, opent u het snelmenu voor het knooppunt opslag, en kies vervolgens opslag-Account maken.

    ![Een nieuw Azure opslag-account maken](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Selecteer of Voer de volgende gegevens voor de nieuwe opslag-account in het dialoogvenster **Opslag-Account maken** .

    - Het Azure abonnement waarnaar u wilt het opslag-account toevoegen.

    - De naam die u wilt gebruiken voor de nieuwe opslag-account.

    - De regio of affiniteit groep (zoals West Amerikaans of Oost-Azië).

    - Het type herhaling die u wilt gebruiken voor de opslag-account, zoals geografische-redundante.

1. Kies **maken**.

    Het nieuwe opslag-account wordt weergegeven in de lijst **opslagruimte** in Solution Explorer.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Een bestaand opslag-account met behulp van de Server Explorer bijvoegen

1. In Server Explorer, opent u het snelmenu voor het Opslagknooppunt Azure, en kies vervolgens **Externe opslag als bijlage toevoegen**.

    ![Een bestaand opslag-account toevoegen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Selecteer of Voer de volgende gegevens voor de nieuwe opslag-account in het dialoogvenster **Opslag-Account maken** .

    - De naam van de bestaande opslag-account dat u wilt koppelen. U kunt Voer een naam of selecteert u deze in de lijst.

    - De sleutel voor het geselecteerde opslag-account. Deze waarde wordt meestal opgegeven wanneer u een account opslag selecteert. Indien Visual Studio gewenst om de opslag accountsleutel onthouden, selecteert u het selectievakje onthouden account key.

    - Het protocol gebruiken om te verbinden met de opslag-account, zoals HTTP, HTTPS of een aangepaste-eindpunt. Lees [hoe u verbindingstekenreeksen configureren](https://msdn.microsoft.com/library/azure/ee758697.aspx) voor meer informatie over aangepaste eindpunten.

### <a name="to-view-the-secondary-endpoints"></a>De secundaire eindpunten weergeven

- Als u een met de **Leestoegang geografische overtollige** replicatie-optie opslag-account hebt gemaakt, kunt u de secundaire eindpunten weergeven. Open het snelmenu voor de accountnaam en kies vervolgens **Eigenschappen**.

    ![Secundaire eindpunten opslag](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Een account opslag uit Server Explorer verwijderen

- In Server Explorer, opent u het snelmenu voor de accountnaam en kies vervolgens **verwijderen**. Als u een account opslag verwijdert, wordt er ook voor dat account, kunt u geen opgeslagen belangrijke gegevens verwijderd.

    >[AZURE.NOTE] Als u een account voor de opslag van Server Explorer verwijdert, deze heeft geen invloed op uw opslag-account of alle gegevens die deze bevat; deze gewoon Hiermee verwijdert u de verwijzing uit Server Explorer. Permanent een opslag als account wilt verwijderen, gebruikt u de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Volgende stappen

Voor meer dat informatie over hoe u bij het gebruik van Azure opslagservices, raadpleegt u [toegang tot de Azure Storage-Services](https://msdn.microsoft.com/library/azure/ee405490.aspx).
