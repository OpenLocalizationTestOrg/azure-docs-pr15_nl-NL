<properties
   pageTitle="Azure AD Connect: Geschiedenis van documentversies Release | Microsoft Azure"
   description="Dit onderwerp ziet u alle versies van Azure AD Connect en Azure AD-synchronisatie"
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
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Geschiedenis van documentversies Release

Azure AD Connect de Azure Active Directory-team regelmatig bijgewerkt met de nieuwe functies en functionaliteit. Niet alle toevoegingen zijn van toepassing op alle doelgroepen.

In dit artikel is bedoeld om te helpen u de versies die zijn vrijgegeven bijhouden, en om te begrijpen of u wilt bijwerken naar de nieuwste versie of niet.

Dit is een lijst met verwante onderwerpen:

Onderwerp |  
--------- | --------- |
Stappen voor het upgraden van Azure AD Connect | Verschillende methoden voor het [upgraden van een eerdere versie naar de laatste](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect release.
Vereiste machtigingen | Zie [accounts en machtigingen](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade) voor machtigingen vereist om een update toepassen
Downloaden| [Download Azure AD verbinding maken](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Uitgebracht: 2016 augustus

**Vaste problemen:**

- Wijzigingen in het interval synchroniseren niet plaatsvindt tot nadat volgende synchronisatie cyclus is voltooid.
- Azure AD Connect-wizard accepteert geen Azure AD-account waarvan gebruikersnaam met een onderstrepingsteken begint (\_).
- Azure AD Connect-wizard mislukt om te verifiëren Azure AD-account als het wachtwoord van account te veel speciale tekens bevat. Foutbericht 'kan geen referenties valideren. Een onverwachte fout opgetreden." Er wordt geretourneerd.
- Verwijderen van server tijdelijke Wachtwoordsynchronisatie in Azure AD-tenant uitschakelt en zorgt ervoor dat Wachtwoordsynchronisatie met active server is mislukt.
- Wachtwoordsynchronisatie mislukt in niet vaak voorkomende gevallen wanneer er geen wachtwoordhash die zijn opgeslagen op de gebruiker.
- Wanneer Azure AD Connect-server is geconfigureerd voor het tijdelijk opslaan modus, is niet tijdelijk wachtwoord write-backs uitgeschakeld.
- Azure AD Connect-wizard wordt de werkelijke Wachtwoordsynchronisatie en het wachtwoord write-backs configuratie niet weergegeven, wanneer de server onderdeel is in de modus waarin u tijdelijk opslaat. Hierin zien altijd ze uitgeschakeld.
- Configuratiewijzigingen Wachtwoordsynchronisatie en wachtwoord write-backs worden niet door de wizard Azure AD Connect behouden wanneer de server onderdeel is in de modus waarin u tijdelijk opslaat.

**Verbeteringen:**

- Bijgewerkte Start-ADSyncSyncCycle-cmdlet om aan te geven of deze kunnen juist een nieuwe synchronisatie cyclus al dan niet kan worden gestart.
- Toegevoegde stoppen-ADSyncSyncCycle-cmdlet beëindigen synchronisatie cyclus en bewerking die momenteel uitgevoerd worden.
- Bijgewerkte stoppen-ADSyncScheduler-cmdlet beëindigen synchronisatie cyclus en bewerking die momenteel uitgevoerd worden.
- Tijdens het configureren van [Directory extensies](active-directory-aadconnectsync-feature-directory-extensions.md) in de wizard Azure AD Connect, kan het zijn dat nu AD-kenmerk van het type 'Teletex tekenreeks' worden geselecteerd.

## <a name="111890"></a>1.1.189.0
Uitgebracht: 2016 juni

**Vaste problemen en verbeteringen:**

- Azure AD Connect kan nu worden geïnstalleerd op een compatibel FIPS-server.
    - Zie [Wachtwoordsynchronisatie en FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips) voor synchronisatie van wachtwoorden
- Een probleem opgelost waar een NetBIOS-naam kan niet worden omgezet naar de FQDN-naam in de Active Directory-Connector.

## <a name="111800"></a>1.1.180.0
Uitgebracht: 2016 mei

**Nieuwe functies:**

- Worden gewaarschuwd en helpt u bij het verifiëren van domeinen als u deze voordat u Azure AD Connect niet hebt gedaan.
- Ondersteuning voor [Microsoft Cloud Duitsland](active-directory-aadconnect-instances.md#microsoft-cloud-germany)toegevoegd.
- Ondersteuning voor de meest recente cloudinfrastructuur van [Microsoft Azure overheid](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) aan de nieuwe URL die zijn toegevoegd.

**Vaste problemen en verbeteringen:**

- Toegevoegd aan de synchronisatie regel Editor zodat u deze gemakkelijk te vinden zijn synchronisatie regels filteren.
- Verbeterde prestaties bij het verwijderen van een spatie verbindingslijn.
- Een problemen opgelost wanneer hetzelfde object is verwijderd en toegevoegd in dezelfde uitvoeren (genoemd verwijderen/toevoegen).
- Een uitgeschakelde synchronisatie-regel wordt niet meer opnieuw inschakelen opgenomen objecten en kenmerken van upgrade- of map schema vernieuwen.

## <a name="111300"></a>1.1.130.0
Uitgebracht: 2016 in April

**Nieuwe functies:**

- Ondersteuning voor meerdere waarden kenmerken naar [Directory extensies](active-directory-aadconnectsync-feature-directory-extensions.md)toegevoegd.
- Ondersteuning voor meer configuratie variaties voor [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) beschouwd in aanmerking komen voor de upgrade.
- Sommige cmdlets voor [aangepaste scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)toegevoegd.

## <a name="111190"></a>1.1.119.0
Uitgebracht: 2016 maart

**Vaste problemen:**

- Aangebracht wordt ervoor snelle installatie kan niet worden gebruikt op Windows Server 2008 (pre-R2) sinds Wachtwoordsynchronisatie niet ondersteund op dit besturingssysteem.
- Upgrade van DirSync met een aangepast filter-configuratie werkt niet zoals verwacht.
- Bij een upgrade naar een nieuwere versie en er zijn geen wijzigingen in de configuratie, een volledige importeren/synchronisatie niet moet worden gepland.

## <a name="111100"></a>1.1.110.0
Uitgebracht: 2016 februari

**Vaste problemen:**

- Upgrade van eerdere versies werkt niet als de installatie is niet in de standaardmap voor **C:\Program Files** .
- Als u installeert en **Start de synchronisatie-proces..** deselecteren aan het einde van de installatiewizard, de installatiewizard opnieuw uit te voeren niet mogelijk de planner.
- De scheduler werkt niet zoals verwacht op servers waar de datum/tijd-indeling niet Amerikaans-nl is. Ook worden geblokkeerd `Get-ADSyncScheduler` om terug te keren juiste tijden.
- Als u een eerdere versie van Azure AD Connect met ADFS als de optie aanmelden en een upgrade hebt geïnstalleerd, niet kunt u de installatiewizard opnieuw uitvoeren.

## <a name="111050"></a>1.1.105.0
Uitgebracht: 2016 februari

**Nieuwe functies:**

- [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) -functie voor Express instellingen klanten.
- Ondersteuning voor de globale beheerder MFA en PIM gebruiken in de installatiewizard.
    - Moet u toestaan dat uw proxy waarmee ook verkeer naar https://secure.aadcdn.microsoftonline-p.com als u MFA gebruikt.
    - U moet https://secure.aadcdn.microsoftonline-p.com toevoegen aan uw lijst met vertrouwde websites voor MFA goed werken.
- Het wijzigen van de gebruiker aanmeldingsproblemen methode na de eerste installatie toegestaan.
- [Domein en organisatie-eenheid filteren](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) in de installatiewizard toestaan. Hierdoor kunnen ook verbinding maken met forests waarin niet alle domeinen beschikbaar zijn.
- [Planner](active-directory-aadconnectsync-feature-scheduler.md) is ingebouwd in de synchronisatie-engine.

**Functies van voorbeeldversie gepromoveerd tot GA:**

- [Apparaat write-backs](active-directory-aadconnect-feature-device-writeback.md).
- [Directory-extensies](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nieuwe functies van de Preview-versie:**

- De nieuwe standaardindeling synchroniseren cyclus interval is 30 minuten. 3 uur voor alle eerdere versies worden gebruikt. Ondersteuning als u wilt wijzigen van het gedrag [scheduler](active-directory-aadconnectsync-feature-scheduler.md) toevoegen.

**Vaste problemen:**

- De pagina verifiëren DNS-domeinen herkent niet altijd de domeinen.
- Aanwijzingen voor het domein beheerdersreferenties tijdens het configureren van ADFS.
- Het lokale AD-accounts worden niet herkend door de installatiewizard als in een domein met een andere DNS-structuur dan het hoofddomein bevindt.

## <a name="1091310"></a>1.0.9131.0
Uitgebracht: 2015 December

**Vaste problemen:**

- Wachtwoordsynchronisatie werken mogelijk niet wanneer u wachtwoorden in AD DS, maar werkt wijzigen wanneer u wachtwoord instelt.
- Wanneer u een proxyserver hebt, Azure AD-verificatie mislukt mogelijk tijdens de installatie of schakel op de configuratiepagina de upgrade.
- Bijwerken vanuit een eerdere versie van Azure AD Connect met een volledige loopt SQL Server vast als u niet SA in SQL bent.
- Bijwerken vanuit een eerdere versie van Azure AD Connect met een externe wordt SQL Server de fout 'Kan geen toegang tot de ADSync SQL-database' weergegeven.

## <a name="1091250"></a>1.0.9125.0
Uitgebracht: 2015 November

**Nieuwe functies:**

- Kan de configuratie van de ADFS naar Azure AD vertrouwen.
- Kan worden vernieuwd Active Directory-schema en synchronisatie regels genereren.
- Kan een regel synchronisatie uitschakelen.
- Kunt "AuthoritativeNull" definiëren als een nieuwe letterlijke waarde in een regel voor het synchroniseren.

**Nieuwe functies van de Preview-versie:**

- [Azure AD verbinding maken met status voor synchronisatie](active-directory-aadconnect-health-sync.md).
- Ondersteuning voor Wachtwoordsynchronisatie [Azure AD Domain Services](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) .

**Nieuw ondersteunde scenario:**

- Ondersteunt meerdere lokale Exchange-organisaties. Zie [hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx) voor meer informatie.

**Vaste problemen:**

- Synchronisatieproblemen wachtwoord:
    - Een object verplaatst van buiten het bereik naar in het bereik geen het wachtwoord die zijn gesynchroniseerd. Deze incudes OU en kenmerk filteren.
    - Een nieuwe organisatie-eenheid om op te nemen in synchronisatie te selecteren, hoeft niet een volledige Wachtwoordsynchronisatie.
    - Wanneer een uitgeschakelde gebruiker is ingeschakeld wordt het wachtwoord wordt niet gesynchroniseerd.
    - Het wachtwoord opnieuw wachtrij is oneindig en de vorige limiet van 5000 objecten die u wilt worden teruggetrokken is verwijderd.
    - [Verbeterde probleemoplossing](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Geen verbinding maken met Active Directory met Windows Server 2016 bos-functionele niveau.
- Kan niet wijzigen van de groep die wordt gebruikt voor het filteren van de groep na de eerste installatie.
- Maakt een nieuw gebruikersprofiel niet meer op de server Azure AD Connect voor elke gebruiker een wachtwoordwijziging doen met wachtwoord write-backs ingeschakeld.
- Niet met lange Integer waarden synchroon regels bereiken.
- Het selectievakje "apparaat write-backs" blijft uitgeschakeld als er niet bereikbaar domeincontrollers.

## <a name="1086670"></a>1.0.8667.0
Uitgebracht: 2015 augustus

**Nieuwe functies:**

- De installatiewizard Azure AD Connect is nu vertaald voor alle talen voor Windows Server.
- Ondersteuning voor account ontgrendelen bij gebruik van Azure AD-wachtwoordbeheer.

**Vaste problemen:**

- Azure AD Connect-installatiewizard loopt vast als een andere gebruiker zich blijft voordoen installatie in plaats van de persoon die eerst de installatie wordt gestart.
- Als een eerdere installatie van Azure AD Connect mislukt Azure AD Connect synchronisatie correct verwijderen, is het niet mogelijk om opnieuw te installeren.
- Niet installeren Azure AD Connect snelle installatie gebruiken als de gebruiker zich niet in het hoofddomein van de bos of als een niet-Engelstalige versie van Active Directory wordt gebruikt.
- Als de FQDN-naam van de Active Directory-gebruikersaccount kan niet worden omgezet, wordt een misleidende foutbericht 'Kan niet worden doorvoeren van het schema' wordt weergegeven.
- Als het account in de Active Directory-Connector gebruikt is gewijzigd buiten de wizard, mislukt de wizard op volgende wordt uitgevoerd.
- Soms kan Azure AD Connect installeren op een domeincontroller.
- Niet kunt inschakelen en "Tijdelijke modus" uitschakelen als extensie kenmerken zijn toegevoegd.
- Wachtwoord write-backs wordt in sommige configuratie mislukt vanwege een onjuist wachtwoord op de Active Directory-Connector.
- DirSync kan niet worden bijgewerkt als dn wordt gebruikt in het kenmerk filteren.
- Overtollige CPU-gebruik bij gebruik van wachtwoorden opnieuw instellen.

**Verwijderde functies:**

- De voorbeeldfunctie [die gebruiker write-backs](active-directory-aadconnect-feature-preview.md#user-writeback) tijdelijk is verwijderd basis op van feedback van onze klanten preview. Deze worden opnieuw toegevoegd later wanneer de meegeleverde feedback is verholpen.

## <a name="1086410"></a>1.0.8641.0
Uitgebracht: 2015 juni

**Eerste release van Azure AD Connect.**

Gewijzigde naam van Azure AD-synchronisatie aan Azure AD Connect.

**Nieuwe functies:**

- [Instellingen voor snelle](./connect/active-directory-aadconnect-get-started-express.md) installatie
- Kan [ADFS configureren](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Kunt u [een upgrade uitvoert vanuit DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Niet per ongeluk verwijderen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- [Tijdelijke modus](active-directory-aadconnectsync-operations.md#staging-mode) geïntroduceerd

**Nieuwe functies van de Preview-versie:**

- [Gebruiker write-backs](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Groep write-backs](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Apparaat write-backs](active-directory-aadconnect-feature-device-writeback.md)
- [Directory-extensies](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Uitgebracht: 2015 mei

**Nieuwe vereiste:**

- Azure AD-synchronisatie is nu vereist .net framework versie 4.5.1 zijn geïnstalleerd.

**Vaste problemen:**

- Wachtwoord write-backs van Azure AD is met een fout servicebus-verbinding verbroken.

## <a name="104910413"></a>1.0.491.0413
Uitgebracht: 2015 April

**Vaste problemen en verbeteringen:**

- Het Active Directory-Connector worden verwijderen niet correct verwerkt als de Prullenbak is ingeschakeld en er meerdere domeinen in de bos.
- De prestaties van importbewerkingen is verbeterd voor de Azure Active Directory-Connector.
- Wanneer een groep de lidmaatschap limiet heeft overschreden (standaard de limiet is ingesteld op 50k objecten), de groep is verwijderd in Azure Active Directory. Het nieuwe gedrag is dat de groep blijft, wordt een fout gegenereerd en geen nieuwe lidmaatschapswijzigingen worden geëxporteerd.
- Een nieuw object kan niet worden ingericht als een gefaseerde verwijderen met de dezelfde DN al aanwezig zijn in de ruimte verbindingslijn.
- Sommige objecten zijn gemarkeerd voor wordt gesynchroniseerd tijdens een synchronisatie delta Hoewel er geen wijziging van de gefaseerde op het object is.
- Een Wachtwoordsynchronisatie dwingen, verwijdert ook de voorkeur domeincontroller-lijst.
- CSExportAnalyzer heeft problemen met sommige staten objecten.

**Nieuwe functies:**

- Een join kan nu verbinding maken met de objecttype 'ANY' in de MV.

## <a name="104850222"></a>1.0.485.0222
Uitgebracht: 2015 februari

**Verbeteringen:**

- Verbeterde importeren prestaties.

**Vaste problemen:**

- Wachtwoordsynchronisatie respecteert het kenmerk cloudFiltered is gebruikt door het kenmerk filteren. Gefilterde objecten wordt niet langer in het bereik voor Wachtwoordsynchronisatie.
- In uitzonderlijke situaties waarin de topologie zeer veel domeincontrollers had, werkt niet Wachtwoordsynchronisatie.
- "Ik heb afgebroken-server" bij het importeren van de Azure AD-Connector na Apparaatbeheer is ingeschakeld in Azure AD/Intune.
- Lid worden van externe beveiliging principes (FSP's) van meerdere domeinen in dezelfde bos, treedt er een fout niet-eenduidige-join op.

## <a name="104751202"></a>1.0.475.1202
Uitgebracht: 2014 December

**Nieuwe functies:**

- Deze wordt nu ondersteund om uit te voeren Wachtwoordsynchronisatie met het kenmerk gebaseerd filteren. Zie [Wachtwoordsynchronisatie waarop een filter](active-directory-aadconnectsync-configure-filtering.md)voor meer informatie.
- Het kenmerk msDS-ExternalDirectoryObjectID is geschreven naar AD. Hiermee voegt ondersteuning voor Office 365-toepassingen OAuth2 gebruiken voor toegang tot, Online en het On-Premises postvakken in een hybride implementatie van Exchange.

**Vaste upgradeproblemen op:**

- Een nieuwere versie van de aanmeldhulp is beschikbaar op de server.
- Een aangepaste installatie-pad is gebruikt voor het installeren van Azure AD-synchronisatie.
- Een ongeldige aangepaste join-criterium voorkomen dat de upgrade.

**Andere oplossingen:**

- De sjablonen voor Office Pro Plus opgelost.
- Vaste installatieproblemen die worden veroorzaakt door de gebruikersnamen die met een streepje beginnen.
- Vaste de instelling sourceAnchor verliezen wanneer u zich de installatiewizard een tweede maal.
- Vaste ETW tracering voor Wachtwoordsynchronisatie

## <a name="104701023"></a>1.0.470.1023
Uitgebracht: 2014 oktober

**Nieuwe functies:**

- Wachtwoordsynchronisatie uit meerdere on-premises implementatie AD naar Azure AD.
- Gelokaliseerde installatie UI op alle talen voor Windows Server.

**Een upgrade van AADSync 1.0 GA**

Als u al Azure AD-synchronisatie hebt geïnstalleerd, is er een extra stap die u uitvoeren moet als u een van de regels van de synchronisatie kant-en-klare hebt gewijzigd. Nadat u hebt bijgewerkt naar de 1.0.470.1023 vrijgeven, de regels die u hebt gewijzigd zijn gedupliceerd synchronisatie. Ga als volgt te werk voor elke regel met gewijzigde synchroniseren:

- Zoek de synchronisatie regel die u hebt gewijzigd en dient u een van de wijzigingen.
- De synchronisatie-regel verwijderen.
- Zoek de nieuwe synchronisatie regel gemaakt door Azure AD-synchronisatie en pas de wijzigingen opnieuw toe.

**Machtigingen voor de AD-account**

Het AD-account moet extra machtigingen kunnen ogen van het lezen van AD worden verleend. De machtigingen om te verlenen zijn benoemde 'Repliceren directorywijzigingen' en 'Repliceren Directory wijzigingen alles'. Beide machtigingen zijn vereist om te kunnen lezen van de ogen

## <a name="104190911"></a>1.0.419.0911
Uitgebracht: 2014 September

**Eerste versie van Azure AD-synchronisatie.**

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
