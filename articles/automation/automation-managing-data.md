<properties 
   pageTitle="Gegevens van Azure automatisering beheren | Microsoft Azure"
   description="In dit artikel bevat meerdere onderwerpen voor het beheren van een Azure automatisering-omgeving.  Momenteel bevat Gegevensretentie en een back-up Azure automatisering herstel in Azure automatisering."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Azure automatisering gegevens beheren

In dit artikel bevat meerdere onderwerpen voor het beheren van een Azure automatisering-omgeving.

## <a name="data-retention"></a>Gegevensretentie

Wanneer u een resource in Azure automatisering verwijdert, blijft deze 90 dagen ter controle voordat het wordt blijvend verwijderd.  U kunt geen zien of de resource gebruiken tijdens deze periode.  Dit beleid geldt ook voor resources die horen bij een account automatisering die is verwijderd.

Azure automatisering wordt automatisch verwijderd en verwijdert u taken die ouder zijn dan 90 dagen duurt.

De volgende tabel bevat een overzicht van het bewaarbeleid voor verschillende resources.

|Gegevens|Beleid|
|:---|:---|
|Accounts|90 dagen nadat het account is verwijderd door een gebruiker het definitief verwijderd.|
|Activa|Worden blijvend verwijderd 90 dagen nadat de activa is verwijderd door een gebruiker of 90 dagen na de rekening met dat de activa is verwijderd door een gebruiker.|
|Modules|Worden blijvend verwijderd 90 dagen nadat de module is verwijderd door een gebruiker of 90 dagen na de rekening met dat de module is verwijderd door een gebruiker.|
|Runbooks|Worden blijvend verwijderd 90 dagen nadat de resource is verwijderd door een gebruiker of 90 dagen na de rekening met dat de resource is verwijderd door een gebruiker.|
|Taken|Verwijderde en worden blijvend verwijderd 90 dagen na de laatste worden gewijzigd. Dit kan zijn nadat de taak is voltooid, wordt gestopt, of is geschorst.|
|Knooppunt configuraties/MOF-bestanden| Configuratie van de oude knooppunt wordt blijvend verwijderd van 90 dagen na de configuratie van een nieuwe knooppunt wordt gegenereerd.|
|DSC knooppunten| Worden blijvend verwijderd 90 dagen nadat het knooppunt is niet geregistreerd van automatisering Account Azure portal of de [Registratie-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) -cmdlet gebruiken in Windows PowerShell. Knooppunten worden 90 dagen na de rekening met dat het knooppunt is verwijderd door een gebruiker ook permanent verwijderd. |
|Knooppunt rapporten| Worden blijvend verwijderd 90 dagen nadat een nieuw rapport voor dat knooppunt is gegenereerd|

Het bewaarbeleid geldt voor alle gebruikers en momenteel kan niet worden aangepast.

## <a name="backing-up-azure-automation"></a>Een back-up Azure automatisering

Wanneer u een account automatisering in Microsoft Azure verwijdert, worden alle objecten in het account verwijderd inclusief runbooks, modules, configuraties, instellingen, taken en activa. De objecten kunnen niet worden hersteld nadat het account is verwijderd.  U kunt de volgende informatie back-up maken de inhoud van uw account automatisering voordat deze wordt verwijderd. 

### <a name="runbooks"></a>Runbooks

U kunt uw runbooks exporteren naar scriptbestanden met de Azure-beheerportal of de cmdlet [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) in Windows PowerShell.  Deze scriptbestanden kunnen worden geïmporteerd in een ander automatisering-account, zoals is beschreven in [maken of importeren van een Runbook](https://msdn.microsoft.com/library/dn643637.aspx).


### <a name="integration-modules"></a>Van integratiemodules

U kunt geen integratiemodules exporteren vanuit Azure automatisering.  Moet u ervoor zorgen dat deze buiten het account dat Automatisering beschikbaar zijn.

### <a name="assets"></a>Activa

U kunt geen [activa](https://msdn.microsoft.com/library/dn939988.aspx) exporteren vanuit Azure automatisering.  De beheerportal Azure gebruikt, moet u de details van variabelen, referenties certificaten, verbindingen en planningen opmerking.  Vervolgens moet u handmatig maken elementen die worden gebruikt door runbooks die u in een andere automatisering importeert.

U kunt [Azure cmdlets](https://msdn.microsoft.com/library/dn690262.aspx) ophalen van de details van niet-versleutelde activa en hetzij opslaan voor toekomstig gebruik of gelijkwaardige activa maken in een ander automatisering-account.

U kunt de waarde voor versleutelde variabelen of het veld voor wachtwoord Gebruik cmdlets referenties niet ophalen.  Als u deze waarden niet weet, klikt u vervolgens u kunt deze weer ophalen uit een runbook met de [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) en [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) activiteiten.

U kunt geen certificaten exporteren vanuit Azure automatisering.  Moet u ervoor zorgen dat alle certificaten buiten Azure beschikbaar zijn.

### <a name="dsc-configurations"></a>DSC configuraties

U kunt uw configuraties exporteren naar scriptbestanden met de Azure-beheerportal of de cmdlet [Exporteren-AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) in Windows PowerShell. Deze configuraties kunnen worden geïmporteerd en gebruikt in een ander automatisering-account.


##<a name="geo-replication-in-azure-automation"></a>Geografische-herhaling in Azure automatisering

Geografische-replicatie, standaard in Azure automatisering-accounts back-ups van gegevens naar een ander geografisch gebied voor redundantie. U kunt een primaire regio kiezen bij het instellen van uw account en vervolgens een secundaire regio is toegewezen aan deze automatisch. Secundaire gegevens hebt gekopieerd uit het gebied van de primaire continu worden bijgewerkt voor het geval verlies van gegevens.  

De volgende tabel ziet u de koppelingen beschikbaar primaire en secundaire regio.

|Primaire            |Secundaire
| ---------------   |----------------
|Zuid-Central US   |Centraal Noord-Amerikaanse
|Amerikaans Oost 2          |Central US
|West Europa        |Noord-Europa
|Zuid-Oost-Azië    |Oost-Azië
|Japan Oost         |Japan West

In zelden dat de gegevens voor een primaire regio verbroken is, probeert Microsoft te herstellen. Als de primaire gegevens kan niet worden hersteld, klikt u vervolgens geografische-failover wordt uitgevoerd en de desbetreffende klanten wordt gewaarschuwd bij dit via hun abonnement.

