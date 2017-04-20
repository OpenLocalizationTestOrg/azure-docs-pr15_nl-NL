<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - bepalen identiteit vereisten | Microsoft Azure"
    description="Geef aan dat van het bedrijf zakelijke behoeften die leiden u de vereisten voor het ontwerp van de identiteit hybride definiëren."
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

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Identiteit vereisten voor uw hybride identiteit oplossing bepalen
De eerste stap bij het ontwerpen van een hybride identiteit oplossing is om te bepalen de vereisten voor de organisatie van de bedrijven die van deze oplossing gebruikmaken.  Hybride identiteit wordt gestart als een ondersteunende rol (dit ondersteunt alle andere oplossingen uit de cloud door verificatie te bieden) en gaat verder voor nieuwe en interessante mogelijkheden die nieuwe werkbelasting voor gebruikers ontgrendelen.  Deze werkbelasting of services die u wilt vaststellen voor uw gebruikers wordt de vereisten voor het ontwerp van de identiteit hybride dicteren.  Deze services en de werkbelasting nodig hebt om te profiteren van hybride identiteit beide on-premises implementatie en in de cloud.  

U moet eens deze belangrijke aspecten van het bedrijf om te begrijpen wat is een vereiste nu en het bedrijf abonnementen voor de toekomst. Als u de zichtbaarheid van de lange termijnstrategie voor hybride identiteit ontwerpen niet hebt, is de kans groot dat uw oplossing niet scalable worden de zakelijke behoeften toenemen en wijzigen.   T hij diagramweergave hieronder ziet u een voorbeeld van een hybride identiteit architectuur en de werklast die zijn voor gebruikers wordt ontgrendeld. Dit is alleen een voorbeeld van de nieuwe mogelijkheden die kunnen worden ontgrendeld en geleverd met een effen hybride identiteit strategie. 
 
