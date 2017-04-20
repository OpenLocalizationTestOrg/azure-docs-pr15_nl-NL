<properties
   pageTitle="Detectiemogelijkheden voor in Azure Beveiligingscentrum | Microsoft Azure"
   description="Dit document kunt u meer informatie over de werking van Azure Beveiligingscentrum detectiemogelijkheden."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Mogelijkheden voor detectie van Azure Beveiligingscentrum
In dit document worden besproken Azure-Beveiligingscentrum van detectie van geavanceerde mogelijkheden, die helpt actieve threats doelgroepen van uw Microsoft Azure-resources identificeren en biedt u de inzichten die nodig zijn voor het snel reageren.

> [AZURE.NOTE] Geavanceerde detectie zijn beschikbaar in Standard laag van Azure Beveiligingscentrum. Er is een gratis proefversie van 90 dagen beschikbaar. U kunt de selectie prijzen laag in het [Beveiligingsbeleid](security-center-policies.md)upgraden. Ga naar [Beveiligingscentrum pagina](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over prijzen. 


## <a name="responding-to-todays-threats"></a>Reageren op vandaag threats
Er zijn belangrijke wijzigingen in het landschap bedreiging de afgelopen 20 jaar. In het verleden moest bedrijven meestal alleen zorgen maken over de website defacement voor afzonderlijke kwaadwillende gebruikers die voornamelijk geïnteresseerd ziet zijn "wat zij kunnen doen'. Vandaag hackers zijn veel meer geavanceerde en geordend. Ze beschikken over vaak bepaalde financiële en strategische doelstellingen. Ze ook beschikken over meer informatiebronnen beschikbaar, zoals ze kunnen worden basis van nationale Staten of georganiseerde misdaad.

Deze methode heeft onder leiding van een ongekende mogelijkheden tintje in de aanvaller classificeert. Niet langer zijn ze geïnteresseerd in web defacement. Ze zijn nu geïnteresseerd in stelen informatie, financiële accounts en persoonlijke gegevens – die zij gebruiken kunnen om te genereren cashflow op de markt openen of om te profiteren van een bepaalde bedrijven, de of militaire positie. Nog meer betreffende dan deze hackers een financiële doelstelling schadelijke die haar netwerken heeft doeleinden infrastructuur en personen.

Organisaties implementeren antwoord wordt vaak verschillende punt oplossingen die richten op de enterprise-omtrek of de eindpunten beschermen door te zoeken naar bekende aanval handtekeningen. Deze oplossingen meestal een groot aantal waarschuwingen van lage kwaliteit, waarvoor een analisten beveiliging om te sorteren en onderzoeken te genereren. De meeste organisaties ontbreken de tijd en expertise nodig is om te reageren op deze waarschuwingen – zo veel Ga niet geadresseerd.  Hackers hebt ondertussen die is voortgekomen hun manieren ondermijnt veel handtekening gebaseerde beveiliging en [aanpassen aan de cloud-omgevingen](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Nieuwe manieren zijn vereist om te sneller nieuwe gevaren identificeren en versnellen detectie en reactie. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Hoe Azure Beveiligingscentrum worden gedetecteerd en moet reageren op threats

Microsoft beveiliginglekken zijn voortdurend op zoek naar threats. Ze hebben toegang tot een uitgebreide set telemetrielogboek ervaring van Microsoft globale aanwezigheid in de cloud en on-premises implementatie. Deze kunnen bereiken van wide en diverse verzameling gegevenssets kunt Microsoft te vinden van nieuwe aanval patronen en trends in de on-premises implementatie consumenten en enterprise-producten, evenals de online services. Hierdoor kunt Beveiligingscentrum de algoritmen detectie snel bijwerken zoals hackers misbruik van nieuwe en steeds los. Deze methode kunt u blijven met een snelle zwevend bedreiging-omgeving. 

Beveiligingscentrum bedreiging detectie werkt door het automatisch verzamelen beveiligingsinformatie uit uw Azure resources, het netwerk en verbonden partneroplossingen. Deze informatie, vaak correleren gegevens uit meerdere bronnen, om aan te geven threats wordt geanalyseerd. Beveiligingsmeldingen voorrang in Beveiligingscentrum en tips voor hoe u een de bedreiging te verhelpen.

![Beveiligingscentrum gegevens verzamelen en presentatie](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Beveiligingscentrum maakt gebruik van geavanceerde beveiligingsinstellingen analytics, die verder gaan uiterst dan methoden op basis van een handtekening. Doorbraken in big data en [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) -technologieën wordt gebruikt als u wilt evalueren gebeurtenissen over de hele cloud stof – detecteren threats die is niet mogelijk om handmatig benaderingen en voorspellingen te doen de ontwikkeling van aanvallen te bepalen. Deze analytics beveiliging opnemen: 

- **Geïntegreerde bedreiging intelligence**: Hiermee wordt gezocht naar bekende bad betrokkenen door gebruikmaken van globale bedreiging bedrijfsinformatie van Microsoft-producten en services, de Microsoft digitale misdrijven eenheid (DCU), het MSRC Microsoft Security antwoord Center () en externe-feeds.
- **Doorgevoerd analytics**: bekende patronen als u wilt ontdekken schadelijke gedrag geldt. 
- **Afwijking detectie**: basis wordt statistische profiel automatisch een historische basislijn. Deze meldingen over afwijkingen van waarop deze is opgezet basislijnen die aan een mogelijke aanvalsvector voldoen.


### <a name="threat-intelligence"></a>Bedreiging bedrijfsinformatie
Microsoft heeft een zeer grote hoeveelheid globale bedreiging intelligence. Telemetrielogboek doorloopt uit meerdere bronnen, zoals Azure, Office 365, Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN.com, de Microsoft digitale misdrijven eenheid (DCU) en MSRC Microsoft Security antwoord Center (). Onderzoekers ontvangt ook bedreiging intelligence-gegevens die worden gedeeld tussen primaire cloud-serviceprovider en afsluit bedreiging intelligence-feeds van derden. Azure Beveiligingscentrum kunnen deze gegevens gebruiken om te waarschuwen u threats van bekende bad betrokkenen. Enkele voorbeelden:

- **Uitgaande communicatie met een schadelijke IP-adres**: uitgaand verkeer naar een bekende botnet of darknet waarschijnlijk wordt aangegeven dat uw resource meer veilig is en dat onbevoegden deze het uitvoeren van de opdrachten op die gegevens systeem of exfiltrate. Azure Beveiligingscentrum verschilt van netwerkverkeer met Microsoft globale bedreiging database en wordt u gewaarschuwd als wordt vastgesteld dat communicatie met een schadelijke IP-adres.

## <a name="behavioral-analytics"></a>Doorgevoerd analytics

Doorgevoerd analytics is een techniek die geanalyseerd en gegevens aan een verzameling bekende patronen vergeleken. Deze patronen zijn echter niet eenvoudige handtekeningen. Ze worden bepaald door complexe machine learning-algoritmen die zijn toegepast op enorme gegevenssets. Ze worden ook zorgvuldige analyse van schadelijk gedrag bepaald door de expert analisten. Azure Beveiligingscentrum kunt doorgevoerd analytics gebruiken om beschadigde resources op basis van analyse van VM Logboeken, virtueel netwerk apparaat Logboeken, stof Logboeken, crashdumps en andere bronnen. 

Er is bovendien correlatie met andere signalen wilt controleren op ondersteunend bewijs van een algemene campagne. Deze correlatie helpt gebeurtenissen die consistent met waarop deze is opgezet indicatoren van compromissen zijn identificeren. Enkele voorbeelden:

- **Uitvoering van het proces Suspicious**: hackers gebruiken verschillende technieken schadelijke software zonder detectie uitvoeren. Bijvoorbeeld onbevoegden mogelijk geven malware dezelfde naam als legitieme systeembestanden maar plaats deze bestanden in een andere locatie, gebruikt u een naam die lijkt op een gunstige bestand of de bestandsextensie waar masker. Beveiligingscentrum model processen gedrag en beeldschermen executions om op te sporen uitschieters zoals de volgende verwerken.  
- **Verborgen malware en kindermisbruik bevat probeert**: kan geavanceerde schadelijke software te omzeilen traditionele antimalware producten per nooit schrijven naar schijf of coderen van software-onderdelen op schijf opgeslagen.  Echter kan dergelijke malware worden gedetecteerd met geheugenanalyse, zoals de schadelijke software sporen in het geheugen laten moet vereist. Wanneer software vastloopt, wordt een deel van het geheugen in een crashdump vastgelegd op het moment van het vastlopen.  Door te analyseren het geheugen in de crashdump, Azure Beveiligingscentrum kan detecteren voor via beveiligingslekken, toegang tot vertrouwelijke gegevens en heimelijk nog steeds met-in technieken een beschadigde machine zonder die invloed hebben op de prestaties van uw computer.
- **Zijkanten verplaatsing en interne reconnaissance**: persistent maken in een beschadigde netwerk en zoek/oogst waardevol, hackers vaak naar probeert te gaan lateraal uit de beschadigde machine anderen binnen hetzelfde netwerk. Proces-en login bewaakt Beveiligingscentrum om te ontdekken pogingen om uit te vouwen van een hacker foothold binnen het netwerk, zoals externe opdracht execution netwerk scannen, en een account inventarisatie.
- **Schadelijke PowerShell-Scripts**: schadelijke code uitvoeren op target virtuele machines voor verschillende doeleinden PowerShell door onbevoegden wordt gebruikt. Beveiligingscentrum Hiermee wordt gecontroleerd PowerShell activiteit aanwezigheid van verdachte activiteiten. 
- **Uitgaande aanvallen**: hackers doelgerichte vaak cloud resources met het doel van het gebruik van deze bronnen voor extra aanvallen koppelen. Beschadigde virtuele machines mogelijk bijvoorbeeld worden gebruikt om te starten brutekrachtaanvallen ten opzichte van andere virtuele machines, SPAM verzenden of scannen open poorten en andere apparaten op internet. Door toe te passen machine learning op netwerkverkeer, kan Beveiligingscentrum waarnemen communicatie uitgaande netwerken groter is dan de norm. Als SPAM, Beveiligingscentrum ook verbindt ongebruikelijke e-verkeer met bedrijfsinformatie van Office 365 om te bepalen of de e-mail waarschijnlijk slechte of het resultaat van een betrouwbare e-campagne.  

### <a name="anomaly-detection"></a>Afwijking detectie

Afwijking detectie Azure Beveiligingscentrum ook gebruikt om te identificeren threats. Anders dan bij doorgevoerd analytics (dat afhankelijk is van bekende patronen afgeleid van grote gegevenssets), afwijking detectie is meer "aangepast" en bevat informatie over basislijnen die specifiek voor uw implementaties zijn. Machine learning om te bepalen de normale activiteit voor uw implementaties wordt toegepast en vervolgens regels worden gegenereerd om uitbijters voorwaarden die u in een beveiligingsgebeurtenis weergeven kunnen te definiëren. Hier volgt een voorbeeld:

- **Inkomende RDP/SSH basis afdwingen aanvallen**: uw implementatie wellicht bezet virtuele machines met een groot aantal aanmeldingen elke dag en andere virtuele machines waarvoor erg weinig of een aanmeldingen. Azure Beveiligingscentrum kunt bepalen basislijn login activiteit voor deze virtuele machines en machine learning-om te bepalen welke buiten de normale login activiteit gebruiken. Als het aantal aanmeldingen of de tijd van de dag van de aanmeldingen of de locatie waar de aanmeldingen zijn aangevraagd of andere kenmerken login gerelateerd sterk afwijkt van de basislijn zijn, kan een waarschuwing worden gegenereerd. Klik nogmaals bepaalt machine learning wat belangrijk is.

## <a name="continuous-threat-intelligence-monitoring"></a>Continue bedreiging intelligence bewaken

Azure Beveiligingscentrum werkt beveiliging onderzoek en gegevens wetenschappelijke teams die wijzigingen in het landschap bedreiging continu controleren. Deze groep omvat de volgende initiatieven:

- **Bedreiging intelligence monitoring**: bedreiging intelligence regelingen, indicatoren, consequenties en sneller advies over bestaande of nieuwe threats bevat. Deze informatie wordt gedeeld in de beveiligingscommunity van en Microsoft bewaakt continu bedreiging intelligence-feeds van interne en externe bronnen.
- **Signaal delen**: inzichten beveiliging teams uit Microsofts breed scala van cloud en on-premises services, servers en client eindpunt apparaten worden gedeeld en geanalyseerd. 
- **Microsoft beveiliging specialisten**: lopende betrokkenheid met teams uit Microsoft die werken in de velden, gespecialiseerde beveiliging, zoals forensics en aanval detectie met webonderdelen.
- **Detectie optimaliseren**: algoritmen voor bestaande klanten gegevensgroepen zijn uitgevoerd en beveiliginglekken werken met klanten voor het valideren van de resultaten. Waar en ONWAAR positieve worden gebruikt om te verfijnen machine learning algoritmen.

Deze gecombineerde inspanningen moet resulteren in nieuwe en verbeterde detectie, waarin u van direct profiteren kunt: Er is geen waarvoor u de actie.

## <a name="see-also"></a>Zie ook
In dit document, hebt u geleerd hoe naar Beveiligingscentrum Azure detectie mogelijkheden werken. Zie de volgende onderwerpen voor meer informatie over Beveiligingscentrum:

- [Azure Beveiligingscentrum plannen en Operations Guide](security-center-planning-and-operations-guide.md)
- [Beheren en beantwoorden van beveiligingswaarschuwingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md)
- [Beveiligingsmeldingen door te typen in Azure Beveiligingscentrum](security-center-alerts-type.md)
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md) , informatie over het controleren van de status van uw Azure resources.
- [Partneroplossingen met Azure Beveiligingscentrum Monitoring](security-center-partner-solutions.md) , informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over Azure beveiliging beheercentrum](security-center-faq.md) , zoeken Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) , weblogberichten over Azure beveiliging en naleving zoeken.
