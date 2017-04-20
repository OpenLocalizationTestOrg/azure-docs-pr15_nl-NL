<properties
   pageTitle="Algemene FabricClient uitzonderingen | Microsoft Azure"
   description="Beschrijft de algemene uitzonderingen en fouten die door de FabricClient APIs kunnen worden gegenereerd tijdens het uitvoeren van toepassing en cluster beheertaken uit te voeren."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Veelvoorkomende uitzonderingen en fouten tijdens het werken met de FabricClient APIs
De [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -API's kunnen cluster en toepassing beheerders beheerderstaken uitvoeren op een Service stof-toepassing, service of cluster. Bijvoorbeeld toepassingsimplementatie, upgrade en verwijderen, de status van een cluster controleren of een service te testen. Softwareontwikkelaars en clusterbeheerders van de kunnen de FabricClient APIs gebruiken voor het ontwikkelen van hulpmiddelen voor het beheren van de Service stof cluster en toepassingen.

Er zijn verschillende soorten bewerkingen die kunnen worden uitgevoerd met FabricClient.  Elke methode kunt uitzonderingen voor fouten vanwege onjuiste invoer, runtimefouten of problemen tijdelijke infrastructuur genereren.  Zie de documentatie van de verwijzing API om te zoeken welke uitzonderingen worden veroorzaakt door een specifieke methode. Er zijn enkele uitzonderingen, maar die kunnen worden gegenereerd door veel verschillende [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -API's. De volgende tabel bevat de uitzonderingen die gebruikt in de FabricClient APIs worden.

|Uitzondering| Gegenereerd wanneer|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Het object [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) is in een gesloten stand. Buitengebruikstelling van het object [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) u gebruikt en een nieuw [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -object wordt gestart. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Time-out van de bewerking. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) wordt geretourneerd als de bewerking heeft meer dan MaxOperationTimeout om te voltooien.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Het selectievakje toegang voor de bewerking is mislukt. E_ACCESSDENIED wordt geretourneerd.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Een runtimefout opgetreden tijdens de bewerking wordt uitgevoerd. Een van de methoden FabricClient [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)potentieel kunt genereren, de eigenschap [foutcode](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) geeft de oorzaak van de uitzondering. Foutcodes zijn gedefinieerd in de opsomming [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) .|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|De bewerking is mislukt vanwege een tijdelijke fout van bepaalde identificatie. Een bewerking kan bijvoorbeeld mislukken omdat een quorum van replica's tijdelijk niet bereikbaar is. Tijdelijke uitzonderingen overeenkomen met mislukte bewerkingen die opnieuw kunnen worden verzonden.|

Enkele veelvoorkomende fouten in [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) die in een [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)kunnen worden geretourneerd:

|Fout| Voorwaarde|
|---------|:-----------|
|CommunicationError|Een communicatiefout veroorzaakt de bewerking is mislukt, probeer het opnieuw.|
|InvalidCredentialType|Het referentietype is ongeldig.|
|InvalidX509FindType|De X509FindType is ongeldig.|
|InvalidX509StoreLocation|De X509 archieflocatie ongeldig is.|
|InvalidX509StoreName|De X509 winkelnaam ongeldig is.|
|InvalidX509Thumbprint|De X509 certificaat vingerafdruk tekenreeks is ongeldig.|
|InvalidProtectionLevel|Het niveau van bescherming is ongeldig.|
|InvalidX509Store|De X509 certificaat store kan niet worden geopend.|
|InvalidSubjectName|De onderwerpnaam is ongeldig.|
|InvalidAllowedCommonNameList|De opmaak van de algemene lijsttekenreeks is ongeldig. Een lijst met door komma's gescheiden moeten worden.|
