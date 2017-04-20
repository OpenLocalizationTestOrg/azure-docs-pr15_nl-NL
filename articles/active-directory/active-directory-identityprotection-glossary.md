<properties
    pageTitle="Woordenlijst voor beveiliging van Azure Active Directory-identiteit | Microsoft Azure"
    description="Woordenlijst voor beveiliging van Azure Active Directory-identiteit"
    services="active-directory"
    keywords="beveiliging in de Azure active directory-identiteit, cloud-app discovery,-toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid, woordenlijst beheren"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Woordenlijst voor beveiliging van Azure Active Directory-identiteit 


### <a name="at-risk-user"></a>Risico (gebruiker)  
Een gebruiker met een of meer active risico gebeurtenissen. 

### <a name="atypical-sign-in-location"></a>Locatie voor atypische aanmelden   
Een aanmeldingsproblemen van een geografische locatie die niet gebruikelijk is voor de specifieke gebruiker, vergelijkbare gebruikers of de tenant.

### <a name="azure-ad-identity-protection"></a>Beveiliging van Azure AD-identiteit    
Een beveiligingsmodule van Azure Active Directory die beschikt u over een geconsolideerde weergave in de risico's en potentiële problemen invloed zijn op de identiteit van de organisatie.

### <a name="conditional-access"></a>Voorwaardelijke toegang  
Een beleid voor het beveiligen van toegang tot bronnen. Regels voor voorwaardelijke toegang worden opgeslagen in de Azure Active Directory en worden geëvalueerd met Azure AD voordat deze toegang krijgen tot de bron.  Voorbeeld van regels voor opnemen beperken van toegang op basis van gebruikerslocatie apparaat systeemstatus of gebruiker verificatiemethode.

### <a name="credentials"></a>Referenties     
Informatie die onder andere identificatiegegevens en bewijs van identificatie die wordt gebruikt om toegang te krijgen van toegang tot lokale en netwerk resources. Voorbeelden van referenties zijn gebruikersnamen en wachtwoorden, smartcards en certificaten.

### <a name="event"></a>Gebeurtenis   
Een record van een activiteit in Azure Active Directory.

### <a name="false-positive-risk-event"></a>Fout-positieve (onvoorziene gebeurtenis) 
Een gebeurtenis Risicostatus handmatig instellen door de gebruiker van een identiteitsbeveiliging, waarmee wordt aangeduid dat de onvoorziene gebeurtenis werd onderzocht en onjuist is gemarkeerd als een onvoorziene gebeurtenis.

### <a name="identity"></a>Identiteit    
Een persoon of entiteit die moet worden geverifieerd door middel van verificatie op basis van criteria zoals wachtwoord of een certificaat.

### <a name="identity-risk-event"></a>Identiteit onvoorziene gebeurtenis     
AAD-gebeurtenis die is gemarkeerd als afwijkende identiteit bescherming en mogelijk aangeven dat een identiteit meer veilig is.

### <a name="ignored-risk-event"></a>Genegeerd (onvoorziene gebeurtenis)    
Een gebeurtenis Risicostatus handmatig instellen door de gebruiker van een identiteitsbeveiliging, die aangeeft dat de onvoorziene gebeurtenis is gesloten zonder een remediation actie te ondernemen.

### <a name="impossible-travel-from-atypical-locations"></a>Waardoor reizen van atypische locaties   
Een onvoorziene gebeurtenis geactiveerd wanneer twee aanmeldingen voor dezelfde gebruiker zijn gevonden, waarbij ten minste één van deze vanaf een locatie voor atypische aanmelden, en de tijd tussen de aanmeldingen is korter dan het minimale duurt naar fysiek afwisselend in deze locaties.  

###<a name="investigation"></a>Onderzoek    
Het proces voor het controleren van de activiteiten, logboeken en andere relevante informatie die betrekking hebben op een onvoorziene gebeurtenis te bepalen of remediation of risicobeperking stappen nodig zijn als begrijpen en hoe de identiteit is gehackt is en hoe de beschadigde identiteit is gebruikt.

### <a name="leaked-credentials"></a>Gestolen referenties  

Een onvoorziene gebeurtenis geactiveerd wanneer de huidige gebruikersreferenties (gebruikersnaam en wachtwoord) worden gevonden door onze onderzoekers openbaar in het donker web geplaatst.

