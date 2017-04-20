<properties
    pageTitle="Azure Active Directory-aanmelding activiteitenrapport API verwijzing | Microsoft Azure"
    description="Overzicht van de Azure Active Directory-aanmelding activiteit rapport-API"
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
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory-aanmelding activiteitenrapport API-verwijzing


In dit onderwerp maakt deel uit van een verzameling onderwerpen over de Azure Active Directory API rapportage.  
Azure AD-rapportage biedt u een API waarmee u kunt de toegang tot aanmeldingsproblemen activiteit report with data met code of gerelateerde hulpprogramma's.
Het bereik van dit onderwerp is om u te voorzien naslaginformatie over de **activiteit rapport API aanmeldingsproblemen**.

Zie:

- [Aanmeldingsproblemen activiteiten](active-directory-reporting-azure-portal.md#sign-in-activities) voor meer informatie
- [Aan de slag met de API in de rapportage Azure Active Directory](active-directory-reporting-api-getting-started.md) voor meer informatie over de rapportage API.

Voor vragen, problemen of feedback, neemt u contact [AAD rapportage Help](mailto:aadreportinghelp@microsoft.com).



## <a name="who-can-access-the-api-data"></a>Wie toegang heeft tot de API-gegevens?

- Gebruikers in de rol van de beheerder van de beveiliging of beveiliging Reader

- Globale beheerders

- Een app met de machtiging voor toegang tot de API (app autorisatie steekt nogal setup alleen op basis van globale beheerder toestemming)



## <a name="prerequisites"></a>Vereisten voor

Voor toegang tot dit rapport via de API voor rapportage, moet u het volgende hebben:

- Een [Azure Active Directory Premium P1 of P2 edition](active-directory-editions.md)

- De [vereisten voor toegang tot de Azure AD rapportage-API](active-directory-reporting-api-prerequisites.md)voltooid. 


##<a name="accessing-the-api"></a>Toegang krijgen tot de API

Kunt u deze API via de [Graph Explorer](https://graphexplorer2.cloudapp.net) of via een programma openen gebruikt, bijvoorbeeld PowerShell. In de volgorde voor PowerShell correct interpreteren van de syntaxis van de OData-filter in AAD Graph REST oproepen gebruikt, moet u de backtick (ook bekend: accent grave) teken "escapeteken' de $. Het teken backtick fungeert als [de PowerShell escape-teken](https://technet.microsoft.com/library/hh847755.aspx), zodat PowerShell moet u een letterlijke interpretatie van het teken $ doen en deze als de naam van een PowerShell-variabele verwarrende voorkomen (ie: $filter).

De nadruk in dit onderwerp is op de grafiek Verkenner. Zie dit [PowerShell-script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script)voor een voorbeeld PowerShell.


## <a name="api-endpoint"></a>API-eindpunt

U kunt deze API voor het gebruik van de volgende basis-URI openen:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Vanwege de hoeveelheid gegevens heeft dit API een limiet van een miljoen records geretourneerd. 

In dit gesprek retourneert de gegevens in batches. Elke batch heeft maximaal 1000 records.  
Als u de volgende batch records, gebruikt u de volgende koppeling. U kunt de [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) -gegevens van de eerste set geretourneerde records. Het token overslaan wordt aan het einde van het resultaat ingesteld.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Ondersteunde filters

U kunt het aantal records die zijn geretourneerd door een API verfijnen belt u in de vorm van een filter.  
Voor aanmelden API gerelateerde gegevens worden weergegeven, de volgende filters worden ondersteund:

- **$top =\<aantal records moeten worden geretourneerd\> ** : om het aantal geretourneerde records te beperken. Dit is een dure bewerking. U moet dit filter niet gebruiken als u wilt retourneren duizenden objecten.  
- **$filter =\<uw filterinstructie\> ** - om op te geven, op basis van de ondersteunde filtervelden, het type records dat u belangrijk vindt



## <a name="supported-filter-fields-and-operators"></a>Ondersteunde filtervelden en operatoren

Als u wilt opgeven van het type records dat u belangrijk vindt, kunt u een filterinstructie met een bestand of een combinatie van de volgende filtervelden kunt maken:

- [signinDateTime](#signindatetime) - Hiermee definieert u een datum of een datumbereik

- [gebruikers-id](#userid) - Hiermee definieert u een specifieke gebruiker op basis van de gebruikers-ID.

- [userPrincipalName](#userprincipalname) - Hiermee definieert u een specifieke gebruiker op basis van de gebruiker UPN (User Principal Name)

- een specifiek Hiermee definieert u [toepassings-id](#appid) - app op basis van de app-ID

- een specifiek Hiermee definieert u [appDisplayName](#appdisplayname) - app op basis van de app-weergavenaam

- [loginStatus](#loginStatus) - definieert de status van de aanmeldingen (success / mislukt)


> [AZURE.NOTE] Wanneer u een grafiek Explorer gebruikt, kunt u hoeft te gebruiken van de juiste hoofdletters/kleine letters voor elke letter is uw filtervelden.


Als u wilt beperken het bereik van de geretourneerde gegevens, kunt u combinaties van de ondersteunde filters en filtervelden maken. De volgende instructie wordt bijvoorbeeld de bovenste 10 records tussen juli 1e 2016 en juli 6e 2016:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**Ondersteunde operatoren**: eq, ge bestand, BT, lt

**Voorbeeld**:

Gebruik van een bepaalde datum

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



Gebruik van een datumbereik    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Notities**:

De datetime-parameter moet in de UTC-indeling 


----------

### <a name="userid"></a>gebruikers-id

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Notities**:

De waarde van de gebruikers-id is een tekenreekswaarde



----------

### <a name="userprincipalname"></a>userPrincipalName

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Notities**:

De waarde van userPrincipalName is een tekenreekswaarde

----------

### <a name="appid"></a>toepassings-id

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Notities**:

De waarde van toepassings-id is een tekenreekswaarde

----------


### <a name="appdisplayname"></a>appDisplayName

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Notities**:

De waarde van appDisplayName is een tekenreekswaarde

----------

### <a name="loginstatus"></a>loginStatus

**Ondersteunde operatoren**: eq

**Voorbeeld**:

    $filter=loginStatus+eq+'1'  


**Notities**:

Er zijn twee opties voor de loginStatus: 0 - succes, 1 - mislukt

----------



## <a name="next-steps"></a>Volgende stappen

- Wilt u ziet u voorbeelden van activiteiten voor gefilterde aanmelden? Bekijk de [Azure Active Directory-aanmelding activiteit rapport API voorbeelden](active-directory-reporting-api-sign-in-activity-samples.md).

- Wilt u meer weten over de Azure AD API rapporteren? Zie [aan de slag met de API in de rapportage Azure Active Directory](active-directory-reporting-api-getting-started.md).