<properties 
    pageTitle="Rapportviewer gebruiken in een website | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe u een Microsoft Azure-website met het besturingselement voor Visual Studio rapportviewer waarin u kunt een rapport dat is opgeslagen op een Microsoft Azure Virtual Machine kunt maken."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Rapportviewer gebruiken in een website in Azure wordt aangegeven

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


U kunt een Microsoft Azure-website met het besturingselement voor Visual Studio rapportviewer waarin u kunt een rapport dat is opgeslagen op een Microsoft Azure Virtual Machine maken. Het besturingselement rapportviewer is in een webtoepassing dat u de sjabloon ASP.NET-toepassing maken.

>[AZURE.IMPORTANT] Het besturingselement rapportviewer wordt niet ondersteund door de sjablonen ASP.NET MVC-webtoepassing.

Als u wilt opnemen rapportviewer in uw Microsoft Azure-website, moet u de volgende taken uitvoeren.

- **Toevoegen** Stroombaan naar het implementatiepakket

- **Configureren** Verificatie en machtiging

- **Publiceren** de ASP.NET webtoepassing Azure

## <a name="prerequisites"></a>Vereisten voor

Raadpleeg de sectie 'algemene aanbeveling en aanbevolen procedures' in [SQL Server Business Intelligence in Azure virtuele Machines](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] Rapportviewer besturingselementen worden verzonden met Visual Studio, Standard Edition of hoger. Als u de Web Developer Express Edition gebruikt, moet u de [MICROSOFT rapport VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) voor het gebruik van de functies van de runtime rapportviewer installeren.
>
>Rapportviewer geconfigureerd in de verwerkingsmodus voor lokale wordt niet ondersteund in Microsoft Azure.

Bekijk de whitepaper [rapportservers op basis van Reporting Services-rapport viewer-besturingselement en Microsoft Azure virtuele machines](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx).

## <a name="adding-assemblies-to-the-deployment-package"></a>Stroombaan toevoegen aan het implementatiepakket

Wanneer u uw ASP.NET-toepassing on-premises host, de stroombaan rapportviewer meestal rechtstreeks in de cache van de globale constructie (GAC) van de IIS-server zijn geïnstalleerd tijdens de installatie van de Visual Studio en rechtstreeks door de toepassing kunnen worden geopend. Echter wanneer u uw ASP.NET-toepassing in de cloud host, kan Microsoft Azure geen invloed op worden geïnstalleerd in de GAC, dus moet u controleren of dat de stroombaan rapportviewer zijn beschikbaar voor uw toepassing lokaal. U kunt dit doen door verwijzingen naar deze toe te voegen in uw project en configureren zodat ze kunnen lokaal worden gekopieerd.

In de modus voor externe verwerking, wordt het besturingselement rapportviewer gebruikt de volgende stroombaan:

- **Microsoft.ReportViewer.WebForms.dll**: de rapportviewer code die u moet rapportviewer gebruiken op uw pagina bevat. Een overzicht van deze constructie wordt toegevoegd aan uw project wanneer u een besturingselement rapportviewer naar een ASP.NET-pagina in uw project plaatst.

- **Microsoft.ReportViewer.Common.dll**: klassen die worden gebruikt door het besturingselement rapportviewer tijdens runtime bevat. Deze niet automatisch toegevoegd aan uw project.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Een verwijzing naar Microsoft.ReportViewer.Common toevoegen

- Met de rechtermuisknop op van uw project **verwijzingen** knooppunt en selecteer **Verwijzing toevoegen**, selecteert u de vergadering in het .NET-tabblad en klik op **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Als u de stroombaan lokaal toegankelijk maken door uw ASP.NET-toepassing

1. Klik op de Microsoft.ReportViewer.Common-verzameling in de map **verwijzingen** , zodat de eigenschappen worden weergegeven in het deelvenster Eigenschappen.

1. Klik in het deelvenster eigenschappen ingesteld **Lokale kopie** op waar.

1. Herhaal stap 1 en 2 voor Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Om rapportviewer taalpakket

1. Installeer het juiste Microsoft rapport Viewer 2012 Runtime-pakket van [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).

1. Selecteer de taal in de vervolgkeuzelijst worden weergegeven en de pagina wordt omgeleid naar de bijbehorende downloadpagina voor center.

1. Klik op **downloaden** om het downloaden van ReportViewerLP.exe te starten.

1. Nadat u ReportViewerLP.exe downloaden, klikt u op **uitvoeren** om onmiddellijk te installeren of klik op **Opslaan** als u wilt opslaan op uw computer. Als u op **Opslaan**klikt, moet u de naam van de map waar u het bestand opslaat.

1. Zoek de map waar u het bestand hebt opgeslagen. Met de rechtermuisknop op ReportViewerLP.exe op **Als administrator uitvoeren**en klik vervolgens op **Ja**.

1. Na het uitvoeren van ReportViewerLP.exe, ziet u de c:\windows\assembly heeft de resource bestanden **Microsoft.ReportViewer.Webforms.Resources** en **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Als u wilt configureren voor gelokaliseerd besturingselement rapportviewer

1. Download en installeer het Microsoft-rapport Viewer 2012 Runtime-pakket door de bovenstaande opgegeven instructies te volgen.

1. Maak <language> map in de project en kopieer de bijbehorende resource constructie bestanden er. De resource constructie-bestanden te kopiëren: **Microsoft.ReportViewer.Webforms.Resources.dll** en **Microsoft.ReportViewer.Common.Resources.dll**. Selecteer de bestanden die resource constructie en stel in het deelvenster Eigenschappen **kopiëren naar uitvoer Directory** **altijd kopiëren"**.

1. De cultuur & UICulture instellen voor het webproject. Zie voor meer informatie over het instellen van de cultuur en UI-cultuur voor een ASP.NET-webpagina [hoe: Stel de cultuur en UI-cultuur voor ASP.NET-webpagina via](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Verificatie en autorisatie configureren

De rapportviewer moet de juiste referenties gebruiken om te verifiëren met de rapportserver en de referenties moeten worden toegestaan door de rapportserver voor toegang tot de gewenste rapporten. Zie de whitepaper [rapportservers op basis van Reporting Services-rapport viewer-besturingselement en Microsoft Azure virtuele machines](https://msdn.microsoft.com/library/azure/dn753698.aspx)voor informatie over verificatie.

## <a name="publish-the-aspnet-web-application-to-azure"></a>De ASP.NET webtoepassing Azure publiceren

Zie voor instructies over het publiceren van een ASP.NET-webtoepassing naar Azure [hoe: migreren en publiceren van een webtoepassing Azure van Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) en [aan de slag met Web Apps en ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Als de opdracht Azure implementatie-Project toevoegen of toevoegen Azure Cloud Service Project niet wordt weergegeven in het snelmenu te openen in Verkenner oplossing, moet u mogelijk het doel-kader voor het project wijzigen in .NET Framework 4.
>
>De twee opdrachten bieden in principe dezelfde functionaliteit. Wanneer u een of de andere opdracht wordt weergegeven in het snelmenu afhankelijk van welke versie van de Microsoft Azure SDK u hebt geïnstalleerd.

## <a name="resources"></a>Resources

[Microsoft-rapporten](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence in Azure virtuele Machines](virtual-machines-windows-classic-ps-sql-bi.md)

[PowerShell gebruiken voor het maken van een Azure VM waarbij een rapportserver Native modus](virtual-machines-windows-classic-ps-sql-report.md)

[Reporting Services-rapport viewer-besturingselement en Microsoft Azure virtuele machines gebaseerd rapportservers](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
