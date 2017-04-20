<properties 
pageTitle="Opstarttaken uitvoeren in Azure Cloudservices | Microsoft Azure" 
description="Opstarttaken kunnen voorbereiden van uw omgeving cloud-service voor de app. Hier leert u de werking van opstarttaken en hoe u ze" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Het configureren en opstarttaken voor een cloudservice uitvoeren

U kunt opstarttaken bewerkingen uitvoeren voordat u een rol wordt gestart. Bewerkingen die u wilt uitvoeren opnemen voor het installeren van een onderdeel, COM-onderdelen registreren, instellen registersleutels of starten van een langdurige proces.

>[AZURE.NOTE] Opstarttaken zijn niet van toepassing op virtuele Machines, alleen op Cloud Service Web en werknemer rollen.

## <a name="how-startup-tasks-work"></a>De werking van opstarttaken

Opstarttaken zijn acties die zijn gemaakt voordat uw rollen begint en zijn gedefinieerd in het bestand [ServiceDefinition.csdef] met behulp van de [taak] -element binnen het element [opstarten] . Vaak zijn opstarttaken batch-bestanden, maar ze kunnen ook worden consoletoepassingen of batchbestanden die beginnen PowerShell-scripts.

Omgevingsvariabelen informatie doorgeven aan een taak opstarten en lokale opslag kan worden gebruikt om gegevens uit een taak opstarten doorgeven. Bijvoorbeeld een omgevingsvariabele kunt opgeven het pad naar een programma dat u wilt installeren, en bestanden kunnen worden geschreven naar de lokale opslag vervolgens door uw rollen later kan worden gelezen.

Uw taak opstarten kan zich aanmelden informatie en fouten naar de map die is opgegeven door de omgevingsvariabele **TEMP** . Tijdens de taak opstarten, de omgevingsvariabele **TEMP** wordt omgezet in de *C:\\Resources\\temp\\[guid]. [ functienaam]\\RoleTemp* directory wanneer u zich in de cloud.

Opstarttaken kunnen ook worden uitgevoerd enkele malen opnieuw wordt gestart. Bijvoorbeeld de taak opstarten wordt uitgevoerd telkens wanneer die de rol wordt herhaald, en rol herhalingen opnieuw opstarten niet altijd mogelijk opnemen. Opstarttaken moeten worden opgeslagen op een manier waarmee ze enkele malen zonder problemen worden uitgevoerd.

Opstarttaken moeten eindigen met een **errorlevel** (of afsluitcode) van nul opstarten om te voltooien. Als een taak opstarten op een niet-nul- **errorlevel eindigt**, kan de rol niet worden gestart.


## <a name="role-startup-order"></a>Rol opstartvolgorde

Hieronder vindt u de procedure voor het opstarten van rol in Azure wordt aangegeven:

1. Het exemplaar is gemarkeerd als **starten** en ontvangt geen verkeer is toegestaan.

2. Alle opstarttaken worden uitgevoerd op basis van hun **taskType** -kenmerk.
    - De **eenvoudige** taken worden uitgevoerd, synchroon, één voor één.
    - De **achtergrond** en **voorgrond** taken worden gestart asynchroon, parallel aan de taak opstarten.  
       
    > [AZURE.WARNING] IIS mogelijk niet volledig worden geconfigureerd in de fase opstarten taak in het opstartproces zodat rol-specifieke gegevens mogelijk niet beschikbaar. Opstarttaken waarvoor specifieke gegevens moeten [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx)gebruiken.

3. Het hostproces voor de rol wordt gestart en de site wordt gemaakt in IIS.

4. De methode [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) wordt genoemd.

5. Het exemplaar is gemarkeerd als **Gereed** en verkeer wordt doorgestuurd naar het exemplaar.

6. De methode [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) wordt genoemd.


## <a name="example-of-a-startup-task"></a>Voorbeeld van een taak opstarten

Opstarttaken worden gedefinieerd in het bestand [ServiceDefinition.csdef] , klikt u in het element **Task** . Het kenmerk **opdrachtregel** geeft de naam en de parameters van het batchbestand opstarten of console-opdracht, het kenmerk **executionContext** Hiermee geeft u het machtigingsniveau van de taak opstarten en het kenmerk **taskType** wordt opgegeven hoe de taak wordt uitgevoerd.

In dit voorbeeld is een omgevingsvariabele, **MyVersionNumber**, gemaakt voor de taak opstarten en ingesteld op de waarde "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

In het volgende voorbeeld wordt wordt het batchbestand **Startup.cmd** de regel 'de huidige versie is 1.0.0.0"naar het bestand StartupLog.txt in de map die is opgegeven door de omgevingsvariabele TEMP. De `EXIT /B 0` regel zorgt ervoor dat de taak opstarten met **errorlevel** nul eindigt.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] Visual Studio, moet de eigenschap **kopiëren naar uitvoer Directory** voor het opstarten batchbestand zijn ingesteld op **Altijd kopiëren** om ervoor te zorgen dat uw batchbestand opstarten juist is geïmplementeerd aan uw project op Azure (**approot\\opslaglocatie** voor Web rollen en **approot** voor werknemer rollen).

## <a name="description-of-task-attributes"></a>Beschrijving van de taak kenmerken

Hier volgen de kenmerken van het element van de **taak** in het bestand [ServiceDefinition.csdef] :

