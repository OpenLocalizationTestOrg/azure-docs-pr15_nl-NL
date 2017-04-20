<properties
   pageTitle="Synchronisatie van Azure AD Connect: operationele taken en overwegingen | Microsoft Azure"
   description="In dit onderwerp wordt beschreven operationele taken voor het synchroniseren van Azure AD Connect en het voorbereiden van het besturingssysteem van dit onderdeel."
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
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Synchronisatie van Azure AD Connect: operationele taken en aandacht
Het doel van dit onderwerp is om te beschrijven beheertaken voor Azure AD Connect synchroniseren.

## <a name="staging-mode"></a>Tijdelijke modus
Tijdelijke modus kan worden gebruikt voor verschillende scenario's, waaronder:

-   Beschikbaarheid.
-   Testen en implementeren van nieuwe configuratiewijzigingen.
-   Een nieuwe server introduceren en de oude schaft.

Met een server in de modus voor tijdelijk opslaan, kunt u wijzigingen aanbrengt in de configuratie en de wijzigingen bekijken voordat u de server actief maken. Bovendien kunt u volledige importeren en volledige synchronisatie om te bevestigen dat alle wijzigingen zijn verwacht voordat u deze wijzigingen in uw productieomgeving aanbrengen uitvoeren.

Tijdens de installatie, kunt u de server in de **modus waarin u tijdelijk opslaat**. Hiermee wordt de server actief voor importeren en synchronisatie, maar niet alle exports uitgevoerd. Een server in de modus voor tijdelijk opslaan is niet actief Wachtwoordsynchronisatie of wachtwoord write-backs, zelfs als u deze functies tijdens de installatie hebt geselecteerd. Wanneer u tijdelijk opslaan modus uitschakelt, de server exporteren wordt gestart, kunt Wachtwoordsynchronisatie, en kunt wachtwoord write-backs.

Met behulp van de synchronisatie-servicebeheer, kunt u nog steeds een exporteren afdwingen.

Een server in de modus voor tijdelijk opslaan blijft wijzigingen uit Active Directory en Azure AD ontvangen. Altijd heeft een kopie van de meest recente wijzigingen en kunnen zeer snel nemen boven de verantwoordelijkheden van een andere server. Als u configuratiewijzigingen in de primaire-server aanbrengt, is uw verantwoordelijkheid dezelfde wijzigingen aanbrengen in de server in de modus waarin u tijdelijk opslaat.

Voor mensen met kennis van oudere technologieën voor synchronisatie is het tijdelijk opslaan modus verschillende, omdat de server een eigen SQL-database heeft. Deze architectuur kan het tijdelijk opslaan modus server zich bevinden in een ander datacenter.

### <a name="verify-the-configuration-of-a-server"></a>Controleer de configuratie van een server
Als u wilt deze methode toepast, als volgt te werk:

