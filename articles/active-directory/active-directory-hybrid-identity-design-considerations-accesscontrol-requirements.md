
<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - vereisten voor het beheer van access bepalen | Microsoft Azure"
    description="Behandelt de stijlen van identiteit en access vereisten voor resources voor gebruikers in een hybride omgeving identificeren."
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

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Vereisten voor het besturingselement voor uw hybride identiteit oplossing access bepalen
Wanneer een organisatie is hun identiteit hybride oplossing ontwerpen kunnen ze ook deze verkoopkans gebruiken moet worden gereviseerd access vereisten voor de resources die ze van plan bent om deze beschikbaar maken voor gebruikers. De toegang tot gegevens cross alle vier pijlers van identiteit, die zijn:

- Beheer
- Verificatie
- Autorisatie
- Controle

De secties die volgt komen verificatie en autorisatie in meer details, controle en beheer maken deel uit van de levenscyclus van de hybride identiteit. Lees [bepalen hybride identiteit beheertaken](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) voor meer informatie over deze functies.

>[AZURE.NOTE]
Lees [De vier pijlers van identiteit - identiteitsbeheer in de leeftijd van hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) voor meer informatie over elk van deze pijlers.

## <a name="authentication-and-authorization"></a>Verificatie en machtiging
Er zijn verschillende scenario's voor verificatie en autorisatie, deze scenario's beschikken over de vereisten die moeten worden voldaan door de hybride identiteit oplossing die het bedrijf wordt vast te stellen. Scenario's voor bedrijven (B2B) communicatie kunnen een extra uitdaging voor IT-beheerders toevoegen, aangezien ze ervoor zorgen moet dat de verificatie en machtiging methode gebruikt door de organisatie met hun zakenpartners communiceren kan. Tijdens de ontwerpen voor verificatie en machtiging vereisten, moet u ervoor zorgen dat de volgende vragen worden beantwoord:

- Uw organisatie wordt verifiëren en Autoriseer alleen gebruikers die zich op hun identiteitsbeheersysteem bevindt?
 - Zijn er een plannen voor B2B scenario's?
 - Indien Ja, u al weet welke protocollen (SAML, OAuth, Kerberos, Tokens of certificaten) wordt gebruikt om verbinding maken met beide bedrijven?
- De hybride identiteit oplossing die u wilt bij het gebruik van ondersteuning voor deze protocollen betekent?

Nog een belangrijk punt u rekening moet houden is waar de verificatie-opslagplaats die worden gebruikt door gebruikers en partners worden geregistreerd en het beheermodel moet worden gebruikt. Houd rekening met de volgende twee core-opties:
- Centrale: in dit model van de gebruiker referenties, beleidsregels en beheer kunnen worden gecentraliseerde on-premises implementatie of in de cloud.
- Hybride: in dit model referenties, beleidsregels en beheer van de gebruiker, worden gecentraliseerde on-premises en een gerepliceerde in de cloud.

Welke model bij het gebruik van uw organisatie wordt variëren op basis van hun zakelijke behoeften, die u wilt beantwoorden van de volgende vragen om te bepalen waar het identiteitsbeheersysteem zich bevinden en de administratieve modus gebruiken:

- Uw organisatie er een identiteitsbeheer on-premises implementatie?
 - Indien Ja: wilt ze behouden?
 - Zijn er een regelgeving of naleving vereisten dat uw organisatie moet volgen die bepaalt waar de identiteitsbeheersysteem moet woont?
- Uw organisatie gebruikt eenmalige aanmelding voor lokale apps bevindt of in de cloud?
 - Indien Ja: de goedkeuring van een hybride identiteitsmodel beïnvloedt dit proces?

## <a name="access-control"></a>Toegangsbeheer
Hoewel verificatie en machtiging hoofdelementen toegang tot zakelijke gegevens via gebruikerswachtwoord validatie in te schakelen zijn, is het ook belangrijk om te bepalen van het niveau van de volgende gebruikers hebben en het niveau van de access-beheerders hebben over de resources die het beheer van toegang. Uw identiteit hybride oplossing moet kunnen gedetailleerde toegang bieden tot resources, delegeren en rol grondtal toegangsbeheer. Zorg ervoor dat de volgende vraag aangaande beheren in access worden beantwoord:

- Heeft uw bedrijf meer dan één gebruiker met verhoogde bevoegdheid voor het beheren van uw systeem identiteit?
 - Indien Ja: elke gebruiker moet hetzelfde toegangsniveau?
- Moet uw bedrijf Gemachtigdentoegang aan gebruikers specifieke bronnen beheren?
 - Ja, hoe vaak dit gebeurt er als?
- Uw bedrijf moeten integreren mogelijkheden voor het beheer van access tussen on-premises en cloud resources?
- Moet uw bedrijf om te beperken van toegang tot resources op basis van bepaalde voorwaarden?
- Zou uw bedrijf hebben een toepassing die nog moet worden aangepast besturingselement toegang tot enkele bronnen?
 - Indien Ja, waar deze apps zich bevinden (on-premises implementatie of in de cloud)?
 - Indien Ja: waar zijn de doelgroep bronnen (on-premises implementatie of in de cloud)?

>[AZURE.NOTE]
Zorg ervoor dat elk antwoord notities maken en het hoe en waarom achter het antwoord te begrijpen. [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) eens worden de beschikbare opties en voordelen/nadelen van elke optie.  Door deze vragen te beantwoorden selecteert u welke optie beste past bij uw zakelijke behoeften.

## <a name="next-steps"></a>Volgende stappen

[Vereisten voor incident antwoord bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
