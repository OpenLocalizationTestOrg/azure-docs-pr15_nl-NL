<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - vereisten voor directory-synchronisatie bepalen | Microsoft Azure"
    description="Bepalen welke vereisten nodig zijn voor het synchroniseren van alle gebruikers tussen on = premises en cloud voor de onderneming."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-directory-synchronization-requirements"></a>Vereisten voor directory-synchronisatie bepalen
Synchronisatie is alles over het geven van een identiteit in de cloud op basis van de identiteit van de on-premises implementatie. Al dan niet ze gesynchroniseerde account voor of federatieve verificatie gebruiken, moeten de gebruikers die nog hebben een identiteit in de cloud.  Deze identiteit moet worden onderhouden en regelmatig bijgewerkt.  De updates kunnen veel formulieren, tegen verdere wijzigingen titel wachtwoord wijzigingen duren.  

Eerst de organisaties on-premises implementatie identiteit oplossing en gebruiker vereisten. Deze evaluatie is belangrijk dat het definiëren van de technische vereisten voor hoe de gebruikersidentiteit worden gemaakt en onderhouden in de cloud.  De meeste organisaties, Active Directory wordt on-premises en worden de on-premises adreslijst die gebruikers door wordt gesynchroniseerd vanuit, maar in sommige gevallen Hiermee niet de hoofdletters/kleine letters worden.  

Zorg ervoor dat de volgende vragen beantwoorden:


- Hebt u een AD-bos, meerdere of geen?
 - Hoeveel Azure AD-mappen wordt u synchroniseert naar?
 
    1. Gebruikt u filteren?
    2. Hebt u meerdere Azure AD Connect-servers gepland?
  
- U hebt een synchronisatie hulpprogramma on-premises implementatie?
  - Indien Ja: betekent uw gebruikers als gebruikers een virtuele map/integratie van identiteit hebben?
- Hebt u een andere map on-premises implementatie die u wilt synchroniseren (bijvoorbeeld LDAP-diagram, HR database, enzovoort)?
  - Gaat u naar een GALSync doen?
  - Wat is de huidige status van UPN in uw organisatie? 
  - Hebt u een andere map die gebruikers worden geverifieerd bij?
  - Uw bedrijf maakt gebruik van Microsoft Exchange?
    - Ze plan dat een hybride implementatie van exchange?

Nu u een idee over uw vereisten voor synchronisatie hebt, moet u om te bepalen welke hulpmiddel is de juiste is om te voldoen aan deze vereisten is voldaan.  Microsoft biedt verschillende hulpmiddelen voor het uitvoeren van directory-integratie en synchronisatie.  Zie de [vergelijkingstabel voor hybride identiteit directory-integratie hulpprogramma's](active-directory-hybrid-identity-design-considerations-tools-comparison.md) voor meer informatie. 
   
Nu u hebt uw vereisten voor synchronisatie en het hulpprogramma dat als deze voor uw bedrijf doen, moet u de toepassingen die gebruikmaken van deze adreslijstservices wordt berekend. Deze evaluatie is belangrijk dat het definiëren van de technische vereisten voor het integreren van deze toepassingen in de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

- Deze toepassingen worden verplaatst naar de cloud en gebruikt u de map?
- Zijn er speciale kenmerken die worden gesynchroniseerd met de cloud moeten zodat deze toepassingen ze wel kunnen gebruiken?
- Deze toepassingen moeten opnieuw worden geschreven om te profiteren van cloud auth?
- Blijft deze toepassingen on-premises implementatie live terwijl gebruikers toegang tot deze met de cloud-identiteit?

U hebt ook nodig om te bepalen de beveiliging vereisten en voorwaarden directory-synchronisatie. Deze evaluatie is het belangrijk om te zien van de vereisten die nodig zijn om te kunnen maken en onderhouden van de identiteit van de gebruiker in de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

- Waar wordt de synchronisatieserver worden gevonden?
- Is het domein toegevoegd?
- De server bevindt zich in een netwerk met een beperkte achter een firewall, zoals een DMZ?
  - Wordt u mogelijk de vereiste firewallpoorten ter ondersteuning van synchronisatie openen?
- Hebt u een gaan voor de synchronisatieserver?
- Hebt u een account in bij de juiste machtigingen voor alle forests die u wilt synchroniseren met?
 - Als uw bedrijf niet het antwoord op deze vraag niet weet, Controleer van het gedeelte 'De machtigingen voor Wachtwoordsynchronisatie' in het artikel [de Azure Active Directory Sync-Service installeren](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService) en bepalen of u al een account met deze machtigingen of als u nodig hebt u een account maakt.
- Als u hebt mutli-bos is synchronisatie van de synchronisatieserver kunt u bij elke bos?
 
>[AZURE.NOTE]
Zorg ervoor dat elk antwoord notities maken en het hoe en waarom achter het antwoord te begrijpen. [Bepalen incident antwoord vereisten](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) gaat over de beschikbare opties. Vereisten voor door deze vragen u selecteert welke optie beste past bij uw bedrijf hebt beantwoord.

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor meervoudige verificatie bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
