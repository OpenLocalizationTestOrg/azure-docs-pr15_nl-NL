<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - bepalen gegevens beveiligingsvereisten | Microsoft Azure"
    description="Bij het plannen van uw identiteit hybride oplossing, geef de beveiligingsvereisten van gegevens voor uw bedrijf en welke opties zijn beschikbaar voor het beste aan deze voorwaarden wordt voldaan."
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

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plan voor het verbeteren van de beveiliging van de gegevens van sterke identiteit oplossing

De eerste stap om de gegevens te beveiligen is bepalen wie toegang heeft tot die gegevens en als onderdeel van dit proces moet u beschikken over een identiteit oplossing die kunt werkt naadloos samen met uw systeem om verificatie en machtiging mogelijk te maken. Verificatie en machtiging zijn vaak verward met elkaar en hun rollen verkeerd begrepen. In feite zijn ze heel anders, zoals weergegeven in de onderstaande afbeelding:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Mobiele apparaat management levenscyclus fasen**

Bij het plannen van uw identiteit hybride oplossing moet u de vereiste gegevens beveiliging begrijpen voor uw bedrijf en welke opties zijn beschikbaar voor het beste aan deze voorwaarden wordt voldaan.
 
>[AZURE.NOTE]
Wanneer u klaar bent met het plannen van de beveiliging van gegevens, Bekijk [vereisten voor meervoudige verificatie bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) om ervoor te zorgen dat uw selecties aangaande meervoudige verificatievereisten niet zijn beïnvloed door de beslissingen die u hebt aangebracht in deze sectie.

## <a name="determine-data-protection-requirements"></a>Vereisten voor beveiliging van gegevens bepalen
In de leeftijd van mobiliteit, de meeste bedrijven hebben een gemeenschappelijk doel: hun gebruikers in staat productief kunt zijn op hun mobiele apparaten tijdens het on-premises implementatie of extern vanaf elke locatie om te kunnen productiviteit verbeteren. Hoewel dit kan een gemeenschappelijk doel, bedrijven die vereist zijn ook worden zorg voor de hoeveelheid threats die moet worden beperkt om te kunnen gegevens van bedrijf te beveiligen en onderhouden van de privacy van gebruiker. Elk bedrijf mogelijk hebben verschillende vereisten daarbij; naleving van de verschillende regels die verschillen volgens welke industrie het bedrijf fungeert leidt tot verschillende Ontwerpeffecten beslissingen. 

Er zijn echter enkele beveiligingsaspecten die moeten worden verkend en gevalideerd, ongeacht de branche, die in het volgende gedeelte wordt uitgelegd.

## <a name="data-protection-paths"></a>Gegevens beveiliging paden

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Gegevens beveiliging paden**

In het bovenstaande diagram is het onderdeel identiteit de eerste fase worden geverifieerd voordat u gegevens wordt geopend. Deze gegevens kan echter zijn in verschillende statussen tijdens de periode dat deze is geopend. Elk nummer in dit diagram vertegenwoordigt een pad waarin gegevens zich op een gegeven moment in tijd bevinden kunt. Deze getallen worden hieronder beschreven:

1. Gegevensbescherming op het apparaatniveau van het.
2. Gegevensbescherming onderweg.
3. Gegevensbescherming terwijl u op de rest on-premises implementatie.
4. Gegevensbescherming terwijl u op de rest in de cloud.

Hoewel de technische dat besturingselementen inschakelen IT om te beveiligen van de gegevens zelf op elke deze fasen worden niet rechtstreeks aangeboden door de hybride identiteit oplossing, hoeft dat de hybride identiteit oplossing staat gebruikmaken van zowel on-premises en cloud is identiteit management resources identificeren van de gebruiker voordat u toegang verlenen tot de gegevens. Wanneer het plannen van uw identiteit hybride oplossing Zorg ervoor dat de volgende vragen worden beantwoord op basis van de behoeften van uw organisatie:

## <a name="data-protection-at-rest"></a>Gegevensbescherming in rust
Ongeacht waar de gegevens is ingesteld op rest (apparaat, cloud of on-premises), is het belangrijk om uit te voeren van een evaluatie daarbij begrip van de behoeften van de organisatie. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

- Heeft uw bedrijf om te beschermen van gegevens in rust nodig?
 - Indien Ja, kan de hybride identiteit oplossing integreren met de infrastructuur van uw huidige on-premises implementatie?
 - Indien Ja, kan de hybride identiteit oplossing integreren met uw werkbelasting zich bevindt in de cloud?
- Is de cloud-identiteitsbeheer mogen beveiligen referenties van de gebruiker en andere gegevens die zijn opgeslagen in de cloud?

## <a name="data-protection-in-transit"></a>Gegevensbescherming tijdens overdracht
Gegevens tijdens overdracht tussen het apparaat en het datacenter of het apparaat en de cloud moeten worden beveiligd. Wordt in de overdracht niet per se betekent echter een communicatie-proces met een component buiten uw cloudservice; Dit kan worden verplaatst intern, ook, bijvoorbeeld tussen twee virtuele netwerken. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

- Moet uw bedrijf worden gegevens tijdens overdracht beschermen?
 - Indien Ja, kan de hybride identiteit oplossing integreren met secure besturingselementen zoals SSL/TLS?
- Houdt de cloud-identiteitsbeheer het verkeer naar en vanuit het Active directory-archief (binnen en tussen datacenters) ondertekend?


## <a name="compliance"></a>Naleving
Regelingen, wettelijke en naleving van wettelijke voorschriften zijn afhankelijk van de bedrijfstak die deel uitmaakt van uw bedrijf. Bedrijven in hoge gereglementeerde sectoren moeten identiteitsbeheer bezwaren betrekking hebben op compatibiliteitsproblemen adres. Regelgeving zoals Sarbanes-Oxley (SOX), de overdraagbaarheid ziektekostenverzekering en verantwoordelijkheid Act (HIPAA), de Gramm-Leach-Bliley Act (GLBA) en de betaling kaart industrie Data beveiliging Standard (PCI DSS) zijn zeer strikte aangaande identiteits- en access. De identiteit hybride oplossing aan die bij het gebruik van uw bedrijf wordt moet de core-functies die worden voldoen aan de vereisten van een of meer van deze voorschriften hebben. Zorg ervoor dat de volgende vragen worden gesteld voor dit gebied:

- De identiteit hybride oplossing voldoet aan de wettelijke vereisten voor uw bedrijf is?
- De hybride identiteit oplossing in functies waarmee u uw bedrijf moeten voldoen wettelijke vereisten heeft gemaakt? 
 
>[AZURE.NOTE]
Zorg ervoor dat elk antwoord notities maken en het hoe en waarom achter het antwoord te begrijpen. [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) eens worden de beschikbare opties en voordelen/nadelen van elke optie.  Vereisten voor door deze vragen u selecteert welke optie beste past bij uw bedrijf hebt beantwoord.

## <a name="next-steps"></a>Volgende stappen
 [Vereisten voor beheer van webinhoud bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
