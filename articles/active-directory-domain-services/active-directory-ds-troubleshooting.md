<properties
    pageTitle="Azure Active Directory Domain Services: Gids voor probleemoplossing | Microsoft Azure"
    description="Gids voor probleemoplossing voor Azure AD-domeinservices"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD-domeinservices - gids voor probleemoplossing
Dit artikel vindt suggesties voor probleemoplossing voor problemen die optreden kunnen bij het instellen van of het beheer van Azure Active Directory (AD) Domain Services.


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>U kunt niet Azure AD Domain Services voor het telefoonboek van uw Azure AD inschakelen
In dit gedeelte vindt u synchronisatiefouten oplossen wanneer u probeert om in te schakelen Azure AD Domain Services voor uw adreslijst en dit mislukt of terug naar 'Uitgeschakeld' wordt ingeschakeld.

Kies het oplossen van problemen stappen die overeenkomen met het foutbericht dat u optreden.

|**Foutbericht**|**Resolutie**|
|---|:---|
|*De naam contoso100.com wordt al gebruikt in dit netwerk. Geef een naam die niet in gebruik.*|[Domein naamconflict in het virtuele netwerk](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Domeinservices kunnen niet worden ingeschakeld in deze Azure AD-tenant. De service heeft geen voldoende machtigingen om de toepassing 'Azure AD domein Services Sync' genoemd. Verwijderen van de toepassing 'Azure AD domein Services Sync' genoemd en vervolgens probeert iets te Domain Services voor uw Azure AD-tenant inschakelen.*|[Domeinservices heeft geen voldoende machtigingen om de synchronisatie van Azure AD Domain Services-toepassing](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Domeinservices kunnen niet worden ingeschakeld in deze Azure AD-tenant. De Domain Services-toepassing in uw Azure AD-tenant heeft geen de vereiste machtigingen om in te schakelen Domain Services. Verwijderen van de toepassing met de toepassing id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en vervolgens probeert iets te Domain Services voor uw Azure AD-tenant inschakelen.*|[De Domain Services-toepassing is niet juist geconfigureerd in uw tenant](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Domeinservices kunnen niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Microsoft Azure AD is uitgeschakeld in uw Azure AD-tenant. De toepassing met de toepassing id 00000002-0000-0000-c000-000000000000 inschakelen en vervolgens probeert iets te Domain Services voor uw Azure AD-tenant inschakelen.*|[De toepassing Microsoft Graph is uitgeschakeld in uw Azure AD-tenant](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Domein naamconflict
**Foutbericht:**

*De naam contoso100.com wordt al gebruikt in dit netwerk. Geef een naam die niet in gebruik.*

**Remediation:**

Zorg ervoor dat er geen een bestaand domein met dezelfde domeinnaam beschikbaar op dat virtuele netwerk. Bijvoorbeeld dat u een domein met de naam 'contoso.com' al beschikbaar in een netwerk met de geselecteerde virtuele hebt. U wilt later een Azure AD-domeinservices beheerde domein met dezelfde domeinnaam (dat wil zeggen ' contoso.com' genoemd) in deze virtuele netwerk inschakelen. Er wordt een fout bij het inschakelen van Azure AD-domeinservices.

Deze fout oplevert naamconflicten voor de naam van het domein op dat virtuele netwerk. In dit geval moet u een andere naam voor het instellen van uw beheerde domeinservices van Azure AD-domein. U kunt ook het bestaande domein opgeheven inrichten en gaat u verder met het inschakelen van Azure AD Domain Services.


### <a name="inadequate-permissions"></a>Onvoldoende machtigingen
**Foutbericht:**

*Domeinservices kunnen niet worden ingeschakeld in deze Azure AD-tenant. De service heeft geen voldoende machtigingen om de toepassing 'Azure AD domein Services Sync' genoemd. Verwijderen van de toepassing 'Azure AD domein Services Sync' genoemd en vervolgens probeert iets te Domain Services voor uw Azure AD-tenant inschakelen.*

**Remediation:**

Controleer of er momenteel een toepassing met de naam 'Azure AD domein Services Sync' in uw adreslijst Azure AD is. Als deze toepassing bestaat, verwijdert u deze en Azure AD-domeinservices opnieuw inschakelen.

Voer de volgende stappen uit om de aanwezigheid van de toepassing te controleren en te verwijderen, als de toepassing bestaat:

  1. Ga naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.
  3. Selecteer de Azure AD-tenant (directory) waarvoor u wilt inschakelen Azure AD Domain Services.
  4. Ga naar het tabblad **toepassingen** .
  5. Selecteer de optie **Mijn bedrijf eigenaar is van toepassingen** in de vervolgkeuzelijst.
  6. Schakel dit selectievakje in voor de toepassing **Azure AD domein Services synchroniseren**. Als de toepassing bestaat, gaat u verder om deze te verwijderen.
  7. Nadat u de toepassing hebt verwijderd, probeert u Azure AD-domeinservices weer inschakelen.


### <a name="invalid-configuration"></a>Ongeldige configuratie
**Foutbericht:**

*Domeinservices kunnen niet worden ingeschakeld in deze Azure AD-tenant. De Domain Services-toepassing in uw Azure AD-tenant heeft geen de vereiste machtigingen om in te schakelen Domain Services. Verwijderen van de toepassing met de toepassing id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en vervolgens probeert iets te Domain Services voor uw Azure AD-tenant inschakelen.*

**Remediation:**

Controleer als u een toepassing met de naam 'AzureActiveDirectoryDomainControllerServices' (met een toepassings-id van d87dcbc6-a371-462e-88e3-28ad15ec4e64) in uw adreslijst Azure AD hebt. Als deze toepassing bestaat, moet u deze verwijderen en Azure AD-domeinservices opnieuw inschakelen.

Gebruik het volgende PowerShell-script om te zoeken van de toepassing en verwijder deze.

> [AZURE.NOTE] Dit script gebruikt **Azure AD PowerShell versie 2** -cmdlets. Lees de [AzureAD PowerShell-documentatie](https://msdn.microsoft.com/library/azure/mt757189.aspx)voor een volledige lijst van alle beschikbare cmdlets en voor het downloaden van de module.

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph uitgeschakeld
**Foutbericht:**

Domeinservices kunnen niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Microsoft Azure AD is uitgeschakeld in uw Azure AD-tenant. De toepassing met de toepassing id 00000002-0000-0000-c000-000000000000 inschakelen en vervolgens probeert iets te Domain Services voor uw Azure AD-tenant inschakelen.

**Remediation:**

Controleer als u een toepassing met de id 00000002-0000-0000-c000-000000000000 hebt uitgeschakeld. Deze toepassing is van de toepassing Microsoft Azure AD en biedt API Graph toegang tot uw Azure AD-tenant. Azure AD-domeinservices moet worden ingeschakeld voor het synchroniseren van uw Azure AD-tenant naar uw beheerde domein deze toepassing.

U lost deze fout door deze toepassing inschakelen en vervolgens probeert iets te Domain Services voor uw Azure AD-tenant inschakelen.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Gebruikers kunnen niet kunt aanmelden bij het beheerde domeinservices van Azure AD-domein
Als een of meer gebruikers in uw Azure AD-tenant niet kunt aanmelden bij het nieuwe beheerde domein, voert u de volgende stappen:

- **Aanmelden met UPN opmaak:** Probeer aan te melden in de notatie UPN (bijvoorbeeld 'joeuser@contoso.com') in plaats van de SAMAccountName-indeling (CONTOSO\joeuser). De SAMAccountName worden automatisch gegenereerd voor gebruikers waarvan voorvoegsel UPN te lang is of is hetzelfde als een andere gebruiker in het beheerde domein. De UPN-indeling is gegarandeerd uniek zijn binnen een Azure AD-tenant.

> [AZURE.NOTE] Het is raadzaam om met de UPN-opmaak te melden bij de beheerde domeinservices van Azure AD-domein.

- Zorg ervoor dat u [ingeschakeld Wachtwoordsynchronisatie](active-directory-ds-getting-started-password-sync.md) volgens de stappen in de handleiding aan de slag hebt.

- **Externe accounts:** Controleer of het gebruikersaccount van de desbetreffende niet een externe account in de Azure AD-tenant is. Voorbeelden van externe accounts zijn Microsoft-accounts (bijvoorbeeld 'joe@live.com') of gebruikersaccounts vanuit een extern Azure AD-map. Aangezien Azure AD-domeinservices geen referenties op voor deze gebruikersaccounts, kunnen deze gebruikers zich niet kunnen aanmelden bij het beheerde domein.

- **Gesynchroniseerd accounts:** Als de desbetreffende gebruikersaccounts wordt gesynchroniseerd vanuit een on-premises adreslijst, Controleer het volgende:
    - U hebt geïmplementeerd of bijgewerkt naar de [meest recente aanbevolen versie van Azure AD Connect](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - U kunt Azure AD Connect om uit te [voeren van een volledige synchronisatie](active-directory-ds-getting-started-password-sync.md)hebt geconfigureerd.

    - Afhankelijk van de grootte van uw adreslijst, kan er even duren voor gebruikersaccounts en referenties ogen van beschikbaar in Azure AD Domain Services. Controleer of dat u wachten lang genoeg voordat deze opnieuw verificatie (afhankelijk van de grootte van uw adreslijst - enkele uren per dag of twee voor grote mappen).

    - Als het probleem zich blijft voordoen nadat u hebt gecontroleerd van de voorgaande stappen, kunt u de Service Microsoft Azure AD-synchronisatie opnieuw te starten. Van uw computer synchroniseren, een opdrachtprompt starten en uitvoeren van de volgende opdrachten:
      1. netto stoppen 'Microsoft Azure AD-synchronisatie'
      2. netto begin 'Microsoft Azure AD-synchronisatie'

- **Alleen de cloud accounts**: als de desbetreffende gebruikersaccount een gebruikersaccount alleen de cloud is, zorg ervoor dat de gebruiker het wachtwoord heeft gewijzigd nadat u Azure AD-domeinservices ingeschakeld. Deze stap zorgt ervoor dat de referentie hashes die zijn vereist voor Azure AD-domeinservices moet worden gegenereerd.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Gebruikers die zijn verwijderd uit uw Azure AD-tenant worden niet verwijderd uit uw beheerde domein
Azure AD voorkomt u per ongeluk wordt verwijderd van gebruikersobjecten. Wanneer u een gebruikersaccount uit uw Azure AD-tenant verwijderen, wordt het bijbehorende gebruikersobject verplaatst naar de Prullenbak. Wanneer deze bewerking wordt gesynchroniseerd naar uw beheerde domein, wordt deze door het bijbehorende gebruikersaccount uitgeschakeld worden gemarkeerd. Deze functie kunt u herstellen of het gebruikersaccount later ongedaan maken.

U kunt het gebruikersaccount volledig vanaf uw beheerde domein verwijderen de gebruiker definitief uit uw Azure AD-tenant. Gebruik de verwijderen-MsolUser PowerShell-cmdlet met de '-RemoveFromRecycleBin' optie, zoals wordt beschreven in dit [artikel MSDN](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Contact met ons opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam naar [feedback delen of voor ondersteuning] (actief-directory-ds-contactpersoon-us.md).
