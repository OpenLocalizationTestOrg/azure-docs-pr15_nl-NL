<properties
   pageTitle="Overzicht van de configuratie van Azure-Service stof betrouwbare betrokkenen KVSActorStateProvider | Microsoft Azure"
   description="Meer informatie over het configureren van Azure-Service stof statuscontrole betrokkenen van het type KVSActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Betrouwbare betrokkenen--KVSActorStateProvider configureren
U kunt de standaardconfiguratie van KVSActorStateProvider wijzigen door te wijzigen van het settings.xml-bestand dat in de hoofdmap van het pakket Microsoft Visual Studio onder de map Config voor de opgegeven acteur is gegenereerd.

De runtime Azure-Service stof Hiermee wordt gezocht naar de vooraf gedefinieerde sectienamen in het bestand settings.xml en gebruikt de configuratiewaarden bij het maken van de onderliggende runtimeonderdelen.

>[AZURE.NOTE] Kan **geen** verwijdert of wijzigt u de sectienamen van de volgende configuraties in het settings.xml-bestand dat in de Visual Studio-oplossing wordt gegenereerd.

## <a name="replicator-security-configuration"></a>Configuratie van replicatie-beveiliging
Replicatie beveiligingsconfiguraties worden gebruikt voor het communicatiekanaal die wordt gebruikt tijdens de replicatie secure. Dit betekent dat services elkaars replicatie-verkeer is toegestaan, ervoor te zorgen dat de gegevens die bestaat uit ten zeerste beschikbaar ook veilig is niet kunnen zien.
Standaard wordt een sectie van de configuratie van lege beveiliging voorkomen dat replicatie beveiliging.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicatie-configuratie
Replicatie configuraties configureren replicatie die verantwoordelijk is voor de status van de acteur staat serviceprovider uiterst betrouwbaar.
De standaardconfiguratie wordt gegenereerd door de Visual Studio-sjabloon en doorgaans voldoende. In dit gedeelte moment spreekt aanvullende configuraties die beschikbaar zijn voor replicatie af te stellen.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Seconden|0.015|De periode waarvoor de distributeur op de secundaire wacht nadat u hebt ontvangen van een bewerking alvorens een bevestiging terug naar de primaire. Een andere bevestigingen wilt sturen voor bewerkingen verwerkt binnen dit interval worden verzonden als één antwoord.|
|ReplicatorEndpoint|N/B|Geen standaard--vereiste parameter|IP-adres en poort dat primaire/secundaire replicatie wordt gebruikt om te communiceren met andere replicators in de replica ingesteld. Dit moet verwijzen naar een resource TCP-eindpunt in het servicemanifest. Raadpleeg [Service manifest bronnen](service-fabric-service-manifest-resources.md) voor meer informatie over het definiëren van eindpunt resources in het servicemanifest. |
|RetryInterval|Seconden|5|Periode waarna replicatie opnieuw een bericht verzendt als deze niet een bevestiging voor een bewerking ontvangt.|
|MaxReplicationMessageSize|Bytes|50 MB|Maximale grootte van replicatiegegevens die kan worden verzonden in één bericht.|
|MaxPrimaryReplicationQueueSize|Aantal bewerkingen|1024|Maximum aantal bewerkingen in de primaire wachtrij. Een bewerking is vrijgemaakt nadat primaire replicatie een bevestiging van de secundaire replicators ontvangt. Deze waarde moet groter zijn dan 64 en een macht van 2.|
|MaxSecondaryReplicationQueueSize|Aantal bewerkingen|2048|Maximum aantal bewerkingen in de secundaire wachtrij. Een bewerking is vrijgemaakt na de staat ten zeerste beschikbaar tot en met permanente maken. Deze waarde moet groter zijn dan 64 en een macht van 2.|

## <a name="store-configuration"></a>Store-configuratie
Store-configuraties worden gebruikt voor het configureren van het lokale archief die wordt gebruikt om de provincie waarvan wordt gerepliceerd.
De standaardconfiguratie wordt gegenereerd door de Visual Studio-sjabloon en doorgaans voldoende. In dit gedeelte moment spreekt aanvullende configuraties die beschikbaar zijn voor het afstemmen van het lokale archief.

### <a name="section-name"></a>De naam van sectie
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Van configuratienamen

|Naam|Eenheid|Standaardwaarde|Opmerkingen|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Milliseconden|200|Hiermee stelt u het maximale aantal interval voor duurzame lokale archief doorvoeracties batchen.|
|MaxVerPages|Aantal pagina 's|16384|Het maximum aantal pagina's versie in de lokale database opslaan. Bepaalt het maximum aantal openstaande transacties.|

## <a name="sample-configuration-file"></a>Voorbeeld van configuratiebestand

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```
## <a name="remarks"></a>Opmerkingen

Replicatielatentie Hiermee stelt u de parameter BatchAcknowledgementInterval. Een waarde van '0' levert de laagste mogelijke latentie, maar oplevert doorvoer (zoals meer bevestigingsberichten moeten worden verzonden en verwerkt, elk met minder bevestigingen).
Des te groter de waarde voor BatchAcknowledgementInterval, het sneller de algehele herhaling worden verwerkt, maar een hogere bewerking latentie oplevert. Dit equivalent rechtstreeks naar de latentie van transactiebevestigingen.
