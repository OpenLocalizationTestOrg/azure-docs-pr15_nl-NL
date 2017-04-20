<properties
   pageTitle="Instellen van uw ontwikkelomgeving | Microsoft Azure"
   description="Installeer de runtime, SDK en hulpprogramma's en maken van een cluster plaatselijke ontwikkeling. Na het voltooien van deze instelling, bent u klaar om u te maken van toepassingen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Maken en uitvoeren van [Azure Service stof toepassingen] [ 1] installeren op uw computer ontwikkeling de runtime, SDK en hulpmiddelen. U moet ook uitvoering van de Windows PowerShell-scripts opgenomen in de SDK inschakelen.

## <a name="prerequisites"></a>Vereisten voor
### <a name="supported-operating-system-versions"></a>Ondersteunde besturingssystemen
De volgende besturingssystemen worden ondersteund voor de ontwikkeling:

- Windows 7
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 bevat alleen Windows PowerShell 2.0 al dan niet standaard. Service stof PowerShell-cmdlets vereist PowerShell 3.0 of hoger. U kunt [downloaden van Windows PowerShell 5.0] [ powershell5-download] van het Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>Installeren van de runtime, SDK en hulpprogramma 's

Het installatieprogramma van de Web-Platform biedt twee configuraties voor de ontwikkeling van de Service stof:

- [Installeren van de Service stof runtime, SDK en hulpprogramma's voor Visual Studio-2015 (hiervoor Visual Studio 2015 Update 2 of hoger)][full-bundle-vs2015]
- [Installeer de Service stof runtime and SDK alleen (geen Visual Studio tools)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Uitvoering van de PowerShell-script inschakelen

Service stof gebruik van Windows PowerShell-scripts voor het maken van een cluster plaatselijke ontwikkeling en voor de implementatie van toepassingen van Visual Studio. Al dan niet standaard kan deze scripts worden uitgevoerd. Als u wilt deze inschakelt, moet u uw PowerShell uitvoering van beleid wijzigen. Opent PowerShell als beheerder en voer de volgende opdracht:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Volgende stappen
Nu u bent klaar instellen van uw ontwikkelomgeving, beginnen bouwen en apps worden uitgevoerd.

- [Uw eerste stof Service-toepassing maken in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Meer informatie over het gebruiken en beheren van toepassingen op uw lokale cluster](service-fabric-get-started-with-a-local-cluster.md)
- [Meer informatie over het programmeren modellen: betrouwbare Services en betrouwbare betrokkenen](service-fabric-choose-framework.md)
- [Bekijk de voorbeelden van de code-Service stof op GitHub](https://aka.ms/servicefabricsamples)
- [Uw cluster met behulp van de Service stof Explorer visualiseren](service-fabric-visualizing-your-cluster.md)
- [Ga als volgt de Service stof leerpad als u een uitgebreide inleiding tot het platform](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service stof campagne pagina"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "TEGENOVER RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Koppeling van de VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI-koppeling"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI koppeling"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
