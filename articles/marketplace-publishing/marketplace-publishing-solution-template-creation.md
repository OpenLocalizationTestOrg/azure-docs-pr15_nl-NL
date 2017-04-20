<properties
   pageTitle="Handleiding voor het maken van een sjabloon oplossing Marketplace | Microsoft Azure"
   description="Uitgebreide instructies over het maken, certificeren en implementeren van een sjabloon Multi-VM afbeelding van de oplossing voor kopen op Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Handleiding voor het maken van een sjabloon oplossing voor Azure Marketplace
Na het voltooien van stap 1, [maken en registratie][link-acct-creation], wordt u begeleid over het maken van een sjabloon Azure-compatibele oplossing [technische vereisten voor het maken van een sjabloon oplossing](marketplace-publishing-solution-template-creation-prerequisites.md). Nu we u door de stappen begeleiden wordt voor het maken van een sjabloon oplossing voor meerdere VMs op de [Portal voor publiceren] [ link-pubportal] Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Uw aanbod oplossing-sjabloon maken in de Portal voor publiceren
Ga naar [https://publish.windowsazure.com](http://publish.windowsazure.com). Als u zich aanmeldt voor de eerste keer bij de [Portal voor publiceren](https://publish.windowsazure.com/), gebruikt u hetzelfde account met die van uw bedrijf verkoper profiel is geregistreerd. U kunt elke werknemer van uw bedrijf later toevoegen als een co-beheerders in de Portal voor publiceren.

### <a name="1-select-solution-templates"></a>1. Selecteer "Oplossing sjablonen"

  ![tekenen][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. een nieuwe oplossing-sjabloon maken

  ![tekenen][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. beginnen met topologieën
Een sjabloon oplossing is een 'parent' op alle bijbehorende topologieën. U kunt meerdere topologieën definiëren in een aanbieding/oplossing sjabloon. Wanneer een aanbod naar tijdelijke is verplaatst, wordt deze verplaatst met alle bijbehorende topologieën. Volg de onderstaande stappen om uw aanbod definiëren:     

- Maken van een topologie: "Topologie id" is meestal de naam van de topologie voor de sjabloon oplossing. De topologie-id wordt gebruikt in de URL, zoals hieronder wordt weergegeven:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure-Portal: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Voeg een nieuwe versie.

### <a name="4-get-your-topology-versions-certified"></a>4. Zorg dat uw topologie versies gecertificeerd
Upload een zipbestand dat alle benodigde bestanden als u wilt dat bepaalde versie van de topologie inrichten bevat. Dit zipbestand moet de volgende elementen bevatten:

- *mainTemplate.json* en *createUiDefinition.json* -bestand in de hoofdmap.
- Eventuele gekoppelde sjablonen en alle vereiste scripts.

  > [AZURE.TIP] Terwijl uw ontwikkelaars bezig bent met het maken van de oplossing sjabloon topologieën en dat ze gecertificeerd, het bedrijf, worden marketingactiviteit en/of juridische afdelingen van uw bedrijf kunnen werken met de marketingactiviteit en juridische inhoud.

## <a name="next-steps"></a>Volgende stappen
Volg de instructies in de [marketingactiviteit inhoud Marketplace gids](marketplace-publishing-push-to-staging.md) voor de aanbieding naar tijdelijke drukken nu dat u uw oplossing sjabloon gemaakt en het zip-bestand hebt geüpload. De volledige reeks marketplace publiceren artikelen vindt u [aan de slag: een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md).

Zijn situaties waarin u ook geïnteresseerd in deze artikelen:

- VM afbeeldingen: [Over VM afbeeldingen in Azure wordt aangegeven](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- VM extensies: [VM Agent en VM extensies overzicht](https://msdn.microsoft.com/library/azure/dn832621.aspx) en [Azure VM Extensions en functies](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- Azure resourcemanager: [Authoring Azure ARM sjablonen](../resource-group-authoring-templates.md) en [voorbeelden van eenvoudige ARM-sjabloon](https://github.com/rjmax/ArmExamples)
- Opslag account bandbreedte: [hoe Monitor voor het beperken van opslag-Account](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) en [Premium-opslag](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
