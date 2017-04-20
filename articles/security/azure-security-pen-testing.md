<properties
   pageTitle="Pen testen | Microsoft Azure"
   description="Het artikel biedt een overzicht van de er (pentest) proces testen en hoe uitvoeren pentest ten opzichte van uw apps uitgevoerd in Azure-infrastructuur."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Pen testen

Een van de uitstekende zaken aan bod over het gebruik van Microsoft Azure voor het testen van toepassingen en -implementatie is dat u niet nodig om samen te stellen een on-premises implementatie-infrastructuur voor de ontwikkeling, testen en implementeren van uw toepassingen. Alle de infrastructuur is afgehandeld door de services van Microsoft Azure platform. U hoeft te bang aanschaf, bij het verkrijgen van, en ' bekabeld en stapelen ' uw eigen hardware on-premises implementatie.

Dit is handig –, maar u nog nodig om ervoor te zorgen dat u de beveiliging van uw normale uitvoeren voor het innen. Een van de dingen die u moet doen is er testen van de toepassingen die u implementeert in Azure wordt aangegeven.

Weet u ook mogelijk dat Microsoft wordt uitgevoerd door [er testen van onze Azure-omgeving](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Hiermee kan wij ons platform verbeteren en begeleidt onze acties in het verbeteren van de besturingselementen voor beveiliging, inleiding tot besturingselementen voor nieuwe beveiliging en onze beveiligingsprocessen verbeteren.

We niet pen testen van de toepassing voor u, maar we begrijpen dat u wilt en hoeft uit te voeren pen testen op uw eigen toepassingen. Goed, komt dit doordat wanneer u de beveiliging van uw toepassingen verbeteren, helpen u het hele Azure-systeem veiliger maken.

Wanneer u pen test uw toepassingen, kan er uitzien zoals een aanval met ons. We [voortdurend bewaken](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) patronen aanvallen en wordt een incident antwoord-proces starten als we moeten. Hiermee wordt niet doen en deze niet ons te helpen als we een incident antwoord wordt verzonden vanwege uw eigen einddatum toewijding inzetten pen testen activeren.

Wat moet u doen?

Wanneer u klaar bent om te pen test uw toepassingen Azure gehost, moet u om aan te geven. Als we weten dat u gaat uitvoeren specifieke tests, won't we per ongeluk afsluiten (zoals het blokkeren van het IP-adres die u van testen bent), zo lang maken als uw tests voldoen aan de Azure pen testen bepalingen en voorwaarden.
Er zijn standaard tests die u kunt uitvoeren:

- Tests op uw eindpunten om de [toepassing van Open Web Project beveiliging (OWASP) bovenste 10 problemen](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
- [Fuzz testen](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) van de eindpunten
- [Poort scannen](https://en.wikipedia.org/wiki/Port_scanner) van uw eindpunten

Een type test die u niet uitvoeren is een soort aanval van [Weigering van Service (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) . Dit geldt ook voor een DoS-aanval zelf starten of het uitvoeren van gerelateerde tests die mogelijk bepalen, laten zien of een willekeurig type DoS aanval simuleren.

Test u wilt gaan met de pen uw ingesloten in een Microsoft Azure-toepassingen? Als dat het geval is, en vervolgens hoofd op boven aan het [Er Test overzicht](https://security-forms.azure.com/penetration-testing/terms) pagina (en klik op de knop testen aanvragen onder aan de pagina maken. Ook vindt u meer informatie over de pen testen voorwaarden en nuttige koppelingen op hoe u zwakke betrekking hebben op Azure of een andere Microsoft-service kan melden.
