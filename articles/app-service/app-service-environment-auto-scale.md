<properties
    pageTitle="AutoScaling en App Service omgeving | Microsoft Azure"
    description="AutoScaling en App Service-omgeving"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>AutoScaling en App Service-omgeving

Azure App Service omgevingen *autoscaling*. U kunt automatisch schalen afzonderlijke werknemer van toepassingen op basis van de doelstellingen of planning.

![Opties voor automatisch schalen voor een werknemer-groep.][intro]

AutoScaling optimaliseert uw Resourcegebruik door automatisch vergroten en verkleinen van een App Service-omgeving als u wilt aanpassen aan uw budget en of profiel laden.

## <a name="configure-worker-pool-autoscale"></a>Werknemer-toepassingen automatisch schalen configureren

U kunt de functionaliteit voor het automatisch schalen openen vanaf het tabblad **Instellingen** van de werknemer-toepassingen.

![Tabblad instellingen van de werknemer-toepassingen.][settings-scale]

Hierin is dat de interface moet bekend zijn sinds deze de dezelfde ervaring die u ziet wanneer u de schaal van een App-serviceplan. 

![Handmatige schaalinstellingen.][scale-manual]

U kunt ook een profiel automatisch schalen configureren.

![Instellingen voor het automatisch schalen.][scale-profile]

Automatisch schalen profielen zijn handig voor beperkingen instellen voor uw schaal. Op deze manier kunt u een consistente prestaties door in te stellen van een waarde van de schaal ondergrens (1) en een initiaal overzichtelijk uitgaven door in te stellen van een bovengrens (2)-ervaring hebben.

![De schaalinstellingen in profiel.][scale-profile2]

Nadat u een profiel definieert, kunt u automatisch schalen regels aan de nieuwe schaal omhoog of omlaag het aantal exemplaren in de groep werknemer binnen de grenzen gedefinieerd door het profiel kunt toevoegen. Automatisch schalen regels op basis van de doelstellingen.

![Regel voor schaal.][scale-rule]

 Een werknemer van toepassingen of het front aan de doelstellingen kan worden gebruikt om automatisch schalen regels te definiëren. Deze gegevens zijn dezelfde parameters u kunt controleren in de resource blade grafieken of waarschuwingen instellen voor.

## <a name="autoscale-example"></a>Voorbeeld van automatisch schalen

Automatisch schalen van een App Service-omgeving kan best worden geïllustreerd door een scenario doorlopen.

In dit artikel wordt uitgelegd van alle benodigde overwegingen bij het instellen van automatisch schalen. Het artikel begeleidt u bij de interacties die rol spelen als u autoscaling App Service-omgevingen die worden gehost in App-omgeving.

### <a name="scenario-introduction"></a>Scenario-Inleiding

Frank is een systeembeheerder voor een onderneming die een deel van de werkbelasting die hij beheert heeft gemigreerd naar een App Service-omgeving.

De App Service-omgeving is geconfigureerd voor het handmatig schalen als volgt:

* **Uiteinden voren:** 3
* **Werknemer van toepassingen 1**: 10
* **Werknemer, groep 2**: 5
* **Werknemer, groep 3**: 5

Werknemer van toepassingen 1 wordt gebruikt voor productie-werkbelastingen, terwijl de werknemer, groep 2 en 3 van de werknemer-toepassingen worden gebruikt voor kwaliteitscontrole (QA) en ontwikkeling werkbelasting.

De App Service-abonnementen voor q & a en ontwikkelaar zijn geconfigureerd voor handmatige schaal. De productie App-abonnement is ingesteld op automatisch schalen te handelen met variaties in laden en -verkeer is toegestaan.

Frank is erg bekend voorkomen met de toepassing. Hij weet dat de piekuren voor laden tussen 9:00 AM en 6:00 zijn omdat dit een LOB-(LOB)-toepassing die werknemers gebruiken is wanneer ze op kantoor bent. Gebruik worden daarna wanneer gebruikers zijn klaar voor die dag. Buiten piekuren is er nog steeds enkele laden omdat gebruikers toegang hebben tot de app extern met behulp van hun mobiele apparaten of pc's. De productie App-abonnement is al geconfigureerd voor het automatisch schalen op basis van CPU-gebruik met de volgende regels:

