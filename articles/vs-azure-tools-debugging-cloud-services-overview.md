<properties 
   pageTitle="Azure Cloudservices foutopsporing | Microsoft Azure"
   description="Voor foutopsporing in Cloudservices van Azure"
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

# <a name="debugging-cloud-services"></a>Voor foutopsporing in cloudservices

U kunt verschillende methoden gebruiken om fouten opsporen in een Azure-toepassing met behulp van de Azure-hulpmiddelen voor Microsoft Visual Studio en de SDK Azure:

- U kunt een Azure-toepassing van Visual Studio foutopsporing wanneer u ontwikkelt, net zoals u zou doen met een visuele C# of Visual Basic-toepassing. Zie [fouten opsporen in de cloudservice op uw lokale computer](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer)voor meer informatie.

- U kunt Diagnostisch hulpprogramma Azure Meld u gedetailleerde informatie uit de code die wordt uitgevoerd binnen rollen, of de rollen worden uitgevoerd in de ontwikkelomgeving of in Azure wordt aangegeven. Zie [verzamelen gegevens met behulp van Azure diagnostische gegevens vastleggen](http://go.microsoft.com/fwlink/p/?LinkId=400450)voor meer informatie.

- Als u Visual Studio Enterprise gebruikt om te schrijven op het .NET Framework-4 of de .NET Framework 4.5 gericht rollen, kunt u IntelliTrace inschakelen op het moment dat u een Azure cloudservice van Visual Studio implementeert. IntelliTrace biedt een logboek die u met Visual Studio gebruiken kunt voor uw toepassing foutopsporing alsof deze in Azure wordt aangegeven uitvoert. Zie [voor foutopsporing in een gepubliceerde cloudservice met IntelliTrace en Visual Studio]( http://go.microsoft.com/fwlink/p/?LinkId=623016)voor meer informatie.

- U kunt de externe foutopsporing op uw cloudservices op het moment waarop het implementeren van de cloudservice van Visual Studio inschakelen. Als u kiest voor foutopsporing op afstand voor een implementatie inschakelen, zijn externe foutopsporing services op de virtuele machines waarop elk exemplaar van de rol geïnstalleerd. Deze services, zoals msvsmon.exe, geen invloed hebben op prestaties en leiden tot extra kosten. Zie [fouten opsporen in een cloudservice in Azure wordt aangegeven](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure)voor meer informatie.



