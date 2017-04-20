<properties
    pageTitle="Azure AD Connect: Het oplossen van fouten tijdens de synchronisatie | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe u synchronisatiefouten oplossen die optreden tijdens de synchronisatie met Azure AD Connect."
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

# <a name="troubleshooting-errors-during-synchronization"></a>Fouten corrigeren tijdens het synchroniseren
Fouten kunnen optreden bij identiteitsgegevens wordt gesynchroniseerd vanuit Windows Server Active Directory (AD DS) met Azure Active Directory (Azure AD). Dit artikel bevat een overzicht van verschillende soorten synchronisatiefouten zijn opgetreden, enkele van de verschillende scenario's die ertoe leiden dat deze fouten en mogelijke manieren de fouten op te lossen. In dit artikel bevat de algemene fouttypen en alle mogelijke fouten kan niet worden besproken.

 In dit artikel wordt ervan uitgegaan dat de lezer is vertrouwd met de onderliggende [ontwerpen concepten van Azure AD en Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Met de meest recente versie van Azure AD Connect \(augustus 2016 of hoger\), een rapport van synchronisatiefouten is beschikbaar in de [Portal van Azure](https://aka.ms/aadconnecthealth) als onderdeel van Azure AD verbinding servicestatus voor synchronisatie.


1 September 2016 beginnend [Azure Active Directory dupliceren kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) functie al dan niet standaard voor alle de *nieuwe* Azure Active Directory-Tenants worden ingeschakeld. Deze functie wordt automatisch worden ingeschakeld voor bestaande tenants in de komende maanden.

Azure AD Connect voert 3 soorten bewerkingen uit de mappen synchroon omzet: importeren, synchronisatie / exporteren. Fouten kunnen plaatsvinden in alle bewerkingen. In dit artikel bevat voornamelijk voor informatie over fouten tijdens het exporteren naar Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Fouten tijdens exporteren naar Azure AD
Sectie volgen, worden de verschillende soorten synchronisatiefouten die kunnen optreden tijdens de exportbewerking naar Azure AD met behulp van de Azure AD-connector beschreven. Deze verbindingslijn kan worden geïdentificeerd door de naamindeling wordt "contoso. *onmicrosoft.com*".
Fouten tijdens exporteren naar Azure AD geven dat de bewerking \(toevoegen, bijwerken, verwijderen enzovoort\) door Azure AD Connect geprobeerd \(synchronisatie-Engine\) op Azure Active Directory is mislukt.

![Overzicht van fouten exporteren](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Gegevens komen niet overeen fouten
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Beschrijving
- Wanneer Azure AD Connect \(synchronisatie-engine\) Hiermee geeft u voor Azure Active Directory wilt toevoegen of bijwerken van objecten, Azure AD overeenkomt met het binnenkomende object met het kenmerk **sourceAnchor** met het kenmerk **immutableId** van objecten in Azure AD. Deze overeenkomst wordt genoemd een **Vaste overeenkomen**.
- Wanneer Azure AD **vindt geen** elk object dat overeenkomt met de **immutableId** kenmerk met het kenmerk **sourceAnchor** van het binnenkomende object, voordat u gaat inrichten van een nieuw object valt terug als u wilt de kenmerken ProxyAddresses en UserPrincipalName gebruiken om een overeenkomst te vinden. Deze overeenkomst wordt genoemd een **Vloeiende overeenkomen**. De vloeiende overeenkomen met is bedoeld om aan te passen objecten die al aanwezig zijn in Azure AD (waarin zijn in Azure AD) met de nieuwe objecten wordt toegevoegd/bijgewerkt tijdens de synchronisatie die dezelfde entiteit (gebruikers, groepen) on-premises vertegenwoordigen.
- **InvalidSoftMatch** fout treedt op wanneer de vaste overeenkomst vindt geen overeenkomende objecten **en** vloeiende overeenkomen met een overeenkomende object wordt gevonden, maar dat object heeft een andere waarde van *immutableId* dan van het binnenkomende object *SourceAnchor*, suggesties voor dat het overeenkomende object is gesynchroniseerd met een ander object uit on-premises Active Directory.

Met andere woorden, in de volgorde voor de vloeiende vergelijken om te werken, mogen het object vloeiende overeengestemd met geen elke waarde voor de *immutableId*. Als een willekeurig object met *immutableId* met instellen wordt een waarde is de harde-overeenkomst verbroken, maar die voldoet aan de criteria vloeiende-overeenkomst, dat de bewerking resulteert in een synchronisatiefout InvalidSoftMatch.

Azure Active Directory-schema kunnen niet worden twee of meer objecten op dezelfde waarde van de volgende kenmerken hebben. \(Dit is niet volledig overzicht.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- Object-id

>[AZURE.NOTE] [Azure AD-kenmerk dupliceren kenmerk tolerantie](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) functie wordt ook als het standaardgedrag van Azure Active Directory die wordt geïmplementeerd.  Hierdoor wordt het aantal synchronisatiefouten zichtbaar voor Azure AD Connect (evenals andere clients synchroniseren) door Azure AD meer robuuste in de manier waarop die het gedupliceerde ProxyAddresses en UserPrincipalName kenmerken die aanwezig zijn in op het lokale AD-omgevingen verwerkt. Deze functie wordt niet oplossen die het dupliceren. Zodat moet de gegevens nog steeds worden gerepareerd. Maar kunt inrichten van nieuwe objecten die anders van wordt ingericht vanwege dubbele waarden in Azure AD worden geblokkeerd. Dit wordt ook Beperk het aantal synchronisatiefouten geretourneerd naar de synchronisatie-client.
Als deze functie is ingeschakeld voor uw Tenant, ziet u niet de InvalidSoftMatch synchronisatiefouten gezien tijdens het inrichten van nieuwe objecten.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Voorbeelden van scenario's voor InvalidSoftMatch
1. Twee of meer objecten met dezelfde waarde van ProxyAddresses-kenmerk bestaat in on-premises Active Directory. Slechts één is ophalen ingericht in Azure AD.
2. Twee of meer objecten met dezelfde waarde van userPrincipalName bestaat in on-premises Active Directory. Slechts één is ophalen ingericht in Azure AD.
3. Een object is toegevoegd in de aan lokale Active Directory met dezelfde waarde van ProxyAddresses-kenmerk als die van een bestaand object in Azure Active Directory. Het object dat on-premises toegevoegd is niet aan de ingericht in Azure Active Directory.
4. Een object is toegevoegd in on-premises Active Directory met dezelfde waarde van userPrincipalName-kenmerk als die van een account in Azure Active Directory. Het object is niet aan de ingericht in Azure Active Directory.
5. Een gesynchroniseerde-account is verplaatst van bos A tot bos B. Azure AD Connect (synchronisatie-engine) is met behulp van ObjectGUID kenmerk te berekenen van de SourceAnchor. Na de verplaatsing bos verschilt de waarde van de SourceAnchor. Het nieuwe object (van bos B) is om te synchroniseren met het bestaande object in Azure AD verbroken.
6. Een gesynchroniseerde object hebt u per ongeluk verwijderd uit on-premises Active Directory en een nieuw object is gemaakt in Active Directory voor dezelfde entiteit (zoals door de gebruiker) zonder te verwijderen van het account weergegeven in de Azure Active Directory. Het nieuwe account mislukt om te synchroniseren met de bestaande Azure AD-object.
7. Azure AD Connect is verwijderd en opnieuw hebt geïnstalleerd. Een ander kenmerk is tijdens de installatie opnieuw gekozen als de SourceAnchor. Alle objecten die u eerder had gesynchroniseerd gestopt met InvalidSoftMatch fout.

#### <a name="example-case"></a>Voorbeeld van hoofdletters/kleine letters:
1. **Bob Smit** is een gesynchroniseerde gebruiker in Azure Active Directory van on-premises Active Directory van *contoso.com*
2. Bob Smith **UserPrincipalName** is ingesteld als **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** is de **SourceAnchor** berekend met Azure AD Connect met Bob Smith **objectGUID** uit aan bedrijfsruimten Active Directory, namelijk de **immutableId** voor Bob Smith in Azure Active Directory.
4. Stefan heeft ook de volgende waarden voor het **proxyAddresses** -kenmerk:
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Een nieuwe gebruiker, **Stefan Taylor**, wordt toegevoegd aan de aan lokale Active Directory.
6. Bob Taylor van **UserPrincipalName** is ingesteld als **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** is de **sourceAnchor** berekend met Azure AD Connect met Bob Taylor van **objectGUID** uit aan bedrijfsruimten Active Directory. Bob Taylor van object is niet gesynchroniseerd met Azure Active Directory nog.
8. Bob Taylor heeft de volgende waarden voor het proxyAddresses-kenmerk
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Tijdens de synchronisatie, zal Azure AD Connect herkennen de toevoeging van Bob Taylor in on-premises Active Directory en vraag Azure AD de dezelfde wijzigingen aanbrengen.
10. Azure AD uitvoert harde overeenkomst eerst. Dat wil zeggen, wordt gezocht als er een willekeurig object met het immutableId gelijk is aan ' abcdefghijkl0123456789 == ". Vaste overeenkomst, aangezien er geen andere objecten in Azure AD die immutableId mislukt.
11. Azure AD vervolgens probeert te Bob Taylor vloeiende-overeenkomst. Dat wil zeggen, wordt gezocht als er een willekeurig object met proxyAddresses gelijk aan de drie waarden is, inclusiefsmtp:bob@contoso.com
12. Azure AD vindt van Bob Smith object om te voldoen aan de criteria vloeiende-overeenkomst. Maar dit object heeft de waarde van immutableId = "abcdefghijklmnopqrstuv ==". waarmee wordt aangegeven dit object is gesynchroniseerd vanuit een ander object uit on-premises Active Directory. Azure AD kan geen vloeiende-overeenkomst deze objecten en dus zijn ingevoegd om een **InvalidSoftMatch** synchronisatiefout is opgetreden.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Hoe u InvalidSoftMatch fout op te lossen
De meest voorkomende reden voor de fout InvalidSoftMatch is twee objecten met verschillende SourceAnchor \(immutableId\) dezelfde waarde voor de ProxyAddresses en/of UserPrincipalName-kenmerken, die worden gebruikt tijdens de vloeiende-overeenkomst op Azure AD hebben. Om op te lossen de ongeldige vloeiende overeenkomen

1.  Geef aan dat de gedupliceerde proxyAddresses-, userPrincipalName- of andere kenmerkwaarde die de fout veroorzaakt. Ook welke twee \(of meer\) objecten betrokken bij het conflict. Het rapport gegenereerd door [Azure AD verbinding servicestatus voor synchronisatie](https://aka.ms/aadchsyncerrors) kunt u bepalen van de twee objecten.
2. Welke welk object moet blijven de gedupliceerde waarde en welk object moet niet.
3. Verwijder de gedupliceerde waarde uit het object dat deze waarde niet nodig hebt. Houd er rekening mee dat moet u de wijziging in de map waar het object is opgehaald. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de aan lokale AD, laat u Azure AD Connect synchroniseren van de wijziging.

Houd er rekening mee dat synchronisatie foutrapport binnen Azure AD verbinding servicestatus voor synchronisatie elke 30 minuten wordt bijgewerkt en de fouten uit de meest recente synchronisatiepoging bevat.

>[AZURE.NOTE] ImmutableId, moet per definitie niet wijzigen in de levensduur van het object. Als Azure AD Connect is niet geconfigureerd met een deel van de scenario's er rekening mee in de bovenstaande lijst, kan het uiteindelijke in een situatie waar Azure AD Connect worden berekend met een andere waarde van de SourceAnchor voor de AD-object met dezelfde entiteit (dezelfde gebruiker/groep/contactpersoon enzovoort) met een bestaand Azure AD-Object, die u wilt blijven gebruiken.

#### <a name="related-articles"></a>Verwante artikelen
- [Dubbele of ongeldige kenmerken voorkomen adreslijstsynchronisatie in Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Beschrijving
Wanneer Azure AD probeert zachte zodat deze overeenkomt met twee objecten, is het mogelijk dat twee objecten van verschillende "objecttype" (zoals gebruiker, groep, contactpersoon, enzovoort) hebben dezelfde waarden voor de kenmerken die wordt gebruikt voor het uitvoeren van de vloeiende overeenkomen. Als u deze kenmerken dubbel is niet toegestaan in Azure AD, wordt de bewerking kan resulteren in synchronisatiefout "ObjectTypeMismatch".

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Voorbeelden van scenario's voor ObjectTypeMismatch fout
- Een beveiligingsgroep mail ingeschakeld is gemaakt in Office 365. Beheerder voegt een nieuwe gebruiker of de contactpersoon in on-premises AD (die wordt niet gesynchroniseerd in Azure AD nog) met dezelfde waarde voor het ProxyAddresses-kenmerk als die van de Office 365-groep.

#### <a name="example-case"></a>Voorbeeld van hoofdletters/kleine letters

1. Beheerder maakt een nieuwe beveiligingsgroep van e-mail is ingeschakeld in Office 365 voor de belasting afdeling en vindt u een e-mailadres als tax@contoso.com. Hiermee wordt het ProxyAddresses-kenmerk voor deze groep met de waarde van toegewezen**smtp:tax@contoso.com**
2. Een nieuwe gebruiker verbindt Contoso.com en een account is gemaakt voor de gebruiker on-premises met de proxyAddress als**smtp:tax@contoso.com**
3. Wanneer Azure AD Connect de nieuwe gebruikersaccount synchroniseert wordt, krijgt deze de fout 'ObjectTypeMismatch'.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Hoe u ObjectTypeMismatch fout op te lossen
De meest voorkomende reden voor de fout ObjectTypeMismatch is twee objecten van een ander type (gebruiker, groep, contactpersoon, enzovoort) hebben dezelfde waarde voor het ProxyAddresses-kenmerk. Als u wilt de ObjectTypeMismatch herstellen:

1.  Identificeer de gedupliceerde proxyAddresses (of een ander kenmerk) waarde die de fout veroorzaakt. Ook welke twee \(of meer\) objecten betrokken bij het conflict. Het rapport gegenereerd door [Azure AD verbinding servicestatus voor synchronisatie](https://aka.ms/aadchsyncerrors) kunt u bepalen van de twee objecten.
2. Welke welk object moet blijven de gedupliceerde waarde en welk object moet niet.
3. Verwijder de gedupliceerde waarde uit het object dat deze waarde niet nodig hebt. Houd er rekening mee dat moet u de wijziging in de map waar het object is opgehaald. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de aan lokale AD, laat u Azure AD Connect synchroniseren van de wijziging. Synchronisatie-foutrapport binnen Azure AD verbinding servicestatus voor synchronisatie elke 30 minuten wordt bijgewerkt en de fouten uit de meest recente synchronisatiepoging bevat.


## <a name="duplicate-attributes"></a>Dubbele kenmerken
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Beschrijving
Azure Active Directory-schema kunnen niet worden twee of meer objecten op dezelfde waarde van de volgende kenmerken hebben. Dat is dat elk object in Azure AD moest een unieke waarde van de volgende kenmerken voor een bepaald exemplaar hebt.

- ProxyAddresses
- UserPrincipalName

Als Azure AD Connect probeert om een nieuw object toevoegen of bijwerken van een bestaand object met een waarde voor de bovenstaande kenmerken die al is toegewezen aan een ander object in Azure Active Directory, wordt de bewerking resulteert in "AttributeValueMustBeUnique" synchronisatiefout is opgetreden.
#### <a name="possible-scenarios"></a>Mogelijke scenario's:
1. Dubbele waarde wordt toegewezen aan een al gesynchroniseerde object, dat een conflict met een ander gesynchroniseerde object veroorzaakt.

#### <a name="example-case"></a>Voorbeeld van hoofdletters/kleine letters:
1. **Bob Smit** is een gesynchroniseerde gebruiker in Azure Active Directory van on-premises Active Directory van contoso.com
2. Bob Smith **UserPrincipalName** on-premises is ingesteld als **bobs@contoso.com**.
3. Stefan heeft ook de volgende waarden voor het **proxyAddresses** -kenmerk:
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Een nieuwe gebruiker, **Stefan Taylor**, wordt toegevoegd aan de aan lokale Active Directory.
5. Bob Taylor van **UserPrincipalName** is ingesteld als **bobt@contoso.com**.
6. **Stefan Taylor** heeft de volgende waarden voor het **ProxyAddresses** -kenmerk i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylor van object wordt gesynchroniseerd met Azure AD succes.
8. Beheerder besloten van Bob Taylor **ProxyAddresses** -kenmerk bijwerken met de volgende waarde: ik. **smtp:bob@contoso.com**
9. Azure AD probeert te Bob Taylor van object-update in Azure AD met de bovenstaande waarde, maar dat betrekking heeft mislukt, als of ProxyAddresses waarde al aan Bob Smith toegewezen is, resulteert in "AttributeValueMustBeUnique" fout.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Hoe u AttributeValueMustBeUnique fout op te lossen
De meest voorkomende reden voor de fout AttributeValueMustBeUnique is twee objecten met verschillende SourceAnchor \(immutableId\) dezelfde waarde voor de ProxyAddresses en/of UserPrincipalName-kenmerken hebben. Om te kunnen AttributeValueMustBeUnique fout op te lossen

1.  Geef aan dat de gedupliceerde proxyAddresses-, userPrincipalName- of andere kenmerkwaarde die de fout veroorzaakt. Ook welke twee \(of meer\) objecten betrokken bij het conflict. Het rapport gegenereerd door [Azure AD verbinding servicestatus voor synchronisatie](https://aka.ms/aadchsyncerrors) kunt u bepalen van de twee objecten.
2. Welke welk object moet blijven de gedupliceerde waarde en welk object moet niet.
3. Verwijder de gedupliceerde waarde uit het object dat deze waarde niet nodig hebt. Houd er rekening mee dat moet u de wijziging in de map waar het object is opgehaald. In sommige gevallen moet u mogelijk een van de objecten in conflict verwijderen.
4. Als u de wijziging hebt aangebracht in de aan lokale AD, laat u Azure AD Connect de wijziging voor de fout ophalen vast te synchroniseren.

#### <a name="related-articles"></a>Verwante artikelen
-[Dubbele of ongeldige kenmerken voorkomen adreslijstsynchronisatie in Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Mislukte gegevensvalidatie
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Beschrijving
Azure Active Directory worden toegepast in verschillende beperkingen instellen voor de gegevens zelf voordat deze gegevens worden geschreven naar de map. Dit is om ervoor te zorgen dat eindgebruikers het best mogelijke ervaringen bij het gebruik van de toepassingen die afhankelijk van deze gegevens zijn ophalen.
#### <a name="scenarios"></a>Scenario 's
een. De waarde van het UserPrincipalName-kenmerk heeft tekens ongeldig.
b. Het kenmerk UserPrincipalName voldoet niet aan de vereiste indeling.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Hoe u IdentityDataValidationFailed fout op te lossen

een. Zorg ervoor dat het userPrincipalName-kenmerk heeft ondersteunde tekens en vereiste indeling.

#### <a name="related-articles"></a>Verwante artikelen
- [Voorbereidingen voor het inrichten van gebruikers via adreslijstsynchronisatie naar Office 365]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Beschrijving
Dit is een zeer specifieke geval die in een synchronisatiefout is opgetreden **"DataValidationFailed resulteert"** wanneer het achtervoegsel van een gebruikerswachtwoord UserPrincipalName van het ene federatieve domein wordt gewijzigd in een andere gefedereerd domein.

#### <a name="scenarios"></a>Scenario 's
Het achtervoegsel UserPrincipalName is voor een gesynchroniseerde gebruiker, naar een ander federatieve domein on-premises gewijzigd van één gefedereerd domein. Bijvoorbeeld *UserPrincipalName = bob@contoso.com * is gewijzigd in *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Voorbeeld
1. Bob Smith, een account voor Contoso.com, wordt toegevoegd als een nieuwe gebruiker in Active Directory met de UserPrincipalNamebob@contoso.com
2. Stefan drukt, verplaatst naar een andere verdeling van Contoso.com Fabrikam.com genoemd en zijn UserPrincipalName wordt gewijzigd inbob@fabrikam.com
3. Contoso.com en fabrikam.com domeinen zijn federatieve domeinen met Azure Active Directory.
4. Van Stefan userPrincipalName bijgewerkt en levert een synchronisatiefout is opgetreden 'DataValidationFailed'.

#### <a name="how-to-fix"></a>Oplossing
Als een gebruikerswachtwoord UserPrincipalName achtervoegsel is bijgewerkt van bob@ **contoso.com** naar bob@ **fabrikam.com**, waar zijn **contoso.com** en **fabrikam.com** **federatieve domeinen**, volg de onderstaande stappen te volgen om op te lossen de synchronisatiefout

1. Van de gebruiker UserPrincipalName-update in Azure AD uit bob@contoso.com naar bob@contoso.onmicrosoft.com. U kunt de volgende PowerShell-opdracht gebruiken met de Azure AD PowerShell-Module:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. De volgende synchronisatie cyclus geprobeerd synchronisatie toestaan. Deze tijdsynchronisatie kan worden geverifieerd en worden deze bijgewerkt via de UserPrincipalName van Stefan naar bob@fabrikam.com zoals verwacht.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Beschrijving
Wanneer een kenmerk overschrijdt de toegestane bestandslimiet, de maximale lengte of de limiet instellen door Azure Active Directory-schema, wordt de synchronisatiebewerking resulteert in de synchronisatiefout **LargeObject** of **ExceededAllowedLength** . Meestal deze fout treedt op voor de volgende kenmerken

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Mogelijke scenario 's
1. Van Stefan Eigenschapsspecifiek kenmerk is te veel certificaten die zijn toegewezen aan Stefan opslaan. Deze kunnen oudere, verlopen certificaten bevatten.
2. Van Stefan thmubnailPhoto instellen in Active Directory is te groot is om te worden gesynchroniseerd in Azure AD.
3. Tijdens de automatische populatie van het ProxyAddresses-kenmerk in Active Directory, een object hebt toegewezen > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Oplossing

1. Zorg ervoor dat het kenmerk dat de fout veroorzaakt binnen de toegestane beperking.

## <a name="related-links"></a>Verwante koppelingen
- [Zoek Active Directory-objecten in Active Directory-Beheerderscentrum] (https://technet.microsoft.com/library/dd560661.aspx)
- [Hoe u query Azure Active Directory voor een object met Azure Active Directory PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
