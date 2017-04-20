<properties
    pageTitle="Azure AD Connect synchronisatie servicefuncties en configuratie | Microsoft Azure"
    description="Beschrijving van service kant functies voor Azure AD Connect synchronisatie-service."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure AD Connect synchronisatie service-functies

De synchronisatiefunctie van Azure AD Connect bestaat uit twee onderdelen:

- De on-premises implementatie-component benoemde **Azure AD Connect-synchronisatie**, ook wel **synchronisatie-engine**genoemd.
- De service wonen in Azure AD ook bekend als **Azure AD Connect synchronisatie-service**

In dit onderwerp wordt uitgelegd de werking van de volgende functies van de **Azure AD Connect synchronisatieservice** en hoe u ze via Windows PowerShell kunt configureren.

Deze instellingen worden geconfigureerd door de [Azure Active Directory-Module voor Windows PowerShell](http://aka.ms/aadposh). Download en installeer deze afzonderlijk van Azure AD Connect. De cmdlets beschreven in dit onderwerp zijn geïntroduceerd in de [2016 maart los (build 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Controleer of dat u de meest recente versie uitvoeren als er geen de cmdlets beschreven in dit onderwerp of ze niet dat hetzelfde resultaat hebben.

Als u wilt zien van de configuratie in uw adreslijst Azure AD, `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures resultaat](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Veel van deze instellingen kunnen alleen worden gewijzigd door Azure AD Connect.

De volgende instellingen kunnen worden geconfigureerd door `Set-MsolDirSyncFeature`:

DirSyncFeature | Opmerking
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Hiermee kunt een kenmerk om te worden in quarantaine geplaatst wanneer dit is een duplicaat van een ander object in plaats van het hele object verbroken tijdens het exporteren.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Kunnen objecten voor het samenvoegen userPrincipalName naast het primaire SMTP-adres.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Hiermee kunt de synchronisatie-engine bij het userPrincipalName-kenmerk voor beheerde/licentie (niet-federatief) gebruikers.

Nadat u een functie hebt ingeschakeld, kunnen deze niet worden uitgeschakeld.

>[AZURE.NOTE] Van 24 augustus 2016 de functie *dupliceren kenmerk tolerantie* is standaard ingeschakeld voor nieuwe Azure AD-mappen. Deze functie wordt ook worden geïmplementeerd en ingeschakeld op mappen die zijn gemaakt voor deze datum. U ontvangt per e-mail een melding wanneer uw adreslijst gaat ophalen van deze functie is ingeschakeld.

De volgende instellingen zijn geconfigureerd door Azure AD Connect en kunnen niet worden gewijzigd door `Set-MsolDirSyncFeature`:

DirSyncFeature | Opmerking
--- | ---
DeviceWriteback | [Azure AD Connect: Apparaat write-backs inschakelen](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Synchronisatie van Azure AD Connect: Directory extensies](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Wachtwoordsynchronisatie bij het synchroniseren van Azure AD Connect implementeren](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Voorbeeld: Groep write-backs](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Momenteel niet ondersteund.

## <a name="duplicate-attribute-resiliency"></a>Dubbel kenmerk tolerantie
Objecten in plaats van mislukte inrichten met dubbele UPN's / proxyAddresses, het gedupliceerde kenmerk is 'in quarantaine geplaatst' en een tijdelijke waarde wordt toegewezen. Als het probleem is opgelost, wordt de tijdelijke UPN automatisch gewijzigd in de juiste waarde. Dit probleem kan worden ingeschakeld voor UPN en proxyAddress afzonderlijk. Zie [identiteit synchronisatie en dubbel kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)voor meer informatie.

## <a name="userprincipalname-soft-match"></a>UserPrincipalName vloeiende vergelijken
Als deze functie is ingeschakeld, is vloeiende-overeenkomst voor UPN ingeschakeld naast het [primaire SMTP-adres](https://support.microsoft.com/kb/2641663), dat altijd is ingeschakeld. Vloeiende-overeenkomst wordt gebruikt voor bestaande gebruikers van de cloud in Azure AD overeenkomen met on-premises gebruikers.

Als u moet de zoekwaarde on-premises implementatie AD-accounts met bestaande accounts die zijn gemaakt in de cloud en u geen gebruikmaakt van Exchange Online en deze functie is handig. In dit scenario hebt u doorgaans niet een reden om de SMTP-kenmerk instellen in de cloud.

Deze functie is op al dan niet standaard voor nieuwe gemaakt Azure AD-mappen. U kunt zien als deze functie is ingeschakeld voor u door te voeren:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Als deze functie is niet ingeschakeld voor uw Azure AD-adreslijst, kunt klik u deze inschakelen door te voeren:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName-updates worden gesynchroniseerd
In het verleden, updates van het UserPrincipalName-kenmerk met de synchronisatieservice vanuit on-premises is geblokkeerd, tenzij aan deze voorwaarden wordt voldaan:

- De gebruiker wordt beheerd (niet-federatief).
- De gebruiker heeft geen licentie is toegewezen.

Zie de [gebruikersnamen in Office 365, Azure of Intune niet overeenkomen met de on-premises implementatie UPN of de alternatieve aanmeldings-ID](https://support.microsoft.com/kb/2523192)voor meer informatie.

Deze functie inschakelt, kunt de synchronisatie-engine de userPrincipalName bijwerken wanneer dit gewijzigde on-premises wordt en u Wachtwoordsynchronisatie gebruiken. Als u Federatie gebruikt, wordt deze functie wordt niet ondersteund.

Deze functie is op al dan niet standaard voor nieuwe gemaakt Azure AD-mappen. U kunt zien als deze functie is ingeschakeld voor u door te voeren:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Als deze functie is niet ingeschakeld voor uw Azure AD-adreslijst, kunt klik u deze inschakelen door te voeren:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Nadat u deze functie inschakelt, bestaande userPrincipalName-waarden, blijven de als-is. Klik op volgende wijziging van het userPrincipalName-kenmerk on-premises implementatie verandert de normale delta synchroniseren op gebruikers de UPN.  

## <a name="see-also"></a>Zie ook

- [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md)
- [Integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
