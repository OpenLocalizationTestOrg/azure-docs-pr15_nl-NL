<properties
   pageTitle="Inleiding tot Azure Beveiligingscentrum | Microsoft Azure"
   description="Meer informatie over Azure Beveiligingscentrum, de belangrijkste mogelijkheden en hoe dit werkt."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Inleiding tot Azure Beveiligingscentrum

Meer informatie over Azure Beveiligingscentrum, de belangrijkste mogelijkheden en hoe dit werkt.

> [AZURE.NOTE] In dit document maakt u kennis met de service met behulp van een voorbeeld-implementatie.

## <a name="what-is-azure-security-center"></a>Wat is Azure Beveiligingscentrum?
 Beveiligingscentrum kunt u voorkomen dat, detecteren en reageren op threats met deze beter kunt zien in en controle over de beveiliging van uw Azure resources. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

##  <a name="key-capabilities"></a>Belangrijkste mogelijkheden
 Beveiligingscentrum biedt eenvoudig te gebruiken en effectieve bedreiging preventie, detectie en reactie mogelijkheden die zijn ingebouwd in Azure. Belangrijke-mogelijkheden zijn:

| | |
|----- |-----|
| Voorkomen dat | De beveiligingsstatus van uw Azure resources bewaakt |
| | Hiermee definieert u beleidsregels voor uw Azure abonnementen en resourcegroepen op basis van de veiligheidsvereisten van uw bedrijf, de soorten toepassingen die u gebruikt en de gevoeligheid van uw gegevens |
| | Gebruik beleid op basis van hoeveelheid werk beveiligingsaanbevelingen naar service-eigenaars in het proces van het implementeren van begeleiden nodig besturingselementen |
| | Snel implementeren beveiligings-mailservices en -apparaten van Microsoft en partners |
| Detecteren |Automatisch worden verzameld en geanalyseerd beveiligingsgegevens uit uw Azure resources, het netwerk en partneroplossingen zoals antimalware-programma's en firewalls |
| | Globale hefboomwerkingen gevaar bedrijfsinformatie van Microsoft-producten en services, de Microsoft digitale misdrijven eenheid (DCU), het MSRC Microsoft Security antwoord Center () en externe-feeds |
| | Van toepassing van geavanceerde analyses, waaronder machine learning en doorgevoerd analyse |
| Beantwoorden | Biedt prioriteit incidenten/beveiligingsmeldingen |
| | Biedt inzicht krijgen in de bron van de aanval en risico resources |
| | Suggesties voor het stoppen van de huidige aanval en toekomstige aanvallen te voorkomen |

