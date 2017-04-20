<properties
   pageTitle="Synchronisatie van Azure AD Connect: Scheduler | Microsoft Azure"
   description="In dit onderwerp worden de ingebouwde scheduler-functie in Azure AD Connect synchroniseren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Synchronisatie van Azure AD Connect: Scheduler
In dit onderwerp worden de ingebouwde planner in Azure AD Connect synchroniseren (ook synchronisatie-engine).

Deze functie is geïntroduceerd met opbouwen 1.1.105.0 (uitgebracht februari 2016).

## <a name="overview"></a>Overzicht
Azure AD Connect-synchronisatie worden de wijzigingen die er in uw on-premises adreslijst met een scheduler gesynchroniseerd. Er zijn twee scheduler processen, één voor Wachtwoordsynchronisatie en andere voor het object/kenmerk synchroniseren en onderhoudstaken. In dit onderwerp wordt uitgelegd hoe de laatste.

In eerdere versies de planner voor objecten en kenmerken buiten de synchronisatie-engine is en de Taakplanner van Windows of een afzonderlijke Windows-service is gebruikt voor het starten van het synchronisatieproces. De planner is met de ingebouwde 1.1 releases voor de synchronisatie engine, en hiervoor sommige aanpassen. De frequentie voor synchronisatie van nieuwe standaard is 30 minuten.

De planner is verantwoordelijk voor twee taken:

- **Synchronisatie bladeren**. Het proces voor het importeren en exporteren van wijzigingen synchroniseren.
- **Onderhoudstaken**. Verlengen sleutels en certificaten voor wachtwoorden opnieuw instellen en apparaat registratie Service (DRS). Oude vermeldingen in het logboek bewerkingen wissen.

De planner zelf altijd wordt uitgevoerd, maar dit kan worden geconfigureerd alleen een of geen van deze taken uitvoeren. Bijvoorbeeld als u beschikken over uw eigen synchronisatie cyclus proces moet, kunt u deze taak in de planner uitschakelen maar nog steeds de onderhoudstaak uitvoeren.

## <a name="scheduler-configuration"></a>Taakplanner configureren
Als u wilt uw huidige configuratie-instellingen zien, gaat u naar PowerShell en uitvoeren `Get-ADSyncScheduler`. Dit wordt uitgelegd u ongeveer zo uitziet:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Als u **de synchronisatieopdracht of de cmdlet is niet beschikbaar** wanneer u deze cmdlet uitvoeren, wordt klikt u vervolgens de PowerShell-module niet geladen. Dit kan gebeuren als u Azure AD Connect op een domeincontroller of op een server met hogere niveaus van PowerShell beperking dan standaardinstellingen uitvoert. Als u deze fout wordt weergegeven, voert u `Import-Module ADSync` de cmdlet om beschikbaar te maken.

