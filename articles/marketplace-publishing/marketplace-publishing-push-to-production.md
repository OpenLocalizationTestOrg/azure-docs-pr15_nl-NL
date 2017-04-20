<properties
   pageTitle="Uw aanbod implementeren naar de Azure Marketplace | Microsoft Azure"
   description="Meer informatie over en doorloop de instructies voor het implementeren van uw voorstel--VM afbeelding, ontwikkelaars service, gegevensservice, enzovoort--met de Azure Marketplace."
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
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Uw aanbod implementeren naar de Azure Marketplace
Wanneer u tevreden bent met uw aanbod (dat wil zeggen, u hebt getest klant scenario's, marketing-inhoud, enzovoort) en u klaar bent voor het starten, vraagt u of **Push naar productie** op het tabblad **publiceren** .  

1. De vier stappen onder de stapsgewijze instructies pagina in de publicatie portal moet zijn voltooid en groen. Zorg ervoor dat de volgende richtlijnen zijn gevolgd voor VM aanbiedingen.

    ![tekenen][img-pubportal-walkthru-checked]

2. Selecteer het tabblad **publiceren** in de lijst aan de linkerkant.

    ![tekenen][img-pubportal-menu-publish]

3. Klik op de knop **goedkeuring naar productie te zenden aanvragen**. Als u de aanvraag is gemaakt, de goedkeuring wordt uitgevoerd door een laatste controle en vervolgens uw aanbod is beschikbaar in de Azure Marketplace.

    ![tekenen][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Voor het geval de virtuele Machines dat na het klikken op de knop toestemming vragen voor push met productie, de volgende stappen worden uitgevoerd achter de scène. Is mogelijk om weer te geven van de voortgang van elke stap onder het tabblad publiceren in de publicatie van de portal. U moet controleren op deze pagina met normale interval (totdat de status luidt "Aanbieding") voor alle mislukt informatie die nodig correctie van uw einde.

> - Bij de eerste gaat uw aanvraag productie u naar het certificering team dat de vhd valideren. Echter als u uw al vermelden aanbieding bijwerken wilt en de aanvraag heeft hebt u alleen marketing wijzigen, is klikt u vervolgens de stap certificering overgeslagen.
> - Het verzoek hoort bij de volgende stap, het team van inhoud validatie die controleren of de marketingactiviteit inhoud van de aanbieding.
> - Als de bovenstaande stappen voltooid zijn, is klikt u vervolgens de aanbieding goedgekeurd in productie. Op dit moment kan de status worden 'weergegeven' in de portal voor publiceren. Deze status "Aanbieding" betekent echter niet dat het proces voltooid is. De volgende stappen moeten zijn voltooid voordat de aanbieding beschikbaar in de Azure Marketplace is.
> - Zodra de aanbieding is goedgekeurd in productie in de stap hierboven, replicatie van de aanbieding wordt gestart via de Azure datacenters. In het algemeen duurt 24-48hours voor de replicatie is voltooid maar duurt maximaal een week afhankelijk van de grootte van de vhd. Als u uw al vermelden aanbieding bijwerken wilt en deze heeft hebt u alleen marketing wijzigen, klikt u vervolgens is de replicatie echter sneller.
> - Wanneer de replicatie voltooid is, klikt u vervolgens zijn de aanbieding beschikbaar in de Azure Marketplace.

> U kunt altijd de aanbieding verwijderen terwijl dit de status van een **concept** (dat wil zeggen nooit **Push naar tijdelijke** of **Push naar productie**). Klik op de knop **Concept verwijderen** onder aan de pagina een voorlopige versie verwijderen op het tabblad **Geschiedenis** .


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Productie checklist kunt maken voor alle VM voorstellen

- Zorg ervoor dat u een Microsoft Azure Certified partner bent
- Klik op het tabblad SKU's moeten de optie 'verbergen deze SKU van Marketplace omdat deze altijd moet worden gekocht via een sjabloon oplossing"worden gemarkeerd als Ja alleen als de SKU een onderdeel van een sjabloon oplossing is. In alle andere gevallen, moet deze optie altijd worden gemarkeerd als Nee.
- Houd rekening met: U moet de instelling niet wijzigen SKU zichtbaarheid zodra de SKU wordt vermeld. Microsoft biedt geen ondersteuning deze functionaliteit.
- Zorg ervoor dat de logo's aan de richtlijnen voor het logo van Azure Marketplace hieronder voldoen.
- Aanbieding en SKU beschrijving niet mag niet gelijk zijn.
- SKU's titel en bieden lang samenvatting, hoeft niet dezelfde.
- SKU titel en overzicht bieden, hoeft niet dezelfde.
- SKU titels mag geen identieke voor een aanbieding met meerdere SKU's.

**Richtlijnen voor het logo van Azure Marketplace**

- Het ontwerp van de Azure heeft een eenvoudige kleurenpalet. Houd het aantal primaire en secundaire kleuren op uw logo laag.
- De themakleuren van de Azure-portal zijn wit en zwart. Er geen deze kleuren gebruiken als de achtergrondkleur van uw logo's. Sommige kleur dat u uw logo's laten opvallen in de portal van Azure gebruiken. U wordt aangeraden eenvoudige primaire kleuren. Als u transparante achtergrond gebruikt, moet u ervoor zorgen dat de logo/tekst niet wit of zwart is.
- Gebruik een achtergrond met kleurovergangen niet op het logo.
- Plaats tekst, zelfs uw bedrijf of merknaam, klik op het logo.
- Het uiterlijk van uw logo moet 'platte' en kleurovergangen moet voorkomen.
- Het logo moet niet worden uitgerekt.

**Aanvullende richtlijnen voor het logo prominente:**

- Het logo prominente is optioneel. De uitgever kunt niet een logo prominente afbeelding uploaden. **Echter eenmaal geüploade het pictogram prominente kan niet worden verwijderd uit de publicatie van de portal. De partner moet de Azure Marketplace-richtlijnen voor het prominente pictogrammen anders die de aanbieding wordt niet worden goedgekeurd volgen voor productie op dat moment.**
- De weergavenaam van Publisher, SKU titel en de aanbieding lang samenvatting worden weergegeven in witte tekstkleur. U moet dus zorg ervoor dat elke lichte kleur op de achtergrond van het pictogram prominente. Zwart, antenne en transparante achtergrond is niet toegestaan voor prominente pictogrammen.
- De uitgever naam, SKU titel, de aanbieding lang samenvatting weergeven en de knop maken zodra de aanbieding wordt weergegeven via een programma in het logo prominente zijn ingesloten. U moet dus niet de tekst invoeren terwijl u het logo prominente ontwerpt. Laat u lege ruimte aan de rechterkant omdat de tekst (dat wil zeggen Publisher-weergave een naam geven, SKU titel, de aanbieding lang samenvatting) worden opgenomen via programmacode door ons hier. De lege ruimte voor de tekst moet 415 x 100 aan de rechterkant (en deze wordt verschoven ten opzichte door 370px van links).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Aanvullende productieorderregel controlelijst voor al vermelden virtuele Machine biedt

- Controleer of er al een aanbod met dezelfde aanbieding naam van uw bedrijf. Indien Ja, moet u een nieuwe versie van de SKU toevoegen in de bestaande aanbieding in plaats van het maken van een nieuwe dubbele aanbieding.
- Gegevensschijf niet tussen twee versies van dezelfde SKU te wijzigen.
- De Azure Marketplace biedt geen ondersteuning voor prijzen wijzigen van de vermelde SKU's zoals dit van invloed op de facturering van de bestaande klanten. Zorg ervoor dat u beter niet wijzigen de prijzen van de vermelde SKU's in de regio's waar de SKU beschikbaar is. U kunt echter nieuwe SKU's toevoegen of nieuwe regio's toevoegen aan een bestaande SKU.


## <a name="next-steps"></a>Volgende stappen
Zodra de aanbieding site te zien, test u de klant-scenario's als u wilt dat alle de contracten en functionaliteit pas goed in de productieomgeving als werkt getest en gevalideerd in de testomgeving valideren.

## <a name="see-also"></a>Zie ook
- [Aan de slag: een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
