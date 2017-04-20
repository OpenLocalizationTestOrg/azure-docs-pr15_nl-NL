<properties
    pageTitle="Azure AD Connect systeemstatus versiegeschiedenis"
    description="In dit document beschreven hoe de releases voor Azure AD verbinding gezondheid en wat is opgenomen in die versies."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect status: Geschiedenis van documentversies Release

Azure AD verbinding systeemstatus de Azure Active Directory-team regelmatig bijgewerkt met de nieuwe functies en functionaliteit. In dit artikel worden de versies en functies die beschikbaar zijn.

## <a name="october-2016"></a>Oktober 2016
**Agent bijwerken:**
- Azure AD verbinding systeemstatus agent voor AD FS \(versie 2.6.408.0\)
    1. Verbeteringen in het opsporen van client IP-adressen in verificatieaanvragen
    2. Correcties die betrekking hebben op waarschuwingen
- Azure AD verbinding systeemstatus agent voor AD DS (versie 2.6.408.0)
    1. Correcties die betrekking hebben op meldingen.
- Azure AD verbinding systeemstatus agent voor synchronisatie (versie 2.6.353.0) vrijgegeven met Azure AD Connect versie 1.1.281.0
    1. De vereiste gegevens bieden voor de synchronisatie-fout-rapporten
    2. Correcties die betrekking hebben op waarschuwingen

**Nieuwe functies van de Preview-versie:**
- Synchronisatie-foutenrapporten voor Azure AD verbinding maken

**Nieuwe functies:**
- Azure AD verbinding servicestatus voor AD FS - veld IP-adres is beschikbaar in het rapport over bovenste 50 gebruikers met ongeldige gebruikersnaam en wachtwoord.

## <a name="july-2016"></a>Juli 2016

**Nieuwe functies van de Preview-versie:**

- [Azure AD Connect servicestatus voor AD DS](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Januari 2016


**Agent bijwerken:**

- Azure AD verbinding systeemstatus agent voor AD FS (versie 2.6.91.1512)


**Nieuwe functies:**

- [Test Connectivity Tool voor Azure AD verbinding maken met de systeemstatus kunt vinden](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>November 2015


**Nieuwe functies:**

- Ondersteuning voor [toegangsbeheer op basis van rollen](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)


**Nieuwe functies van de Preview-versie:**

- [Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md).

**Vaste problemen:**

- Correcties op fouten tijdens agent registraties zichtbaar.

## <a name="september-2015"></a>September 2015

**Nieuwe functies:**

- Onjuiste gebruikersnaam wachtwoord rapport voor AD FS
- Ondersteuning voor het configureren van niet-geverifieerde HTTP-Proxy
- Ondersteuning voor het configureren van agent op Server core
- Verbeteringen in waarschuwingen voor AD FS
- Verbeteringen in Azure AD verbinding systeemstatus Agent voor AD FS voor connectiviteit en gegevens uploaden.


**Vaste problemen:**

- Correcties in inzichten gebruik voor AD FS fout typen.


## <a name="june-2015"></a>Juni 2015

**Eerste versie van Azure AD verbinding servicestatus voor AD FS en AD FS-Proxy.**

**Nieuwe functies:**

- Waarschuwingen voor het controleren van AD FS en AD FS-Proxy servers met e-mailmeldingen.
- Eenvoudig toegang tot de AD FS-topologie en patronen in AD FS prestatie-items.
- Trend in geslaagd token aanvragen op AD FS-servers die zijn gegroepeerd op toepassingen, verificatiemethoden, aanvragen netwerk locatie enzovoort.
- Trends in mislukte aanvraag op AD FS-servers die zijn gegroepeerd op toepassingen, fout typen enzovoort.
- Eenvoudiger Agent-implementatie met referenties van Azure AD globale beheerder.  




## <a name="next-steps"></a>Volgende stappen
Meer informatie over [Monitor uw on-premises identiteit infrastructuur en synchronisatie services in de cloud](active-directory-aadconnect-health.md).