![Bepaalde instellingen voor LOB-app.][asp-scale]

|   **Automatisch schalen profiel – weekdagen – de App-abonnement**     |   **Automatisch schalen profiel – Weekends – de App-abonnement**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Naam:** Weekdag profiel                               |   **Naam:** Weekend profiel                               |
|   **Met wilt verkleinen:** Regels voor planning en prestaties            |   **Met wilt verkleinen:** Regels voor planning en prestaties            |
|   **Profiel:** Weekdagen                                   |   **Profiel:** Weekend                                    |
|   **Type:** Terugkeerpatroon                                    |   **Type:** Terugkeerpatroon                                    |
|   **Bereik van doeltoepassing:** 5 tot en met 20 exemplaren                     |   **Bereik van doeltoepassing:** 3 tot en met 10 exemplaren                     |
|   **Dagen:** Maandag, dinsdag, woensdag, donderdag, vrijdag  |   **Dagen:** Zaterdag, zondag                              |
|   **Begindatum:** 9:00 AM                                 |   **Begindatum:** 9:00 AM                                 |
|   **Tijdzone:** UTC-08                                   |   **Tijdzone:** UTC-08                                   |
|                                                           |                                                           |
|   **Automatisch schalen regel (schaal omhoog)**                           |   **Automatisch schalen regel (schaal omhoog)**                           |
|   **Resource:** Productie (App Service omgeving)      |   **Resource:** Productie (App Service omgeving)      |
|   **Metrisch:** CPU %                                       |   **Metrisch:** CPU %                                       |
|   **Bewerking:** Groter dan 60%                         |   **Bewerking:** Meer dan 80%                         |
|   **Duur:** 5 minuten                                 |   **Duur:** 10 minuten                                |
|   **Afstemmen aggregatie:** Gemiddelde                           |   **Afstemmen aggregatie:** Gemiddelde                           |
|   **Actie:** Aantal vergroten door 2                         |   **Actie:** Aantal verhogen met 1                         |
|   **Leuke omlaag (minuten):** 15                             |   **Leuke omlaag (minuten):** 20                             |
|                                                           |                                                           |
  	|   **Automatisch schalen regel (schaal omlaag)**                     |   **Automatisch schalen regel (schaal omlaag)**                         |
|   **Resource:** Productie (App Service omgeving)      |   **Resource:** Productie (App Service omgeving)      |
|   **Metrisch:** CPU %                                       |   **Metrisch:** CPU %                                       |
|   **Bewerking:** Minder dan 30%                            |   **Bewerking:** Minder dan 20%                            |
|   **Duur:** 10 minuten                                |   **Duur:** 15 minuten                                |
|   **Afstemmen aggregatie:** Gemiddelde                           |   **Afstemmen aggregatie:** Gemiddelde                           |
|   **Actie:** Aantal verlagen met 1                         |   **Actie:** Aantal verlagen met 1                         |
|   **Leuke omlaag (minuten):** 20                             |   **Leuke omlaag (minuten):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>App-Service plannen inflatie

App-Service-abonnementen die zijn geconfigureerd om automatisch schalen te doen bij een maximumsnelheid per uur. Dit tarief kan worden berekend op basis van de waarden die op de regel automatisch schalen zijn ingevoerd.

Informatie over en het berekenen van de *App Service abonnement inflatie* is belangrijk voor App Service-omgeving automatisch schalen omdat schaalwijzigingen naar een groep met werknemer niet onmiddellijk zijn.

De App Service plan de rente wordt als volgt berekend:

![App-Service plannen tarief van de berekening.][ASP-Inflation]

Op basis van het automatisch schalen – schaal omhoog regel voor het profiel weekdag van de productie App-abonnement:

![App Service plan de rentepercentage voor weekdagen op basis van automatisch schalen – schaal van de regel.][Equation1]