## <a name="introductory-walkthrough"></a>Inleidende Stapsgewijze instructies
 U opent Beveiligingscentrum vanuit de [Azure-portal](https://azure.microsoft.com/features/azure-portal/). [Meld u aan bij de portal](https://portal.azure.com), selecteert u **Bladeren**, en schuift u naar de optie **Beveiligingscentrum** of Selecteer de tegel **Beveiligingscentrum** die u zijn vastgemaakt aan de portal dashboard.

![Beveiliging-tegel in Azure-portal][1]

Van Beveiligingscentrum, kunt u beveiligingsbeleid voor apparaten instellen, beveiligingsconfiguraties controleren en beveiligingsmeldingen weergeven.

### <a name="security-policies"></a>Beveiligingsbeleid voor apparaten

U kunt beleidsregels voor uw Azure abonnementen en resourcegroepen op basis van uw bedrijf beveiligingsvereisten die zijn definiëren. U kunt ze naar de soorten toepassingen die u gebruikt of naar de gevoeligheid van de gegevens ook aanpassen in elk abonnement. Resources die worden gebruikt voor de ontwikkeling of testen kunnen bijvoorbeeld verschillende beveiligingsvereisten die zijn dan die worden gebruikt voor productietoepassingen. Toepassingen gebruiken met gereglementeerde gegevens zoals PII wellicht ook een hoger niveau van beveiliging.

> [AZURE.NOTE] Als u wilt een beveiligingsbeleid op het niveau van het abonnement of de resource groep wijzigen, moet u de eigenaar van het abonnement of een Inzender toe.

Selecteer op het blad **Beveiligingscentrum** de tegel **beleid** voor een lijst van uw abonnementen en resourcegroepen.   

![Beveiligingscentrum blade][2]

Selecteer op het blad **beveiligingsbeleid voor** een abonnement op de beleidsdetails van het weergeven.

![Beveiliging beleid blade abonnement][3]

**Gegevens verzamelen** (Zie boven) kunt verzamelen van gegevens voor een beveiligingsbeleid. Het inschakelen van biedt:

- Dagelijkse scannen van alle virtuele machines ondersteund voor beveiliging cmdlets voor controle en aanbevelingen.
- Verzameling beveiligingsgebeurtenissen voor analyse en bedreiging detectie.

**Kies een account opslagruimte per regio** (Zie boven) kunt u kiezen, voor de regio waarin u virtuele machines uitgevoerd, hebben de opslag-account waarin de gegevens die worden verzameld uit deze virtuele machines is opgeslagen. Als u niet een opslag-account voor elk gebied kiest, wordt het wordt gemaakt voor u. De gegevens die worden verzameld is logisch geïsoleerd van andere klanten gegevens vanwege de beveiliging.

> [AZURE.NOTE] Gegevens verzamelen en kiezen van een account opslagruimte per regio is ingesteld op het niveau van abonnement.

Selecteer **beleid voor voorkoming** (Zie boven) te openen van het **beleid voor voorkoming van** blad. **Aanbevelingen voor weergeven** , kunt u kiezen de beveiliging-besturingselementen die u wilt controleren en aangeraden op basis van de beveiligingsbehoeften van de bronnen binnen het abonnement.

Selecteer vervolgens een resourcegroep om beleid berichtdetails te bekijken.

![Beveiligingsgroep beleid blade resource][4]

**Overname** (Zie boven) kunt u de resourcegroep als definiëren:

- Overgenomen (standaard) hetgeen betekent dat alle beveiligingsbeleid voor apparaten voor deze resourcegroep worden overgenomen van het abonnement.
- Unieke hetgeen betekent dat de resourcegroep heeft een aangepaste beveiligingsbeleid. U moet wijzigen onder **aanbevelingen voor weergeven**.

> [AZURE.NOTE] Als er een conflict tussen abonnement niveau beleid en niveau Groepsbeleid van de resource, wordt het niveau Groepsbeleid van resource voorrang.

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

 Beveiligingscentrum analyseert de beveiligingsstatus van uw Azure resources identificeren mogelijke beveiligingsrisico's. Een lijst met aanbevelingen begeleidt u bij het proces van het configureren van de benodigde besturingselementen. Voorbeelden hiervan zijn:

- Inrichting antimalware om te identificeren en schadelijke software
- Beveiligingsgroepen netwerk en regels voor het besturingselement verkeer naar virtuele machines configureren
- Inrichting van web application firewalls om te helpen beschermen tegen aanvallen dat doel van uw webtoepassingen
- De ontbrekende systeemupdates
- Adressering van OS configuraties die niet overeenkomen met de aanbevolen basislijnen

Klik op de tegel **aanbevelingen** voor een lijst met aanbevelingen. Klik op elke aanbeveling om extra informatie te bekijken of te doen om het probleem te verhelpen.

![Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum][5]

### <a name="resource-health"></a>Status van de resource

De tegel **Resource beveiliging systeemstatus** ziet u de algehele beveiliging houding van de omgeving door resourcetype, inclusief virtuele machines, webtoepassingen en andere resources.   

Selecteer een type resource op de tegel van de **Resource beveiliging systeemstatus** om meer informatie, inclusief een overzicht van alle mogelijke beveiligingsrisico's die zijn geïdentificeerd te bekijken. (**Virtuele machines** is geselecteerd in het onderstaande voorbeeld).

![Resources systeemstatus tegel][6]

### <a name="security-alerts"></a>Beveiligingsmeldingen

 Beveiligingscentrum automatisch worden verzameld, worden geanalyseerd en integreert logboekgegevens uit uw Azure resources, het netwerk en partneroplossingen zoals antimalware-programma's en firewalls. Wanneer threats zijn gevonden, wordt een beveiligingswaarschuwing wordt gemaakt. Voorbeelden hiervan zijn detectie van:

- Beschadigde virtuele machines communiceren met bekende schadelijke IP-adressen
- Geavanceerde malware gedetecteerd met behulp van Windows Foutrapportage
- Brutekrachtaanvallen tegen virtuele machines
- Beveiligingsmeldingen via geïntegreerde antimalware programma's en firewalls

De tegel **beveiligingsmeldingen** klikt, ziet u een lijst met prioriteit waarschuwingen.

![Beveiligingsmeldingen][7]

Een waarschuwing selecteren, ziet u meer informatie over de aanval en suggesties voor het verhelpen van deze.

![Waarschuwing beveiligingsinformatie][8]

### <a name="partner-solutions"></a>Partneroplossingen

De tegel **partneroplossingen** kunt u monitor in één oogopslag de status van uw partneroplossingen geïntegreerd met uw Azure-abonnement. Beveiligingscentrum geeft waarschuwingen die afkomstig zijn uit de oplossingen.

Selecteer de tegel **partneroplossingen** . Een blade wordt geopend met een lijst met alle verbonden partneroplossingen.

![Partneroplossingen][9]

## <a name="get-started"></a>Aan de slag
Als u wilt beginnen met Beveiligingscentrum, moet u eerst een abonnement op Microsoft Azure. Beveiligingscentrum is aan uw Azure-abonnement ingeschakeld. Als u niet een abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

 U opent Beveiligingscentrum vanuit de [Azure-portal](https://azure.microsoft.com/features/azure-portal/). Zie de [portal documentatie](https://azure.microsoft.com/documentation/services/azure-portal/) voor meer informatie.

[Aan de slag met Azure Beveiligingscentrum](security-center-get-started.md) begeleidt snel u door de beveiliging-cmdlets voor controle en beheer van-onderdelen van Beveiligingscentrum.

## <a name="see-also"></a>Zie ook
In dit document, zijn u kennis met Beveiligingscentrum, de belangrijkste mogelijkheden en hoe u aan de slag. Zie de volgende onderwerpen voor meer informatie:

- [Beveiligingsbeleid voor apparaten in Azure Beveiligingscentrum instellen](security-center-policies.md) , informatie over het configureren van beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen.
- [Aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren](security-center-recommendations.md) , kijk hoe aanbevelingen ervoor zorgen dat u uw Azure resources beveiligen.
- [Beveiliging servicestatus bewaken in Azure Beveiligingscentrum](security-center-monitoring.md) , informatie over het controleren van de status van uw Azure resources.
- [Beheren en beantwoorden van beveiligingsmeldingen in Azure Beveiligingscentrum](security-center-managing-and-responding-alerts.md) , informatie over het beheren en reageren op beveiligingsmeldingen.
- [Partneroplossingen met Azure Beveiligingscentrum Monitoring](security-center-partner-solutions.md) , informatie over het controleren van de status van uw partneroplossingen.
- [Veelgestelde vragen over Azure beveiliging beheercentrum](security-center-faq.md) , zoeken Veelgestelde vragen over het gebruik van de service.
- [Azure-beveiliging blog](http://blogs.msdn.com/b/azuresecurity/) , het laatste nieuws van Azure beveiliging en informatie.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
