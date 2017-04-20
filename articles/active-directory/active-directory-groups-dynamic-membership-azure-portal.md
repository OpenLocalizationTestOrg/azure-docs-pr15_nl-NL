
<properties
    pageTitle="Geavanceerde regels voor groepslidmaatschap maken in de proefversie van Azure Active Directory met kenmerken | Microsoft Azure"
    description="Het maken van geavanceerde regels voor dynamische groepslidmaatschap inclusief ondersteund expressie regel operatoren en parameters."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules-for-group-membership-in-azure-active-directory-preview"></a>Kenmerken gebruiken om te maken van geavanceerde regels voor het groepslidmaatschap in de proefversie van Azure Active Directory

De portal van Azure biedt de mogelijkheid om geavanceerde regels om te schakelen complexere kenmerk gebaseerde dynamische lidmaatschappen van Azure Active Directory (Azure AD) preview-groepen te maken. [Wat staat er in de Preview-versie?](active-directory-preview-explainer.md) Dit artikel wordt uitgelegd de kenmerken van de regel- en syntaxisfouten deze geavanceerde regels maken.

## <a name="to-create-the-advanced-rule"></a>De geavanceerde regel maken

1.  Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account dat is een globale beheerder voor de adreslijst.

2.  **Meer services**selecteren, voert u **gebruikers en groepen** in het tekstvak en druk vervolgens op **Enter**.

  ![Gebruikersbeheer openen](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Selecteer **alle groepen**op het blad **gebruikers en groepen** .

  ![Het blad groepen openen](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Klik op het blad **gebruikers en groepen: alle groepen** , selecteer de opdracht **toevoegen** .

  ![Nieuwe groep toevoegen](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Voer een naam en beschrijving voor de nieuwe groep op het blad **groep** . Selecteer een **typt u het lidmaatschap** van **Dynamische gebruiker** of **Dynamische apparaat**, afhankelijk van wat u wilt een regel maken voor gebruikers of apparaten, en selecteer vervolgens **de dynamische query toevoegen**. Voor de kenmerken die wordt gebruikt voor regels die apparaat, raadpleegt u [werken met kenmerken om regels voor apparaatobjecten te maken](#using-attributes-to-create-rules-for-device-objects).

  ![Dynamische lidmaatschap regel toevoegen](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. Klik op het blad **dynamische lidmaatschapsregels** , voert u de regel in het vak **Geavanceerde dynamische lidmaatschap-regel toevoegen** , drukt u op Enter en selecteer vervolgens **maken** onder aan het blad.

7. Selecteer **maken** op het blad **groep** naar de groep hebt gemaakt.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Het bouwen van de hoofdtekst van een geavanceerde regel

De geavanceerde regel die u voor de dynamische lidmaatschappen voor groepen maken kunt is in principe een binaire expressie die bestaat uit drie delen en resulteert in een uitkomst true of false. De drie onderdelen zijn:

- Links parameter
- Binaire operator
- Rechts constante

Een volledige geavanceerde regel ziet er ongeveer als volgt: (leftParameter binaryOperator "RightConstant"), waarbij het openen en een haakje sluiten zijn vereist voor de hele binaire expressie, dubbele aanhalingstekens is geplaatst zijn vereist voor de juiste constante en de syntaxis voor de parameter links user.property. Een geavanceerde regel kan bestaan uit meer dan één binaire expressies gescheiden door de - en, - of, en - niet logische operatoren.

Hier volgen enkele voorbeelden van een goed gebouwd geavanceerde regel:

- ("Verkoop" user.department - eq)- of (user.department - eq "Marketing")
- ("Verkoop" user.department - eq)- en - niet (user.jobTitle-bevat "SDE")

Zie de onderstaande secties voor de volledige lijst met ondersteunde parameters en expressie regel operatoren. Voor de kenmerken die wordt gebruikt voor regels die apparaat, raadpleegt u [werken met kenmerken om regels voor apparaatobjecten te maken](#using-attributes-to-create-rules-for-device-objects).

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

1. Volg de stappen 1 tot en met 5 in [de geavanceerde regel maken](#to-create-the-advanced-rule)en selecteer een **type lidmaatschap** van **Dynamische gebruiker**.

2. Voer op het blad **dynamische lidmaatschapsregels** in de regel met de volgende syntaxis:

    Directe rapporten voor *directe ondergeschikten voor {obectID_of_manager}*. Een voorbeeld van een geldige regel voor directe ondergeschikten is

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    waar is "62e19b97-8b3d-4d4a-a106-4ce66896a863" id van de manager. De object-ID vindt u in de Azure AD op het **tabblad profiel** van de pagina voor de gebruiker die de manager.

3. Bij het opslaan van deze regel, worden alle gebruikers die voldoen aan de regel worden toegevoegd als lid van de groep. Het kan enkele minuten duren voordat de groep in eerste instantie vullen.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Regels voor het apparaatobjecten maken met behulp van kenmerken

U kunt ook een regel die Hiermee selecteert u apparaatobjecten voor lidmaatschap in een groep maken. De volgende kenmerken van apparaat kunnen worden gebruikt:

| Eigenschappen           | Toegestane waarden                  | Gebruik                                                |
|----------------------|---------------------------------|------------------------------------------------------|
| Weergavenaam          | een tekenreekswaarde                | (device.displayName - eq "Flip Iphone")                 |
| deviceOSType         | een tekenreekswaarde                | (device.deviceOSType - eq "IOS")                      |
| deviceOSVersion      | een tekenreekswaarde                | (apparaat. Versie_besturing - eq "9.1")                         |
| isDirSynced          | waar, ONWAAR null                 | (device.isDirSynced - eq "true")                      |
| isManaged            | waar, ONWAAR null                 | (device.isManaged - eq 'onwaar')                       |
| isCompliant          | waar, ONWAAR null                 | (device.isCompliant - eq "true")                      |


## <a name="additional-information"></a>Aanvullende informatie
Deze artikelen bevatten aanvullende informatie over groepen in Azure Active Directory.

* [Zie bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en toevoegen van leden](active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Lidmaatschappen van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
