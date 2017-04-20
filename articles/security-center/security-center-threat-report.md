<properties
   pageTitle="Azure Beveiligingscentrum bedreiging Intelligence rapport | Microsoft Azure"
   description="Dit document helpt u bij het gebruik van Azure beveiliging Center bedreiging intelligente rapporten tijdens een onderzoek voor meer informatie aangaande de melding voor een waardepapier."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Azure Beveiligingscentrum bedreiging Intelligence rapport
In dit document wordt uitgelegd hoe Azure beveiliging Center bedreiging intelligente rapporten kunt u meer informatie over het risico dat een beveiligingswaarschuwing gegenereerd.

## <a name="what-is-a-threat-intelligence-report"></a>Wat is een bedreiging intelligence rapport?
Beveiligingscentrum bedreiging detectie werkt door beveiligingsinformatie van uw Azure resources, het netwerk en verbonden partneroplossingen controleren. Deze informatie, vaak correleren gegevens uit meerdere bronnen, om aan te geven threats wordt geanalyseerd. Dit proces maakt deel uit van de Beveiligingscentrum [detectiemogelijkheden](security-center-detection-capabilities.md). 

Wanneer Beveiligingscentrum worden aangegeven een bedreiging, wordt deze een [Beveiligingswaarschuwing](security-center-managing-and-responding-alerts.md)waarin gedetailleerde informatie over een bepaalde gebeurtenis, met inbegrip van suggesties voor remediation te starten. Om te helpen incident antwoord teams onderzoeken en een verhelpen threats, Beveiligingscentrum bevat een bedreiging intelligence-rapport met informatie over de bedreiging die is gedetecteerd, met inbegrip van informatie, zoals de: 

- Hacker identiteit of koppelingen (als deze gegevens toegankelijk zijn)
- Vereisen doelstellingen
- Huidige en historische aanval campagnes (als deze gegevens toegankelijk zijn)
- Vereisen tactieken, hulpmiddelen en procedures
- Gekoppeld indicatoren van compromissen (IoC) zoals URL's en bestands-hashes
- Victimology, dat wil zeggen de bedrijfstak en geografische invloed om u te helpen bij het bepalen of uw Azure resources risico zijn
- Informatie voor risicobeperking en herstellen

>[AZURE.NOTE] De hoeveelheid gegevens in een bepaald rapport varieert; het detailniveau is gebaseerd op de activiteit en de invloed van de schadelijke software.

Beveiligingscentrum heeft drie soorten bedreiging rapporten op basis van de aanval variëren kunnen. De rapporten die beschikbaar zijn:

- **Activiteit groepsrapport**: verdiepen in hackers, hun doelstellingen en tactieken biedt.
- **Campagne rapport**: bevat informatie over de details van specifieke aanval campagnes. 
- **Overzichtsrapport bedreiging**: behandelt alle items in de vorige twee rapporten.

Dit soort informatie is bijzonder nuttig zijn tijdens het [incident antwoord](security-center-incident-response.md) wanneer er een lopend onderzoek voor meer informatie over de bron van de aanval, de hacker motiveringen en wat u moet doen om dit probleem vooruit te beperken. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Hoe kan ik toegang tot het bedreiging intelligence rapport?

U kunt uw huidige waarschuwingen controleren door te kijken op de tegel **beveiligingsmeldingen** . Open de Portal Azure en volg de stappen hieronder voor meer details over elke waarschuwing:

1. Klik op het dashboard Beveiligingscentrum ziet u de tegel **beveiligingsmeldingen** .

2. Klik op de tegel om te openen van het blad **beveiligingsmeldingen** met meer informatie over de meldingen en klik in de Beveiligingsmelding van die u meer informatie krijgen wilt over.

    ![Beveiligingsmeldingen](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. In dit geval wordt het blad **verdachte proces uitgevoerd** de details van de melding zoals weergegeven in de onderstaande afbeelding:

    ![Waarschuwing detais beveiliging](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  De hoeveelheid informatie die beschikbaar zijn voor elke beveiligingsmelding zijn afhankelijk van het type van een melding. In het veld **rapporten** hebt u een koppeling naar het bedreiging intelligence-rapport. Klik erop en een ander browservenster worden weergegeven met PDF-bestand.

    ![Opslag selectie](./media/security-center-threat-report/security-center-threat-report-fig3.png)

U kunt hier downloaden van het PDF-bestand voor dit rapport en lees meer over het beveiligingsprobleem die is gedetecteerd en op basis van de opgegeven acties uitvoeren.

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd hoe Azure beveiliging Center bedreiging intelligente rapporten kunnen helpen bij het onderzoeken over beveiligingsmeldingen. Zie de volgende onderwerpen voor meer informatie over Azure Beveiligingscentrum:

- [Veelgestelde vragen over azure Beveiligingscentrum](security-center-faq.md). Zoek de veelgestelde vragen over het gebruik van de service.
- [Gebruikmaken van Azure Beveiligingscentrum voor Incident antwoord](security-center-incident-response.md)
- [Mogelijkheden voor detectie van Azure Beveiligingscentrum](security-center-detection-capabilities.md)
- [Azure Beveiligingscentrum planning en bedrijfsactiviteiten gids](security-center-planning-and-operations-guide.md). Informatie over het plannen en meer informatie over de ontwerpoverwegingen Azure Beveiligingscentrum vast te stellen.
- [Beheren en beveiligingswaarschuwingen in Azure Beveiligingscentrum beantwoorden](security-center-managing-and-responding-alerts.md). Informatie over het beheren en reageren op beveiligingsmeldingen.
- [Verwerking van beveiligingsincident in Azure Beveiligingscentrum](security-center-incident.md)
- [Azure Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Zoek weblogberichten over Azure beveiliging en naleving.
