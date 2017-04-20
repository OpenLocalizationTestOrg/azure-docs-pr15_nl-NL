<properties
    pageTitle="Aan de slag met Azure-opslag in vijf minuten | Microsoft Azure"
    description="Snel wordt output op Microsoft Azure BLOB's, tabel en wachtrijen met Azure opslag snelle wordt gestart, Visual Studio en de emulator Azure opslag. Voer uw eerste Azure Storage-toepassing in vijf minuten."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Aan de slag met Azure-opslag in vijf minuten

## <a name="overview"></a>Overzicht

Het is eenvoudig aan de slag met Azure opslagmedia ontwikkelen. Deze zelfstudie wordt getoond hoe kom ik aan een toepassing voor de opslag van Azure omhoog en snel aan de slag. Gebruikt u de sjablonen snel aan de slag voor .NET wordt geleverd bij de SDK Azure. Deze snelle wordt gestart, bevatten kant-en-klaar-code die ziet u enkele eenvoudige programming scenario's met Azure opslagmedia.

Meer informatie over de opslag van Azure voordat u de code verdiepen, Zie [Volgende stappen](#next-steps).

## <a name="prerequisites"></a>Vereisten voor

U moet de volgende vereisten voordat u begint:

1. Als u wilt compileren en de toepassing bouwen, moet u een versie van [Visual Studio](https://www.visualstudio.com/) is geïnstalleerd op uw computer.

2. Installeer de meest recente versie van [Azure SDK voor .NET](https://azure.microsoft.com/downloads/). De SDK bevat de projecten van de steekproef Azure QuickStart, de emulator Azure opslag en de [Bibliotheek van Azure opslag Client voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Zorg ervoor dat het er [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) geïnstalleerd op uw computer, als dat nodig is voor de Azure QuickStart steekproef-projecten die we in deze zelfstudie voortaan gebruikt.

    Als u niet zeker welke versie van .NET Framework op uw computer is geïnstalleerd weet, raadpleegt u [hoe: bepalen welke .NET Framework versies zijn geïnstalleerd](https://msdn.microsoft.com/vstudio/hh925568.aspx). Of druk op de knop **Start** of de Windows-toets, typt u **Configuratiescherm**. Klik vervolgens op **programma's** > **programma's en onderdelen**, en bepalen of de .NET Framework 4.5 wordt weergegeven onder de geïnstalleerde programma's.

4. U hebt een Azure-abonnement en een Azure opslag-account nodig.

    - Als u een Azure-abonnement, raadpleegt u de [Gratis proefversie](https://azure.microsoft.com/pricing/free-trial/), [Aankoop opties](https://azure.microsoft.com/pricing/purchase-options/)en [Lid biedt](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, Microsoft Partner Network, en BizSpark en andere Microsoft-programma's).
    - Zie voor informatie over het maken van een opslag-account in Azure wordt aangegeven [hoe u een opslag-account maken](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Uw eerste Azure Storage-toepassing voor Azure-opslag in de cloud uitgevoerd

Nadat u een account hebt, kunt u een eenvoudige Azure Storage-toepassing met een van de steekproef Azure snelle begint projecten in Visual Studio kunt maken. Deze zelfstudie bevat informatie over de projecten steekproef voor de opslag van Azure: **Azure Storage: BLOB's**, **Azure Storage: bestanden**, **Azure Storage: wachtrijen**, en **Azure Storage: tabellen**:

1. Visual Studio starten.
2. Klik in het menu **bestand** op **Nieuw Project**.
3. Klik in het dialoogvenster **Nieuw Project** op **geïnstalleerde** > **sjablonen** > **Visual C#** > **Cloud** > **QuickStart** > **Gegevensservices**.
    een. Kies een van de volgende sjablonen: **Azure Storage: BLOB's**, **Azure Storage: bestanden**, **Azure Storage: wachtrijen**, of **Azure Storage: tabellen**.
    b. Zorg ervoor dat **.NET Framework 4.5** is geselecteerd als het doel-kader.
    - 3.c. Geef een naam voor uw project en maak de nieuwe Visual Studio-oplossing, zoals wordt weergegeven:

    ![Azure snel aan de slag][Image1]

U kunt de broncode bekijken voordat u de toepassing uitvoert. Als u wilt bekijken van de code, selecteert u **Solution Explorer** op het menu **Beeld** in Visual Studio. Klik vervolgens Dubbelklik op het bestand Program.cs.

Volgende, voer de voorbeeldtoepassing:

1.  Selecteer in Visual Studio **Solution Explorer** in het menu **Beeld** . Open het bestand App.config en de verbindingsreeks commentaar voor de opslag van Azure-emulator:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Verwijder de opmerkingen bij de verbindingsreeks voor de Azure Storage-Service en geef de opslagruimte naam en access accountsleutel in het bestand App.config:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Als u wilt ophalen de toegangstoets van uw opslag-account, raadpleegt u [uw toegangstoetsen opslag beheren](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Nadat u de naam van de account opslag opgeven en toegangstoets in het bestand App.config, klikt u in het menu **bestand** , klikt u op **Alles opslaan** om op te slaan alle projectbestanden.
4.  Klik op **Oplossing maken**in het menu **maken** .
5.  Druk op **F11** wilt uitvoeren van de oplossing stap voor stap of druk op **F5** om uit te voeren van de oplossing in het menu **Foutopsporing** .


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Uw eerste Azure Storage-toepassing voor de Azure opslag Emulator lokaal uitgevoerd

De [Azure opslag Emulator](storage-use-emulator.md) biedt een lokale omgeving die emuleert van de Azure Blob, wachtrij en tabel services om software te ontwikkelen. U kunt de opslagruimte emulator testen uw toepassing opslag lokaal, zonder te maken van een Azure-abonnement of opslag-account en zonder dat van toepassing kosten.

Als u wilt proberen, laten we een eenvoudige Azure Storage-toepassing met een van de steekproef Azure snelle begint projecten in Visual Studio maken. Deze zelfstudie is gericht aan de steekproef **Azure-blobopslag**, **Azure Table Storage**en **Azure wachtrij Storage** projecten:

1. Visual Studio starten.
2. Klik in het menu **bestand** op **Nieuw Project**.
3. Klik in het dialoogvenster **Nieuw Project** op **geïnstalleerde** > **sjablonen** > **Visual C#** > **Cloud** > **QuickStart** > **Gegevensservices**.
    een. Kies een van de volgende sjablonen: **Azure Storage: BLOB's**, **Azure Storage: bestanden**, **Azure Storage: wachtrijen**, of **Azure Storage: tabellen**.
    b. Zorg ervoor dat **.NET Framework 4.5** is geselecteerd als het doel-kader.
    c. Geef een naam voor uw project en maak de nieuwe Visual Studio-oplossing, zoals wordt weergegeven:

    ![Azure snel aan de slag][Image1]

4.  Selecteer in Visual Studio **Solution Explorer** in het menu **Beeld** . Open het bestand App.config en de opmerking uit de verbindingsreeks voor uw account Azure opslag als u deze al hebt toegevoegd. Verwijder vervolgens de opmerkingen bij de verbindingsreeks voor de opslag van Azure-emulator:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

U kunt de broncode bekijken voordat u de toepassing uitvoert. Als u wilt bekijken van de code, selecteert u **Solution Explorer** op het menu **Beeld** in Visual Studio. Klik vervolgens Dubbelklik op het bestand Program.cs.

Voer de voorbeeldtoepassing in de Emulator Azure-opslag:

1.  Druk op de knop **Start** of de Windows-toets, zoek naar *Microsoft Azure Storage emulator*, en start de toepassing. Wanneer de emulator wordt gestart, ziet u een pictogram en een melding in het gebied taakweergave van Windows.
2.  Visual Studio, klikt u op de **Oplossing maken** in het menu **maken** .
3.  Druk op **F11** om uit te voeren van de oplossing stap voor stap in het menu **Foutopsporing** of druk op **F5** om uit te voeren van de oplossing van begin tot einde.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over Azure Storage:

* [Inleiding tot Microsoft Azure-opslag](storage-introduction.md)
* [Aan de slag met Azure opslag Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md)
* [Aan de slag met Azure-tabelopslag met .NET](storage-dotnet-how-to-use-tables.md)
* [Aan de slag met Azure wachtrij opslagruimte met .NET](storage-dotnet-how-to-use-queues.md)
* [Aan de slag met Azure bestandsopslag in Windows](storage-dotnet-how-to-use-files.md)
* [Gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md)
* [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure opslag clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure opslagservices REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
