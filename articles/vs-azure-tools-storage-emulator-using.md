<properties 
   pageTitle="Configureren en gebruiken van de Emulator opslagruimte met Visual Studio | Microsoft Azure"
   description="Configureren en gebruiken van de Emulator opslagruimte met Visual Studio"
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

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Configureren en gebruiken van de Emulator opslagruimte met Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Overzicht
De ontwikkelomgeving Azure SDK bevat de opslagruimte emulator, een hulpprogramma waarmee de Blob, wachtrij en tabel storage-services beschikbaar zijn in Azure op uw computer plaatselijke ontwikkeling simuleert. Als u bouwen van een cloudservice die gebruikmaakt van de Azure storage-services, of u schrijft alle externe toepassingen die de services opslagruimte u belt, kunt u uw code lokaal ten opzichte van de opslagruimte emulator testen. Beheer van de emulator opslag integreren de Azure's voor Microsoft Visual Studio in Visual Studio. De hulpmiddelen Azure initialisatie van de database opslag emulator bij eerste gebruik, wordt de emulator opslagservice gestart wanneer u uitvoeren of fouten opsporen in uw Visual Studio-code en biedt alleen-lezen toegang tot de gegevens van de emulator opslag via de Verkenner Azure-opslag.

Zie voor gedetailleerde informatie over de opslag emulator, inclusief systeemvereisten en aangepaste configuratie-instructies [gebruiken de Emulator Azure opslag voor ontwikkeling en testen](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Er zijn enkele functionele verschillen tussen de opslagruimte emulator simulatie en de Azure storage-services. Zie [verschillen tussen de opslagruimte Emulator en Azure opslagservices](./storage/storage-use-emulator.md) in de documentatie Azure SDK voor meer informatie over de specifieke verschillen.

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Een verbindingsreeks configureren voor de opslag emulator

U wilt een verbindingsreeks die verwijst naar de emulator opslag en die later kunnen worden gewijzigd zodat deze verwijzen naar een Azure opslag-account configureren voor toegang tot de emulator opslag vanuit de programmacode binnen een rol. Een verbindingsreeks is een configuratie-instelling die uw rol kan worden gelezen gedurende runtime verbinding maken met een account opslag. Zie [de Azure-toepassingen configureren](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage)voor meer informatie over het maken van verbindingstekenreeksen.

>[AZURE.NOTE] Met behulp van de eigenschap **DevelopmentStorageAccount** kunt u een verwijzing naar de opslag emulator account vanuit uw code terugkeren. Deze methode werkt goed als u wilt de emulator opslag openen vanuit uw code, maar als u van plan bent de toepassing Azure publiceren, u een verbindingsreeks toegang tot uw account Azure opslag en het wijzigen van uw code als u wilt gebruiken die verbindingsreeks moet voordat u de afbeelding publiceert maken. Als u tussen de opslagruimte emulator-account en een account Azure opslag vaak overschakelt, wordt een verbindingsreeks dit proces vereenvoudigen.

## <a name="initializing-and-running-the-storage-emulator"></a>Initialisatie en uit te voeren de emulator opslag

U kunt opgeven dat wanneer u uitvoeren of fouten opsporen in uw-service in Visual Studio, Visual Studio automatisch is gestart de emulator opslag. Open het snelmenu voor uw project **Azure** in Solution Explorer en kies **Eigenschappen**. Klik op het tabblad **ontwikkeling** in de lijst **Starten Azure opslag Emulator** kiezen **waar** (als dit nog niet is ingesteld op die waarde).

De eerste keer uitvoeren of fouten opsporen in uw service uit Visual Studio, een initialisatieproces Hiermee start u de emulator opslag. Dit proces lokale poorten gereserveerd voor de opslag emulator en de opslag emulator database wordt gemaakt. Als voltooid, hoeft dit proces niet opnieuw uitvoeren, tenzij de database opslag emulator wordt verwijderd.

>[AZURE.NOTE] Beginnen met de juni 2012-versie van de Azure-hulpmiddelen, de opslag emulator wordt uitgevoerd, standaard in SQL Express LocalDB. In eerdere versies van de Azure-hulpmiddelen, de opslag emulator wordt uitgevoerd op een standaardexemplaar van SQL Express 2005 of 2008, die u moet installeren voordat u de SDK Azure kunnen installeren. U kunt ook de emulator opslag ten opzichte van een benoemd exemplaar van SQL Express of een benoemd of standaardexemplaar van Microsoft SQL Server uitvoeren. Als u nodig hebt voor het configureren van de emulator opslag in te voeren op een exemplaar dan de standaardexemplaar, raadpleegt u [Gebruik de Emulator Azure opslag voor ontwikkeling en testen](./storage/storage-use-emulator.md).

De emulator opslagruimte biedt een gebruikersinterface om te bekijken van de status van de lokale opslag-services en wilt beginnen, stoppen en deze opnieuw ingesteld. Zodra de emulator opslagservice wordt gestart, kunt u de gebruikersinterface weergeven of start of stop de service met de rechtermuisknop op het pictogram voor de Microsoft Azure Emulator in de Windows-taakbalk.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Opslag emulator gegevens weergeven in een Server Explorer

Het knooppunt Azure-opslag in Server Explorer kunt u gegevens weergeven en instellingen wijzigen voor de gegevens in uw opslag-accounts, inclusief de emulator opslag blob en de tabel. Zie [surfen en Resources op de opslag beheren met de Server Explorer](https://msdn.microsoft.com/library/azure/ff683677.aspx) voor meer informatie.
