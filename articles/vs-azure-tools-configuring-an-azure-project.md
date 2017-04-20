<properties
   pageTitle="Een Project Azure Cloud-Service configureren met Visual Studio | Microsoft Azure"
   description="Leer hoe u een project Azure cloud-service configureren in Visual Studio, afhankelijk van uw vereisten voor dit project."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Een Project Azure Cloud-Service configureren met Visual Studio

Afhankelijk van uw vereisten voor dat project kunt u een project Azure cloud-service configureren. U kunt de eigenschappen voor het project voor de volgende categorieën instellen:

- **Een cloudservice publiceren naar Azure**

  Een eigenschap om ervoor te zorgen dat een bestaande cloudservice geïmplementeerd in Azure niet per ongeluk wordt verwijderd, kunt u instellen.

- **Fouten opsporen in een cloudservice op de lokale computer of uitvoeren**

  U kunt een service te configureren en aangeven of u wilt starten van de Azure opslag emulator selecteren.

- **Een servicepakket cloud valideren wanneer deze is gemaakt**

  U kunt alle waarschuwingen als fouten behandelen, zodat u kunt ervoor zorgen dat de cloud-servicepakket zonder problemen wordt implementeren. Hierdoor wordt de wachttijd als u implementeren en vervolgens ontdekken dat is een fout opgetreden.

De volgende afbeelding ziet hoe u een configuratie gebruiken tijdens het uitvoeren of fouten opsporen in de cloudservice lokaal selecteren. U kunt de project-eigenschappen die u nodig in dit venster hebt instellen, zoals weergegeven in de afbeelding.

![Een Microsoft Azure-Project configureren](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Een project Azure cloud-service configureren

1. Een project cloud-service in **Solution Explorer**configureren, open het snelmenu voor het project cloud-service en kies vervolgens **Eigenschappen**.

  Een pagina met de naam van het project cloud-service wordt weergegeven in de Visual Studio-editor.

1. Kies het tabblad **ontwikkeling** .

1. Kies **waar**om ervoor te zorgen dat u per ongeluk een bestaande implementatie in Azure wordt aangegeven, in de prompt voor het verwijderen van een bestaande lijst van de implementatie, niet verwijderen.

1. De configuratie van de service die u wilt gebruiken wanneer u uitvoeren of fouten opsporen in de cloudservice lokaal, in de lijst **serviceconfiguratie** Kies de service te configureren om te selecteren.

  >[AZURE.NOTE] Als u maken van een service te configureren wilt voor gebruik, raadpleegt u hoe u: configuraties beheren en profielen. Als u wijzigen van een service te configureren voor een rol wilt, raadpleegt u [het configureren van de rollen voor een Azure cloudservice met Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Als u wilt de emulator Azure opslag starten wanneer u uitvoeren of fouten opsporen in de cloudservice lokaal, in de **starten Azure opslag emulator**, kiest u **waar**.

1. Om ervoor te zorgen dat u niet publiceren als er pakket validatie Kies fouten, **waarschuwingen als fout behandelen**, in **waar**.

1. Om ervoor te zorgen dat uw Webrol dezelfde poort gebruikt telkens wanneer die deze wordt gestart lokaal in IIS Express in **Gebruik web project poorten**en kiest u **waar**. Naar een specifieke poort gebruiken voor een bepaalde webproject, opent u het snelmenu voor het webproject, kiest u het tabblad **Eigenschappen** , kies het tabblad **Web** en het poortnummer in de **Url van Project** -instelling in de sectie **IIS Express** wijzigen. Voer bijvoorbeeld `http://localhost:14020` als de URL van het project.

1. Kies de knop **Opslaan** op de werkbalk eventuele wijzigingen die u hebt aangebracht in de eigenschappen van het project cloud-service op te slaan.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van Azure cloud serviceprojecten in Visual Studio, raadpleegt u [uw Azure configureren project met meerdere configuraties](vs-azure-tools-multiple-services-project-configurations.md).
