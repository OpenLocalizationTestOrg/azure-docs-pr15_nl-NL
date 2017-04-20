<properties
    pageTitle="Synchronisatie van Azure AD Connect: lidmaatschap declaratieve inrichting expressies | Microsoft Azure"
    description="Dit artikel wordt uitgelegd de declaratieve inrichten expressies."
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
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Synchronisatie van Azure AD Connect: lidmaatschap declaratieve inrichting van expressies
Azure AD Connect-synchronisatie dat is gebaseerd op het declaratieve inrichting geïntroduceerd in Forefront identiteit Manager 2010. U kunt uw volledige identiteit integratie bedrijfslogica zonder te hoeven gecompileerde programmacode schrijven implementeren.

Een essentieel onderdeel van de inrichting van declaratieve is de expressietaal die wordt gebruikt in kenmerk loopt. De taal die wordt gebruikt, is een subset van Microsoft Visual Basic® for Applications (VBA). Deze taal wordt gebruikt in Microsoft Office en ook door gebruikers met ervaring van VBScript wordt herkend. De expressietaal declaratieve inrichting wordt alleen gebruikt voor functies en is niet een gestructureerde taal. Er zijn geen methoden of -instructies. Functies zijn in plaats daarvan genest naar express programma-mailstroom.

Zie [Welkom bij de Visual Basic for Applications-Naslaggids voor Office 2013](https://msdn.microsoft.com/library/gg264383.aspx)voor meer informatie.

De kenmerken die zijn ingevoerd. Een functie accepteert alleen kenmerken van het juiste type. Het is ook hoofdlettergevoelig. Zowel de functienamen van de en kenmerk moet juiste hoofdlettergebruik of een fout gegenereerd.

## <a name="language-definitions-and-identifiers"></a>Definities van taal- en -id 's

- Functies hebben een andere naam gevolgd door de argumenten vierkante haken: functienaam (1 van de argumenten, argument N).
- Kenmerken worden aangeduid met vierkante haken: [kenmerknaam]
- Parameters worden aangeduid met percentage positief of negatief: ParameterName %
- Tekenreeksconstanten tussen aanhalingstekens zijn geplaatst: bijvoorbeeld "Contoso" (notitie: rechte aanhalingstekens "" en geen gekrulde aanhalingstekens "")
- Numerieke waarden zijn uitgedrukt zonder offertes en verwacht decimaal. Hexadecimale waarden worden voorafgegaan door & H. Bijvoorbeeld 98052 & HFF
- Booleaanse waarden worden uitgedrukt met constanten: waar, ONWAAR.
- Ingebouwde constanten en de letterlijke waarden worden uitgedrukt met alleen hun naam: null-waarden, CRLF, IgnoreThisFlow

### <a name="functions"></a>Functies
Declaratieve inrichting veel functies gebruikt om in te schakelen van de mogelijkheid om te transformeren kenmerkwaarden. Deze functies kunnen worden genest zodat het resultaat van een functie wordt doorgegeven aan een andere functie.

`Function1(Function2(Function3()))`

De volledige lijst met functies vindt u in de [functieverwijzing](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parameters
Een parameter is gedefinieerd door een verbindingslijn of door een beheerder via PowerShell. Parameters bevatten meestal waarden die verschillen per systeem, bijvoorbeeld de naam van het domein dat de gebruiker bevindt zich in. Deze parameters kunnen worden gebruikt in kenmerk loopt.

Het Active Directory-Connector opgegeven de volgende parameters voor binnenkomende synchronisatieregels:

| Parameternaam | Opmerking |
| --- | --- |
| Domain.Netbios | NetBIOS-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMSALES |
| Domain.FQDN | Opmaak van de FQDN-naam van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld sales.fabrikam.com |
| Domain.LDAP | LDAP-indeling van het domein dat momenteel wordt geïmporteerd, bijvoorbeeld domeincontroller = sales, domeincontroller = fabrikam, domeincontroller = com |
| Forest.Netbios | NetBIOS-notatie van het bos dat momenteel wordt geïmporteerd, bijvoorbeeld FABRIKAMCORP |
| Forest.FQDN | FQDN notatie van het bos dat momenteel wordt geïmporteerd, bijvoorbeeld fabrikam.com |
| Forest.LDAP | LDAP-indeling van de bos naam dat momenteel wordt geïmporteerd, bijvoorbeeld domeincontroller = fabrikam, domeincontroller = com |

Het systeem biedt de volgende parameter wordt gebruikt om toegang te krijgen van de identificatie van de verbindingslijn dat moment actief zijn:  
`Connector.ID`

Hier volgt een voorbeeld waarmee wordt gevuld van het domein metaverse kenmerk met de NetBIOS-naam van het domein waar de gebruiker zich bevindt:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operatoren
De volgende operators kunnen worden gebruikt:

- **Vergelijking**: <, < =, <>, =, >, > =
- **Wiskunde**: +, -, \*, -
- **Tekenreeks**: & (samenvoegen)
- **Logische**: & & (en) || (of)
- **Volgorde van de evaluatie**:)

Operatoren links naar rechts worden geëvalueerd en dezelfde evaluatie prioriteit. Dat wil zeggen de \* (vermenigvuldiger) wordt niet geëvalueerd vóór - (aftrekken). 2\*(5 + 3) is niet hetzelfde als 2\*5 + 3. De haakjes () worden gebruikt voor het wijzigen van de evaluatievolgorde als van links naar rechts evaluatievolgorde niet nodig is.

## <a name="multi-valued-attributes"></a>Met meerdere waarden kenmerken
De functies geldt voor één waarde zowel met meerdere waarden kenmerken. De functie werkt via elke waarde en dezelfde functie geldt voor elke waarde voor meerdere waarden kenmerken.

Bijvoorbeeld:  
`Trim([proxyAddresses])`Voer een knippen van elke waarde in het kenmerk proxyAddress.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Voor elke waarde met een @-sign, vervangen van het domein met @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Zoek het SIP-adres en deze te verwijderen uit de waarden.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configuratiemodel in het [Lidmaatschap declaratieve inrichten](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Zie hoe declaratieve inrichting gebruikte kant-en-klare begrijpen van [de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md)is.
- Lees hoe u een praktische wijziging met declaratieve inrichting in [hoe u een wijziging in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md)aanbrengen.

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

**Naslaginformatie**

- [Synchronisatie van Azure AD Connect: overzicht van functies](active-directory-aadconnectsync-functions-reference.md)
