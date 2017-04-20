
<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - incident rResponse vereisten bepalen | Microsoft Azure vereisten "
    description="Voorzieningen voor controle en rapporten voor de hybride identiteit oplossing die kunnen worden gebruikt door bepalen IT acties herkennen en een mogelijke threats Verklein te voeren"
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Incident antwoord vereisten voor uw hybride identiteit oplossing bepalen

Grote of middelgrote organisaties waarschijnlijk heeft een [incident antwoord beveiliging](https://technet.microsoft.com/library/cc700825.aspx) in een plaats om u te helpen IT dienovereenkomstig te handelen naar het niveau van incident. De identiteitsbeheersysteem is een belangrijk onderdeel in het proces incident antwoord omdat deze kan worden gebruikt om te identificeren die een bepaalde actie vergeleken met het doel uitgevoerd. De identiteit hybride oplossing moet kunnen bieden mogelijkheden voor controle en rapporten die kunnen worden gebruikt door IT acties herkennen en een mogelijke bedreiging Verklein te voeren. In een normale incident antwoord-abonnement hebt u de volgende stappen als onderdeel van het abonnement:

1.  Eerste beoordeling.
2.  Incident communicatie.
3.  Beheer van de schade en het risico wilt verkleinen.
4.  Identificatie van wat was compromissen en ernst.
5.  Bewijs bewaren.
6.  De melding naar de juiste partijen.
7.  Systeem te herstellen.
8.  Documentatie.
9.  Schade en de kosten assessment.
10. Proces en plannen revisie.

Tijdens de identificatie van wat dit is compromissen en ernst-fase, worden de systemen die is geknoeid, bestanden die zijn geopend en te bepalen de gevoeligheid van deze bestanden identificeren. Uw hybride identiteit systeem moet mogelijk om te voldoen aan deze vereisten is voldaan om u te helpen u identificatie van de gebruiker die deze wijzigingen hebt aangebracht. 

## <a name="monitoring-and-reporting"></a>Cmdlets voor controle en rapportage
Vaak de identiteit systeem kunt ook in de eerste beoordeling fase voornamelijk als het systeem heeft ingebouwde controle en de rapportagemogelijkheden. IT-beheerder moet kunnen een verdachte activiteit identificeren tijdens de eerste beoordeling, of het systeem moet kunnen trigger die deze automatisch op basis van een vooraf geconfigureerde taak. Veel activiteiten kunnen duiden op een mogelijke aanval, maar in andere gevallen een onjuist geconfigureerde systeem tot een getal onrechte van het inbraakdetectiesysteem leiden kan. 

De identiteitsbeheersysteem moet IT-beheerders herkennen en deze verdachte activiteiten rapporteren helpen. Meestal kunnen deze technische vereisten worden voldaan door alle systemen voor controle en problemen een rapportage mogelijkheid die mogelijke threats kunt markeren. Met de onderstaande vragen kunt u uw hybride identiteit oplossing waarbij rekening aandacht incident antwoord vereisten ontwerpen:

- Heeft uw bedrijf een incident antwoord beveiliging op hun plaats staan?
 - Indien Ja: de huidige identiteitsbeheersysteem deel uitmaakt van het proces?
- Heeft uw bedrijf om te identificeren verdachte aanmelding pogingen van gebruikers op andere apparaten nodig?
- Heeft uw bedrijf om te bepalen de referenties van beschadigde gebruiker mogelijke nodig?
- Heeft uw bedrijf om te controleren van de gebruiker toegang en actie nodig?
- Heeft uw bedrijf om te weten wanneer een gebruiker opnieuw instellen zijn/haar wachtwoord nodig?

## <a name="policy-enforcement"></a>Afdwingen

Tijdens het beheer van de schade en het risico wilt verkleinen-fase is het belangrijk dat u snel de werkelijke en mogelijke effecten van een aanval verminderen. Deze actie die u wordt uitgevoerd kunt op dit moment het verschil tussen een secundaire en een primaire aanbrengen. De exacte antwoord, is afhankelijk van uw organisatie en de aard van de aanval die u betrokken. Als de eerste beoordeling gesloten dat een account is is gehackt, moet u naar het afdwingen van beleid als u wilt blokkeren van dit account. Dat is slechts één voorbeeld waarin de identiteitsbeheersysteem wordt worden gebruikt. Met de onderstaande vragen kunt u uw identiteit hybride oplossing rekening houdend hoe beleid worden afgedwongen als u wilt reageren op een lopend incident ontwerpen:

- Uw bedrijf heeft beleidsregels op hun plaats staan voorkomen dat gebruikers van access het netwerk zo nodig?
 - Indien Ja, de huidige oplossing niet integreren met de hybride identiteitsbeheersysteem die u wilt nemen?
- Heeft uw bedrijf om te dwingen voor voorwaardelijke toegang voor gebruikers die zich in quarantaine nodig? 
 
>[AZURE.NOTE]
Zorg ervoor dat elk antwoord notities maken en het hoe en waarom achter het antwoord te begrijpen. [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) eens worden de beschikbare opties en voordelen/nadelen van elke optie.  Vereisten voor door deze vragen u selecteert welke optie beste past bij uw bedrijf hebt beantwoord.

## <a name="next-steps"></a>Volgende stappen
[Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