### <a name="mitigation"></a>Risicobeperking  
Een actie te beperken of de mogelijkheid van een hacker misbruik van een beschadigde identiteit of het apparaat de identiteit of het apparaat hersteld in een veilige staat te verwijderen. Een risicobeperking vorige risico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet is opgelost.

### <a name="multi-factor-authentication"></a>Meervoudige verificatie 
Een verificatiemethode die moeten worden twee of meer verificatiemethoden, waaronder iets kunnen de gebruiker heeft, zoals een certificaat; iets de gebruiker kent, zoals gebruikersnamen, wachtwoorden of keer zinnen; fysiek kenmerken, zoals een vingerafdruk; en persoonlijke kenmerken, zoals een persoonlijke handtekening.

### <a name="offline-detection"></a>Offline-detectie   
De detectie van afwijkingen en evaluatie van het risico van een gebeurtenis zoals aanmeldingspoging na het feit, voor een gebeurtenis die al is gebeurd.

### <a name="policy-condition"></a>Beleidsvoorwaarde    
Een deel van een beveiligingsbeleid waarin de entiteiten (groepen, gebruikers, apps, platforms voor apparaten, apparaat Staten, IP-bereiken, clienttypen) opgenomen in het beleid of deze wordt gehouden.

### <a name="policy-rule"></a>Beleidsregel     
Het gedeelte van een beveiligingsbeleid waarin de omstandigheden die het beleid en de acties die u hebt gemaakt wanneer het beleid wordt geactiveerd wordt geactiveerd.

### <a name="prevention"></a>Preventie  
Een actie om te voorkomen dat schade aan de organisatie door misbruik van een identiteit of het apparaat verdachte of weten dat ze moeten worden geknoeid. Een actie preventie het apparaat of de identiteit niet beveiligd en niet is opgelost vorige risico's.

### <a name="privileged-user"></a>Rechten (gebruiker)
Een gebruiker die op het moment van een onvoorziene gebeurtenis, permanent of tijdelijk beschikken over beheerdersmachtigingen aan een of meer resource had in Azure Active Directory, zoals een globale beheerder, factureringsbeheerder, servicebeheerder Gebruikersbeheerder en wachtwoordbeheerder. 

###<a name="real-time"></a>Realtime    
Zie realtime detectie.

###<a name="real-time-detection"></a>Realtime detectie  
De detectie afwijkingen en evaluatie van het risico van een gebeurtenis zoals aanmeldingspoging vóór de gebeurtenis is toegestaan om door te gaan.

### <a name="remediated-risk-event"></a>Verholpen (onvoorziene gebeurtenis)     
Een gebeurtenis Risicostatus automatisch instellen door identiteitsbeveiliging, die aangeeft dat de onvoorziene gebeurtenis is verholpen met behulp van de actie standaard remediation voor dit type onvoorziene gebeurtenis. Bijvoorbeeld wanneer het wachtwoord opnieuw is ingesteld, worden veel risico gebeurtenissen die aangeven dat het vorige wachtwoord is is gehackt automatisch verholpen.

### <a name="remediation"></a>Remediation     
Een actie voor het beveiligen van een identiteit of een apparaat die eerder zijn verdachte of bekende gevaar lopen. Een actie remediation herstelt u de identiteit of het apparaat naar een veilige status en lost u vorige risico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat.

### <a name="resolved-risk-event"></a>Opgelost (onvoorziene gebeurtenis)   
Een gebeurtenis Risicostatus handmatig instellen door de gebruiker van een identiteitsbeveiliging, waarmee wordt aangeduid dat de gebruiker een juiste remediation actie buiten identiteitsbeveiliging heeft en dat de onvoorziene gebeurtenis moet worden beschouwd gesloten.

###<a name="risk-event-status"></a>Status van de gebeurtenis risico    
Een eigenschap van een onvoorziene gebeurtenis, die aangeeft of de gebeurtenis actief is, en gesloten, de reden voor gesloten.

###<a name="risk-event-type"></a>Gebeurtenistype risico  
Een categorie voor de gebeurtenis risico's, waarin wordt aangegeven dat het type die van de gebeurtenis moet worden beschouwd risicogevoelig gevonden veroorzaakt.

###<a name="risk-level-risk-event"></a>Risiconiveau (onvoorziene gebeurtenis)  
Een vermelding (hoog, normaal of laag) van de ernst van de onvoorziene gebeurtenis om identiteitsbeveiliging gebruikers prioriteit voorzien, zodat de acties te helpen die genomen worden om het risico verlagen naar hun organisatie. 

