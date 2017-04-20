<properties
    pageTitle="Wachtwoordsynchronisatie bij het synchroniseren van Azure AD Connect implementeren | Microsoft Azure"
    description="Vindt u informatie over de werking van Wachtwoordsynchronisatie en hoe u deze inschakelen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="markusvi;andkjell"/>


# <a name="implementing-password-synchronization-with-azure-ad-connect-sync"></a>Wachtwoordsynchronisatie bij het synchroniseren van Azure AD Connect implementeren
In dit onderwerp vindt u de informatie die u nodig hebt voor het synchroniseren van uw gebruikerswachtwoorden vanuit een on-premises implementatie Active Directory (AD) met een cloudgebaseerde Azure Active Directory (Azure AD).

## <a name="what-is-password-synchronization"></a>Wat is Wachtwoordsynchronisatie
De kans dat u bent niet aan het werk vanwege een vergeten wachtwoord zijn gerelateerd aan het aantal verschillende wachtwoorden die u moet onthouden. De meer wachtwoorden die u onthouden moet, des te groter de kans op een vergeten. Vraag de meeste helpdesk bronnen vragen en oproepen over wachtwoorden en andere problemen met wachtwoord.

Wachtwoordsynchronisatie is een functie voor het synchroniseren van gebruikerswachtwoorden vanuit een on-premises implementatie Active Directory een cloudgebaseerde Azure Active Directory (Azure AD).
Deze functie kunt u aanmelden bij Azure Active Directory-services (zoals Office 365, Microsoft Intune CRM Online en Azure AD Domain Services) met hetzelfde wachtwoord u werkt aan te melden met uw lokale Active Directory.

![Wat is Azure AD Connect](./media/active-directory-aadconnectsync-implement-password-synchronization/arch1.png)

Verlaagt het aantal wachtwoorden moeten gebruikers met slechts één behouden blijven, helpt Wachtwoordsynchronisatie u bij:

- De productiviteit van uw gebruikers te verbeteren
- Uw helpdesk verlagen  

Ook als u het gebruik van [**Federatie met AD FS**](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect)selecteert, kunt u desgewenst inschakelen Wachtwoordsynchronisatie als een back-up geval de infrastructuur van uw AD FS mislukt.

Wachtwoordsynchronisatie is een uitbreiding op de directory-synchronisatie-functie geïmplementeerd Azure AD Connect-synchronisatie. Als u Wachtwoordsynchronisatie in uw omgeving, moet u:

- Installatie Azure AD verbinding maken  
- Adreslijstsynchronisatie tussen uw on-premises configureren AD en uw Azure Active Directory
- Wachtwoordsynchronisatie inschakelen

Zie voor meer informatie, [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md)