**opdrachtregel** - geeft de opdrachtregel voor de taak opstarten:

- De opdracht, met optionele opdrachtregelparameters, die de taak opstarten begint.
- Dit is vaak de bestandsnaam van een batchbestand cmd of type.
- De taak is ten opzichte van de AppRoot\\Bin-map voor de implementatie. Omgevingsvariabelen worden niet uitgevouwen bij het bepalen van het pad en de bestandsnaam van de taak. Als uitbreiding van de omgeving vereist is, kunt u een kleine CMD-script dat u uw taak opstarten belt.
- Kan zijn een consoletoepassing of een batchbestand waarmee een [PowerShell-script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)kan worden gestart.

**executionContext** - Hiermee geeft u het machtigingsniveau voor de taak opstarten. Het machtigingsniveau kan worden beperkt of verhoogde:

- **beperkte**  
De taak opstarten wordt uitgevoerd met dezelfde bevoegdheden als de rol. Wanneer het kenmerk **executionContext** voor de [Runtime] -element ook **beperkt is**, worden gebruikersbevoegdheden gebruikt.

- **verhoogde**  
De taak opstarten wordt uitgevoerd met beheerdersbevoegdheden. In deze sectie geeft opstarttaken voor het programma's installeren, configuratiewijzigingen IIS, het uitvoeren van wijzigingen in het register en andere niveau beheerderstaken, zonder het vergroten van het machtigingsniveau van de rol zelf.  

> [AZURE.NOTE] Het machtigingsniveau van een taak opstarten hoeft niet hetzelfde als de rol zelf.

**taskType** - Hiermee geeft u de manier waarop een taak opstarten wordt uitgevoerd.

- **eenvoudige**  
Taken uitgevoerd synchroon, één per keer, in de volgorde in het bestand [ServiceDefinition.csdef] hebt opgegeven. Wanneer u een **eenvoudige** opstarten taak eindigt op een **errorlevel** nul, wordt de volgende **eenvoudige** opstarten taak wordt uitgevoerd. Als er geen meer **eenvoudige** opstarttaken uitvoeren, wordt klikt u vervolgens de rol zelf gestart.   

    > [AZURE.NOTE] Als de **eenvoudige** taak op een niet-nul- **errorlevel eindigt**, worden het exemplaar geblokkeerd. Volgende **eenvoudige** opstarttaken en de rol zelf, niet worden gestart.

    De opdracht uitvoeren om ervoor te zorgen dat uw batchbestand met **errorlevel** nul eindigt, `EXIT /B 0` aan het einde van het batchproces-bestand.

- **achtergrond**  
Taken uitgevoerd asynchroon parallel met het starten van de rol.

- **voorgrond**  
Taken uitgevoerd asynchroon parallel met het starten van de rol. Het belangrijkste verschil tussen een **voorgrond** en **achtergrond** taak is dat een taak **voorgrond** wordt verhinderd door de rol van hergebruik of afgesloten totdat de taak is beëindigd. **De achtergrondtaken** hebben geen deze beperking.

## <a name="environment-variables"></a>Omgevingsvariabelen

Omgevingsvariabelen zijn een manier om informatie doorgeven aan een taak opstarten. U kunt bijvoorbeeld het pad naar een blob met een programma te installeren, of poortnummers die worden gebruikt door uw rol of instellingen voor functies van de besturing van uw taak opstarten plaatsen.

Er zijn twee soorten omgevingsvariabelen voor opstarttaken. statische omgevingsvariabelen en de variabelen op basis van de leden van de klasse [RoleEnvironment] . Beide zich in de sectie [omgeving] van het bestand [ServiceDefinition.csdef] en beide het [variabele] element en **de naam van** het kenmerk gebruiken.

Het **value** -kenmerk van de [variabele] element maakt gebruik van statische omgevingsvariabelen. Het bovenstaande voorbeeld wordt de omgevingsvariabele **MyVersionNumber** waarvoor een statische waarde van "**1.0.0.0**". Een ander voorbeeld is om te maken van een **StagingOrProduction** omgevingsvariabele die u handmatig op waarden van "**tijdelijke**" of "**productie**" instellen kunt verschillende opstarten acties op basis van de waarde van de omgevingsvariabele **StagingOrProduction** wilt uitvoeren.

Omgevingsvariabelen op basis van de leden van de klasse RoleEnvironment gebruik niet het **value** -kenmerk van de [variabele] element. In plaats daarvan de [RoleInstanceValue] onderliggend element, met de juiste **XPath** kenmerkwaarde, worden gebruikt voor het maken van een omgevingsvariabele op basis van een specifiek lid van de klasse [RoleEnvironment] . Waarden voor het kenmerk **XPath** voor toegang tot verschillende [RoleEnvironment] waarden vindt u [hier](cloud-services-role-config-xpath.md).



Bijvoorbeeld als u wilt een omgevingsvariabele die is "**true**" wanneer het exemplaar wordt uitgevoerd op de berekeningscluster emulator en "**false**" wanneer u zich in de cloud maken, gebruikt u de volgende [variabele] en [RoleInstanceValue] elementen:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Volgende stappen
Informatie over het uitvoeren van bepaalde [algemene opstarttaken](cloud-services-startup-tasks-common.md) met de Cloud-Service.

[Pakket](cloud-services-model-and-package.md) uw Cloudservice.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Taak]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Opstarten]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Omgeving]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variabele]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx