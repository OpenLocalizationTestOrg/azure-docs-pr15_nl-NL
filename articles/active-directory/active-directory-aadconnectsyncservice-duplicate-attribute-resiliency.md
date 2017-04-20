<properties
    pageTitle="Tolerantie voor synchronisatie en dupliceren kenmerk | Microsoft Azure"
    description="Nieuwe gedrag van hoe u omgaat met objecten met de UPN of ProxyAddress conflicten tijdens directory-synchronisatie met Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Tolerantie voor synchronisatie en dupliceren kenmerk
Dubbele kenmerk tolerantie is een functie in Azure Active Directory die wrijving door **UserPrincipalName** en **ProxyAddress** conflicten wanneer u zich op een van de Microsoft synchronisatie hulpmiddelen, voorkomen.

Deze twee kenmerken zijn over het algemeen moet uniek zijn voor alle **gebruiker**, **groep**of **contactpersoon** objecten in een bepaald Azure Active Directory-tenant.

> [AZURE.NOTE] Alleen gebruikers kunnen UPN hebben.

Het nieuwe gedrag waarmee deze functie is in de cloud-gedeelte van de pijplijn synchroniseren, is deze client agnostische en relevant zijn voor een Microsoft-synchronisatie-product inclusief Azure AD Connect, DirSync en MIM + verbindingslijn. De algemene term "synchronisatieclient" wordt gebruikt in dit document om aan te geven van een van deze producten.

## <a name="current-behavior"></a>Huidige gedrag
Als er een poging voor het inrichten van een nieuw object met een waarde UPN of ProxyAddress die in overtreding is met deze beperking van uniekheid, blokkeert Azure Active Directory dat object wordt gemaakt. Op dezelfde manier als een object wordt bijgewerkt met een niet-unieke UPN of ProxyAddress, mislukt de update. Het inrichten poging update opnieuw door de synchronisatieclient na elke cyclus exporteren wordt gestart, of blijft mislukken totdat het conflict opgelost is. Een e-mailbericht fout rapport wordt gegenereerd na elke poging en een fout door de synchronisatieclient zijn vastgelegd.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Gedrag met dubbel kenmerk tolerantie
In plaats van volledig verbroken inrichten of bijwerken van een object met een dubbel kenmerk, Azure Active Directory "quarantaine geplaatst" het dubbele kenmerk dat zou handelen in strijd met de beperking van uniekheid. Als dit kenmerk vereist is voor het inrichten van, zoals UserPrincipalName, wordt de waarde van een tijdelijke aanduiding voor door de service toegewezen. De indeling van deze tijdelijke waarden is  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Als het kenmerk niet vereist, zoals een **ProxyAddress is**, wordt Azure Active Directory gewoon quarantaine geplaatst het kenmerk conflict en verloopt in met de objecten maken of bijwerken.

Informatie over het conflict wordt na het kenmerk in quarantaine plaatsen, verzonden in het dezelfde fout rapport e-mailbericht in het oude gedrag gebruikt. Echter deze informatie wordt alleen weergegeven in het foutenrapport eenmalig, als de quarantaine gebeurt, deze bevat niet steeds worden vastgelegd in toekomstige e-mailberichten. Ook, omdat de export voor dit object is voltooid, de synchronisatieclient wordt een fout niet geregistreerd en wordt niet probeer het opnieuw maken / bewerking na het volgende synchronisatie maal bijwerken.

Als u wilt dit ondersteunen is een nieuw kenmerk toegevoegd aan de gebruiker, groep en Contact objectklassen:  
**DirSyncProvisioningErrors**

Dit is een kenmerk met meerdere waarden die wordt gebruikt voor de opslag van de conflicterende kenmerken die zou handelen in strijd met de beperking van uniekheid moeten ze worden toegevoegd normaal. Een achtergrond timer taak is ingeschakeld in Azure Active Directory, die elk uur om dubbele kenmerk conflicten die zijn opgelost en worden de betreffende kenmerken automatisch verwijderd uit quarantaine te vinden die wordt uitgevoerd.

### <a name="enabling-duplicate-attribute-resiliency"></a>Dubbel kenmerk tolerantie inschakelen
Dubbele kenmerk tolerantie is het nieuwe standaardgedrag over alle Azure Active Directory-tenants. Dit is op al dan niet standaard voor alle tenants die ingeschakeld synchronisatie voor de eerste keer op 22 augustus 2016 of hoger. Tenants die ingeschakeld synchroniseren voordat u deze datum wordt de functie is ingeschakeld in batches hebben. Deze uitrol begint in September 2016 en per e-mail een melding ontvangt van elke tenant technische melding contactpersoon met de specifieke datum wanneer de functie wordt ingeschakeld.

Zodra het kenmerk tolerantie dupliceren is ingeschakeld, kan deze niet worden uitgeschakeld.

Als u wilt controleren of er als de functie voor uw tenant is ingeschakeld, kunt u doen door de nieuwste versie van de Azure Active Directory PowerShell-module downloaden en uit te voeren:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Als u de functie proactief inschakelen voordat deze is ingeschakeld voor uw tenant wilt, kunt u dit doen door de nieuwste versie van de Azure Active Directory PowerShell-module downloaden en uit te voeren:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Objecten met DirSyncProvisioningErrors identificeren
Zijn er momenteel twee methoden voor het aanduiden van objecten met deze fouten vanwege dubbele eigenschap conflicten, Azure Active Directory PowerShell en de Office 365-beheerportal. Er zijn plannen om uit te breiden naar aanvullende portal gebaseerd rapportage in de toekomst.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Voor de PowerShell-cmdlets in dit onderwerp, het volgende geldt:

- Alle van de volgende cmdlets zijn hoofdlettergevoelig.
- De **– ErrorCategory PropertyConflict** moeten altijd worden opgenomen. Er zijn momenteel geen andere soorten **ErrorCategory**, maar dit in de toekomst kan worden verlengd.

Eerst aan de slag door te voeren **Connect-MsolService** en invoeren van referenties voor een tenantbeheerder.

Gebruik vervolgens de volgende cmdlets en operatoren fouten op verschillende manieren weergeven:

