<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - vereisten voor beheer van webinhoud bepalen | Microsoft Azure"
    description="Biedt inzicht in hoe bepaalt u de vereisten van het beheer van webinhoud van uw bedrijf. Meestal wanneer een gebruiker heeft diens apparaat hij mogelijk ook meerdere referenties die wordt worden variërende op basis van de toepassing die hij gebruikt. Het is belangrijk om onderscheid te maken welke inhoud is gemaakt met persoonlijke referenties versus de bestanden die zijn gemaakt met behulp van bedrijfsreferenties. Uw identiteit-oplossing moet kunnen werken met de cloud services om de eindgebruiker tijdens een naadloze ervaring te bieden verzekeren zijn privacy en de beveiliging tegen gegevens lekkage vergroten."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Beheer van webinhoud vereisten voor uw hybride identiteit oplossing bepalen

Het beheer van webinhoud vereisten voor uw bedrijf mogelijk rechtstreeks van invloed op uw beslissing aan welke hybride identiteit oplossing te gebruiken. Het bedrijf moet een eigen gegevens beschermen met de verspreiding van meerdere apparaten en de mogelijkheid van gebruikers om hun eigen apparaten ([BYOD](http://aka.ms/byodcg)) weer te geven, maar deze ook moet dat de privacy van gebruiker intact blijft. Meestal wanneer een gebruiker heeft diens apparaat hij mogelijk ook meerdere referenties die wordt worden variërende op basis van de toepassing die hij gebruikt. Het is belangrijk om onderscheid te maken welke inhoud is gemaakt met persoonlijke referenties versus de bestanden die zijn gemaakt met behulp van bedrijfsreferenties. Uw identiteit-oplossing moet kunnen werken met de cloud services om de eindgebruiker tijdens een naadloze ervaring te bieden verzekeren zijn privacy en de beveiliging tegen gegevens lekkage vergroten. 

Uw identiteit-oplossing wordt worden gebruikt door verschillende technische besturingselementen kan alleen worden aangeboden beheer van webinhoud zoals wordt weergegeven in de onderstaande afbeelding:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Beveiliging-besturingselementen die van uw identiteitsbeheersysteem gebruikmaken**

Beheer van webinhoud vereisten wordt in het algemeen, gebruikmaken van uw identiteitsbeheersysteem in de volgende stadia:

- Privacy: de gebruiker die eigenaar is van een resource identificeren en de juiste besturingselementen voor het behoud van integriteit toe te passen.
- Classificatie van gegevens: de gebruiker of groep en toegangsniveau voor een object op basis van de indeling identificeren 
- Gegevensbescherming lekkage: besturingselementen voor beveiliging verantwoordelijk voor het beschermen van gegevens om te voorkomen dat lekkage moet om te communiceren met het systeem identiteit voor het valideren van de gebruikers id. Dit is ook belangrijk is voor het controleren van audittrail doel.

>[AZURE.NOTE]
Lees de [classificatie van gegevens voor de gereedheid van de cloud](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) voor meer informatie over aanbevolen procedures en richtlijnen voor classificatie van gegevens.

Wanneer het plannen van uw identiteit hybride oplossing Zorg ervoor dat de volgende vragen worden beantwoord op basis van de behoeften van uw organisatie:

- Heeft uw bedrijf besturingselementen voor beveiliging op hun plaats staan om af te dwingen gegevensprivacy?
 - Indien Ja: deze besturingselementen beveiliging kunnen integreren met de hybride identiteit oplossing aan die u gaat vast te stellen?
- Uw bedrijf classificatie van gegevens gebruiken?
 - Zo ja, kan de huidige oplossing integreren met de hybride identiteit oplossing aan die u gaat vast te stellen?
- Heeft uw bedrijf momenteel een oplossing voor gegevens lekkage? 
 - Zo ja, kan de huidige oplossing integreren met de hybride identiteit oplossing aan die u gaat vast te stellen?
- Heeft uw bedrijf toegang tot resources controleren nodig?
 - Indien Ja: welk type resources?
 - Indien Ja: is welk niveau informatie nodig?
 - Indien Ja: waar het controlelogboek moet woont? On-premises implementatie of in de cloud?
- Heeft uw bedrijf voor het coderen van een e-mailberichten met vertrouwelijke gegevens (SSNs, creditcardnummers, enzovoort) nodig?
- Heeft uw bedrijf voor het coderen van alle documenten/inhoud gedeeld met externe zakenpartners nodig?
- Heeft uw bedrijf nodig bedrijfsbeleid op bepaalde soorten e-mailberichten (kan geen allen beantwoorden, niet doorsturen)?
 
>[AZURE.NOTE]
Zorg ervoor dat elk antwoord notities maken en het hoe en waarom achter het antwoord te begrijpen. [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) eens worden de beschikbare opties en voordelen/nadelen van elke optie.  Vereisten voor door deze vragen u selecteert welke optie beste past bij uw bedrijf hebt beantwoord.


## <a name="next-steps"></a>Volgende stappen
[Vereisten voor het beheer van access bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Zie ook
[Ontwerp overwegingen overzicht](active-directory-hybrid-identity-design-considerations-overview.md)