De formule zou voor het automatisch schalen – schaal omhoog regel voor het profiel Weekend van de productie App Service-abonnement omzetten in:

![App Service plan de rentepercentage voor weekends op basis van automatisch schalen – schaal van de regel.][Equation2]

Deze waarde kan ook worden berekend voor schaal omlaag bewerkingen.

Op basis van het automatisch schalen – schaal omlaag regel voor het profiel weekdag van de productie App serviceplan, er dit als volgt:

![App Service plan de rentepercentage voor weekdagen op basis van automatisch schalen – schaal omlaag regel.][Equation3]

De formule zou voor het automatisch schalen – schaal omlaag regel voor het profiel Weekend van de productie App Service-abonnement omzetten in:  

![App Service plan de rentepercentage voor weekends op basis van automatisch schalen – schaal omlaag regel.][Equation4]

De productie App serviceplan te aan de snelheid van een maximum van acht exemplaren/uur week en vier exemplaren/uur tijdens het weekend vergroten. Dit kunt exemplaren met een maximum snelheid van vier exemplaren/uur week en zes exemplaren/uur tijdens weekends vrijgeven.

Als meerdere App Service-abonnementen zijn wordt gehost in een groep werknemer, moet u het *totale de rente* wordt berekend als de som van de inflatie voor alle App Service-abonnementen die worden hostingprovider in die groep werknemer.