> [AZURE.NOTE] Zie voor meer informatie over Active Directory Domain Services die zijn geconfigureerd voor synchronisatie FIPS en wachtwoord, [Wachtwoordsynchronisatie en FIPS](#password-synchronization-and-fips).

## <a name="how-password-synchronization-works"></a>De werking van Wachtwoordsynchronisatie
Er worden wachtwoorden opgeslagen in de Active Directory domain service in de vorm van een weergave van de waarde hash van het werkelijke gebruikerswachtwoord. Een hashwaarde is een eenzijdige wiskundige functie (de "*hashing algoritme*") als resultaat. Er is geen enkele manier wilt terugdraaien van het resultaat van een eenzijdige functie naar de tekst zonder opmaak versie van een wachtwoord. U kunt een wachtwoordhash niet gebruiken om aan te melden bij uw on-premises netwerk.

Als u wilt synchroniseren uw wachtwoord, haalt Azure AD Connect synchroniseren op uw wachtwoordhash uit de lokale Active Directory. Verwerking van extra beveiliging wordt toegepast op de wachtwoordhash voordat deze wordt gesynchroniseerd met de verificatieservice Azure Active Directory. Wachtwoorden worden gesynchroniseerd op per gebruiker en in chronologische volgorde.

De werkelijke gegevensstroom van het proces van de synchronisatie wachtwoord is vergelijkbaar met de synchronisatie van gebruikersgegevens, zoals naam of e-mailadressen. Wachtwoorden zijn echter vaker dan het venster standaard directory-synchronisatie voor andere kenmerken gesynchroniseerd. Het proces van de synchronisatie wachtwoord elke 2 minuten wordt uitgevoerd. U kunt de frequentie van dit proces niet wijzigen. Wanneer u een wachtwoord synchroniseert, worden het bestaande wachtwoord in de cloud overschreven.

De eerste keer, u de functie voor het wachtwoord van synchronisatie inschakelen, eerste synchronisatie van de wachtwoorden van alle relevante gebruikers worden uitgevoerd. U kunt een subset van gebruikerswachtwoorden die u wilt synchroniseren niet expliciet definiëren.

Wanneer u het wachtwoord van een on-premises implementatie wijzigt, wordt de bijgewerkte wachtwoord gesynchroniseerd, meestal in een kwestie van minuten.
De synchronisatiefunctie wachtwoord wordt automatisch synchronisatiepogingen mislukte pogingen. Als u een fout is opgetreden tijdens een poging om te synchroniseren van een wachtwoord is een fout in de logboeken vastgelegd.

De synchronisatie van een wachtwoord heeft geen invloed op de aangemelde gebruiker.
De huidige sessie van de cloud-service wordt niet direct beïnvloed door gesynchroniseerde wachtwoord wijzigen die optreedt terwijl u bent aangemeld bij een cloudservice. Wanneer echter de cloudservice, moet u opnieuw te identificeren, u moet uw nieuwe wachtwoord in te voeren.

> [AZURE.NOTE] Wachtwoordsynchronisatie wordt alleen ondersteund voor de gebruiker van het type object in Active Directory. Deze wordt niet ondersteund voor het objecttype iNetOrgPerson.

### <a name="how-password-synchronization-works-with-azure-ad-domain-services"></a>De werking van Wachtwoordsynchronisatie met Azure AD-domeinservices
U kunt ook de synchronisatiefunctie wachtwoord gebruiken voor het synchroniseren van uw on-premises implementatie wachtwoorden met de [Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). In dit scenario kunt u de Azure AD-domeinservices verificatie van uw gebruikers in de cloud met alle methoden beschikbaar in uw on-premises AD. De ervaring van dit scenario is vergelijkbaar met het gebruik van de Active Directory-migratie (ADMT) in een on-premises omgeving.

### <a name="security-considerations"></a>Veiligheidsoverwegingen
Bij het synchroniseren van wachtwoorden, wordt de tekst zonder opmaak versie van uw wachtwoord wordt niet blootgesteld aan de synchronisatiefunctie wachtwoord naar Azure AD of vanaf een van de bijbehorende services.

Er is ook geen vereiste op de lokale Active Directory dat het wachtwoord opslaan in een omkeerbare indeling. Een overzicht van de Active Directory-wachtwoordhash wordt gebruikt voor de overdracht tussen de on-premises implementatie AD en Azure Active Directory. De samenvatting van de wachtwoordhash kan niet worden gebruikt voor toegang tot bronnen in uw on-premises omgeving.

### <a name="password-policy-considerations"></a>Overwegingen bij het beleid van wachtwoord
Er zijn twee soorten wachtwoordbeleidsregels die worden beïnvloed door het inschakelen van Wachtwoordsynchronisatie:

1. Wachtwoordbeleid complexiteit
2. Wachtwoord verloopbeleid

**Wachtwoordbeleid complexiteit**  
Als u Wachtwoordsynchronisatie inschakelt, wordt in het beleid voor de complexiteit van wachtwoorden in uw lokale Active Directory complexiteit beleidsregels in de cloud voor gesynchroniseerde gebruikers overschrijven. U kunt alle geldige wachtwoorden van uw lokale Active Directory gebruiken voor toegang tot Azure AD-services.

> [AZURE.NOTE] Wachtwoorden voor gebruikers die zijn gemaakt rechtstreeks in de cloud zijn nog steeds gelden wachtwoordbeleidsregels gedefinieerd in de cloud.

**Wachtwoord verloopbeleid**  
Als een gebruiker zich in het bereik van Wachtwoordsynchronisatie, worden het accountwachtwoord cloud is ingesteld op '*Dat het nooit verloopt*'.
U kunt blijven te melden bij uw cloudservices met een gesynchroniseerde wachtwoord die in uw on-premises omgeving is verlopen. Uw wachtwoord cloud, wordt de volgende keer dat u het wachtwoord in de on-premises omgeving wijzigt bijgewerkt.

### <a name="overwriting-synchronized-passwords"></a>Wachtwoorden overschrijven gesynchroniseerd
Een beheerder kan handmatig uw wachtwoord via Windows PowerShell.

In dit geval het nieuwe wachtwoord uw gesynchroniseerde wachtwoord wordt genegeerd en alle wachtwoordbeleidsregels gedefinieerd in de cloud worden toegepast op het nieuwe wachtwoord.

Als u het wachtwoord voor uw on-premises implementatie wijzigen klik nogmaals het nieuwe wachtwoord is gesynchroniseerd met de cloud en het wachtwoord handmatig bijgewerkte overschrijft.

## <a name="enabling-password-synchronization"></a>Wachtwoordsynchronisatie inschakelen
Wachtwoordsynchronisatie is automatisch ingeschakeld, tijdens de installatie van Azure AD Connect met de **Expresinstellingen**. Zie [aan de slag met Azure AD Connect express-instellingen gebruiken](./connect/active-directory-aadconnect-get-started-express.md)voor meer informatie.

Als u aangepaste instellingen gebruiken tijdens de installatie van Azure AD Connect, kunt u Wachtwoordsynchronisatie op de aanmeldingspagina van gebruiker inschakelen. Zie [aangepaste installatie van Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md)voor meer informatie.

![Wachtwoordsynchronisatie inschakelen](./media/active-directory-aadconnectsync-implement-password-synchronization/usersignin.png)

### <a name="password-synchronization-and-fips"></a>Wachtwoordsynchronisatie en FIPS
Als uw server is vergrendeld op basis van de federale Information Processing Standard (FIPS), is klikt u vervolgens MD5 uitgeschakeld.

**Als u wilt inschakelen MD5 voor Wachtwoordsynchronisatie, moet u de volgende stappen uitvoeren:**

1. Ga naar **%programfiles%\Azure AD Sync\Bin**.
2. Open **miiserver.exe.config**.
3. Ga naar het knooppunt **configuratie/runtime** (aan het einde van het bestand).
4. Het volgende knooppunt toevoegen:`<enforceFIPSPolicy enabled="false"/>`
5. Sla uw wijzigingen op.

Voor verwijzing is dit knipsel hoe ziet dit eruit als:

```
    <configuration>
        <runtime>
            <enforceFIPSPolicy enabled="false"/>
        </runtime>
    </configuration>
```

Zie voor informatie over beveiligings- en FIPS [AAD Wachtwoordsynchronisatie, versleuteling en FIPS naleving](https://blogs.technet.microsoft.com/enterprisemobility/2014/06/28/aad-password-sync-encryption-and-fips-compliance/)

## <a name="troubleshooting-password-synchronization"></a>Probleemoplossing Wachtwoordsynchronisatie
Als u wachtwoorden niet zoals verwacht synchroniseert, kan deze voor een subset van gebruikers of voor alle gebruikers zijn.

- Als u een probleem met de afzonderlijke objecten hebt, raadpleegt u [problemen oplossen met één object dat niet kan worden gesynchroniseerd wachtwoorden](#troubleshoot-one-object-that-is-not-synchronizing-passwords).
- Als u een probleem waarbij geen wachtwoorden zijn gesynchroniseerd hebt, raadpleegt u [problemen oplossen waarbij geen wachtwoorden zijn gesynchroniseerd](#troubleshoot-issues-where-no-passwords-are-synchronized).

### <a name="troubleshoot-one-object-that-is-not-synchronizing-passwords"></a>Problemen met één object dat wachtwoorden niet synchroniseert
Aan de hand van de status van een object, kunt u eenvoudig wachtwoord synchronisatieproblemen oplossen.

Beginnen in **Active Directory-gebruikers en Computers**. Zoek de gebruiker en controleer of **dat gebruiker moet wachtwoord bij volgende aanmelding wijzigen** is niet ingeschakeld.
![Active Directory productief wachtwoorden](./media/active-directory-aadconnectsync-implement-password-synchronization/adprodpassword.png)  
Als deze optie is geselecteerd, vraagt u vervolgens de gebruiker aanmelden bij en het wachtwoord wijzigen. Tijdelijke wachtwoorden worden niet gesynchroniseerd met Azure AD.

Als deze in Active Directory uitziet, klikt u vervolgens is de volgende stap moet volgen van de gebruiker in de synchronisatie-engine. Door de gebruiker aan Azure AD vanuit lokale Active Directory te volgen, kunt u zien als er een beschrijvende fout is opgetreden op het object.

1. Start de **[Synchronisatie-servicebeheer](active-directory-aadconnectsync-service-manager-ui.md)**.
2. Klik op **verbindingslijnen**.
3. Selecteer de **Active Directory Connector** de gebruiker bevindt zich in.
4. Selecteer **Zoeken verbindingslijn ruimte**.
5. Zoek de gebruiker die u zoekt.
6. Selecteer het tabblad **over de afkomst** en zorg ervoor dat ten minste één synchronisatie regel ziet u **Wachtwoordsynchronisatie** als **waar**. In de standaard-configuratie, de naam van de regel van de synchronisatie is **In uit Active Directory - gebruiker AccountEnabled**.  
    ![Informatie over de afkomst over een gebruiker](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync.png)  
7. Daarna [volgt u de gebruiker](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system) door de metaverse naar de ruimte Azure AD verbindingslijn. De verbindingslijn ruimte object moet een uitgaande regel hebben bij het **Wachtwoordsynchronisatie** ingesteld op **waar**. In de standaard-configuratie is de naam van de synchronisatie-regel **Out naar AAD - gebruiker toevoegen**.  
    ![Connector ruimte eigenschappen van een gebruiker](./media/active-directory-aadconnectsync-implement-password-synchronization/cspasswordsync2.png)  
8. Als u wilt zien van de details van de synchronisatie wachtwoord van het object voor de laatste week, klikt u op **logbestand...**.  
    ![Object logboekgegevens](./media/active-directory-aadconnectsync-implement-password-synchronization/csobjectlog.png)  

De statuskolom kan de volgende waarden hebben:

Status | Beschrijving
---- | -----
Succes | Wachtwoord is gesynchroniseerd.
FilteredByTarget | Wachtwoord is ingesteld op de **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**. Wachtwoord is niet gesynchroniseerd.
NoTargetConnection | Geen object in de metaverse of in de ruimte Azure AD-connector.
SourceConnectorNotPresent | Geen object gevonden in de lokale Active Directory connector ruimte.
TargetNotExportedToDirectory | Het object in de ruimte Azure AD-connector is nog niet geëxporteerd.
MigratedCheckDetailsForMoreInfo | Vermelding voordat opbouwen 1.0.9125.0 is gemaakt en wordt weergegeven in de oudere staat.

### <a name="troubleshoot-issues-where-no-passwords-are-synchronized"></a>Problemen oplossen waarbij geen wachtwoorden worden gesynchroniseerd
Begin door het script in de sectie die [de status van de synchronisatie-wachtwoordinstellingen ophalen](#get-the-status-of-password-sync-settings)uit te voeren. Hebt u een overzicht van de configuratie van de synchronisatie wachtwoord.  
![PowerShell-script uitvoer van wachtwoord synchronisatie-instellingen](./media/active-directory-aadconnectsync-implement-password-synchronization/psverifyconfig.png)  
Als de functie is niet ingeschakeld in Azure AD of als de synchronisatiestatus van het kanaal niet is ingeschakeld, moet u vervolgens de installatiewizard verbinding maken met uitvoeren. Selecteert u **Synchronisatieopties aanpassen** en Wachtwoordsynchronisatie deselecteren. Deze wijziging wordt de functie tijdelijk uitgeschakeld. Vervolgens de wizard opnieuw uitvoeren en Wachtwoordsynchronisatie opnieuw in te schakelen. Voer het script opnieuw op om dit te bevestigen dat de configuratie correct is.

Als u het script ziet dat is er geen heartbeat, voer het script in [Trigger voor een volledige synchronisatie van alle wachtwoorden](#trigger-a-full-sync-of-all-passwords). Dit script kan ook worden gebruikt voor andere scenario's waarin de configuratie juist is maar wachtwoorden worden niet gesynchroniseerd.

#### <a name="get-the-status-of-password-sync-settings"></a>De status van de synchronisatie-wachtwoordinstellingen ophalen

```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Een volledige synchronisatie van alle wachtwoorden activeren
U kunt een volledige synchronisatie van alle wachtwoorden met het volgende script activeren:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Volgende stappen

* [Azure AD verbinden met synchronisatie: Opties voor het aanpassen van synchronisatie](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
