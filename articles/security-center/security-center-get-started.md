<properties
   pageTitle="Azure Beveiligingscentrum aan de slag met | Microsoft Azure"
   description="In dit artikel vindt u snel aan de slag met Azure Beveiligingscentrum door leidt u door de onderdelen voor controle en beleid van beveiliging en koppeling naar de volgende stappen."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Azure Beveiligingscentrum aan de slag met

In dit artikel vindt u snel aan de slag met Beveiligingscentrum Azure doordat die u door de beveiliging cmdlets voor controle en beleid beheeronderdelen van Beveiligingscentrum.

> [AZURE.NOTE] In dit artikel maakt u kennis met de service met behulp van een voorbeeld-implementatie. In dit artikel is niet een stapsgewijze handleiding.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt beginnen met Beveiligingscentrum, moet u een abonnement op Microsoft Azure hebben. Als u niet een abonnement hebt, kunt u zich kunt aanmelden voor een [gratis-account](https://azure.microsoft.com/pricing/free-trial/).

De gratis laag van Beveiligingscentrum automatisch aan uw abonnement is ingeschakeld en biedt inzicht in de beveiligingsstatus van uw Azure resources. Het verschaft beheer van eenvoudige beveiligingsbeleid, aanbevelingen voor beveiliging en integratie met beveiligingsproducten en services van Azure partners.

U opent Beveiligingscentrum vanuit de [Azure-portal](https://azure.microsoft.com/features/azure-portal/). Meer informatie over de Azure-portal, raadpleegt u de [portal documentatie](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Gegevens verzamelen

Beveiligingscentrum verzamelt gegevens van uw virtuele machines (VMs) Beoordeel de beveiliging staat, aanbevelingen voor beveiliging bieden en u attent maken op threats. Wanneer u eerst Beveiligingscentrum, wordt het verzamelen van gegevens is ingeschakeld op alle VMs in uw abonnement. U wordt aangeraden de gegevens verzamelen, maar u door het uitschakelen van gegevens verzamelen in het beleid Beveiligingscentrum kan afmelden.

De volgende stappen wordt beschreven hoe u opent en de onderdelen van Beveiligingscentrum gebruikt. In deze stappen wordt uitgelegd hoe u gegevens verzamelen uitschakelen als u zich wilt afmelden.

## <a name="access-security-center"></a>Access-Beveiligingscentrum

Volg deze stappen om het Beveiligingscentrum openen in de portal:

1. Selecteer in het menu **Microsoft Azure** **Beveiligingscentrum**.
![Azure menu][1]

2. Als u toegang krijgt Beveiligingscentrum voor de eerste keer tot, wordt het **Welkom** blad geopend. Selecteer Ja **! Ik wil starten Azure Beveiligingscentrum** openen van het blad **Beveiligingscentrum** en verzamelen inschakelen.
![Welkomstscherm][10]

3. Nadat u Beveiligingscentrum vanuit het Welkom blad starten of selecteer Beveiligingscentrum in het menu Microsoft Azure, wordt het blad **Beveiligingscentrum** wordt geopend. Voor eenvoudige toegang tot het blad **Beveiligingscentrum** in de toekomst, de optie **pincode blade naar dashboard** (rechtsboven).
![Pincode blade naar dashboard optie][2]

## <a name="use-security-center"></a>Met Beveiligingscentrum

U kunt beveiligingsbeleid voor apparaten voor uw Azure abonnementen en resourcegroepen configureren. Laten we een beveiligingsbeleid voor uw abonnement configureren:

1. Selecteer de tegel **beleid** op het blad **Beveiligingscentrum** .
![Beveiligingsbeleid][3]

2. Selecteer een abonnement op het blad **beveiligingsbeleid - beleid per abonnement of de resourcegroep definiëren** .
3. **Gegevens verzamelen** is ingeschakeld op het blad **beveiligingsbeleid** automatisch logboeken verzamelen. De controle extensie is op alle huidige en nieuwe VMs in het abonnement dat ingericht. (U kunt afmelden bij het verzamelen van gegevens **verzamelen van gegevens** door op te stellen **uitschakelen**, maar Hiermee voorkomt u dat Beveiligingscentrum zodat u beveiligingsmeldingen en aanbevelingen.)
4. Selecteer op het blad **beveiligingsbeleid voor** **een account opslagruimte per regio kiezen**. Voor elke regio waarin u VMs uitgevoerd hebt, kunt u het opslag-account waarin de gegevens die worden verzameld uit deze VMs is opgeslagen kiezen. Als u niet een opslag-account voor elk gebied kiest, is het voor u gemaakt. De gegevens die worden verzameld is logisch geïsoleerd van andere klanten gegevens vanwege de beveiliging.

     > [AZURE.NOTE] Het is raadzaam dat u verzamelen inschakelen en kiest u eerst een account opslagruimte op het niveau van abonnement. Beveiligingsbeleid voor apparaten kunnen worden ingesteld op het niveau van de Azure-abonnement en het niveau van de resourcegroep, maar de configuratie van gegevens verzamelen en opslag account doet zich voor op alleen het abonnement.

5. Selecteer op het blad **beveiligingsbeleid** **beleid voor voorkoming**. Hiermee opent u het **beleid voor voorkoming van** blad.
![Beleid voor voorkoming van][4]

6. Schakel op het blad **beleid voor voorkoming van** de aanbevelingen die u wilt zien als onderdeel van uw beveiligingsbeleid. Voorbeelden:

 - **Systeem-updates** instellen **op** alle ondersteunde virtuele machines voor het ontbrekende updates voor OS gescand.
 - Als u **problemen OS** op **op** gescand alle ondersteunde virtuele machines om aan te duiden OS configuraties die mogelijk de virtuele machine toegankelijk voor aanvallen maken.

### <a name="view-recommendations"></a>Weergave aanbevelingen

1. Ga terug naar het blad **Beveiligingscentrum** en selecteer de tegel **aanbevelingen** . Beveiligingscentrum analyseert regelmatig de beveiligingsstatus van uw Azure resources. Wanneer Beveiligingscentrum worden aangegeven mogelijke beveiligingsrisico's, aanbevelingen wordt weergegeven op het blad **aanbevelingen** .
![Aanbevelingen in Azure Beveiligingscentrum][5]

2.  Selecteer een aanbeveling op het blad **aanbevelingen** meer informatie kunnen bekijken en/of stappen te ondernemen om dit probleem.

### <a name="view-the-health-and-security-state-of-your-resources"></a>De status gezondheid en beveiliging van uw resources weergeven

1.  Ga terug naar het blad **Beveiligingscentrum** . De tegel **Resources beveiliging status** bevat indicatoren van de beveiligingsstatus van de voor virtuele machines, netwerken, gegevens en toepassingen.
2.  Selecteer **virtuele machines** meer informatie kunnen bekijken. Het **virtuele machines** blad geopend en een statusoverzicht van antimalware-programma's, systeemupdates opnieuw opstarten en OS problemen van uw VMs weergegeven.
![De tegel van de servicestatus Resources in Azure Beveiligingscentrum][6]

3.  Selecteer een aanbeveling onder **VM aanbevelingen** voor meer informatie weergeven en/of actie uitvoeren voor het configureren van de benodigde besturingselementen.
4.  Selecteer een VM onder **virtuele machines** u meer details.

### <a name="view-security-alerts"></a>Beveiligingsmeldingen voor weergave

1.  Ga terug naar het blad **Beveiligingscentrum** en selecteer de tegel **beveiligingsmeldingen** . Het blad **beveiligingsmeldingen** wordt geopend en bevat een overzicht van waarschuwingen. De analyse Beveiligingscentrum van uw beveiligingslogboeken en netwerkactiviteiten genereert deze waarschuwingen. Meldingen van geïntegreerde partneroplossingen zijn opgenomen.
![Beveiligingswaarschuwingen in Azure Beveiligingscentrum][7]

    > [AZURE.NOTE] Beveiligingsmeldingen zijn alleen beschikbaar als de standaard laag van Beveiligingscentrum is ingeschakeld. Er is een gratis proefversie van 90 dagen van de standaard laag beschikbaar. Zie de [volgende stappen](#next-steps) voor meer informatie over hoe u de standaard laag.

2.  Selecteer een melding in om extra informatie te bekijken. In dit voorbeeld laten we Selecteer **gewijzigd systeem binaire heeft gevonden**. Hiermee opent u de bladen die vindt u meer informatie over de waarschuwing.
![Waarschuwing beveiligingsinformatie in Azure Beveiligingscentrum][8]

### <a name="view-the-health-of-your-partner-solutions"></a>De status van uw partneroplossingen weergeven

1. Ga terug naar het blad **Beveiligingscentrum** . De tegel **partneroplossingen** kunt u controleren, in één oogopslag, de status van uw partneroplossingen geïntegreerd met uw Azure-abonnement.
2. Selecteer de tegel **partneroplossingen** . Een blade wordt geopend en bevat een overzicht van uw partneroplossingen verbonden met Beveiligingscentrum.
![Partneroplossingen][9]

3. Selecteer een partneroplossing. Selecteer in dit voorbeeld laten we de oplossing **F5-WAF** .  Een blade wordt geopend en ziet u de status van de partneroplossing en de bijbehorende hulpbronnen van de oplossing. Selecteer **oplossing console** openen van de partner management ervaring voor deze oplossing.

## <a name="next-steps"></a>Volgende stappen
In dit artikel kennis u met de beveiliging cmdlets voor controle en beleid beheeronderdelen van Beveiligingscentrum. Nu dat u bekend met Beveiligingscentrum bent, probeert u de volgende stappen uit:

- Een beveiligingsbeleid voor uw abonnement Azure configureren. Meer informatie raadpleegt u de [instelling beveiligingsbeleid voor apparaten in Beveiligingscentrum Azure](security-center-policies.md).
- Met de aanbevelingen in Beveiligingscentrum kunt u uw Azure resources beveiligen. Meer informatie raadpleegt u [aanbevelingen voor de beveiliging in Azure Beveiligingscentrum beheren](security-center-recommendations.md).
- Controleer en beheren van uw huidige beveiligingsmeldingen. Zie voor meer informatie, [beheren en beveiligingswaarschuwingen in Azure Beveiligingscentrum beantwoorden](security-center-managing-and-responding-alerts.md).
- Meer informatie over de [Geavanceerde bedreiging detectie functies](security-center-detection-capabilities.md) die worden meegeleverd bij de [standaard laag](security-center-pricing.md) van Beveiligingscentrum. Er is een gratis proefversie van 90 dagen van de standaard laag beschikbaar.
- Als u vragen over het gebruik van Beveiligingscentrum hebt, raadpleegt u de [Veelgestelde vragen over Azure beveiliging beheercentrum](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