![De berekening van de totale inflatie voor meerdere App Service-abonnementen die in een groep werknemer worden gehost.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Gebruik van de inflatie van de App Service-abonnement om te werknemer van toepassingen automatisch schalen regels definiëren

Werknemer voor groepen die host App Service-abonnementen die zijn geconfigureerd om automatisch schalen te moeten een buffer van capaciteit worden toegewezen. De buffer kan voor de bewerkingen die automatisch schalen moeten groter en kleiner maken van het abonnement dat App naar wens. De minimale buffer zou de berekende totale App Service van plan bent de frequentie.

Omdat App Services-omgeving schaal enige tijd om toe te passen worden, moet er een wijziging account voor verdere wijzigingen aanvraag dit gebeuren kunnen terwijl een bewerking schaal uitgevoerd wordt. Als u wilt deze latentie, is het raadzaam dat u de berekende totale App Service van plan bent de frequentie gebruiken als het minimum aantal exemplaren die zijn toegevoegd voor elke bewerking automatisch schalen.

Met deze gegevens kunt Frank het volgende automatisch schalen profiel en de regels definiëren:

![Automatisch schalen profiel regels LOB bijvoorbeeld.][Worker-Pool-Scale]

|   **Automatisch schalen profiel – weekdagen**                        |   **Automatisch schalen profiel – Weekends**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Naam:** Weekdag profiel                               |   **Naam:** Weekend profiel                       |
|   **Met wilt verkleinen:** Regels voor planning en prestaties            |   **Met wilt verkleinen:** Regels voor planning en prestaties    |
|   **Profiel:** Weekdagen                                   |   **Profiel:** Weekend                            |
|   **Type:** Terugkeerpatroon                                    |   **Type:** Terugkeerpatroon                            |
|   **Bereik van doeltoepassing:** 13 tot en met 25 exemplaren                    |   **Bereik van doeltoepassing:** 6 tot en met 15 exemplaren             |
|   **Dagen:** Maandag, dinsdag, woensdag, donderdag, vrijdag  |   **Dagen:** Zaterdag, zondag                      |
|   **Begindatum:** 7:00 AM                                 |   **Begindatum:** 9:00 AM                         |
|   **Tijdzone:** UTC-08                                   |   **Tijdzone:** UTC-08                           |
|                                                           |                                                   |
|   **Automatisch schalen regel (schaal omhoog)**                           |   **Automatisch schalen regel (schaal omhoog)**                   |
|   **Resource:** Werknemer, groep 1                             |   **Resource:** Werknemer, groep 1                     |
|   **Metrisch:** WorkersAvailable                            |   **Metrisch:** WorkersAvailable                    |
|   **Bewerking:** Kleiner dan 8                              |   **Bewerking:** Minder dan 3                      |
|   **Duur:** 20 minuten                                |   **Duur:** 30 minuten                        |
|   **Afstemmen aggregatie:** Gemiddelde                           |   **Afstemmen aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verhogen met 8                         |   **Actie:** Aantal verhogen met 3                 |
|   **Leuke omlaag (minuten):** 180                            |   **Leuke omlaag (minuten):** 180                    |
|                                                           |                                                   |
|   **Automatisch schalen regel (schaal omlaag)**                         |   **Automatisch schalen regel (schaal omlaag)**                 |
|   **Resource:** Werknemer, groep 1                             |   **Resource:** Werknemer, groep 1                     |
|   **Metrisch:** WorkersAvailable                            |   **Metrisch:** WorkersAvailable                    |
|   **Bewerking:** Groter dan 8                           |   **Bewerking:** Groter dan 3                   |
|   **Duur:** 20 minuten                                |   **Duur:** 15 minuten                        |
|   **Afstemmen aggregatie:** Gemiddelde                           |   **Afstemmen aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verlagen met 2                         |   **Actie:** Aantal verlagen met 3                 |
|   **Leuke omlaag (minuten):** 120                            |   **Leuke omlaag (minuten):** 120                    |

Het doel-bereik gedefinieerd in het profiel wordt berekend door de minimale exemplaren die zijn gedefinieerd in het profiel voor de App serviceplan + buffer.

Het Maximum-bereik is de som van de maximale bereiken voor alle App Service-abonnementen die in de groep werknemer worden gehost.

Het aantal grotere voor de schaal van regels moet worden ingesteld op ten minste 1 X de inflatie App-Service plannen voor schaal.

Verkleinen tellen kan worden aangepast aan een ander nummer tussen 1/2 X of 1 X de inflatie App-Service plannen voor schaal omlaag.

### <a name="autoscale-for-front-end-pool"></a>Automatisch schalen voor front van toepassingen

Regels voor het front automatisch schalen zijn eenvoudiger voor werknemer van toepassingen. Hoofdzakelijk, moet u  
Zorg ervoor dat de duur van de afmetingen en de timers cooldown rekening houdt schaal bewerkingen op een App-serviceplan zijn niet onmiddellijk.

In dit scenario weet Frank dat de snelheid van de fout neemt toe nadat voorste uiteinden hebt bereikt 80% CPU-gebruik en Hiermee stelt u de regel automatisch schalen om uit te breiden exemplaren als volgt:

![Instellingen voor het automatisch schalen voor front-toepassingen.][Front-End-Scale]

|   **Automatisch schalen profiel – voorgrond eindigt**              |
|   --------------------------------------------    |
|   **Naam:** Automatisch schalen – voorgrond eindigt                |
|   **Met wilt verkleinen:** Regels voor planning en prestaties    |
|   **Profiel:** Dagelijks gebruik                           |
|   **Type:** Terugkeerpatroon                            |
|   **Bereik van doeltoepassing:** 3 tot en met 10 exemplaren             |
|   **Dagen:** Dagelijks gebruik                              |
|   **Begindatum:** 9:00 AM                         |
|   **Tijdzone:** UTC-08                           |
|                                                   |
|   **Automatisch schalen regel (schaal omhoog)**                   |
|   **Resource:** Front van toepassingen                    |
|   **Metrisch:** CPU %                               |
|   **Bewerking:** Groter dan 60%                 |
|   **Duur:** 20 minuten                        |
|   **Afstemmen aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verhogen met 3                 |
|   **Leuke omlaag (minuten):** 120                    |
|                                                   |
|   **Automatisch schalen regel (schaal omlaag)**                 |
|   **Resource:** Werknemer, groep 1                     |
|   **Metrisch:** CPU %                               |
|   **Bewerking:** Minder dan 30%                    |
|   **Duur:** 20 minuten                        |
|   **Afstemmen aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verlagen met 3                 |
|   **Leuke omlaag (minuten):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
