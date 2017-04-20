<properties
   pageTitle="Azure AD Connect: Automatische upgrade | Microsoft Azure"
   description="In dit onderwerp worden de ingebouwde automatische functie voor het upgraden in Azure AD Connect synchroniseren."
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
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Automatische upgrade
Deze functie is geïntroduceerd met opbouwen 1.1.105.0 (uitgebracht februari 2016).

## <a name="overview"></a>Overzicht
Ervoor te zorgen dat uw installatie Azure AD Connect is altijd up-to-date is dan ooit gemakkelijker met de functie **Automatische upgrade** . Deze functie is standaard ingeschakeld voor express installaties en DirSync upgrades. Wanneer u een nieuwe versie loslaat, wordt uw installatie automatisch bijgewerkt.

Automatische upgrade is standaard ingeschakeld voor het volgende:

- Instellingen voor installatie- en DirSync upgrades Express.
- SQL Express LocalDB gebruikt, dit is wat Express instellingen altijd gebruiken. DirSync met SQL Express ook LocalDB gebruiken.
- De AD-account is het standaardaccount MSOL_ gemaakt door Express-instellingen en DirSync.
- Minder dan 100.000 objecten in de metaverse hebben.

De huidige status van een automatische upgrade kan worden weergegeven met de PowerShell-cmdlet `Get-ADSyncAutoUpgrade`. Deze heeft de volgende staten:

De staat | Opmerking
---- | ----
Ingeschakeld | Automatische upgrade is ingeschakeld.
Geschorst | Stel door alleen het systeem. Het systeem is niet meer in aanmerking voor automatische upgrades.
Uitgeschakeld | Automatische upgrades is uitgeschakeld.

U kunt schakelen tussen **ingeschakeld** en **uitgeschakeld** met `Set-ADSyncAutoUpgrade`. Alleen het systeem moet de status **geschorst**instellen.

Automatische upgrade is Azure AD verbinding systeemstatus gebruikt voor de upgrade infrastructuur. Zorg dat u hebt de URL's geopend in uw proxyserver voor **Azure AD verbinding systeemstatus** zoals beschreven in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)voor automatische upgrade om te werken.

Als de **Synchronisatie-servicebeheer** UI op de server wordt uitgevoerd, is klikt u vervolgens de upgrade geblokkeerd totdat de gebruikersinterface is gesloten.

## <a name="troubleshooting"></a>Problemen oplossen
Als uw Connect-installatie niet zelf bijgewerkt zoals verwacht, voert u deze stappen om vast te stellen wat mogelijk onjuist.

U moet eerst niet verwacht dat de automatische upgrade naar de eerste dag die een nieuwe versie wordt uitgebracht worden uitgevoerd. Er is een opzettelijk onzekerheid voordat een upgrade is uitgevoerd, dus normaal als de installatie is niet direct bijgewerkt.

Als u denkt iets niet goed is dat, klikt u vervolgens voert u eerst `Get-ADSyncAutoUpgrade` om ervoor te zorgen automatische upgrade is ingeschakeld.

Controleer vervolgens of dat u de gewenste URL's in uw proxy of firewall hebt geopend. Automatische updates is Azure AD verbinding servicestatus gebruiken zoals beschreven in het [Overzicht](#overview). Als u een proxyserver gebruikt, zorg er dan voor dat status is geconfigureerd voor het gebruik van een [proxyserver](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Ook de [status connectiviteit](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) met Azure AD testen.