1. [Zie al](#see-all)

2. [Door de eigenschapstype](#by-property-type)

3. [Door conflicterende waarde](#by-conflicting-value)

4. [Gebruik van een tekenreeks zoeken](#using-a-string-search)

5. [Gesorteerd](#sorted)

6. [In een beperkt aantal of alle](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Zie al
Wanneer een verbinding, een algemeen overzicht van de inrichting van kenmerk uitvoeren fouten in de tenant:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Dit levert een resultaat als volgt uit:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Door de eigenschapstype
Als u wilt zien fouten door de eigenschapstype, voegt u de vlag **- NaamEigenschap** met het argument **UserPrincipalName** of **ProxyAddresses** :

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Of

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Door conflicterende waarde
Een specifieke eigenschap toevoegen als u wilt zien van fouten met betrekking tot de **- Eigenschapwaarde** vlag (**- NaamEigenschap** moet worden gebruikt als u ook bij het toevoegen van deze vlag):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Gebruik van een tekenreeks zoeken
Een zoekopdracht globaal tekenreeks gebruik de vlag **Zoekreeks** . Dit kan worden gebruikt onafhankelijk van alle bovenstaande vlaggen, met uitzondering van **-ErrorCategory PropertyConflict**, die is vereist:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>In een beperkt aantal of alle
1. **MaxResults <Int> ** kunnen worden gebruikt om de query beperken tot een bepaald aantal waarden.

2. **Alle** kan worden gebruikt om ervoor te zorgen dat alle resultaten worden opgehaald in het geval dat een groot aantal fouten bestaat.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365-beheerportal

U kunt synchronisatiefouten directory weergeven in het Office 365-beheercentrum. Het rapport in de Office 365-portal bevat alleen **gebruikersobjecten waarvoor deze fouten** . Informatie over conflicten tussen de **groepen** en **contactpersonen**wordt niet weergegeven.


![Actieve gebruikers] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Actieve gebruikers")

Zie voor instructies over het weergeven van directory-synchronisatiefouten in het Office 365-beheercentrum, [identificeren directory-synchronisatiefouten in Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Identiteit synchronisatie foutrapport
Wanneer een object met een conflict dubbel kenmerk is verwerkt met dit nieuwe gedrag een melding is opgenomen in de standaard identiteit synchronisatie foutrapport e-mail die wordt verzonden naar de technische melding contact opnemen met de knop voor de tenant. Er is echter een belangrijke wijziging in dit probleem. In het verleden, zou informatie over een conflict dubbel kenmerk worden opgenomen in elke volgende foutrapport totdat het probleem is opgelost. Met dit gedrag nieuwe wordt foutmelding dat de voor een bepaald conflict alleen weergegeven eenmaal - op het moment dat het conflicterende kenmerk is in quarantaine geplaatst.

Hier volgt een voorbeeld van hoe de e-mailmelding voor een conflict ProxyAddress eruitziet:  
    ![Actieve gebruikers](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Hoe u conflicten
Problemen oplossen strategie en -resolutie tactieken voor deze fouten moet niet verschillen van de manier waarop dubbel kenmerk fouten zijn verwerkt in het verleden. De enige verschil is dat de taak timer steeds de tenant is ingeschakeld voor de service-kant naar het betreffende kenmerk automatisch toevoegen aan het juiste object als het probleem opgelost is.

Het volgende artikel bevat verschillende problemen oplossen en resolutie strategieën: [dubbele of ongeldige kenmerken voorkomen adreslijstsynchronisatie in Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Bekende problemen
Geen van deze bekende problemen zorgt ervoor dat de gegevens verslechtering van verlies of service. Meerdere esthetische zijn, anderen resulteren in een standaard '*oude tolerantie*"dubbel kenmerk fout gegenereerd in plaats van het kenmerk conflict in quarantaine plaatsen en een ander veroorzaakt bepaalde fouten vereisen extra handmatig bijwerken.

**Core gedrag:**

1. Objecten met specifieke kenmerk configuraties blijven ontvangen exporteren fouten in plaats van de dubbele kenmerken worden in quarantaine geplaatst.  
Bijvoorbeeld:

    een. Nieuwe gebruiker wordt gemaakt in AD met de UPN- **Joe@contoso.com** en ProxyAddress**smtp:Joe@contoso.com**

    b. De eigenschappen van dit object conflicteren met een bestaande groep, waar ProxyAddress is **SMTP:Joe@contoso.com**.

    c. Bij het exporteren, een **conflict ProxyAddress** fout gegenereerd in plaats van na de conflict kenmerken in quarantaine geplaatst. De bewerking is opnieuw uitgevoerd bij elke volgende synchronisatie cyclus, zoals u zou zijn voordat u de tolerantiefunctie is ingeschakeld.

2. Als twee groepen on-premises implementatie met hetzelfde SMTP-adres gemaakt worden, mislukt een inrichten op de eerste poging met een standaard dubbele **ProxyAddress** -fout. Dubbele waarden is echter goed in quarantaine na de volgende synchronisatie cyclus.

**Office-Portal rapport**:

1. Het gedetailleerde foutbericht voor twee objecten in een set met UPN conflict is dezelfde. Dit betekent dat ze hebben beide hun UPN gewijzigd / in quarantaine geplaatst, wanneer u in feite alleen een van deze gegevens gewijzigd had.

2. Het gedetailleerde foutbericht wordt weergegeven voor een conflict UPN ziet u de verkeerde weergavenaam voor een gebruiker aan wie hun UPN gewijzigd heeft/in quarantaine geplaatst. Bijvoorbeeld:

    een. **Gebruiker A** gesynchroniseerd eerst met **UPN = User@contoso.com **.

    b. **Gebruiker B** wordt geprobeerd om te worden gesynchroniseerd met jarig **UPN = User@contoso.com **.

    c. **Gebruiker van B** UPN wordt gewijzigd in **User1234@contoso.onmicrosoft.com** en **User@contoso.com** wordt toegevoegd aan **DirSyncProvisioningErrors**.

    d. Het foutbericht wordt weergegeven voor de **Gebruiker B** moet aan te geven dat **een gebruiker** al heeft **User@contoso.com** zoals een UPN, maar ziet u **Van gebruiker B** eigen weergavenaam.



**Identiteit synchronisatie foutrapport**:

De koppeling voor *Stapsgewijze instructies voor het oplossen van dit probleem* is onjuist:  
    ![Actieve gebruikers](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

Het moet verwijzen naar [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).


## <a name="see-also"></a>Zie ook

- [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md)

- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

- [Directory-synchronisatiefouten in Office 365 identificeren](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
