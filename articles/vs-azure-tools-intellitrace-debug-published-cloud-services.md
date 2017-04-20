<properties 
   pageTitle="Voor foutopsporing in een gepubliceerde cloudservice met IntelliTrace en Visual Studio | Microsoft Azure"
   description="Voor foutopsporing in een gepubliceerde cloudservice met IntelliTrace en Visual Studio"
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

# <a name="debugging-a-published-cloud-service-with-intellitrace-and-visual-studio"></a>Voor foutopsporing in een gepubliceerde cloudservice met IntelliTrace en Visual Studio

##<a name="overview"></a>Overzicht

Met IntelliTrace, kunt u de uitgebreide informatie over foutopsporing voor een exemplaar van de rol registreren wanneer deze wordt uitgevoerd in Azure wordt aangegeven. Als u nodig hebt om de oorzaak van een probleem, kunt u de logboeken IntelliTrace gebruiken om te doorlopen uw code uit Visual Studio alsof deze in Azure wordt aangegeven uitvoert. In feite IntelliTrace records de belangrijkste code worden uitgevoerd en omgevingsgegevens wanneer uw Azure-toepassing wordt uitgevoerd als een cloudservice in Azure wordt aangegeven en kunt u de opgenomen gegevens uit Visual Studio afspelen. Als alternatief, kunt u foutopsporing op afstand bijvoegen rechtstreeks naar een cloudservice die wordt uitgevoerd in Azure wordt aangegeven. Zie [voor foutopsporing in Cloudservices](http://go.microsoft.com/fwlink/p/?LinkId=623041).

>[AZURE.IMPORTANT] IntelliTrace is bedoeld voor foutopsporing scenario's alleen en mogen niet worden gebruikt voor een productie-implementatie.

>[AZURE.NOTE] U kunt IntelliTrace hebt u Visual Studio Enterprise hebt geïnstalleerd en uw Azure-toepassing doelen .NET Framework 4 of hoger. IntelliTrace verzamelt gegevens voor uw Azure rollen. De virtuele machines voor deze rollen uitvoeren altijd 64-bits besturingssystemen.

## <a name="to-configure-an-azure-application-for-intellitrace"></a>Een Azure-toepassing configureren voor IntelliTrace

Als u wilt IntelliTrace inschakelen voor een Azure-toepassing, moet u maken en publiceren van de toepassing uit een project Visual Studio Azure. Voordat u uw project naar Azure publiceren, moet u de IntelliTrace configureren voor uw Azure-toepassing. Als u uw toepassing publiceren zonder het configureren van IntelliTrace, maar u besluit dat u wilt doen, moet u de toepassing opnieuw uit Visual Studio publiceren. Zie [publiceren van een Cloudservice met de hulpmiddelen Azure](http://go.microsoft.com/fwlink/p/?LinkId=623012)voor meer informatie.

1. Wanneer u klaar bent voor uw Azure-toepassing implementeren, moet u controleren of uw project opbouwen doelen zijn ingesteld op **fouten opsporen in**.

1. Open het snelmenu voor het project Azure in Solution Explorer en kies **publiceren**.
 
    De wizard Publiceren Azure-toepassing wordt weergegeven.

1. Als u wilt verzamelen IntelliTrace logboeken voor uw toepassing wanneer deze wordt gepubliceerd in de cloud, schakel het selectievakje **IntelliTrace inschakelen** uit.

    >[AZURE.NOTE] U kunt inschakelen IntelliTrace of profiel wanneer u uw Azure-toepassing publiceert. U kunt niet beide inschakelen.

1. Als u wilt aanpassen van de eenvoudige IntelliTrace-configuratie, kiest u de hyperlink **Instellingen** .

    Het dialoogvenster met instellingen voor IntelliTrace wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding. U kunt opgeven welke gebeurtenissen worden log, of u voor het verzamelen van oproepgegevens welke modules en processen voor het verzamelen van Logboeken voor en hoeveel ruimte toewijzen aan de opname. Zie voor meer informatie over IntelliTrace, [Foutopsporing met IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).

    ![VST_IntelliTraceSettings](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Het logboek IntelliTrace is een kringverwijzing logboekbestand van de maximumgrootte blijft die is opgegeven in de IntelliTrace-instellingen (de standaardgrootte is 250 MB). IntelliTrace logboeken zijn naar een bestand in het bestandssysteem van de virtuele machine verzameld. Wanneer u de logboeken aanvraagt, is een momentopname die u hebt gemaakt op dat moment en dan ook gedownload naar uw lokale computer.

Nadat de Azure-toepassing is gepubliceerd naar Azure, kunt u bepalen als IntelliTrace is ingeschakeld van het knooppunt Azure berekenen in Server Explorer, zoals wordt weergegeven in de volgende afbeelding:

![VST_DeployComputeNode](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="downloading-intellitrace-logs-for-a-role-instance"></a>IntelliTrace logboeken downloaden voor een exemplaar van de rol

U kunt Logboeken aan de IntelliTrace voor een exemplaar van de rol downloaden van het knooppunt **Cloud Services** in **Server Explorer**. Vouw het knooppunt **Cloudservices** totdat u het exemplaar dat u geïnteresseerd bent hebt gevonden, opent u het snelmenu voor dit exemplaar en kies **Weergave IntelliTrace logboeken**. De logboeken IntelliTrace worden gedownload naar een bestand in een map op uw lokale computer. Elke keer dat u de IntelliTrace aanvragen Logboeken, een nieuwe momentopname wordt gemaakt.

Wanneer de logboeken worden gedownload, wordt de voortgang van de bewerking in Visual Studio weergegeven in het venster Azure activiteitenlogboek. Zoals u ziet in de volgende afbeelding, kunt u het artikel voor de bewerking om meer details weer te geven uitbreiden.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

U kunt blijven werken in Visual Studio, terwijl de logboeken IntelliTrace worden gedownload. Wanneer het logboek is gedownload, wordt deze automatisch geopend in Visual Studio.

>[AZURE.NOTE] De logboeken IntelliTrace mogelijk uitzonderingen die het kader wordt gegenereerd en vervolgens worden verwerkt. Interne framework code genereert deze uitzonderingen als een normale onderdeel van het starten van een rol, zodat u ze gewoon mogelijk negeren.

## <a name="see-also"></a>Zie ook

[Voor foutopsporing in Cloudservices](https://msdn.microsoft.com/library/ee405479.aspx)