###<a name="risk-level-sign-in"></a>Risiconiveau (aanmelden) 
Een vermelding (hoog, normaal of laag) van de kans dat voor een specifieke aanmeldingsproblemen, iemand anders probeert te gebruiken van de gebruikers id.

###<a name="risk-level-user-compromise"></a>Risiconiveau (inbreuk op gebruiker) 
Een vermelding (hoog, normaal of laag) van de kans dat een identiteit meer veilig is.

### <a name="risk-level-vulnerability"></a>Risiconiveau (beveiligingsprobleem)  
Een vermelding (hoog, normaal of laag) van de ernst van het probleem om identiteitsbeveiliging gebruikers prioriteit voorzien, zodat de acties te helpen die genomen worden om het risico verlagen naar hun organisatie.

### <a name="secure-identity"></a>Secure (identiteit)   
Remediation actie zoals een machine reimaging als een mogelijk schadelijke identiteit weer in een compromisloze staat op wachtwoord wijzigen of uitvoeren.

### <a name="security-policy"></a>Beveiligingsbeleid
Een verzameling beleidsregels en voorwaarde. Een beleid kan worden toegepast op entiteiten zoals gebruikers, groepen, apps, apparaten, platforms voor apparaten, apparaat Staten, IP-bereiken en clienttypen voor Auth2.0. Wanneer u een beleid is ingeschakeld, wordt dit geëvalueerd wanneer een token voor een resource is uitgegeven door een entiteit opgenomen in het beleid.

### <a name="sign-in-v"></a>Meld u aan (v) 
Om te verifiëren voor een identiteit in Azure Active Directory.

### <a name="sign-in-n"></a>Aanmeldingsproblemen (n) 
Het proces of de actie een identiteit in Azure Active Directory en de gebeurtenis die wordt vastgelegd deze bewerking te verifiëren.

###<a name="sign-in-from-anonymous-ip-address"></a>Aanmelden vanuit anonieme IP-adres    
Een onvoorziene gebeurtenis geactiveerd na een geslaagde aanmeldingsproblemen van IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres.

###<a name="sign-in-from-infected-device"></a>Aanmelden vanuit geïnfecteerd apparaat 
Een onvoorziene gebeurtenis gestart wanneer een aanmeldingsproblemen afkomstig van een IP-adres waarvan bekend is worden gebruikt door een of meer beschadigde apparaten die actief probeert zijn te communiceren met een bots-server.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Aanmelden vanuit IP-adres met verdachte activiteiten 
Een onvoorziene gebeurtenis geactiveerd nadat een succesvolle aanmeldingsproblemen van een-IP-adres met een groot aantal mislukte aanmelding pogingen tussen meerdere gebruikersaccounts over een korte tijdsperiode.

###<a name="sign-in-from-unfamiliar-location"></a>Aanmeldingsproblemen van onbekende locatie 
Een onvoorziene gebeurtenis geactiveerd wanneer een gebruiker zich succes vanaf een nieuwe locatie (IP-, breedtegraad/lengtegraad en ASN).

###<a name="sign-in-risk"></a>Aanmeldingsproblemen risico     
Zie risico niveau (aanmelden)

###<a name="sign-in-risk-policy"></a>Aanmeldingsproblemen risico beleid  
Een voorwaardelijke--beleid dat het risico aan een specifieke aanmeldingsproblemen evalueert en is van toepassing op basis van vooraf gedefinieerde voorwaarden en regels beperkingen.

###<a name="user-compromise-risk"></a>Gebruiker inbreuk op het risico     
Zie risico niveau (inbreuk op gebruiker)

###<a name="user-risk"></a>Gebruiker risico    
Zie risico niveau (gebruiker compromissen).

###<a name="user-risk-policy"></a>Gebruikersbeleid risico     
Een voorwaardelijke-beleid die acht de aanmeldingsproblemen en is van toepassing op basis van vooraf gedefinieerde voorwaarden en regels beperkingen.

###<a name="users-flagged-for-risk"></a>Gebruikers die zijn gemarkeerd voor risico   
Gebruikers aan wie een risico gebeurtenissen die actieve of verholpen zijn

###<a name="vulnerability"></a>Beveiligingsprobleem    
Een configuratie of voorwaarde in Azure Active Directory, waardoor u de map vatbaar voor aanvallen of threats.


## <a name="see-also"></a>Zie ook

- [Beveiliging van Azure Active Directory-identiteit](active-directory-identityprotection.md)
