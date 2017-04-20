<properties
   pageTitle="Standaardlengte van map TEMP te klein is voor een rol is | Microsoft Azure"
   description="De rol van een cloud-service heeft een beperkte hoeveelheid ruimte voor de map TEMP. Dit artikel vindt enkele suggesties voor het vermijden bij ruimte te weinig."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Standaardlengte van map TEMP is te klein is voor een functie cloud service web/werknemer

De standaard tijdelijke map van een werknemer of web-rol voor de service voor cloud heeft een maximale grootte van 100 MB, die volledige op een gegeven moment worden mogelijk. In dit artikel wordt beschreven hoe voorkomen gebrek aan ruimte voor de tijdelijke map.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Waarom kan ik geen ruimte meer uitvoeren?

De standaard Windows-omgevingsvariabelen TEMP en TMP zijn beschikbaar voor de code die wordt uitgevoerd in uw toepassing. Zowel TEMP en TMP wijst u één map met een maximale grootte van 100 MB. Alle gegevens die zijn opgeslagen in deze map worden niet bewaard over de levenscyclus van de cloudservice; Als de rol exemplaren in een cloudservice gerecycled zijn, wordt de map is verwijderd.

## <a name="suggestion-to-fix-the-problem"></a>Suggestie om het probleem te verhelpen

Een van de volgende alternatieven implementeren:

- Een resource lokale opslag configureren en deze rechtstreeks in plaats van het gebruik van TEMP of TMP te openen. Voor toegang tot een resource lokale opslag van de code die binnen de toepassing wordt uitgevoerd, belt u de methode [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) . 

- Een resource lokale opslag configureren en wijst u de mappen TEMP en TMP zodat deze verwijzen naar het pad van de resource lokale opslag. Deze wijziging moet worden uitgevoerd binnen de methode [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) .

Het volgende voorbeeld ziet u hoe de doel-mappen voor TEMP en TMP uit binnen de methode OnStart wijzigen:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Lees een blog die wordt beschreven [hoe u de tekengrootte van de Azure Web rol ASP.NET tijdelijke map](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Meer [artikelen probleemoplossing](/?tag=top-support-issue&product=cloud-services) voor cloudservices weergeven

Als u wilt weten hoe u problemen met de rol serviceproblemen cloud met behulp van Azure PaaS computer naar diagnostische gegevens, [de reeks blogartikelen van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)weergeven
