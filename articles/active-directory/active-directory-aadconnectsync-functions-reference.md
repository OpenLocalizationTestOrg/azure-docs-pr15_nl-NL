<properties
    pageTitle="Synchronisatie van Azure AD Connect: functieverwijzing | Microsoft Azure"
    description="Overzicht van declaratieve inrichten expressies in Azure AD Connect synchroniseren."
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Synchronisatie van Azure AD Connect: overzicht van functies

In Azure AD Connect worden functies gebruikt voor het bewerken van een kenmerkwaarde tijdens de synchronisatie.  
De syntaxis van de functies wordt uitgedrukt in de volgende notatie:  
`<output type> FunctionName(<input type> <position name>, ..)`

Als een functie overbelast is en de syntaxis van de meerdere accepteert, wordt elke geldige syntaxis worden vermeld.  
De functies zijn ingevoerd en ze controleren dat het type overeenkomsten de beschreven typen doorgegeven.  
Als het type niet overeenkomen, wordt een fout gegenereerd.

De typen worden uitgedrukt met de volgende syntaxis:

- **opslaglocatie** – binair getal
- **bool** – Booleaanse waarde
- **dt** – UTC datum/tijd
- **opsommen** – inventarisatie van bekende constanten
- **exp** -expressie die resulteren in een Booleaanse waarde wordt verwacht
- **mvbin** – binaire meerdere waarden
- **mvstr** – tekenreeks met meerdere waarden
- **mvref** – met meerdere waarden verwijzing
- **num** – numerieke
- **ref** -verwijzing
- **str** – tekenreeks
- **var** – een variatie van (bijna) een ander type
- **void** – geen waarde als resultaat

De functies met de typen **mvbin**, **mvstr**en **mvref** kunnen alleen werken met meerdere waarden kenmerken. Functies met **opslaglocatie**, **str**en **ref** werken met zowel één waarde als meerdere waarden kenmerken.

## <a name="functions-reference"></a>Overzicht van functies