- **AllowedSyncCycleInterval**. Synchronisatie van voordoen kunt het meest van Azure AD. U vaker kan niet worden gesynchroniseerd en nog steeds worden ondersteund.
- **CurrentlyEffectiveSyncCycleInterval**. De planning momenteel in feite. Heeft hetzelfde resultaat als CustomizedSyncInterval (als instellen) als het is niet vaker dan AllowedSyncInterval. Als u CustomizedSyncCycleInterval wijzigt, wordt dit pas van kracht na het volgende synchronisatie cyclus.
- **CustomizedSyncCycleInterval**. Als u de planner om uit te voeren met een andere frequentie dan het standaardgeluid 30 minuten wilt, configureert u deze instelling. In de afbeelding boven de planner is ingesteld op elk uur in plaats daarvan uitvoeren. Als u dit naar een waarde lager dan AllowedSyncInterval, wordt u deze gebruikt.
- **NextSyncCyclePolicyType**. Delta of initiaal. Hiermee definieert u als de volgende sessie alleen proces deltawijzigingen moet, of de volgende sessie moet doen als een volledige importeren en synchroniseren, die ook opnieuw de nieuwe of gewijzigde regels wilt verwerken.
- **NextSyncCycleStartTimeInUTC**. Zodra de planner zijn verzonden, de volgende synchronisatie cyclus.
- **PurgeRunHistoryInterval**. De tijd bewerking logboeken moeten worden bewaard. Deze kunnen worden gecontroleerd in het servicebeheer van synchronisatie. De standaardinstelling is dat deze zeven dagen.
- **SyncCycleEnabled**. Geeft aan als de planner de importeren, synchroniseren en exporteren processen wordt uitgevoerd als onderdeel van de werking ervan.
- **MaintenanceEnabled**. Geeft als het onderhoud-proces is ingeschakeld. Er wordt de certificaten/toetsen bijwerken en verwijderen van het logboek bewerkingen.
- **IsStagingModeEnabled**. Geeft als [tijdelijke modus](active-directory-aadconnectsync-operations.md#staging-mode) is ingeschakeld.

Kunt u sommige van deze instellingen met `Set-ADSyncScheduler`. De volgende parameters kunnen worden gewijzigd:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

De configuratie scheduler wordt opgeslagen in Azure AD. Als u een tijdelijk opslaan server hebt, wordt er een wijziging op de primaire server de server tijdelijk opslaan (met uitzondering van IsStagingModeEnabled) ook effect.

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Syntaxis:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dagen, [HH - uur, mm - minuten, ss - seconden

Voorbeeld:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Wordt de planner om uit te voeren om 3 uur gewijzigd.

Voorbeeld:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Wordt de planner om te worden dagelijks uitgevoerd gewijzigd.

## <a name="start-the-scheduler"></a>De scheduler starten
Al dan niet standaard uitgevoerd de planner elke 30 minuten. In sommige gevallen wilt u mogelijk een cyclus synchroniseren tussen de geplande maal uitvoeren of voor het uitvoeren van een ander type.

**Delta synchronisatie cyclus**  
Een cyclus van de synchronisatie delta bevat de volgende stappen uit:

- Delta importeren op alle verbindingslijnen
- Delta synchroniseren op alle verbindingslijnen
- Klik op alle verbindingslijnen exporteren

Kan het zijn dat er een dringende wijzigen die moet worden gesynchroniseerd onmiddellijk daarom moet u handmatig uitvoeren van een cyclus. Als u moet uitvoeren een cyclus, handmatig vervolgens vanaf PowerShell uitvoeren `Start-ADSyncSyncCycle -PolicyType Delta`.

**Volledige synchronisatie cyclus**  
Als u een van de volgende configuratiewijzigingen hebt aangebracht, moet u een volledige synchronisatie cyclus (ook uitvoeren Initiaal):

- Meer objecten of kenmerken te importeren vanuit een adreslijst bron toegevoegd
- Wijzigingen in de synchronisatie-regels
- Gewijzigd [filteren](active-directory-aadconnectsync-configure-filtering.md) zodat een verschillend aantal objecten opgenomen worden moet

Als u een van deze wijzigingen hebt aangebracht, moet u een volledige synchronisatie cyclus voeren zodat de synchronisatie-engine de mogelijkheid heeft om opnieuw de verbindingslijn spaties samenvoegen. Een volledige synchronisatie cyclus bevat de volgende stappen uit:

- Volledige importeren op alle verbindingslijnen
- Volledige synchroniseren op alle verbindingslijnen
- Klik op alle verbindingslijnen exporteren

Als u wilt de cyclus van een volledige synchronisatie starten, uitvoeren `Start-ADSyncSyncCycle -PolicyType Initial` vanuit een PowerShell-prompt. Hiermee start u de cyclus van een volledige synchroniseren.

## <a name="stop-the-scheduler"></a>De scheduler stoppen
Als de planner is momenteel een synchronisatie cyclus die mogelijk moet u deze niet meer actief. Als u de installatiewizard te starten en u dit foutbericht krijgt bijvoorbeeld:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Wanneer de cyclus van een synchronisatie wordt uitgevoerd, kunt u configuratiewijzigingen aanbrengen. U kunt wachten tot de planner het proces is voltooid, maar u dit ook stoppen kunt zodat u meteen uw wijzigingen kunt aanbrengen. De huidige cyclus stoppen is niet schadelijk en wijzigingen nog steeds niet verwerkt worden verwerkt met de volgende keer wordt uitgevoerd.

1. Begint u met de scheduler stoppen met de huidige cyclus met de PowerShell-cmdlet vertellen `Stop-ADSyncSyncCycle`.
2. De planner stoppen, wordt de huidige verbindingslijn van de huidige taak niet gestopt. Als u wilt dat de verbindingslijn om te stoppen, de volgende acties uitvoeren: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - **Synchronisatie-Service** starten vanuit het startmenu. Ga naar **verbindingslijnen**, markeer de verbindingslijn met de status die wordt **uitgevoerd** en selecteert u **stoppen** uit de acties.

De planner nog steeds actief is en opnieuw begint op de volgende verkoopkans.

## <a name="custom-scheduler"></a>Aangepaste scheduler
De cmdlets beschreven in deze sectie zijn alleen beschikbaar in opbouwen [1.1.130.0](active-directory-aadconnect-version-history.md#111300) en hoger.

Als de ingebouwde planner niet aan uw vereisten voldoet, kunt u de verbindingslijnen via PowerShell plannen.

### <a name="invoke-adsyncrunprofile"></a>Roepen ADSyncRunProfile
U kunt een profiel starten voor een verbindingslijn op deze manier:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

De namen die worden gebruikt voor [de namen van de verbindingslijn](active-directory-aadconnectsync-service-manager-ui-connectors.md) en [profiel uitvoeren](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) vindt u in de [Gebruikersinterface van de synchronisatie-servicebeheer](active-directory-aadconnectsync-service-manager-ui.md).

![Roepen uitvoeren profiel](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

De `Invoke-ADSyncRunProfile` cmdlet synchroon is, dat wil zeggen wordt niet geretourneerd besturingselement totdat de verbindingslijn is de bewerking voltooid is of met een fout.

Wanneer u uw verbindingslijnen plant, wordt de aanbeveling inplannen in de volgende volgorde:

1. (Volledige/Delta) Importeren vanuit een on-premises implementatie mappen, zoals Active Directory
2. (Volledige/Delta) Importeren uit Azure AD
3. (Volledige/Delta) Synchronisatie van adreslijsten van de on-premises implementatie, zoals Active Directory
4. (Volledige/Delta) Synchronisatie van Azure AD
5. Exporteren naar Azure AD
6. Exporteren naar on-premises implementatie mappen, zoals Active Directory

Als u de ingebouwde planner bekijkt, is dit de volgorde die kunnen worden uitgevoerd door de verbindingslijnen.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
U kunt ook de synchronisatie-engine om te zien als deze bezet of niet-actieve is controleren. Deze cmdlet retourneert een leeg resultaat als de synchronisatie-engine niet actief is en niet een verbindingslijn wordt uitgevoerd. Als een verbindingslijn wordt uitgevoerd, wordt de naam van de verbindingslijn geretourneerd.

```
Get-ADSyncConnectorRunStatus
```

![Connector Status uitvoeren](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
In de bovenstaande afbeelding is de eerste regel een plaats waar de synchronisatie-engine niet actief is. De tweede regel uit wanneer de Azure AD-Connector wordt uitgevoerd.

## <a name="scheduler-and-installation-wizard"></a>Wizard Scheduler / installatie
Als u de installatiewizard te starten, wordt klikt u vervolgens de planner worden tijdelijk geschorst. Dit is omdat wordt aangenomen brengt u configuratiewijzigingen en deze kunnen niet worden toegepast als de synchronisatie-engine actief wordt uitgevoerd. Daarom laat niet de installatiewizard openen aangezien stopt de synchronisatie-engine synchronisatie bewerkingen uitvoeren.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
