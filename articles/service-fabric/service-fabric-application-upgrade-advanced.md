<properties
   pageTitle="Upgrade van toepassing: geavanceerde onderwerpen | Microsoft Azure"
   description="In dit artikel komen enkele geavanceerde onderwerpen over het upgraden van een toepassing voor de Service stof."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Service stof toepassing upgrade: geavanceerde onderwerpen

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Toevoegen of verwijderen van services tijdens de upgrade van een toepassing

Als u een nieuwe service wordt toegevoegd aan een toepassing die is al geïmplementeerd en als een upgrade hebt gepubliceerd, wordt de nieuwe service wordt toegevoegd aan de gedistribueerde toepassing.  Een dergelijke upgrade heeft geen invloed op een van de services die al onderdeel van de toepassing waren. Echter een exemplaar van de service die is toegevoegd voor de nieuwe service actief moet worden gestart (met de `New-ServiceFabricService` cmdlet).

Services kunnen ook worden verwijderd uit een toepassing als onderdeel van een upgrade. Echter alle huidige exemplaren van de service naar / worden verwijderd voordat u verder gaat met de upgrade moeten worden gestopt (met de `Remove-ServiceFabricService` cmdlet). 

## <a name="manual-upgrade-mode"></a>Handmatige upgrade modus

> [AZURE.NOTE]  De niet-beheerde handmatige modus gelden alleen voor een upgrade is mislukt of geschorst. De gecontroleerde modus is de aanbevolen upgrade modus voor stof Service-toepassingen.

Azure-Service stof biedt meerdere upgrade modi ter ondersteuning van ontwikkel- en productieomgeving clusters. Distributieopties gekozen afwijken voor verschillende omgevingen.

De gecontroleerde lopende toepassing upgrade is de meestgebruikte upgrade in productie gebruiken. Wanneer het beleid voor de upgrade is opgegeven, Service stof zorgt ervoor dat de toepassing orde voordat de upgrade verloopt.

 De beheerder van de toepassing kunt u de handmatige lopende toepassing upgrade modus gebruiken om volledige controle over de upgrade voortgang tot en met de domeinen van de verschillende upgrade. Deze modus is nuttig als u een aangepaste of complexe systeemstatus evaluatie beleid is vereist, of een ongebruikelijke upgrade komt (bijvoorbeeld de toepassing is al in verlies van gegevens).

De automatische lopende toepassing upgrade is ten slotte handig voor de ontwikkeling of testen omgevingen op te geven van een iteratiecyclus snel tijdens de serviceontwikkeling.

## <a name="change-to-manual-upgrade-mode"></a>Handmatige upgrade modus wijzigen
**Handmatige**--de upgrade van toepassing op de huidige per stoppen en wijzig de upgrade-modus op niet-beheerde handmatig. De beheerder moet handmatig bellen **MoveNextApplicationUpgradeDomainAsync** als u wilt doorgaan met de upgrade of een ongedaan maken door een nieuwe upgrade starten activeren. Wanneer de upgrade heeft ingevoerd in de handmatige modus, blijft dit in de handmatige modus totdat een nieuwe upgrade wordt gestart. De opdracht **GetApplicationUpgradeProgressAsync** geeft stof\_toepassing\_upgraden\_staat\_rollend\_doorsturen\_in behandeling.

## <a name="upgrade-with-a-diff-package"></a>Upgraden met een pakket vergelijken

Een toepassing voor de Service stof kan worden bijgewerkt door de inrichting van met een volledig en zelfstandige toepassingspakket. Een toepassing kan worden bijgewerkt met behulp van een vergelijking-pakket dat alleen de bijgewerkte toepassingsbestanden, manifest van de bijgewerkte toepassing en de service manifest-bestanden bevat.

Een volledige toepassing-pakket bevat alle benodigde starten en uitvoeren van een toepassing voor de Service stof bestanden. Een vergelijking-pakket bevat alleen de bestanden die tussen de laatste voorziening en de huidige upgrade is gewijzigd, plus manifest van de volledige toepassing en de service bestandenlijst bestanden. Een verwijzing in de toepassingsmanifest of de servicemanifest dat niet kan worden gevonden in de indeling opbouwen wordt gezocht naar in de winkel afbeelding.

Volledige toepassing-pakketten zijn vereist voor de eerste installatie van een toepassing aan het cluster. Bijwerkingen kunnen zijn op een volledige toepassing of een pakket vergelijken.

Gelegenheden bij het gebruik van een pakket vergelijken is een goede keuze:

* Een vergelijking-pakket is voorkeur wanneer u een grote toepassingspakket dat verwijst naar verschillende manifest service-bestanden en/of verschillende code-pakketten, config-pakketten of gegevenspakketten hebt.

* Een vergelijking-pakket is voorkeur wanneer u een implementatie-systeem dat wordt gegenereerd van de indeling opbouwen rechtstreeks vanuit uw toepassing opbouwen proces hebt. Hoewel de code is niet gewijzigd, krijgen zojuist-en-klare stroombaan in dit geval een verschillende controlesom. Gebruik van een volledige toepassing-pakket, moet u de versie op alle code-pakketten bijwerken. Een pakket vergelijking gebruikt, geef u alleen de bestanden die gewijzigd en de manifest-bestanden waar de versie is gewijzigd.

Wanneer een toepassing wordt bijgewerkt met het gebruik van Visual Studio, wordt automatisch het vergelijken van het pakket gepubliceerd. Als u wilt een pakket vergelijking handmatig maakt, manifest van de toepassing en de service-manifesten moeten worden bijgewerkt, maar alleen de gewijzigde pakketten moeten worden opgenomen in het definitieve toepassingspakket. 

Bijvoorbeeld, laten we beginnen met de volgende toepassing (versienummers die zijn opgegeven voor gebruiksgemak lidmaatschap):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Nu kunnen we wordt ervan uitgegaan dat u wilt bijwerken, alleen de code-pakket van service1 met een verschil-pakket via PowerShell. Uw bijgewerkte toepassing heeft nu de mapstructuur van de volgende:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

In dit geval bijwerken u manifest van de toepassing naar 2.0.0 en de servicemanifest voor service1 om weer te geven van de code-pakket-update. De map voor uw toepassingspakket, moet de volgende structuur:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Volgende stappen

[Een upgrade van uw toepassing gebruik Visual Studio](service-fabric-application-upgrade-tutorial.md) , wordt u begeleidt bij de upgrade van een toepassing gebruik van Visual Studio.

[Een upgrade van uw Powershell voor het gebruiken van toepassing](service-fabric-application-upgrade-tutorial-powershell.md) , wordt u begeleidt bij de upgrade van een toepassing via PowerShell.

Bepalen hoe uw toepassing upgrades met behulp van [Parameters upgraden](service-fabric-application-upgrade-parameters.md).

Uw toepassingsupgrades compatibele maken door te leren hoe u [Gegevens serialisatie](service-fabric-application-upgrade-data-serialization.md)gebruiken.

Veelvoorkomende problemen oplossen in toepassingsupgrades door te verwijzen naar de stappen in de [Toepassingsupgrades probleemoplossing](service-fabric-application-upgrade-troubleshooting.md).
 
