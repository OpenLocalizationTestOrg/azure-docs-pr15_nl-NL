
<properties
    pageTitle="Geavanceerde regels maken met kenmerken | Microsoft Azure"
    description="Hoe-aan de van geavanceerde regels maken voor een groep inclusief expressie regel operatoren en parameters ondersteunde."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules"></a>Geavanceerde regels maken met behulp van kenmerken

De portal van Azure klassieke biedt de mogelijkheid om geavanceerde regels om te schakelen complexere kenmerk gebaseerde dynamische lidmaatschappen voor (Azure AD) Azure Active Directory-groepen te maken.  

Wanneer het systeem evalueert eventuele kenmerken van een gebruiker wijzigen, worden alle regels voor dynamische groep in een map om te zien als de wijziging van de gebruiker een groep wilt activeren toegevoegd of verwijderd. Als een gebruiker voldoet aan een regel op een groep, worden ze toegevoegd als lid aan die groep. Als ze niet meer voldoen aan de regel van een groep die ze lid zijn, worden ze verwijderd als een lid uit die groep.

## <a name="to-create-the-advanced-rule"></a>De geavanceerde regel maken

1. Klik in de [portal van Azure klassieke](https://manage.windowsazure.com)Selecteer **Active Directory**en open vervolgens de adreslijst van uw organisatie.

2. Selecteer het tabblad **groepen** en open vervolgens de groep die u wilt bewerken.

3. Selecteer het tabblad **configureren** , selecteer de optie **geavanceerde regel** en voer vervolgens de geavanceerde regel in het tekstvak.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Het bouwen van de hoofdtekst van een geavanceerde regel

De geavanceerde regel die u voor de dynamische lidmaatschappen voor groepen maken kunt is in principe een binaire expressie die bestaat uit drie delen en resulteert in een uitkomst true of false. De drie onderdelen zijn:

- Links parameter
- Binaire operator
- Rechts constante

Een volledige geavanceerde regel ziet er ongeveer als volgt: (leftParameter binaryOperator "RightConstant"), waarbij het openen en een haakje sluiten zijn vereist voor de hele binaire expressie, dubbele aanhalingstekens is geplaatst zijn vereist voor de juiste constante en de syntaxis voor de parameter links user.property. Een geavanceerde regel kan bestaan uit meer dan één binaire expressies gescheiden door de - en, - of, en - niet logische operatoren.
Hier volgen enkele voorbeelden van een goed gebouwd geavanceerde regel:

- ("Verkoop" user.department - eq)- of (user.department - eq "Marketing")
- ("Verkoop" user.department - eq)- en - niet (user.jobTitle-bevat "SDE")

Zie de onderstaande secties voor de volledige lijst met ondersteunde parameters en expressie regel operatoren.

De totale duur van de hoofdtekst van uw geavanceerde regel niet langer zijn dan de limiet van 2048 tekens.

> [AZURE.NOTE]
>Tekenreeks en regex bewerkingen zijn hoofdlettergevoelig. U kunt ook een null-waarden controles, via $null als een constante, zoals, user.department - eq $null uitvoeren.
Tekenreeksen met aanhalingstekens "moet worden voorafgegaan met ' teken, bijvoorbeeld user.department - eq \`'Verkoop'.

## <a name="supported-expression-rule-operators"></a>Ondersteunde expressie regel operatoren
De volgende tabel bevat alle ondersteunde expressie regel operatoren en hun syntaxis moet worden gebruikt in de hoofdtekst van de geavanceerde regel:

| Operator        | Syntaxis         |
|-----------------|----------------|
| Niet gelijk is aan      | -nieuwe            |
| Is gelijk aan          | -eq            |
| Niet begint met | -notStartsWith |
| Begint met     | -startsWith    |
| Niet bevat    | -notContains   |
| Bevat        | -bevat      |
| Niet van de zoekwaarde       | -notMatch      |
| De zoekwaarde           | -overeenkomen         |


## <a name="query-error-remediation"></a>Query fout remediation
De volgende tabel worden mogelijke fouten en hoe deze worden gecorrigeerd die zich voordoen

| Fout van query-parser     | Gebruik van de fout       | Gecorrigeerde gebruik             |
|-----------------------|-------------------|-----------------------------|
| Fout: Kenmerk wordt niet ondersteund.                                      | (user.invalidProperty - eq 'Waarde')       | (user.department - eq 'waarde')<br/>Eigenschap moet overeenkomen met een van de [lijst met eigenschappen ondersteund](#supported-properties).                          |
| Fout: Operator wordt niet ondersteund op kenmerk.                       | (user.accountEnabled-bevat waar)                                                                               | (user.accountEnabled - eq waar)<br/>Eigenschap bevat Typ Booleaanse waarde. Gebruik van de ondersteunde operators (-eq of - nieuwe) op Booleaans type in de bovenstaande lijst.                                                                                                                                   |
| : Fout Query-gecompileerd.                                      | ("Verkoop" user.department - eq)- en (user.department - eq "Marketing")(user.userPrincipalName-match"*@domain.ext") | ("Verkoop" user.department - eq)- en (user.department - eq "Marketing")<br/>Logische operators moet overeenkomen met een van de bovenstaande lijst van ondersteunde eigenschappen. (user.userPrincipalName-overeenkomen met ".*@domain.ext")or(user.userPrincipalName -overeenkomen met "@domain.ext$")Error in reguliere expressie. |
| Fout: Binaire expressie is niet in de juiste indeling.                     | (user.department – eq 'Verkoop') (user.department - eq 'Verkoop') (user.department-eq 'Verkoop')                             | (user.accountEnabled - eq waar)- en (user.userPrincipalName-bevat"alias@domain")<br/>Query bevat meerdere fouten. Haakjes niet in de juiste plaats verzonden.                                                                                                                            |
| Fout: Onbekende fout opgetreden tijdens het instellen van dynamische lidmaatschappen. | (user.accountEnabled - eq "True" AND user.userPrincipalName-bevat"alias@domain")                               | (user.accountEnabled - eq waar)- en (user.userPrincipalName-bevat"alias@domain")<br/>Query bevat meerdere fouten. Haakjes niet in de juiste plaats verzonden.                                                                                                                            |

## <a name="supported-properties"></a>Ondersteunde eigenschappen
Hierna ziet u alle gebruikerseigenschappen die u in de geavanceerde regel gebruiken kunt:

### <a name="properties-of-type-boolean"></a>Eigenschappen van Typ Booleaanse waarde

Toegestane operatoren

* -eq


* -nieuwe


| Eigenschappen     | Toegestane waarden  | Gebruik                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | waar onwaar      | user.accountEnabled - eq waar)  |
| dirSyncEnabled | waar, ONWAAR null | (user.dirSyncEnabled - eq waar) |

### <a name="properties-of-type-string"></a>Eigenschappen van het type tekenreeks

Toegestane operatoren

* -eq


* -nieuwe


* -notStartsWith


* -StartsWith


* -bevat


* -notContains


* -overeenkomen


* -notMatch

| Eigenschappen                 | Toegestane waarden                                                                                        | Gebruik                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| plaats                       | Een tekenreekswaarde of $null                                                                           | (user.city - eq 'waarde')                                   |
| land                    | Een tekenreekswaarde of $null                                                                            | (user.country - eq 'waarde')                                |
| afdeling                 | Een tekenreekswaarde of $null                                                                          | (user.department - eq 'waarde')                             |
| Weergavenaam                | Een tekenreekswaarde                                                                                 | (user.displayName - eq 'waarde')                            |
| facsimileTelephoneNumber   | Een tekenreekswaarde of $null                                                                           | (user.facsimileTelephoneNumber - eq 'waarde')               |
| givenName                  | Een tekenreekswaarde of $null                                                                           | (user.givenName - eq 'waarde')                              |
| Functie                   | Een tekenreekswaarde of $null                                                                           | (user.jobTitle - eq 'waarde')                               |
| e-mail                       | Een tekenreekswaarde of $null (SMTP-adres van de gebruiker)                                                  | (user.mail - eq 'waarde')                                   |
| mailNickName               | Een tekenreekswaarde (e-mail alias van de gebruiker)                                                            | (user.mailNickName - eq 'waarde')                           |
| mobiele                     | Een tekenreekswaarde of $null                                                                           | (user.mobile - eq 'waarde')                                 |
| object-id                   | GUID van het gebruikersobject                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Geen DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Een tekenreekswaarde of $null                                                                            | (user.physicalDeliveryOfficeName - eq 'waarde')             |
| Postcode                 | Een tekenreekswaarde of $null                                                                            | (user.postalCode - eq 'waarde')                             |
| preferredLanguage          | ISO 639-1-code                                                                                        | (user.preferredLanguage - eq "en-US")                      |
| sipProxyAddress            | Een tekenreekswaarde of $null                                                                            | (user.sipProxyAddress - eq 'waarde')                        |
| de staat                      | Een tekenreekswaarde of $null                                                                            | (user.state - eq 'waarde')                                  |
| streetAddress              | Een tekenreekswaarde of $null                                                                            | (user.streetAddress - eq 'waarde')                          |
| Achternaam                    | Een tekenreekswaarde of $null                                                                            | (user.surname - eq 'waarde')                                |
| telephoneNumber            | Een tekenreekswaarde of $null                                                                            | (user.telephoneNumber - eq 'waarde')                        |
| usageLocation              | Twee letters landnummer                                                                           | (user.usageLocation - eq "VS")                             |
| userPrincipalName          | Een tekenreekswaarde                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | lid Gast $null                                                                                    | (user.userType - eq "Lid")                              |

### <a name="properties-of-type-string-collection"></a>Eigenschappen van het type tekenreeks siteverzameling

Toegestane operatoren

* -bevat


* -notContains

| Poperties      | Toegestane waarden                        | Gebruik                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Een tekenreekswaarde                      | (user.otherMails-bevat"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-bevat "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Extensie kenmerken en aangepaste kenmerken
Extensie kenmerken en aangepaste kenmerken worden ondersteund in dynamische lidmaatschapsregels.

Extensie kenmerken worden gesynchroniseerd vanuit on-premises venster Server AD en maakt u de opmaak van "ExtensionAttributeX", waarbij X gelijk is aan 1-15.
Een voorbeeld van een regel die gebruikmaakt van een extensie-kenmerk is

(user.extensionAttribute15 - eq "Marketing")

Aangepaste kenmerken worden gesynchroniseerd vanuit on-premises Windows Server AD of vanuit een verbonden SaaS-toepassing en het de notatie van "user.extension_[GUID]\__ [kenmerk]", waarbij [GUID] de unieke id in AAD voor de toepassing waarmee het kenmerk AAD gemaakt en [kenmerk] de naam van het kenmerk is wanneer deze is gemaakt.
Een voorbeeld van een regel die gebruikmaakt van een aangepaste kenmerk is

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

De aangepaste kenmerknaam vindt u in de map door de query's uitvoeren van een gebruiker bevindt zich kenmerk Graph Verkenner en zoeken naar de naam van het kenmerk.

## <a name="direct-reports-rule"></a>Ondergeschikten regel
U kunt nu leden in een groep op basis van het kenmerk manager van een gebruiker vullen.

**Een groep configureren als een groep "Manager"**

1. Klik in de klassieke portal Azure **Active Directory**op en klik vervolgens op de naam van de adreslijst van uw organisatie.

2. Selecteer het tabblad **groepen** en open vervolgens de groep die u wilt bewerken.

3. Selecteer het tabblad **configureren** en selecteer **Geavanceerde regel**.

4. Typ de regel met de volgende syntaxis:

    Directe rapporten voor *directe ondergeschikten voor {obectID_of_manager}*. Een voorbeeld van een geldige regel voor directe ondergeschikten is

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    waar is "62e19b97-8b3d-4d4a-a106-4ce66896a863" id van de manager. De object-ID vindt u in de Azure AD op het **tabblad profiel** van de pagina voor de gebruiker die de manager.

3. Bij het opslaan van deze regel, worden alle gebruikers die voldoen aan de regel worden toegevoegd als lid van de groep. Het kan enkele minuten duren voordat de groep in eerste instantie vullen.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Regels voor het apparaatobjecten maken met behulp van kenmerken

U kunt ook een regel die Hiermee selecteert u apparaatobjecten voor lidmaatschap in een groep maken. De volgende kenmerken van apparaat kunnen worden gebruikt:

| Eigenschappen              | Toegestane waarden                  | Gebruik                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| Weergavenaam             | een tekenreekswaarde                | (device.displayName - eq "Flip Iphone")                       |
| deviceOSType            | een tekenreekswaarde                | (device.deviceOSType - eq "IOS")                             |
| deviceOSVersion         | een tekenreekswaarde                | (apparaat. Versie_besturing - eq "9.1")                                |
| isDirSynced             | waar, ONWAAR null                 | (device.isDirSynced - eq "true")                             |
| isManaged               | waar, ONWAAR null                 | (device.isManaged - eq 'onwaar')                              |
| isCompliant             | waar, ONWAAR null                 | (device.isCompliant - eq "true")                             |
| deviceCategory          | een tekenreekswaarde                | (device.deviceCategory - eq "")                              |
| deviceManufacturer      | een tekenreekswaarde                | (device.deviceManufacturer - eq 'Microsoft')                 |
| deviceModel             | een tekenreekswaarde                | (device.deviceModel - eq "IPhone 7 +")                        |
| deviceOwnership         | een tekenreekswaarde                | (device.deviceOwnership - eq "")                             |
| Domeinnaam              | een tekenreekswaarde                | (device.domainName - eq "contoso.com")                       |
| enrollmentProfileName   | een tekenreekswaarde                | (device.enrollmentProfileName - eq "")                       |
| isRooted                | waar, ONWAAR null                 | (device.deviceOSType - eq "true")                            |
| managementType          | een tekenreekswaarde                | (device.managementType - eq "")                              |
| organizationalUnit      | een tekenreekswaarde                | (device.organizationalUnit - eq "")                          |
| deviceId                | een geldig deviceId                | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [AZURE.NOTE]
> Deze regels apparaat kunnen niet worden gemaakt met de vervolgkeuzelijst 'eenvoudige regel' in de portal van Azure klassieke.


## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Problemen met dynamische lidmaatschappen voor groepen](active-directory-accessmanagement-troubleshooting.md)

* [Toegang tot resources met Azure Active Directory-groepen beheren](active-directory-manage-groups.md)

* [Azure Active Directory-cmdlets voor het configureren van de groepsinstellingen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
