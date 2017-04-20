<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - overzicht | Microsoft Azure"
    description="Overzicht en inhoud overzicht van een hybride identiteit ontwerp overwegingen handleiding"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory hybride identiteit ontwerpoverwegingen

Consumenten-apparaten zijn proliferating werkt, en software-als-een-service (SaaS)-toepassingen cloudgebaseerde eenvoudig vast te stellen. Hierdoor onderhouden van de besturing van de toepassing toegang van gebruikers voor interne datacenters en cloud platforms is het lastig om.  Microsoft identiteit oplossingen bereik on-premises implementatie- en cloudgebaseerde mogelijkheden, een enkele gebruikers-id voor verificatie en autorisatie voor alle resources, ongeacht de locatie te maken. Verwijst naar deze hybride identiteit. Er zijn verschillende Ontwerpeffecten en configuratieopties voor hybride identiteit Microsoft-oplossingen en in sommige gevallen die het kan lastig zijn om te bepalen welke combinatie wordt aanbevolen de behoeften van uw organisatie. Deze hybride identiteit ontwerp overwegingen handleiding helpt u om te begrijpen hoe bij het ontwerpen van een hybride identiteit oplossing die beste past bij het bedrijf en technologie nodig heeft voor uw organisatie.  Deze handleiding wordt een reeks stappen en taken die u volgen kunt zodat u een hybride identiteit oplossing ontwerpen die voldoet aan specifieke vereisten van uw organisatie beschreven. Overal in de stappen en taken, wordt de handleiding presenteren de relevante technologieën en de functieopties beschikbaar voor organisaties vergaderen functionele en servicekwaliteit (zoals beschikbaarheid, schaalbaarheid, prestaties, beheerbaarheid en beveiliging) vereisten voor het niveau. De hybride identiteit ontwerp overwegingen handleiding doelstellingen zijn specifiek, naar de volgende vragen beantwoorden: 

- Welke vragen heb ik nodig om te vragen en beantwoorden als u wilt sturen van een hybride identiteit / regiospecifieke ontwerp voor een domein technologie of een probleem dat mijn vereisten voldoet aan de aanbevolen?
- Welke volgorde van activiteiten moet ik Voltooien om het ontwerpen van een hybride identiteit oplossing voor het domein technologie of een probleem? 
- Welke hybride identiteit technologie en configuratie van de opties zijn beschikbaar waarmee mij aan mijn vereisten voldoet? Wat zijn de voor-en nadelen tussen deze opties zodat ik de beste optie voor mijn bedrijf selecteren kan?


## <a name="who-is-this-guide-intended-for"></a>Wie is deze handleiding bedoeld voor?
 CIO, CITO, hoofd identiteit architecten, Enterprise Architects en IT-architecten die verantwoordelijk is voor het ontwerpen van een hybride identiteit oplossing voor middelgrote of grote organisaties.

## <a name="how-can-this-guide-help-you"></a>Wat u kunt deze handleiding doen? 
U kunt deze handleiding voor meer informatie over het ontwerpen van een hybride identiteit-oplossing die is kunnen een cloud gebaseerd identiteitsbeheersysteem integreren met uw huidige on-premises implementatie identiteit oplossing. De volgende afbeelding ziet u een voorbeeld van een hybride identiteit-oplossing waarmee IT-beheerders voor het beheren van hun huidige Windows Server Active Directory-oplossing integreren on-premises implementatie met Microsoft Azure Active Directory zodat gebruikers kunnen eenmalige aanmelding (SSO) gebruik webtoepassingen zich bevindt in de cloud en on-premises bevindt.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


In de bovenstaande afbeelding is een voorbeeld van een hybride identiteit-oplossing die gebruikmaken van cloudservices voor integratie met on-premises implementatie mogelijkheden alleen worden aangeboden als een eenmalige uit naar de eindgebruiker-verificatieproces en te vergemakkelijken is IT om deze resources te beheren. Hoewel dit kan een veelvoorkomende scenario, is van elke organisatie hybride identiteit ontwerpen kunnen afwijken van het voorbeeld aangegeven in figuur 1 vanwege verschillende vereisten. Deze handleiding bevat een reeks stappen en taken die u volgen kunt om een hybride identiteit oplossing ontwerpen die voldoet aan specifieke vereisten van uw organisatie. Overal in de volgende stappen en taken wordt de handleiding de relevante technologieën en de beschikbare functieopties voor u om te voldoen aan functionele en service kwaliteitsvereisten voor uw organisatie.

**Hypothesen**: U enige ervaring hebben met Windows Server, Active Directory Domain Services en Azure Active Directory. In dit document, we wordt ervan uitgegaan dat u zoekt hoe deze oplossingen kunnen voldoen aan de behoeften van uw bedrijf op hun eigen of in een geïntegreerde oplossing.

## <a name="design-considerations-overview"></a>Ontwerp overwegingen overzicht
Dit document bevat een reeks stappen en taken die u volgen kunt om een hybride identiteit oplossing die het beste aan uw vereisten voor ontwerpen. De stappen worden in een geordende volgorde gepresenteerd. Ontwerpoverwegingen u in de volgende stappen leert moet u mogelijk beslissingen die u hebt aangebracht in eerdere stappen, echter vanwege conflicterende ontwerpkeuzen wijzigen. Wordt elke geprobeerd om u te waarschuwen naar potentiële ontwerp conflicten in het hele document. 

U aankomt in het ontwerp dat beste overeenkomt met uw vereisten voor alleen na de stappen zo vaak als nodig kunt u alle van de overwegingen binnen het document doorlopen. 

| Hybride identiteit fase                                             | De lijst met onderwerpen                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vereisten voor de identiteit bepalen                                   | [Zakelijke behoeften bepalen](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Vereisten voor directory-synchronisatie bepalen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Vereisten voor meervoudige verificatie bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Een hybride identiteit aanpassingsstrategie voor definiëren](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Plan voor het verbeteren van de beveiliging van de gegevens van sterke identiteit oplossing | [Vereisten voor beveiliging van gegevens bepalen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Vereisten voor beheer van webinhoud bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Vereisten voor het beheer van access bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Vereisten voor incident antwoord bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Plan voor de levenscyclus van de hybride-identiteit                                | [Beheertaken voor hybride identiteit bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Beheer van de synchronisatie](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hybride identiteit management aanpassingsstrategie voor bepalen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Deze handleiding downloaden
U kunt een PDF-versie van de hybride identiteit ontwerpoverwegingen-handleiding downloaden van de [Technet-galerie](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
