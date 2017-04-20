<properties
    pageTitle="Azure Active Directory controle API verwijzing | Microsoft Azure"
    description="Hoe u aan de slag met de controle-API van Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory controle API-verwijzing

In dit onderwerp maakt deel uit van een verzameling onderwerpen over de Azure Active Directory API rapportage.  
Azure AD-rapportage biedt u een API waarmee u kunt de toegang tot controlegegevens met code of gerelateerde hulpprogramma's.
Het bereik van dit onderwerp is om u te voorzien naslaginformatie over de **controle-API**.

Zie:

- [Controlelogboeken bijhouden](active-directory-reporting-azure-portal.md#audit-logs) voor meer informatie
- [Aan de slag met de API in de rapportage Azure Active Directory](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage API.

Voor vragen, problemen of feedback, neemt u contact [AAD rapportage Help](mailto:aadreportinghelp@microsoft.com).


## <a name="who-can-access-the-data"></a>Wie toegang heeft tot de gegevens?

- Gebruikers in de rol van de beheerder van de beveiliging of beveiliging Reader

- Globale beheerders

- Een app met de machtiging voor toegang tot de API (app autorisatie steekt nogal setup alleen op basis van globale beheerder toestemming)



## <a name="prerequisites"></a>Vereisten voor

Wilt u toegang tot dit rapport via de API rapportage, moet u het volgende hebben:

- Een [Azure Active Directory gratis of betere edition](active-directory-editions.md)

- De [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md)voltooid. 
 

##<a name="accessing-the-api"></a>Toegang krijgen tot de API

Kunt u deze API via de [Graph Explorer](https://graphexplorer2.cloudapp.net) of via een programma openen via, bijvoorbeeld PowerShell. In de volgorde voor PowerShell correct interpreteren van de syntaxis van de OData-filter in AAD Graph REST oproepen gebruikt, moet u de backtick (ook bekend: accent grave) teken "escapeteken' de $. Het teken backtick fungeert als [de PowerShell escape-teken](https://technet.microsoft.com/library/hh847755.aspx), zodat PowerShell moet u een letterlijke interpretatie van het teken $ doen en deze als de naam van een PowerShell-variabele verwarrende voorkomen (ie: $filter).

De nadruk in dit onderwerp is op de grafiek Verkenner. Zie dit [PowerShell-script](active-directory-reporting-api-audit-samples.md#powershell-script)voor een voorbeeld PowerShell.

 
 

## <a name="api-endpoint"></a>API-eindpunt


U kunt deze API voor het gebruik van de volgende URI openen:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Er is geen limiet van het aantal records geretourneerd door de Azure AD controle API (via OData paginering).
Voor de grenzen van het bewaarbeleid voor rapportagegegevens, raadpleegt u [Bewaarbeleid rapportage](active-directory-reporting-retention.md).

In dit gesprek retourneert de gegevens in batches. Elke batch heeft maximaal 1000 records.  
Als u de volgende batch records, gebruikt u de volgende koppeling. U kunt de skiptoken-gegevens van de eerste set geretourneerde records. Het token overslaan wordt aan het einde van het resultaat ingesteld.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Ondersteunde filters

U kunt het aantal records die zijn geretourneerd door een API verfijnen bellen in de vorm van een filter.  
Voor aanmelden API gerelateerde gegevens worden weergegeven, de volgende filters worden ondersteund:

- **$top =\<aantal records moeten worden geretourneerd\> ** : om het aantal geretourneerde records te beperken. Dit is een dure bewerking. U moet dit filter niet gebruiken als u wilt retourneren duizenden objecten.     
- **$filter =\<uw filterinstructie\> ** - om op te geven, op basis van de ondersteunde filtervelden, het type records dat u belangrijk vindt



## <a name="supported-filter-fields-and-operators"></a>Ondersteunde filtervelden en operatoren

Als u wilt opgeven van het type records dat u belangrijk vindt, kunt u een filterinstructie met een bestand of een combinatie van de volgende filtervelden kunt maken:

- [ActiviteitDatum](#activitydate) - Hiermee definieert u een datum of een datumbereik
- [activityType](#activitytype) - definieert het type van een activiteit
- [activiteit](#activity) - definieert de activiteit als tekenreeks  
- [naam/naam van de acteur](#actorname) - definieert de acteur in de vorm van de naam van de acteur
- [acteur/object-id](#actorobjectid) - definieert de acteur in de vorm van een van de acteur-ID   
- [acteur/upn](#actorupn) - definieert de acteur in de vorm van een van de acteur principe (user Principal name) 
- [target/naam](#targetname) - de doelgroep wordt gedefinieerd in de vorm van de naam van de acteur
- [target/object-id](#targetobjectid) - de doelgroep wordt gedefinieerd in de vorm van van het doel-ID  
- [target/upn](#targetupn) - definieert de acteur in de vorm van een van de acteur principe (user Principal name)   




----------

### <a name="activitydate"></a>activityDate

**Ondersteunde operatoren**: eq, ge, bestand, BT, lt

**Voorbeeld**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**Notities**:

datum-/ moet UTC-indeling

----------

### <a name="activitytype"></a>activityType

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=activityType eq 'User'  

**Notities**:

hoofdlettergevoelig

----------

### <a name="activity"></a>activiteit

**Ondersteunde operatoren**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**Notities**:

hoofdlettergevoelig

----------

### <a name="actorname"></a>naam van de acteur /

**Ondersteunde operatoren**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**Notities**:

niet hoofdlettergevoelig

    

----------
### <a name="actorobjectid"></a>acteur/object-id

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>TARGET/naam

**Ondersteunde operatoren**: eq, bevat, startsWith

**Voorbeeld**:

    $filter=targets/any(t: t/name eq 'some name')   

**Notities**:

Niet hoofdlettergevoelig

----------

### <a name="targetupn"></a>TARGET/upn

**Ondersteunde operatoren**: eq, startsWith

**Voorbeeld**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**Notities**:

- Niet hoofdlettergevoelig
- Moet u de volledige naamruimte toevoegen bij het Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity opvragen

----------

### <a name="targetobjectid"></a>target/object-id

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>acteur/upn

**Ondersteunde operatoren**: eq, startsWith

**Voorbeeld**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**Notities**:

- Niet hoofdlettergevoelig 
- Moet u de volledige naamruimte toevoegen bij het Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity opvragen

----------




## <a name="next-steps"></a>Volgende stappen

- Wilt u voorbeelden voor het gefilterde systeemactiviteiten zien? Bekijk de [Azure Active Directory controle API voorbeelden](active-directory-reporting-api-audit-samples.md).

- Wilt u meer weten over de Azure AD API rapporteren? Zie [aan de slag met de API in de rapportage Azure Active Directory](active-directory-reporting-api-getting-started.md).