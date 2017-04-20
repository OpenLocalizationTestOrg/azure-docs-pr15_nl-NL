<properties
   pageTitle="Opdrachtregel opbouwen voor Azure | Microsoft Azure"
   description="Opdrachtregel opbouwen voor Azure"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Opdrachtregel opbouwen voor Azure

## <a name="overview"></a>Overzicht

U kunt een pakket voor Azure-implementatie maken door MSBuild opdrachtprompt uit te voeren. U kunt configureren en definieert builds voor foutopsporing, tijdelijke en geproduceerd, naast het automatiseren van enkele van deze procedure.


## <a name="microsoft-build-engine-msbuild"></a>Microsoft opbouwen-Engine (MSBuild)

Met behulp van de Microsoft bouwen-Engine (MSBuild), kunt u producten in opbouwen samenstellen testomgeving omgevingen waar Visual Studio niet is geïnstalleerd. MSBuild wordt een XML-indeling voor projectbestanden die van extensible en volledig ondersteund door Microsoft gebruikt. In deze bestandsindeling, kunt u aangeven welke items moeten worden geoptimaliseerd voor een of meer platforms en configuraties.

U kunt ook MSBuild uitvoeren op een opdrachtprompt en dit onderwerp wordt beschreven die benadering. Door in te stellen eigenschappen opdrachtprompt, kunt u specifieke configuraties van een project maken. U kunt op dezelfde manier ook de doelen die de opdracht MSBuild bouwt definiëren. Zie voor meer informatie over opdrachtregelparameters en MSBuild [MSBuild opdrachtregel verwijzing](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Installatie

Als de volgende procedure wordt beschreven, moet u software en hulpprogramma's voor op de server opbouwen voordat u een Azure-pakket maken kunt met behulp van MSBuild:

1. Installeer de .NET Framework-4 of hoger gebruikt, waaronder MSBuild.

1. Installeren van de [Azure Authoring extra](http://go.microsoft.com/fwlink/?LinkId=394615) (uiterlijk voor MicrosoftAzureAuthoringTools-x64.msi of MicrosoftAzureAuthoringTools-x86.msi.

1. Installeren van de [Azure bibliotheken voor .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (uiterlijk voor MicrosoftAzureLibsForNet-x64.msi of MicrosoftAzureLibs-x86.msi.

1. Kopieer het bestand Microsoft.WebApplication.targets van een Visual Studio-installatie op een andere computer.

    Het bestand zich bevindt in de directory C:\Program Files (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 voor Visual Studio 2012) en moet u deze kopiëren naar dezelfde map op de server opbouwen.

1. Installeer de [Azure Tools voor Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Zoek naar WindowsAzureTools.vs120.exe Visual Studio-2013-projecten maken.

## <a name="msbuild-parameters"></a>MSBuild Parameters

De eenvoudigste manier om een pakket te maken is om uit te voeren MSBuild met de `/t:Publish` optie. Deze opdracht maakt standaard een map ten opzichte van de hoofdmap voor het project, zoals ProjectDir\bin\Configuration\app.publish\. wanneer u een Azure project maakt, genereert u twee bestanden, het pakketbestand zelf en de bijbehorende configuratiebestand:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Standaard elk Azure project bestaat uit één service-configuratiebestand voor lokale (foutopsporing) genereert en andere versies van de cloud (staging- of), maar u kunt toevoegen of verwijderen van bestanden van de service-configuratie als dat nodig. Als u een pakket in Visual Studio samenstelt, krijgt u welke service-configuratiebestand om op te nemen samen met het pakket. Als u een pakket samenstelt met behulp van MSBuild, wordt het lokale service-configuratiebestand standaard opgenomen. Als u wilt opnemen in een ander bestand in de configuratie van de service, stel de `TargetProfile` eigenschap van de opdracht MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Als u wilt u een alternatieve map voor de opgeslagen pakket en van configuratiebestanden, stelt u het pad met behulp van de `/p:PublishDir=Directory\` optie, inclusief het afsluitende backslash-scheidingsteken.

## <a name="deployment"></a>Implementatie

Nadat het pakket is gebouwd, kunt u deze implementeert bij Azure. Zie de website van Azure voor een zelfstudie die laat dat proces zien. Zie voor informatie over het automatiseren van dat proces, [Continue bezorging voor Cloud Services in Azure wordt aangegeven](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
