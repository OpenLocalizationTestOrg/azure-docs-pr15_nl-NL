<properties
   pageTitle="Werkt met Emulator Express uit te voeren en fouten opsporen in een cloudservice op een lokale computer | Microsoft Azure"
   description="Werkt met Emulator Express uit te voeren en fouten opsporen in een cloudservice op een lokale computer"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="using-emulator-express-to-run-and-debug-a-cloud-service-on-a-local-machine"></a>Werkt met Emulator Express uit te voeren en fouten opsporen in een cloudservice op een lokale computer

U kunt met behulp van Emulator Express, testen en fouten opsporen in een cloudservice zonder Visual Studio uitvoeren als beheerder. U kunt de projectinstellingen van uw Emulator Express of de volledige emulator, afhankelijk van de vereisten van uw cloudservice instellen. Zie [een Azure-toepassing in de Emulator berekenen uitvoeren](./storage/storage-use-emulator.md)voor meer informatie over de volledige emulator. Emulator Express voor het eerst is opgenomen in Azure SDK 2.1 en vanaf Azure SDK 2.3, is de standaard-emulator.

## <a name="using-emulator-express-in-the-visual-studio-ide"></a>Emulator Express gebruiken in de Visual Studio IDE

Wanneer u een nieuw project in Azure SDK 2.3 of later maakt, is Emulator Express al geselecteerd. Volg deze stappen om te selecteren Emulator Express voor bestaande projecten die zijn gemaakt met een eerdere versie van de SDK.

### <a name="to-configure-a-project-to-use-emulator-express"></a>Voor het configureren van een project als u wilt gebruiken Emulator Express

1. Kies **Eigenschappen**in het snelmenu voor het Azure project, en kies vervolgens het tabblad **Web** .

1. Kies de knop **optie Gebruik IIS Express** onder **Lokale Development-Server**. Emulator Express is niet compatibel met IIS-webserver.

1. Kies onder **Emulator**, de optieknop **Gebruik Emulator Express** .

    ![Emulator Express](./media/vs-azure-tools-emulator-express-debug-run/IC673363.gif)

## <a name="launching-emulator-express-at-a-command-prompt"></a>Emulator Express opdrachtprompt te starten

U kunt de express-versie van de Emulator bij het berekenen van Azure, csrun.exe, met behulp van de optie /useemulatorexpress starten bij een opdrachtprompt.

## <a name="limitations"></a>Beperkingen

Voordat u Emulator Express gebruikt, moet u rekening houden met enkele beperkingen zijn:

- Emulator Express is niet compatibel met IIS-webserver.

- Uw cloudservice bevatten meerdere rollen, maar elke rol is beperkt tot één exemplaar.

- U geen toegang tot poortnummers minder dan 1000. Als u een verificatieprovider die gewoonlijk een poort minder dan 1000 gebruikt gebruikt, moet u mogelijk verander deze waarde in een poortnummer in dat groter is dan 1000.

- Eventuele beperkingen die van toepassing op de Emulator voor het berekenen van Azure zijn ook van toepassing op Emulator Express. Er geen bijvoorbeeld meer dan 50 rol exemplaren per implementatie. Zie [een Azure-toepassing in de Emulator berekeningscluster uitvoeren](http://go.microsoft.com/fwlink/p/?LinkId=623050)

## <a name="next-steps"></a>Volgende stappen

[Voor foutopsporing in Cloudservices](https://msdn.microsoft.com/library/azure/ee405479.aspx)