Lijst met functies | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Conversie** |  
[CBool](#cbool) | [CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Datum / tijd** |  
[DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Nu](#now)
[NumFromDate](#numfromdate) |  
**Directory** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Evaluatie** |  
[IsBitSet](#isbitset) | [IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Wiskundige** |  
[Bit.en](#bitand) | [Bit.of](#bitor) | [RandomNum](#randomnum)
**Met meerdere waarden** |  
[Bevat](#contains) | [Tellen](#count) | [Item](#item) | [ItemOrNull](#itemornull)
[Deelnemen aan](#join) | [RemoveDuplicates](#removeduplicates) | [Gesplitste](#split) |
**Programma-mailstroom** |  
[Fout](#error) | [IIF](#iif)  | [Schakeloptie](#switch)
**Tekst** |  
[GUID](#guid) | [InStr](#instr) | [InStrRev](#instrrev) | [LCase](#lcase)
[Links](#left) | [Lengte](#len) | [De functies LTrim](#ltrim) | [Mid](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Vervangen](#replace)
[ReplaceChars](#replacechars) | [Rechts](#right) | [RTrim](#rtrim) | [Knippen](#trim)
[UCase](#ucase) | [Word](#word)

----------
### <a name="bitand"></a>Bit.en

**Beschrijving:**  
De functie bit.en stelt opgegeven bits van een waarde.

**Syntaxis:**  
`num BitAnd(num value1, num value2)`

- waarde1, waarde2: numerieke waarden die functiesleutels samen moeten worden

**Opmerking:**  
Deze functie beide parameters converteert het binaire getal en wordt een stapje op:

- 0 - als een of beide van de corresponderende bits in *invoermasker* en een *vlag* zijn aan 0
- 1 - als beide of beide van de corresponderende bits 1.

Het resultaat met andere woorden, 0 in alle gevallen, behalve wanneer de bijbehorende bits van beide parameters 1.

**Voorbeeld:**  
`BitAnd(&HF, &HF7)`  
Retourneert 7 omdat deze waarde hexadecimale "F" en "F7" als resultaat.

----------
### <a name="bitor"></a>Bit.of

**Beschrijving:**  
De functie bit.of stelt opgegeven bits van een waarde.

**Syntaxis:**  
`num BitOr(num value1, num value2)`

- waarde1, waarde2: numerieke waarden die samen worden moeten

**Opmerking:**  
Deze functie beide parameters converteert naar de binaire weergave en Hiermee stelt u een stapje 1 als een of beide van de corresponderende bits in invoermasker en een vlag 1 en 0 als beide van de corresponderende bits zijn aan 0. Met andere woorden, wordt 1 in alle gevallen behalve wanneer de bijbehorende bits van beide parameters 0.

----------
### <a name="cbool"></a>CBool

**Beschrijving:**  
De functie CBool geeft een Booleaanse waarde op basis van de geëvalueerde expressie

**Syntaxis:**  
`bool CBool(exp Expression)`

**Opmerking:**  
Als de expressie geëvalueerd naar een andere waarde dan nul, en vervolgens CBool geeft als waar resultaat, anders wordt False.

**Voorbeeld:**  
`CBool([attrib1] = [attrib2])`  

Geeft als resultaat waar als beide kenmerken hebben dezelfde waarde.

----------
### <a name="cdate"></a>CDate

**Beschrijving:**  
De functie CDate geeft als resultaat een DateTime UTC van een tekenreeks bevat. Datum-/ is niet een kenmerktype systeemeigen gesynchroniseerd, maar wordt gebruikt door sommige functies.

**Syntaxis:**  
`dt CDate(str value)`

- Value: Een tekenreeks met een datum, tijd, en eventueel tijdzone

**Opmerking:**  
De geretourneerde tekenreeks is altijd in UTC.

**Voorbeeld:**  
`CDate([employeeStartTime])`  
Geeft als resultaat die een DateTime op basis van de werknemer keer starten

`CDate("2013-01-10 4:00 PM -8")`  
Geeft als resultaat een DateTime dat staat voor ' 2013-01-11 12:00 AM "

----------
### <a name="cguid"></a>CGuid

**Beschrijving:**  
De functie CGuid zet de tekenreeks de vorm van een GUID naar de binaire weergave.

**Syntaxis:**  
`bin CGuid(str GUID)`

- Een tekenreeks die is opgemaakt in dit patroon: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Bevat

**Beschrijving:**  
De functie bevat vindt een tekenreeks binnen een kenmerk met meerdere waarden

**Syntaxis:**  
`num Contains (mvstring attribute, str search)`-hoofdlettergevoelig  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-hoofdlettergevoelig

- kenmerk: het kenmerk met meerdere waarden wilt zoeken.
- zoeken: tekenreeks zoeken in het kenmerk.
- Casetype: CaseInsensitive of CaseSensitive.

Retourneert de index in het kenmerk met meerdere waarden waarin de tekenreeks is gevonden. Als de tekenreeks niet wordt gevonden, wordt 0 geretourneerd.

**Opmerking:**  
Voor meerdere waarden tekenreekskenmerken vindt u de zoekopdracht subreeksen in de waarden.  
Voor verwijzing kenmerken, moet de gezochte tekenreeks precies overeenkomen met de waarde wordt als een overeenkomst beschouwd.

**Voorbeeld:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Als het proxyAddresses-kenmerk een primaire e-mailadres heeft (aangegeven met een hoofdletter "SMTP:"), retourneert het kenmerk proxyAddress, anders een fout geretourneerd.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Beschrijving:**  
De functie ConvertFromBase64 omgezet de waarde van de opgegeven base64-gecodeerde in een normale tekenreeks.

**Syntaxis:**  
`str ConvertFromBase64(str source)`-wordt ervan uitgegaan Unicode voor het coderen van  
`str ConvertFromBase64(str source, enum Encoding)`

- bron: Base64 gecodeerde tekenreeks  
- Codering: Unicode, ASCII, UTF8

**Voorbeeld**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Beide voorbeelden retourneren "*Hallo, wereld!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Beschrijving:**  
De functie ConvertFromUTF8Hex omgezet de opgegeven UTF8 Hex codering waarde in een tekenreeks.

**Syntaxis:**  
`str ConvertFromUTF8Hex(str source)`

- bron: UTF8 2 bytes gecodeerd String

**Opmerking:**  
Het verschil tussen deze functie en ConvertFromBase64([],UTF8) die het resultaat is geschikt voor het kenmerk DN.  
Deze indeling wordt gebruikt door Azure Active Directory als DN-naam.

**Voorbeeld:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Geeft als resultaat '*Hallo, wereld!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Beschrijving:**  
Een tekenreeks met converteert de functie ConvertToBase64 naar een Unicode-base64-tekenreeks.  
De waarde van een matrix van gehele getallen converteert naar de overeenkomstige tekenreeks voorstelling die is gecodeerd met base 64 cijfers.

**Syntaxis:**  
`str ConvertToBase64(str source)`

**Voorbeeld:**  
`ConvertToBase64("Hello world!")`  
Geeft als resultaat "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Beschrijving:**  
Een tekenreeks met converteert de functie ConvertToUTF8Hex naar een waarde UTF8 Hex codering.

**Syntaxis:**  
`str ConvertToUTF8Hex(str source)`

**Opmerking:**  
De indeling van de uitvoer van deze functie wordt gebruikt door Azure Active Directory als DN-kenmerkindeling.

**Voorbeeld:**  
`ConvertToUTF8Hex("Hello world!")`  
Geeft als resultaat 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Tellen

**Beschrijving:**  
De functie aantal geeft als resultaat het aantal elementen in een kenmerk met meerdere waarden

**Syntaxis:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Beschrijving:**  
De functie CNum een tekenreeks en geeft als resultaat een numerieke gegevenstype.

**Syntaxis:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Beschrijving:**  
Converteert een tekenreeks met het kenmerk van een verwijzing

**Syntaxis:**  
`ref CRef(str value)`

**Voorbeeld:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>CStr

**Beschrijving:**  
De functie CStr converteert naar een tekenreeksgegevenstype.

**Syntaxis:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- waarde: een numerieke waarde, een verwijzing kenmerk of een Booleaanse waarde.

**Voorbeeld:**  
`CStr([dn])`  
Kan retourneren "cn = Jan, domeincontroller = contoso, domeincontroller = com"

----------
### <a name="dateadd"></a>DateAdd

**Beschrijving:**  
Geeft als resultaat een datum met een datum die een bepaald tijdsinterval is opgeteld.

**Syntaxis:**  
`dt DateAdd(str interval, num value, dt date)`

- interval: tekenreeksexpressie die het tijdsinterval u wilt toevoegen. De tekenreeks moet beschikken over een van de volgende waarden:
 - yyyy jaar
 - q kwartaal
 - m maand
 - y-dag van jaar
 - d dag
 - w weekdag
 - ww Week
 - h uur
 - n minuten
 - s tweede
- waarde: het aantal eenheden dat u wilt toevoegen. Het kan zijn (voor datums in de toekomst) positief of negatief zijn (voor datums in het verleden).
- datum: DateTime die datum waarop het interval wordt opgeteld.

**Voorbeeld:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Telt 3 maanden en geeft als resultaat een DateTime dat staat voor '2001-04-01'.

----------
### <a name="datefromnum"></a>DateFromNum

**Beschrijving:**  
De functie DateFromNum converteert een waarde in de AD datum opmaken op het type DateTime.

**Syntaxis:**  
`dt DateFromNum(num value)`

**Voorbeeld:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Geeft als resultaat een DateTime dat staat voor 01-01-2012 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Beschrijving:**  
De functie DNComponent retourneert de waarde van een opgegeven DN-component van links gaan.

**Syntaxis:**  
`str DNComponent(ref dn, num ComponentNumber)`

- DN: het kenmerk verwijzing interpreteren
- ComponentNumber: Het onderdeel in de DN om terug te keren

**Voorbeeld:**  
`DNComponent([dn],1)`  
Als DN "cn = Jan, ou =...," wordt Jan

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Beschrijving:**  
De functie DNComponentRev retourneert de waarde van een opgegeven DN-component die van rechts (het einde).

**Syntaxis:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- DN: het kenmerk verwijzing interpreteren
- ComponentNumber - het onderdeel in de DN om terug te keren
- Opties voor: Domeincontroller – negeren van alle onderdelen met ' domeincontroller = "

**Voorbeeld:**  
Als DN "cn = Jan, ou = Utrecht ou GA, ou = = US, domeincontroller = contoso, domeincontroller = com" vervolgens  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Beide retourneren ons te.

----------
### <a name="error"></a>Fout

**Beschrijving:**  
De foutfunctie wordt gebruikt om te retourneren van een aangepaste fout.

**Syntaxis:**  
`void Error(str ErrorMessage)`

**Voorbeeld:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Als het kenmerk accountnaam niet aanwezig is, genereert een fout op het object.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Beschrijving:**  
De functie EscapeDNComponent heeft van één onderdeel van een DN en deze verlaat, zodat deze kan worden weergegeven in de LDAP.

**Syntaxis:**  
`str EscapeDNComponent(str value)`

**Voorbeeld:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Wordt gecontroleerd of dat het object kan worden gemaakt in een LDAP-diagram, zelfs als het kenmerk weergavenaam bevat tekens die moeten worden voorafgegaan in LDAP.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Beschrijving:**  
De functie FormatDateTime gebruiken om een DateTime met een tekenreeks met een opgegeven opmaak

**Syntaxis:**  
`str FormatDateTime(dt value, str format)`

- waarde: een waarde in de notatie DateTime
- opmaak: een tekenreeks die de indeling te converteren naar vertegenwoordigt.

**Opmerking:**  
De mogelijke waarden voor de indeling vindt u hier: [Door de gebruiker gedefinieerde datum/tijdnotaties (functie Format)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Voorbeeld:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Resulteert in "2007-12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Kan resulteren in "20140905081453.0Z"

----------
### <a name="guid"></a>GUID

**Beschrijving:**  
De functie GUID genereert een nieuwe willekeurige GUID

**Syntaxis:**  
`str GUID()`

----------
### <a name="iif"></a>IIF

**Beschrijving:**  
De functie IIF geeft als resultaat een lijst met mogelijke waarden op basis van een opgegeven voorwaarde.

**Syntaxis:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- voorwaarde: een waarde of expressie die kan worden geëvalueerd op true of false.
- waardeindienwaar: als de voorwaarde waar, de resulterende waarde.
- WaardeAlsOnwaar: als de voorwaarde onwaar, wordt de resulterende waarde.

**Voorbeeld:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Als de gebruiker een stagiaires is, geeft als resultaat de alias van een gebruiker met "t-" toegevoegd aan het begin van deze, anders geeft als resultaat de alias van de gebruiker is.

----------
### <a name="instr"></a>InStr

**Beschrijving:**  
De functie InStr vindt het eerste exemplaar van een subtekenreeks u in een tekenreeks

**Syntaxis:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- reekscontroleren: tekenreeks moet worden gezocht
- reeksvergelijken: tekenreeks te vinden
- starten: beginpositie als u wilt de subtekenreeks zoeken
- vergelijken: vbTextCompare of vbBinaryCompare

**Opmerking:**  
Geeft als resultaat de positie waar de subtekenreeks is gevonden of 0 als dat niet is gevonden.

**Voorbeeld:**  
`InStr("The quick brown fox","quick")`  
Evalues tot en met 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Evalueert tot en met 7

----------
### <a name="instrrev"></a>InStrRev

**Beschrijving:**  
De functie InStrRev vindt het laatste exemplaar van een subtekenreeks u in een tekenreeks

**Syntaxis:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- reekscontroleren: tekenreeks moet worden gezocht
- reeksvergelijken: tekenreeks te vinden
- starten: beginpositie als u wilt de subtekenreeks zoeken
- vergelijken: vbTextCompare of vbBinaryCompare

**Opmerking:**  
Geeft als resultaat de positie waar de subtekenreeks is gevonden of 0 als dat niet is gevonden.

**Voorbeeld:**  
`InStrRev("abbcdbbbef","bb")`  
Geeft als resultaat 7

----------
### <a name="isbitset"></a>IsBitSet

**Beschrijving:**  
De functie IsBitSet Tests als een bit is of niet ingesteld

**Syntaxis:**  
`bool IsBitSet(num value, num flag)`

- waarde: een numerieke waarde die is evaluated.flag: een numerieke waarde waarop de bits moet worden geëvalueerd

**Voorbeeld:**  
`IsBitSet(&HF,4)`  
Retourneert waar omdat bit '4' in de hexadecimale waarde "F" is ingesteld

----------
### <a name="isdate"></a>IsDate

**Beschrijving:**  
Als de expressie kan worden geëvalueerd als een DateTime-type, en vervolgens de functie IsDate in waar resulteert.

**Syntaxis:**  
`bool IsDate(var Expression)`

**Opmerking:**  
Wordt gebruikt om te bepalen als CDate() uitgevoerd worden kan.

----------
### <a name="isempty"></a>IsEmpty

**Beschrijving:**  
Als het kenmerk aanwezig in de CS of MV is maar in een lege tekenreeks resulteert, klikt u vervolgens resulteert de IsEmpty-functie in waar.

**Syntaxis:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Beschrijving:**  
Als de tekenreeks kan worden geconverteerd naar een GUID, klikt u vervolgens de IsGuid-functie geëvalueerd als true.

**Syntaxis:**  
`bool IsGuid(str GUID)`

**Opmerking:**  
Een GUID is gedefinieerd als een tekenreeks die een van deze patronen te volgen: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx of {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Wordt gebruikt om te bepalen als CGuid() uitgevoerd worden kan.

**Voorbeeld:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Als de StrAttribute heeft een GUID-indeling, een binaire weergave terug, anders een null-waarden te retourneren.

----------
### <a name="isnull"></a>IsNull

**Beschrijving:**  
Als de expressie in null-waarden resulteert, klikt u vervolgens retourneert de functie IsNull true.

**Syntaxis:**  
`bool IsNull(var Expression)`

**Opmerking:**  
Voor een kenmerk is, wordt een null-waarden uitgedrukt door gebrek aan het kenmerk.

**Voorbeeld:**  
`IsNull([displayName])`  
Geeft als resultaat waar als het kenmerk niet voorkomt in de CS of MV is.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Beschrijving:**  
Als de expressie null of een lege tekenreeks is, klikt u vervolgens retourneert de functie IsNullOrEmpty true.

**Syntaxis:**  
`bool IsNullOrEmpty(var Expression)`

**Opmerking:**  
Voor een kenmerk, zou dit resulteren in waar als het kenmerk afwezig is of aanwezig is maar een lege tekenreeks.  
De inverse van deze functie heet IsPresent.

**Voorbeeld:**  
`IsNullOrEmpty([displayName])`  
Geeft als resultaat waar als het kenmerk niet aanwezig is of een lege tekenreeks in de CS of MV.

----------
### <a name="isnumeric"></a>IsNumeric

**Beschrijving:**  
De functie IsNumeric geeft een Booleaanse waarde die aangeeft of een expressie als een getal kan worden geëvalueerd.

**Syntaxis:**  
`bool IsNumeric(var Expression)`

**Opmerking:**  
Wordt gebruikt om te bepalen of CNum() kunt succesvolle parseren van de expressie.

----------
### <a name="isstring"></a>IsString

**Beschrijving:**  
Als de expressie kan worden geëvalueerd als een tekenreeks typt, klikt u vervolgens de IsString functie resulteert in waar.

**Syntaxis:**  
`bool IsString(var expression)`

**Opmerking:**  
Wordt gebruikt om te bepalen of CStr() kunt succesvolle parseren van de expressie.

----------
### <a name="ispresent"></a>IsPresent

**Beschrijving:**  
Als de expressie in een tekenreeks die is niet Null en niet leeg is resulteert, klikt u vervolgens retourneert de functie IsPresent true.

**Syntaxis:**  
`bool IsPresent(var expression)`

**Opmerking:**  
De inverse van deze functie heet IsNullOrEmpty.

**Voorbeeld:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Item

**Beschrijving:**  
De functie Item retourneert één item uit een tekenreeks/kenmerk met meerdere waarden.

**Syntaxis:**  
`var Item(mvstr attribute, num index)`

- kenmerk: kenmerk met meerdere waarden
- index: index naar een item in de tekenreeks met meerdere waarden.

**Opmerking:**  
De functie Item is handig samen met de functie bevat sinds de laatste functie de index voor een item in het kenmerk met meerdere waarden retourneert.

Genereert een fout als index buiten het bereik valt.

**Voorbeeld:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Geeft als resultaat het primaire e-mailadres.

----------
### <a name="itemornull"></a>ItemOrNull

**Beschrijving:**  
De functie ItemOrNull retourneert één item uit een tekenreeks/kenmerk met meerdere waarden.

**Syntaxis:**  
`var ItemOrNull(mvstr attribute, num index)`

- kenmerk: kenmerk met meerdere waarden
- index: index naar een item in de tekenreeks met meerdere waarden.

**Opmerking:**  
De functie ItemOrNull is handig samen met de functie bevat sinds de laatste functie de index voor een item in het kenmerk met meerdere waarden retourneert.

Als index buiten het bereik valt, wordt als resultaat een Null-waarde.

----------
### <a name="join"></a>Deelnemen aan

**Beschrijving:**  
De functie deelnemen aan een tekenreeks met meerdere waarden en geeft als resultaat een tekenreeks met één waarde met de opgegeven scheidingsteken ingevoegd tussen elk item.

**Syntaxis:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- kenmerk: kenmerk met meerdere waarden met tekenreeksen wilt samenvoegen.
- een scheidingsteken: een willekeurige tekenreeks, gebruikt om te scheiden van de subreeksen in de geretourneerde tekenreeks. Indien weggelaten, een spatie ("") wordt gebruikt. Als scheidingsteken een tekenreeks met lengte nul is ("") of niets, alle items in de lijst met zonder scheidingstekens worden samengevoegd.

**Opmerkingen**  
Er is overeenkomst tussen de functies Join en splitsen. De Join-functie wordt een matrix van tekenreeksen en verbindt deze met de waarde voor een scheidingsteken, om een één tekenreeks te retourneren. De functie gesplitst een tekenreeks en deze worden gescheiden door op het scheidingsteken, om te retourneren van een matrix van tekenreeksen. Een belangrijke verschil is echter dat Join tekenreeksen met een willekeurige tekenreeks scheidingsteken kunt samenvoegen, splitsen kan alleen scheidt u tekenreeksen met behulp van een scheidingsteken willekeurig teken.

**Voorbeeld:**  
`Join([proxyAddresses],",")`  
Kan retourneren:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>LCase

**Beschrijving:**  
De functie LCase converteert alle tekens in een tekenreeks in kleine letters.

**Syntaxis:**  
`str LCase(str value)`

**Voorbeeld:**  
`LCase("TeSt")`  
Geeft als resultaat "test".

----------
### <a name="left"></a>Links

**Beschrijving:**  
De functie links geeft als resultaat een bepaald aantal tekens vanaf de linkerkant van een tekenreeks bevat.

**Syntaxis:**  
`str Left(str string, num NumChars)`

- tekenreeks: de tekenreeks om terug te keren tekens uit
- NumChars: een getal aangeeft het aantal tekens om vanaf het begin (links) van tekenreeks te retourneren

**Opmerking:**  
Een tekenreeks die de eerste numChars-tekens in een tekenreeks bevat:

- Als numChars = 0, lege tekenreeks te retourneren.
- Als numChars < 0, invoer tekenreeks als resultaat.
- Als tekenreeks null is, retourneert lege tekenreeks.

Als tekenreeks minder tekens dan het aantal opgegeven in numChars bevat, een tekenreeks met identieke op tekenreeks (die zich bevindt, met alle tekens in parameter 1) geretourneerd.

**Voorbeeld:**  
`Left("John Doe", 3)`  
Geeft als resultaat "Joh".

----------
### <a name="len"></a>Lengte

**Beschrijving:**  
De functie lengte geeft als resultaat het aantal tekens in een tekenreeks.

**Syntaxis:**  
`num Len(str value)`

**Voorbeeld:**  
`Len("John Doe")`  
Geeft als resultaat 8

----------
### <a name="ltrim"></a>De functies LTrim

**Beschrijving:**  
De functie LTrim Hiermee verwijdert u voorloopspaties wit van een tekenreeks bevat.

**Syntaxis:**  
`str LTrim(str value)`

**Voorbeeld:**  
`LTrim(" Test ")`  
Geeft als resultaat "Test"

----------
### <a name="mid"></a>Mid

**Beschrijving:**  
De functie deel geeft als resultaat een bepaald aantal tekens van een opgegeven positie in een tekenreeks.

**Syntaxis:**  
`str Mid(str string, num start, num NumChars)`

- tekenreeks: de tekenreeks om terug te keren tekens uit
- starten: een getal aangeeft van het eerste plaats in de tekenreeks om terug te keren tekens uit
- NumChars: een getal aangeeft het aantal tekens om terug te keren op positie in een tekenreeks

**Opmerking:**  
Afzender numChars tekens vanaf positie start in de tekenreeks.  
Een tekenreeks met numChars tekens vanaf het begin van de positie in de tekenreeks:

- Als numChars = 0, lege tekenreeks te retourneren.
- Als numChars < 0, invoer tekenreeks als resultaat.
- Als start > de lengte van de tekenreeks, invoer tekenreeks als resultaat.
- Als starten < = 0, ingevoerde tekenreeks te retourneren.
- Als tekenreeks null is, retourneert lege tekenreeks.

Als er geen numChar tekens worden in een tekenreeks vanaf het begin van de positie, zoveel resterende tekens mogelijk geretourneerd.

**Voorbeeld:**  
`Mid("John Doe", 3, 5)`  
Geeft als resultaat "hn Doe".

`Mid("John Doe", 6, 999)`  
Geeft als resultaat "Splinter"

----------
### <a name="now"></a>Nu

**Beschrijving:**  
De functie Now geeft als resultaat een DateTime die de huidige datum en tijd, op basis van de systeemdatum en -tijd van uw computer.

**Syntaxis:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Beschrijving:**  
De functie NumFromDate retourneert een datum in de AD-datumnotatie.

**Syntaxis:**  
`num NumFromDate(dt value)`

**Voorbeeld:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Geeft als resultaat 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Beschrijving:**  
De PadLeft functie links-pad een tekenreeks naar een bepaalde lengte met een teken dat meegeleverde opvulling.

**Syntaxis:**  
`str PadLeft(str string, num length, str padCharacter)`

- tekenreeks: de tekenreeks om aan te vullen.
- lengte: een geheel getal dat staat voor de gewenste lengte van tekenreeks.
- padCharacter: een tekenreeks bestaande uit een willekeurig teken wilt gebruiken als het toetsenblok-teken

**Opmerking:**

- Als de lengte van tekenreeks kleiner dan de lengte is, klikt u vervolgens padCharacter herhaaldelijk toegevoegd aan het begin (links) van tekenreeks totdat er een lengte gelijk is aan lengte.
- PadCharacter soms een spatie, maar deze mag geen null-waarde.
- Als de lengte van tekenreeks gelijk aan of groter is dan de lengte is, tekenreeks ongewijzigd geretourneerd.
- Als tekenreeks een lengte groter is dan of gelijk is aan lengte bevat, wordt een tekenreeks met identieke op tekenreeks als resultaat gegeven.
- Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks opgevuld met een padCharacter geretourneerd.
- Als tekenreeks null is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadLeft("User", 10, "0")`  
Geeft als resultaat "000000User".

----------
### <a name="padright"></a>PadRight

**Beschrijving:**  
De PadRight functie rechts-pad een tekenreeks naar een bepaalde lengte met een teken dat meegeleverde opvulling.

**Syntaxis:**  
`str PadRight(str string, num length, str padCharacter)`

- tekenreeks: de tekenreeks om aan te vullen.
- lengte: een geheel getal dat staat voor de gewenste lengte van tekenreeks.
- padCharacter: een tekenreeks bestaande uit een willekeurig teken wilt gebruiken als het toetsenblok-teken

**Opmerking:**

- Als de lengte van tekenreeks kleiner dan de lengte is, is klikt u vervolgens padCharacter herhaaldelijk toegevoegd aan het einde (rechts) van tekenreeks totdat er een lengte gelijk is aan lengte.
- padCharacter soms een spatie, maar deze mag geen null-waarde.
- Als de lengte van tekenreeks gelijk aan of groter is dan de lengte is, tekenreeks ongewijzigd geretourneerd.
- Als tekenreeks een lengte groter is dan of gelijk is aan lengte bevat, wordt een tekenreeks met identieke op tekenreeks als resultaat gegeven.
- Als de lengte van tekenreeks kleiner dan de lengte is, wordt een nieuwe tekenreeks van de gewenste lengte met tekenreeks opgevuld met een padCharacter geretourneerd.
- Als tekenreeks null is, retourneert de functie een lege tekenreeks.

**Voorbeeld:**  
`PadRight("User", 10, "0")`  
Geeft als resultaat "User000000".

----------
### <a name="pcase"></a>PCase

**Beschrijving:**  
De functie PCase converteert het eerste teken van elk woord door spaties gescheiden in een tekenreeks naar hoofdletters en alle andere tekens worden geconverteerd naar kleine letters.

**Syntaxis:**  
`String PCase(string)`

**Opmerking:**

- Deze functie biedt momenteel geen juiste hoofdlettergebruik als u wilt converteren van een woord dat is een geheel hoofdletter, zoals een acroniem.

**Voorbeeld:**  
`PCase("TEsT")`  
Geeft als resultaat "Test".

`PCase(LCase("TEST"))`  
Geeft als resultaat "Test"

----------
### <a name="randomnum"></a>RandomNum

**Beschrijving:**  
De functie RandomNum retourneert een willekeurig getal tussen een opgegeven interval.

**Syntaxis:**  
`num RandomNum(num start, num end)`

- starten: een nummer voor de ondergrens van de willekeurige waarde te genereren
- einde: een nummer voor de bovengrens van de willekeurige waarde te genereren

**Voorbeeld:**  
`Random(100,999)`  
Kan 734 retourneren.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Beschrijving:**  
De functie RemoveDuplicates heeft van een tekenreeks met meerdere waarden en zorg ervoor dat elke waarde uniek is.

**Syntaxis:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Voorbeeld:**  
`RemoveDuplicates([proxyAddresses])`  
Geeft als resultaat een kenmerk gezuiverde proxyAddress waar alle dubbele waarden zijn verwijderd.

----------
### <a name="replace"></a>Vervangen

**Beschrijving:**  
De functie vervangen Hiermee vervangt u alle exemplaren van een tekenreeks naar een andere tekenreeks.

**Syntaxis:**  
`str Replace(str string, str OldValue, str NewValue)`

- tekenreeks: een tekenreeks voor het vervangen van waarden in.
- Oude: De tekenreeks zoeken en vervangen.
- Nieuwewaarde: De tekenreeks te vervangen.

**Opmerking:**  
De functie herkent de volgende speciale bijnamen:

- \n – nieuwe regel
- \r – regelterugloop
- \t-tabblad

**Voorbeeld:**  
`Replace([address],"\r\n",", ")`  
CRLF vervangt door een komma en een spatie, en kan leiden tot "Één Microsoft manier, Redmond, WA, VS"

----------
### <a name="replacechars"></a>ReplaceChars

**Beschrijving:**  
De functie ReplaceChars Hiermee vervangt u alle exemplaren van tekens die zijn gevonden in de tekenreeks ReplacePattern.

**Syntaxis:**  
`str ReplaceChars(str string, str ReplacePattern)`

- tekenreeks: een tekenreeks voor het vervangen van tekens in.
- ReplacePattern: een tekenreeks met een woordenlijst met tekens wilt vervangen.

De indeling is {bron1}: {target1}, {bron2}: {target2}, {bronN}, {targetN} waar bron is voor het teken om te zoeken en de tekenreeks vervangen doellijst.

**Opmerking:**

- De functie wordt elk exemplaar van de gedefinieerde bronnen en vervangen door de doelen.
- De bron moet precies één (unicode)-teken.
- De bron mogen niet leeg zijn of meer dan één teken (parseren fout).
- Het doel kan meerdere tekens op, bijvoorbeeld ö:oe, β:ss hebben.
- Het doel kan zich lege dat aangeeft dat het teken dat moet worden verwijderd.
- De bron is hoofdlettergevoelig en een exacte overeenkomst moet zijn.
- De, (komma) en: (dubbele punt) zijn gereserveerde tekens en het gebruik van deze functie kan niet worden vervangen.
- Spaties en andere witte tekens in de tekenreeks ReplacePattern worden genegeerd.

**Voorbeeld:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Geeft als resultaat Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Geeft als resultaat 'ONeil', de één maatstreepjes is gedefinieerd als u wilt worden verwijderd.

----------
### <a name="right"></a>Rechts

**Beschrijving:**  
De juiste functie geeft als resultaat een bepaald aantal tekens vanaf de rechterkant (end) van een tekenreeks bevat.

**Syntaxis:**  
`str Right(str string, num NumChars)`

- tekenreeks: de tekenreeks om terug te keren tekens uit
- NumChars: een getal aangeeft het aantal tekens om vanaf het einde (rechts) van tekenreeks te retourneren

**Opmerking:**  
NumChars tekens worden vanaf de laatste positie van tekenreeks geretourneerd.

Een tekenreeks met de laatste tekens numChars in tekenreeks:

- Als numChars = 0, lege tekenreeks te retourneren.
- Als numChars < 0, invoer tekenreeks als resultaat.
- Als tekenreeks null is, retourneert lege tekenreeks.

Als tekenreeks minder tekens dan het aantal opgegeven in NumChars bevat, wordt een tekenreeks met identieke op tekenreeks als resultaat gegeven.

**Voorbeeld:**  
`Right("John Doe", 3)`  
Geeft als resultaat "Splinter".

----------
### <a name="rtrim"></a>RTrim

**Beschrijving:**  
De functie RTrim Hiermee verwijdert u volgspaties wit van een tekenreeks bevat.

**Syntaxis:**  
`str RTrim(str value)`

**Voorbeeld:**  
`RTrim(" Test ")`  
Geeft als resultaat "Test".

----------
### <a name="split"></a>Gesplitste

**Beschrijving:**  
De functie gesplitst heeft van een tekenreeks gescheiden met een scheidingsteken en kunt u een tekenreeks met meerdere waarden.

**Syntaxis:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- waarde: de tekenreeks met een scheidingsteken om te scheiden.
- een scheidingsteken: willekeurig teken dat moet worden gebruikt als scheidingsteken.
- limiet: maximum aantal waarden die kan retourneren.

**Voorbeeld:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Retourneert een tekenreeks met meerdere waarden met 2 elementen handig voor het kenmerk proxyAddress.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Beschrijving:**  
De functie StringFromGuid een binaire GUID wordt ingevoegd en wordt geconverteerd naar een tekenreeks

**Syntaxis:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Beschrijving:**  
De functie StringFromSid converteert een beveiligings-id in een tekenreeks met byte-matrix.

**Syntaxis:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Schakeloptie

**Beschrijving:**  
De functie schakeloptie wordt gebruikt om terug te keren één waarde op basis van voorwaarden geëvalueerd.

**Syntaxis:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- expr: Variant-expressie die u wilt evalueren.
- waarde: waarde die moet worden geretourneerd als de bijbehorende expressie waar is.

**Opmerking:**  
Lijst met argumenten functie Switch bestaat uit combinaties van expressies en waarden. De expressies worden geëvalueerd van links naar rechts en de waarde die is gekoppeld aan de eerste expressie die resulteren in waar als resultaat gegeven. Als de onderdelen worden niet goed gepaard, treedt er een fout op.

Bijvoorbeeld: als expr1 waar is, retourneert schakeloptie waarde1. Als expr-1 onwaar is, maar expr-2 waar is, retourneert de schakeloptie waarde-2, enzovoort.

Schakeloptie geeft als resultaat een als:

- Geen van de expressies zijn waar.
- De eerste waar expressie is een overeenkomstige waarde die is Null.

Schakeloptie evalueert alle expressies, ook al wordt slechts één van beide geretourneerd. Daarom moet u bekijken voor bijwerkingen kant. Als u de evaluatie van een expressie die resulteert in een deling door nul, wordt bijvoorbeeld een fout optreedt.

Waarden zijn ook de functie fout, die een aangepaste tekenreeks als resultaat wilt geven.

**Voorbeeld:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Geeft als resultaat de taal gesproken in sommige grote steden, anders wordt een fout.

----------
### <a name="trim"></a>Knippen

**Beschrijving:**  
De functie spaties.wissen Hiermee verwijdert u voorloop- en wit volgspaties van een tekenreeks bevat.

**Syntaxis:**  
`str Trim(str value)`  

**Voorbeeld:**  
`Trim(" Test ")`  
Geeft als resultaat "Test".

`Trim([proxyAddresses])`  
Hiermee verwijdert u voorloop- en vervolgspaties voor elke waarde in het kenmerk proxyAddress.

----------
### <a name="ucase"></a>UCase

**Beschrijving:**  
De functie UCase omgezet alle tekens in een tekenreeks in hoofdletters.

**Syntaxis:**  
`str UCase(str string)`

**Voorbeeld:**  
`UCase("TeSt")`  
Retourneert de waarde "TEST".

----------
### <a name="word"></a>Word

**Beschrijving:**  
De Word-functie retourneert een woord opgenomen in een tekenreeks, op basis van parameters met een beschrijving van de scheidingstekens wilt gebruiken en het getal word om terug te keren.

**Syntaxis:**  
`str Word(str string, num WordNumber, str delimiters)`

- tekenreeks: de tekenreeks te retourneren van een woord uit.
- WordNumber: een getal aangeeft welk nummer word moet worden geretourneerd.
- scheidingstekens: een tekenreeks die de delimiter(s) die moet worden gebruikt om te identificeren woorden vertegenwoordigt

**Opmerking:**  
Elke tekenreeks in een tekenreeks gescheiden door een van de tekens in scheidingstekens zijn geïdentificeerd als woorden:

- Als getal < 1, geeft als resultaat lege tekenreeks.
- Als tekenreeks null is, geeft als resultaat lege tekenreeks.

Als tekenreeks kleiner is dan het aantal woorden bevat of tekenreeks geen willekeurige woorden die wordt aangeduid met scheidingstekens bevat, wordt een lege tekenreeks als resultaat gegeven.

**Voorbeeld:**  
`Word("The quick brown fox",3," ")`  
Geeft als resultaat "Pa"

`Word("This,string!has&many separators",3,",!&#")`  
Het resultaat "heeft"

## <a name="additional-resources"></a>Aanvullende informatie

* [Lidmaatschap declaratieve inrichten expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD verbinden met synchronisatie: Opties voor het aanpassen van synchronisatie](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