Met de connectiviteit met Azure AD geverifieerd, is het tijd om te zoeken in de gebeurtenislogboeken. Start Logboeken en kijkt u in het gebeurtenislogboek **toepassing** . Een filter gebeurtenislogboek voor de bron **Azure AD Connect Upgrade** en de gebeurtenis-id bereik **300-399**toevoegen.  
![Gebeurtenislogboek filter voor automatische upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

U ziet nu de gebeurtenislogboeken die is gekoppeld aan het veld status voor automatische upgrade.  
![Gebeurtenislogboek filter voor automatische upgrade](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

De resultaatcode heeft een voorvoegsel een goed beeld van de staat.

Resultaat code voorvoegsel | Beschrijving
--- | ---
Succes | De installatie is bijgewerkt.
UpgradeAborted | Een tijdelijke voorwaarde gestopt de upgrade. Dit wordt opnieuw geprobeerd opnieuw en de verwachting is dat deze later lukt.
UpgradeNotSupported | Er is een configuratie die wordt geblokkeerd door het systeem automatisch wordt bijgewerkt. Dit wordt opnieuw geprobeerd om te zien als de status wordt gewijzigd, maar de verwachting is dat het systeem handmatig moet worden bijgewerkt.

Hier volgt een lijst met de meest voorkomende berichten die u hebt gevonden. Deze lijst bevat niet alle, maar het resultaat bericht moet wissen met wat het probleem is.

Resultaat bericht | Beschrijving
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Kan niet in het register schrijven.
UpgradeAbortedInsufficientDatabasePermissions | De ingebouwde beheerdersgroep heeft geen machtigingen voor de database. Handmatig een upgrade uitvoeren naar de nieuwste versie van Azure AD Connect om dit probleem te verhelpen.
UpgradeAbortedInsufficientDiskSpace | Er is voldoende schijfruimte om een upgrade te ondersteunen.
UpgradeAbortedSecurityGroupsNotPresent | Kan niet vinden en alle beveiligingsgroepen gebruikt door de synchronisatie-engine oplossen.
UpgradeAbortedServiceCanNotBeStarted | De NT-Service **Microsoft Azure AD-synchronisatie** kan niet worden gestart.
UpgradeAbortedServiceCanNotBeStopped | De NT-Service **Microsoft Azure AD-synchronisatie** kan niet stoppen.
UpgradeAbortedServiceIsNotRunning | De NT-Service **Microsoft Azure AD-synchronisatie** wordt niet uitgevoerd.
UpgradeAbortedSyncCycleDisabled | De optie SyncCycle in de [planner](active-directory-aadconnectsync-feature-scheduler.md) is uitgeschakeld.
UpgradeAbortedSyncExeInUse | De [synchronisatie-servicebeheer UI](active-directory-aadconnectsync-service-manager-ui.md) is geopend op de server.
UpgradeAbortedSyncOrConfigurationInProgress | De installatiewizard wordt uitgevoerd, of een synchronisatie buiten de planner is gepland.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | U kunt uw eigen aangepaste regels hebt toegevoegd aan de configuratie.
UpgradeNotSupportedDeviceWritebackEnabled | U kunt de functie [apparaat write-backs](active-directory-aadconnect-feature-device-writeback.md) hebt ingeschakeld.
UpgradeNotSupportedGroupWritebackEnabled | U kunt de functie [groep write-backs](active-directory-aadconnect-feature-preview.md#group-writeback) hebt ingeschakeld.
UpgradeNotSupportedInvalidPersistedState | De installatie is niet een Express-instellingen of een upgrade DirSync.
UpgradeNotSupportedMetaverseSizeExceeeded | U hebt meer dan 100.000 objecten in de metaverse.
UpgradeNotSupportedMultiForestSetup | U verbinding maakt met meer dan één bos. Express-instelling is alleen maakt verbinding met één bos.
UpgradeNotSupportedNonLocalDbInstall | U maakt geen gebruik van een database van SQL Server Express LocalDB.
UpgradeNotSupportedNonMsolAccount | Het [AD-Connector-account](active-directory-aadconnect-accounts-permissions.md#active-directory-account) is niet het standaardaccount MSOL_ meer.
UpgradeNotSupportedStagingModeEnabled | De server is ingesteld in de [modus waarin u tijdelijk opslaat](active-directory-aadconnectsync-operations.md#staging-mode).
UpgradeNotSupportedUserWritebackEnabled | U kunt de functie van de [gebruiker write-backs](active-directory-aadconnect-feature-preview.md#user-writeback) hebt ingeschakeld.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
