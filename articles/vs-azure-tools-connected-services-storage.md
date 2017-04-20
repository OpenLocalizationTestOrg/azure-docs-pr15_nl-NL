<properties 
   pageTitle="Azure-opslag toevoegen met behulp van verbonden Services in Visual Studio | Microsoft Azure"
   description="Azure opslagruimte aan uw app toevoegen met behulp van het dialoogvenster Visual Studio verbonden Services toevoegen"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Azure opslag toevoegen met behulp van Visual Studio verbonden Services

## <a name="overview"></a>Overzicht

Met Visual Studio-2015 verlengt, kunt u een C#-cloudservice, mobiele .NET backend-service, ASP.NET-website of service, ASP.NET-5-service of WebJob Azure-service met Azure Storage met behulp van het dialoogvenster **Verbonden Services toevoegen** . De functionaliteit voor verbonden service telt alle benodigde verwijzingen en verbinding code, en uw configuratiebestanden correct wijzigt. Het dialoogvenster gaat u ook naar documentatie ziet u wat zijn de volgende stappen om te beginnen blobopslag, wachtrijen en tabellen.

## <a name="supported-project-types"></a>Ondersteunde projecttypen

Verbinding maken met Azure-opslag in de volgende projecttypen kunt u het dialoogvenster verbonden Services.

- ASP.NET-webpagina projecten

- ASP.NET 5 projecten

- .NET cloud Service Webrol- en werknemer rol projecten

- .NET mobiele Services-projecten

- Azure WebJob projecten


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Verbinding maken met behulp van het dialoogvenster verbonden Services Azure-Storage

1. Zorg ervoor dat u een Azure-account hebt. Als u niet een Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](http://go.microsoft.com/fwlink/?LinkId=518146). Nadat u een Azure-account hebt, kunt u opslagruimte-accounts maken, mobiele services maken en configureren van Azure Active Directory.

1. Uw project openen in Visual Studio, het snelmenu voor het knooppunt **verwijzingen** in Solution Explorer openen en kies vervolgens **Verbonden Service toevoegen**.

    ![Het toevoegen van een verbonden service](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. Kies **Azure Storage**in het dialoogvenster **Verbonden Service toevoegen** en kies vervolgens de knop **configureren** . Mogelijk wordt u gevraagd aan te melden bij Azure als u dat nog niet had gedaan.

    ![Het dialoogvenster verbonden Service - opslag toevoegen](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. Klik in het dialoogvenster **Azure Storage** selecteert u een bestaand account voor de opslag en selecteer **toevoegen**.

    Als u nodig hebt om een nieuwe opslag-account te maken, gaat u naar de volgende stap. Ga anders verder met stap 6.

    ![Azure opslag-dialoogvenster](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Een nieuw account voor de opslag maken: 

    1. Kies de knop **maken een nieuw Account voor de opslag** onderaan in het dialoogvenster Azure Storage.

    1. Het dialoogvenster **Opslag-Account maken** invullen en kies vervolgens de knop **maken** .
    
        ![Azure opslag-dialoogvenster](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Wanneer u terug in het dialoogvenster **Azure opslag bent** , wordt de nieuwe opslag weergegeven in de lijst.

    1. Selecteer de nieuwe opslag in de lijst en selecteer **toevoegen**.

1. De verbonden storage-service wordt weergegeven onder het knooppunt Serviceverwijzingen van uw project WebJob.

    ![Azure opslagruimte in web taken project](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Bekijk de pagina aan de slag dat wordt weergegeven en ontdek hoe uw project is gewijzigd. Een pagina aan de slag weergegeven in uw browser wanneer u een verbonden service toevoegen. U kunt de voorgestelde Vervolgstappen en codevoorbeelden bekijken of overschakelen naar de pagina Wat is er gebeurd om te zien welke verwijzingen zijn toegevoegd aan uw project en hoe uw code en configuratie-bestanden zijn gewijzigd.

## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd

Wanneer u klaar bent met het dialoogvenster, wordt er in Visual Studio wordt toegevoegd verwijzingen en bepaalde configuratiebestanden wijzigt. De specifieke wijzigingen, hangt af van het projecttype. 

 - Zie [Wat is er gebeurd – ASP.NET-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513126)voor ASP.NET-projecten. 
 - Zie [Wat is er gebeurd – ASP.NET-5-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513124)voor ASP.NET-5-projecten. 
 - Serviceprojecten (web rollen en werknemer rollen), Zie [Wat is er gebeurd – Cloudservice projecten](http://go.microsoft.com/fwlink/p/?LinkId=516965)voor de cloud. 
 - Zie [Wat is er gebeurd - WebJob projecten](./storage/vs-storage-webjobs-what-happened.md)voor WebJob projecten.

## <a name="next-steps"></a>Volgende stappen

1. Aan de slag met voorbeelden van de code als een hulplijn, het type opslag die u wilt maken en typt u schrijven van code voor toegang tot uw account opslag!

1. Vragen kunt stellen en Help-informatie opvragen
     - [MSDN-Forum: Azure opslag](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure opslag teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Opslagruimte met azure.microsoft.com](https://azure.microsoft.com/services/storage/)

     - [Opslag documentatie op azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)