Sommige onderdelen die deel van de hybride identiteit architectuur uitmaken![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Zakelijke behoeften bepalen
Elk bedrijf heeft verschillende vereisten, zelfs als deze bedrijven maken deel uit van de dezelfde branche, de zakelijke vereisten kunnen variëren. U kunt nog steeds gebruikmaken van aanbevolen procedures uit de industrie, maar uiteindelijk is van het bedrijf zakelijke behoeften die leiden u de vereisten voor het ontwerp van de identiteit hybride definiëren. 

Zorg ervoor dat de volgende vragen om te identificeren van uw zakelijke behoeften beantwoorden:

- Is uw bedrijf willen IT operationele kosten?
- Uw bedrijf is wordt gezocht naar cloud activa (SaaS apps, infrastructuur) secure?
- Uw bedrijf is wordt gezocht naar uw IT moderniseren?
  - Zijn uw gebruikers mobiele en veeleisende IT uitzonderingen in uw DMZ toe te staan dat verschillende soorten verkeer naar andere informatiebronnen maken?
  - Heeft uw bedrijf oudere apps die nodig zijn om te worden gepubliceerd naar deze moderne gebruikers, maar kunnen niet eenvoudig worden herschreven fragment?
  - Heeft uw bedrijf aan al deze taken uitvoeren en klik onder beheer op hetzelfde moment brengen nodig?
- Is uw bedrijf willen beveiligen gebruikers identiteiten en risico's verminderen doordat nieuwe hulpmiddelen die profiteren van de kennis van Microsoft Azure beveiliging expertise on-premises?
- Uw bedrijf probeert te verwijderen van de gevreesde "externe" accounts on-premises en verplaats deze naar de cloud waar ze zich niet langer een inactieve bedreiging binnen uw on-premises omgeving bevinden?

## <a name="analyze-on-premises-identity-infrastructure"></a>On-premises implementatie identiteit infrastructuur analyseren
Nu u een idee met betrekking tot de vereisten van uw bedrijf bedrijven hebt, moet u de infrastructuur van uw on-premises implementatie-identiteit wordt berekend. Deze evaluatie is belangrijk voor de technische vereisten voor het integreren van uw huidige identiteit oplossing naar de cloud-identiteitsbeheersysteem definiëren. Zorg ervoor dat de volgende vragen beantwoorden:

- Welke oplossing verificatie en machtiging heeft uw bedrijf on-premises implementatie gebruiken? 
- Heeft uw bedrijf momenteel een on-premises implementatie Synchronisatieservices?
- Maakt gebruik van uw bedrijf een identiteitsprovider van derden (IdP)?

U moet ook rekening houden met de cloudservices van uw bedrijf kan hebben. Uitvoering van een evaluatie begrip van de huidige integratie met SaaS, IaaS of PaaS modellen in uw omgeving is heel belangrijk. Zorg ervoor dat de volgende vragen beantwoorden tijdens deze beoordeling:
- Heeft uw bedrijf een integratie met een cloud-serviceprovider?
- Indien Ja: welke services worden gebruikt?
- Deze integratie momenteel is in productie of is het een testfase?


>[AZURE.NOTE]
Als u niet een nauwkeurige toewijzing van al uw apps en services cloud, kunt u het hulpmiddel Cloud-App Discovery. Dit hulpmiddel kunt bieden uw IT-afdeling zicht op van uw organisatie bedrijven en consumenten cloud-apps. Die makkelijker dan ooit om te ontdekken schaduw IT in uw organisatie, met inbegrip van informatie over het gebruikspatronen en alle gebruikers met toegang tot uw cloud-toepassingen. Dit hulpmiddel Ga naar [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Softwarevereisten voor integratie van identiteit evalueren
Vervolgens moet u de vereisten van de integratie identiteit wordt berekend. Deze evaluatie is belangrijk dat het definiëren van de technische vereisten voor hoe gebruikers verifieert, hoe de aanwezigheidsgegevens van de organisatie ziet er in de cloud, hoe de organisatie kan autorisatie en wat de gebruikerservaring dient te zijn. Zorg ervoor dat de volgende vragen beantwoorden:

- Uw organisatie gebruikt Federatie, standaard verificatie of beide?
- Federatie een vereiste is?  Vanwege de volgende handelingen uit:
 - Kerberos gebaseerde eenmalige aanmelding
 - Uw bedrijf heeft een on-premises implementatie toepassingen (hetzij gebouwd partijen interne of 3e) dat SAML of soortgelijke Federatie mogelijkheden gebruikt.
 - MFA via smartcards. RSA SecurID, enzovoort.
 - Client-toegangsregels die de onderstaande vragen beantwoorden:
     1. Kan ik alle externe toegang tot Office 365 op basis van het IP-adres van de client blokkeren?
     1. Kan ik alle externe toegang tot Office 365, met uitzondering van Exchange ActiveSync blokkeren?
     1. Ik kan alle externe toegang tot Office 365, behalve voor apps op basis van een browser (OWA, SPO) blokkeren
     1. Ik kan alle externe toegang tot Office 365 blokkeren voor leden van aangewezen AD-groepen
- Bezwaren/controleren
- Reeds bestaande investering in federatieve verificatie
- Welke naam wordt onze organisatie gebruiken voor onze domein in de cloud?
- Beschikt over de organisatie een aangepast domein?
    1. Is dat domein openbare en eenvoudig geverifieerd via DNS?
    1. Als dit nog niet hebt geklikt, klikt u vervolgens hebt u een openbare domein die kunnen worden gebruikt om een alternatief UPN registreren in AD?
- Zijn de gebruikers-id's consistent zijn voor weergave van de cloud? 
- Beschikt over de organisatie apps waarvoor integratie met cloudservices?
- Beschikt over de organisatie meerdere domeinen en worden alle standaard- of federatieve verificatie gebruikt?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Toepassingen die worden uitgevoerd in uw omgeving evalueren
Nu u een idee over uw on-premises hebt en cloud infrastructuur, moet u de toepassingen die worden uitgevoerd in deze omgevingen wordt berekend. Deze evaluatie is belangrijk dat het definiëren van de technische vereisten voor het integreren van deze toepassingen naar de cloud-identiteitsbeheersysteem. Zorg ervoor dat de volgende vragen beantwoorden:

- Waar wordt onze toepassingen live?
- Gebruikers toegang tot on-premises implementatie-toepassingen?  In de cloud? Of beide?
- Zijn er plannen kunt u de bestaande toepassing werkbelasting en verplaats deze naar de cloud?
- Zijn er plannen voor de ontwikkeling van nieuwe toepassingen die wel een on-premises implementatie bevinden of in de cloud die worden gebruikt door cloud verificatie?

## <a name="evaluate-user-requirements"></a>Gebruikersvereisten evalueren
U hebt ook de gebruikersvereisten wordt berekend. Deze evaluatie is belangrijk dat het definiëren van de stappen beschreven die nodig zijn voor in dienst nemen aan te brengen en gebruikers terwijl ze overgang naar de cloud. Zorg ervoor dat de volgende vragen beantwoorden:

- Gebruikers toegang tot lokale toepassingen?
- Gebruikers toegang tot toepassingen in de cloud?
- Hoe gebruikers meestal kan aanmelden bij hun on-premises omgeving?
- Hoe worden gebruikers Meld u aan bij de cloud?

>[AZURE.NOTE]
Zorg ervoor dat elk antwoord notities maken en het hoe en waarom achter het antwoord te begrijpen. [Bepalen incident antwoord vereisten](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) eens worden de beschikbare opties en professionals/nadelen van elke optie.  Vereisten voor door deze vragen u selecteert welke optie beste past bij uw bedrijf hebt beantwoord.

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor directory-synchronisatie bepalen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
