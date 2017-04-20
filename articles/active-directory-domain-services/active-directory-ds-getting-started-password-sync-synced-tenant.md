<properties
    pageTitle="Azure AD-domeinservices: Wachtwoordsynchronisatie inschakelen | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
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
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Wachtwoordsynchronisatie Azure AD Domain Services inschakelen
In de voorafgaande taken, moet u Azure AD Domain Services voor uw Azure AD-tenant ingeschakeld. De volgende taak is synchronisatie van wachtwoorden Azure AD Domain Services. Zodra referentie synchronisatie is ingesteld, kunnen gebruikers zich kunnen aanmelden bij het beheerde domein met de referenties van hun bedrijf.

De benodigde stappen voor zijn verschillende op basis van of uw organisatie een Azure AD alleen de cloud heeft-tenant of om te synchroniseren met uw on-premises adreslijst met Azure AD Connect is ingesteld.

<br>

> [AZURE.SELECTOR]
- [Alleen de cloud Azure AD-tenant](active-directory-ds-getting-started-password-sync.md)
- [Azure AD-tenant gesynchroniseerd](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Taak 5: Inschakelen Wachtwoordsynchronisatie AAD Domain Services voor een gesynchroniseerde Azure AD-tenant
A gesynchroniseerd Azure AD-tenant is ingesteld om te synchroniseren met adreslijst van on-premises implementatie van uw organisatie met Azure AD Connect. Azure AD Connect NTLM niet wordt gesynchroniseerd en referentie-Kerberos ogen van Azure AD al dan niet standaard. Als u wilt gebruiken Azure AD-domeinservices, moet u configureren Azure AD Connect als u wilt synchroniseren referentie hashes voor NTLM als Kerberos-verificatie is vereist. De volgende stappen inschakelen synchronisatie van de ogen vereiste referenties van uw Azure AD-tenant.


### <a name="install-or-update-azure-ad-connect"></a>Installeren of bijwerken van Azure AD Connect
Installeer de meest recente aanbevolen versie van Azure AD Connect op een domein gekoppelde computer. Als u een bestaand exemplaar van Azure AD Connect-instelling, die u wilt bijwerken, zodat de nieuwste versie van Azure AD Connect kan gebruiken. Controleer of dat u altijd de nieuwste versie van Azure AD Connect gebruiken om te voorkomen bekende problemen/fouten die mogelijk al zijn gecorrigeerd.

**[Download Azure AD verbinding maken](http://www.microsoft.com/download/details.aspx?id=47594)**

Aanbevolen versie: **1.1.281.0** - gepubliceerd op 7 September 2016.

  > [AZURE.WARNING] De meest recente aanbevolen versie van Azure AD Connect om in te schakelen van de oude wachtwoordreferenties (vereist voor NTLM als Kerberos-verificatie) om te synchroniseren naar uw Azure AD-tenant, moet u installeren. Deze functie is niet beschikbaar in eerdere versies van Azure AD Connect of met de oudere DirSync-hulpmiddel.

Installatie-instructies voor Azure AD Connect zijn beschikbaar in het volgende artikel - [aan de slag met Azure AD Connect](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Synchronisatie van NTLM als Kerberos referentie ogen van Azure AD inschakelen
Het volgende PowerShell-script uitvoeren op elke bos AD, afdwingen van volledige Wachtwoordsynchronisatie, en alle on-premises gebruikers referentie ogen van synchroniseren met uw Azure AD-tenant inschakelen. Dit script kunt de referentie-hashes vereist voor NTLM/Kerberos-verificatie moeten worden gesynchroniseerd naar uw Azure AD-tenant.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Afhankelijk van de grootte van uw adreslijst (aantal gebruikers, groepen, enz.), synchronisatie van referentie ogen van Azure AD duurt. De wachtwoorden, worden gebruikt in het Azure AD-domeinservices beheerde domein kort nadat de referentie-hashes hebt gesynchroniseerd met Azure AD.


<br>

## <a name="related-content"></a>Verwante onderwerpen

- [Wachtwoordsynchronisatie AAD Domain Services voor een Azure alleen de cloud inschakelen AD directory](active-directory-ds-getting-started-password-sync.md)

- [Een beheerde domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)

- [Deelnemen aan een virtuele Windows-computer naar een beheerde domeinservices van Azure AD-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Een rood rol Enterprise Linux virtuele machine te koppelen aan een beheerde domeinservices van Azure AD-domein](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