1. [Voorbereiden](#prepare)
2. [Importeren en synchroniseren](#import-and-synchronize)
3. [Controleer of](#verify)
4. [Schakeloptie active server](#switch-active-server)

#### <a name="prepare"></a>Voorbereiden

1. Installeren van Azure AD Connect, selecteert u **tijdelijke modus**en hef de selectie van **synchronisatie starten** op de laatste pagina in de installatiewizard. Deze modus kunt u de synchronisatie-engine handmatig uitvoeren.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Aanmeldingsadres uitschakelen/aanmelden en selecteer in het startmenu **Synchronisatieservice**.

#### <a name="import-and-synchronize"></a>Importeren en synchroniseren

1. Selecteer de **verbindingslijnen**en selecteert u de eerste verbindingslijn met het type **Active Directory Domain Services**. Klik op **uitvoeren**, selecteert u **volledig importeren**en **OK**. Voer deze stappen uit voor alle verbindingslijnen van dit type.
2. Selecteer de verbindingslijn met type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**, selecteert u **volledig importeren**en **OK**.
3. Controleer of dat het tabblad verbindingslijnen nog steeds is geselecteerd. Voor elke verbindingslijn die met type **Active Directory Domain Services**, klik op **uitvoeren**, selecteert u **De synchronisatie Delta**en kies **OK**.
4. Selecteer de verbindingslijn met type **Azure Active Directory (Microsoft)**. Klik op **uitvoeren**, selecteert u **De synchronisatie Delta**en kies **OK**.

U hebt nu gefaseerde exporteren wordt gewijzigd in Azure AD en on-premises AD (als u van hybride implementatie van Exchange gebruikmaakt). De volgende stappen kunnen u controleren wat wordt gewijzigd voordat u de exporteren naar de mappen daadwerkelijk begint.

#### <a name="verify"></a>Controleer of

1. Start een cmd-prompt en Ga naar`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Uitvoeren:`csexport "Name of Connector" %temp%\export.xml /f:x`  
De naam van de verbindingslijn vindt u in de synchronisatieservice. Een naam die vergelijkbaar is met "contoso.com – AAD" voor Azure AD.
3. Uitvoeren:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. U hebt een bestand in % temp % benoemde export.csv die kan worden onderzocht in Microsoft Excel. Dit bestand bevat alle wijzigingen die zullen worden geëxporteerd.
5. Noodzakelijke wijzigingen aanbrengen in de gegevens of de configuratie en uitvoeren van deze stappen nogmaals (importeren en synchroniseren en verifiëren) totdat de wijzigingen die moeten worden geëxporteerd naar verwachting.

**Informatie over het export.csv-bestand**

Grootste deel van het bestand is geen uitleg. Sommige afkortingen voor meer informatie over de inhoud:

- OMODT – wijziging objecttype. Hiermee wordt aangegeven of de bewerking op het objectniveau van een een toevoegen, bijwerken of verwijderen.
- AMODT-kenmerk wijzigingstype. Geeft aan of de bewerking op het kenmerkniveau van een een toevoegen, bijwerken of verwijderen is.

Als de waarde van het kenmerk met meerdere waarden is, wordt niet elke wijziging weergegeven. Alleen het aantal waarden toegevoegd of verwijderd is zichtbaar.

#### <a name="switch-active-server"></a>Schakeloptie active server

1. Klik op de server momenteel actief uitschakelen, de server (DirSync/FIM/Azure AD-synchronisatie) zodat deze niet worden geëxporteerd naar Azure AD of stel deze in de modus (Azure AD Connect) tijdelijke.
2. De installatiewizard uitgevoerd op de server in de **modus waarin u tijdelijk opslaat** en **tijdelijke modus**uitschakelen.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Problemen oplossen
Deel van het implementatieontwerp van de is voor het plannen van wat u moet doen als er een noodgevallen waar u de synchronisatieserver gaan verloren. Er zijn verschillende modellen wilt gebruiken en welke een gebruiken afhankelijk van de volgende factoren, waaronder:

-   Wat is uw tolerantie voor zijn niet wijzigingen kunnen aanbrengen aan objecten in Azure AD tijdens de downtime?
-   Als u Wachtwoordsynchronisatie gebruikt, de gebruikers accepteert dat ze het oude wachtwoord in Azure AD gebruiken moeten wanneer ze deze on-premises implementatie wijzigen?
-   Hebt u een afhankelijkheid in realtime bewerkingen uitvoeren, zoals wachtwoord write-backs?

Afhankelijk van de antwoorden op deze vragen en beleid van uw organisatie, kan een van de volgende strategieën worden geïmplementeerd:

-   Opnieuw opbouwen als dat nodig is.
-   Een vrije stand-by server, bekend als **tijdelijke modus**hebt.
-   Gebruik virtuele machines.

Als u de ingebouwde Express van de SQL-database niet gebruikt, moet u ook het gedeelte [SQL-beschikbaarheid](#sql-high-availability) controleren.

### <a name="rebuild-when-needed"></a>Bouw die tabellen opnieuw als dat nodig is
Er is een goede strategie voor het plannen van een server opnieuw opbouwen als dat nodig is. Meestal installatie van de synchronisatie-engine en Ga op die de eerste importeren en synchroniseren binnen een paar uur kunnen worden voltooid. Als er een vrije-server niet beschikbaar is, is het mogelijk een domeincontroller tijdelijk te gebruiken voor het hosten van de synchronisatie-engine.

De synchronisatie-engine server worden niet opgeslagen voor elke staat over de objecten zodat de database kan worden gemaakt van de gegevens in Active Directory en Azure AD. Het kenmerk **sourceAnchor** wordt gebruikt voor deelname aan de objecten uit on-premises implementatie en de cloud. Als u opnieuw de server met bestaande objecten on-premises implementatie en de cloud, klikt u vervolgens de synchronisatie-engine komt overeen met deze objecten samen opnieuw op opnieuw installeren. Wat u moet het document en sla zijn de configuratiewijzigingen op de server, zoals filteren en synchronisatie regels. Deze aangepaste configuraties moeten opnieuw worden toegepast voordat u begint te synchroniseren.

### <a name="have-a-spare-standby-server---staging-mode"></a>Hebt u een vervangende stand-by server - modus tijdelijke
Als u een complexere omgeving hebt, wordt klikt u vervolgens met een of meer stand-by servers aanbevolen. Tijdens de installatie, kunt u een server in **tijdelijke modus**inschakelen.

Zie [tijdelijke modus](#staging-mode)voor meer informatie.

### <a name="use-virtual-machines"></a>Virtuele machines gebruiken
Een methode veelvoorkomende en ondersteunde is de synchronisatie-engine uitvoeren in een virtuele machine. In het geval er is een probleem met de host, kan de afbeelding met de synchronisatie-engine server worden gemigreerd naar een andere server.

### <a name="sql-high-availability"></a>SQL-beschikbaarheid
Als u de SQL Server Express die wordt geleverd met Azure AD Connect niet gebruikt, moet klikt u vervolgens de beschikbaarheid voor SQL Server ook beschouwd. De alleen beschikbaarheid-oplossing ondersteund is SQL cluster. Niet-ondersteunde oplossingen opnemen spiegelen en altijd op.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**  

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)  
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)  
