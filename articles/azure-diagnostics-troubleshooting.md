<properties
    pageTitle="Azure diagnostisch hulpprogramma probleemoplossing"
    description="Problemen oplossen bij het gebruik van Azure diagnostische gegevens in Azure Cloud Services, virtuele Machines en "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure diagnostisch hulpprogramma probleemoplossing
Voor informatie die relevant zijn voor het gebruik van Azure diagnostisch hulpprogramma probleemoplossing. Zie [Overzicht van de Azure-diagnostische hulpprogramma's](azure-diagnostics.md#cloud-services)voor meer informatie over Azure diagnostisch hulpprogramma's.

## <a name="azure-diagnostics-is-not-starting"></a>Azure diagnostische gegevens wordt niet gestart

Diagnostisch hulpprogramma bestaat uit twee onderdelen: een gast-Plug agent en de controle-agent. U kunt de logboekbestanden **DiagnosticsPluginLauncher.log** en **DiagnosticsPlugin.log** voor meer informatie over waarom diagnostische hulpprogramma's kan niet worden gestart controleren.  
  
Logboekbestanden voor de invoegtoepassing voor gast agent bevinden zich in een rol Cloudservice in: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

In een Azure Virtual Machine zich logboekbestanden voor de invoegtoepassing voor gast agent bevinden in: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
De laatste regel van de logboekbestanden bevat de afsluitcode.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

De invoegtoepassing geeft als resultaat de volgende afsluitcodes:

Code afsluiten|Beschrijving
---|---
0|Success.
-1|Algemene fout.
-2.|Kan niet het bestand rcf laden.<p>Dit is een interne fout die alleen gebeuren moet als het startpictogram voor Plug agent Gast handmatig, verkeerde manier wordt aangeroepen, klik op de VM.
-3|Het configuratiebestand diagnostische gegevens kunnen niet worden geladen.<p><p>Oplossing: Dit is het resultaat van een configuratiebestand geen door schema gegevensvalidatie. De oplossing is om te creëren van een configuratiebestand die voldoet aan het schema.
-4|Een ander exemplaar van de controle-agent diagnostische gegevens wordt al gebruikt voor de map lokale bron.<p><p>Oplossing: Geef een andere waarde voor **LocalResourceDirectory**.
-6|Het startpictogram voor Plug agent Gast heeft geprobeerd diagnostische hulpprogramma's starten met een ongeldige opdrachtregel.<p><p>Dit is een interne fout die alleen gebeuren moet als het startpictogram voor Plug agent Gast handmatig, verkeerde manier wordt aangeroepen, klik op de VM.
-10|De invoegtoepassing hulpprogramma's voor diagnose afgesloten met een onverwerkte uitzondering.
-11|De Gast-agent is niet verantwoordelijk voor het starten van en het monitoren van de controle-agent maken van het proces.<p><p>Oplossing: Controleer of voldoende systeembronnen zijn beschikbaar voor het starten van nieuwe processen.<p>
-101|Ongeldige argumenten bij het aanroepen van de invoegtoepassing hulpprogramma's voor diagnose.<p><p>Dit is een interne fout die alleen gebeuren moet als het startpictogram voor Plug agent Gast handmatig, verkeerde manier wordt aangeroepen, klik op de VM.
-102|Het proces van de invoegtoepassing is kan niet worden geïnitialiseerd zelf.<p><p>Oplossing: Controleer of voldoende systeembronnen zijn beschikbaar voor het starten van nieuwe processen.
-103|Het proces van de invoegtoepassing is kan niet worden geïnitialiseerd zelf. Specifiek is het niet mogelijk om het logboek-object te maken.<p><p>Oplossing: Controleer of voldoende systeembronnen zijn beschikbaar voor het starten van nieuwe processen.
-104|Kan niet het rcf-bestand dat is verstrekt door de Gast-agent laden.<p><p>Dit is een interne fout die alleen gebeuren moet als het startpictogram voor Plug agent Gast handmatig, verkeerde manier wordt aangeroepen, klik op de VM.
-105|De invoegtoepassing hulpprogramma's voor diagnose openen niet het configuratiebestand diagnostische hulpprogramma's.<p><p>Dit is een interne fout die alleen gebeuren moet als de invoegtoepassing hulpprogramma's voor diagnose handmatig, verkeerde manier wordt aangeroepen, klik op de VM.
-106|Het configuratiebestand diagnostische hulpprogramma's kan niet worden gelezen.<p><p>Oplossing: Dit is het resultaat van een configuratiebestand geen door schema gegevensvalidatie. De oplossing staat dus op te geven van een configuratiebestand die voldoet aan het schema. U vindt de XML die wordt bezorgd in de extensie diagnostische gegevens in de map *%SystemDrive%\WindowsAzure\Config* op de VM. Open het juiste XML-bestand en zoeken voor **Microsoft.Azure.Diagnostics**en vervolgens voor het veld **xmlCfg** . De gegevens zijn base64-gecodeerde dus u [dit decoderen moet](http://www.bing.com/search?q=base64+decoder) om de XML die is geladen door diagnostische gegevens weer te geven.<p>
-107|De resource directory-laat tot de controle-agent is ongeldig.<p><p>Dit is een interne fout die alleen gebeuren moet als de controle-agent handmatig, onjuist worden gegroepeerd, klik op de VM aangeroepen is.</p>
-108    |Kan niet het configuratiebestand diagnostische hulpprogramma's converteren naar de controle agent configuratiebestand.<p><p>Dit is een interne fout die alleen gebeuren moet als de invoegtoepassing hulpprogramma's voor diagnose handmatig wordt aangeroepen met een ongeldig configuratiebestand.
-110|Configuratiefout algemeen diagnostische gegevens.<p><p>Dit is een interne fout die alleen gebeuren moet als de invoegtoepassing hulpprogramma's voor diagnose handmatig wordt aangeroepen met een ongeldig configuratiebestand.
-111|Kan niet controleren agent te starten.<p><p>Oplossing: Controleer of dat er voldoende systeembronnen beschikbaar zijn.
-112|Algemene fout


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostische gegevens is niet aangemeld met Azure Storage
Alle gegevens Azure diagnostische gegevens opgeslagen in Azure Storage.

De meest voorkomende oorzaak van het ontbrekende gebeurtenisgegevens zijn onjuist gedefinieerde opslag-accountgegevens.

Oplossing: Corrigeer het configuratiebestand diagnostische hulpprogramma's en diagnostische gegevens opnieuw te installeren.
Als het probleem zich blijft voordoen nadat de extensie diagnostische gegevens opnieuw te installeren en vervolgens mogelijk moet u fouten opsporen in verdere door ze opzoeken via de controle agent fouten. Voordat u gebeurtenisgegevens wordt geüpload naar uw account opslag wordt deze opgeslagen in de LocalResourceDirectory.

Voor Cloud Service rol die de LocalResourceDirectory is:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Voor virtuele Machines is de LocalResourceDirectory:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Als er geen bestanden in de map LocalResourceDirectory, wordt de controle-agent is niet kunt starten. Dit is meestal veroorzaakt door een ongeldig configuratiebestand, een gebeurtenis die moet worden gerapporteerd in de CommandExecution.log.

Als de controle-Agent succes van gebeurtenisgegevens verzamelen is ziet u de bestanden .tsf voor elke gebeurtenis die is gedefinieerd in het configuratiebestand. De controle-Agent logboeken op fouten in het bestand MaEventTable.tsf. Als u wilt controleren van de inhoud van dit bestand kunt u de toepassing tabel2csv het .tsf-bestand converteren naar een bestand met door komma's gescheiden values(.csv):

Klik op de rol van een Cloud-Service:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*% Systeemstation %* van de rol van een Cloud-Service is meestal D:

Klik op een virtuele Machine:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

De bovenstaande opdrachten genereert de log bestand *maeventtable.csv*, die u kunt openen en controleren voor foutberichten.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnostische gegevens tabellen niet gevonden
De tabellen in Azure opslag die gegevens van Azure diagnostisch hulpprogramma heten met de volgende code:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Hier volgt een voorbeeld:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

Die 4 tabellen wordt gegenereerd:

Gebeurtenis|Tabelnaam
---|---
Provider = "prov1" &lt;gebeurtenis-id = '1' /&gt;|WADEvent + MD5("prov1") + '1'
Provider = "prov1" &lt;gebeurtenis-id "2" eventDestination = "dest1" = /&gt;|WADdest1
Provider = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
Provider = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt;|WADdest2
