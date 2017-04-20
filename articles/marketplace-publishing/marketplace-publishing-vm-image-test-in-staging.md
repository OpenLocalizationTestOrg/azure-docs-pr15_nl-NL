<properties
   pageTitle="Test uw aanbod VM Marketplace | Microsoft Azure"
   description="Meer informatie over hoe u uw VM testafbeelding Azure Marketplace."
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
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Uw aanbod VM Azure Marketplace testen in tijdelijke

Tijdelijke betekent dat uw SKU in een privé "sandbox" waar u kunt testen en de functionaliteit valideren voordat u deze implementeert naar de marktplaats implementeren. Welke SKU wordt weergegeven in de tijdelijke net zoals u deze naar een klant die deze heeft geïmplementeerd zou doen. Uw afbeelding VM moet worden gecertificeerd moet worden geplaatst naar tijdelijke.

## <a name="step-1-push-your-offer-to-staging"></a>Stap 1: Uw aanbieding wilt tijdelijke Push

1. Klik op het tabblad **publiceren** op **Push naar tijdelijke**.

    ![tekenen](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Als de Portal voor publiceren u geïnformeerd over eventuele fouten, corrigeren.
3.  In de **wie toegang heeft tot uw gefaseerde aanbieding?** dialoogvenster Typ de lijst met Azure abonnementen die u gebruiken wilt om een voorbeeld van uw voorstel in de [portal van Azure preview](https://portal.azure.com)te.

    >[AZURE.NOTE] Voor het geval de virtuele Machines en oplossing sjablonen, kunt u **niet** "witte" lijst abonnementen van het type CSP, DreamSpark of Azure in die is geopend.


    > Voor het geval de virtuele Machines dat na het klikken op de knop **Tijdelijke PUSH**, de volgende stappen worden uitgevoerd achter de scène. Is mogelijk om weer te geven van de voortgang van elke stap onder het tabblad publiceren in de publicatie van de portal. U moet controleren op deze pagina met normale interval (totdat de status ziet u de GEFASEERDE) mislukt gegevens die moeten correctie van uw einde.

    > - Bij de eerste gaat uw aanvraag tijdelijk opslaan naar de certificering team dat de vhd valideren. Echter als uw aanvraag kunt invullen heeft hebt u alleen marketing wijzigen, is klikt u vervolgens de stap certificering overgeslagen.
    > - Zodra het certificaat is voltooid, replicatie van de aanbieding wordt gestart via de Azure datacenters. In het algemeen duurt 24-48hours voor de replicatie is voltooid maar duurt maximaal een week afhankelijk van de grootte van de vhd. Als uw aanvraag kunt invullen heeft hebt u alleen marketing wijzigen, klikt u vervolgens is de replicatie echter sneller.
    > - Wanneer de replicatie voltooid is, klikt u vervolgens zijn de aanbieding beschikbaar in de [portal van Azure](http:/portal.azure.com). AT die tijd van de status in de publicatie worden GEFASEERDE portal. Een gefaseerde aanbieding wordt weergegeven in de [portal van Azure](http:/portal.azure.com) alleen met de e-mailbericht (s) die zijn gekoppeld aan het abonnement waaraan u de aanbieding is gefaseerde.

4. Meld u aan bij de [Azure voorbeeld portal](https://portal.azure.com) met behulp van een van de Azure abonnementen weergegeven in de vorige stap.
5. Uw aanbod vinden en valideren van uw afbeelding wordt verwezen VM:
  - Zorg ervoor dat marketinginhoud correct in de Marketplace weergegeven.
  - End-to-end-implementatie van de afbeelding VM.

      ![IMG-kaart-portal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Uw aanbod blijft in tijdelijke totdat u Microsoft via de Portal voor publiceren melden [tabblad**publiceren** > Klik op de knop **"aanvragen goedkeuring naar Push naar productie"**] waarvan u bent klaar om te voeren naar productie. Dit is een ideaal tijd te alle leden van uw team selectievakje beschikken over alles in voorbereiden voor de aanbieding gaan vermelden.

> Het tijdelijk opslaan platform is bedoeld voor de aanbieding testen in een modus Afdrukvoorbeeld door de uitgever. We ontmoedigen ten zeerste met behulp van deze platofrm voor commerciële doeleinden.

## <a name="next-steps"></a>Volgende stappen
Nu uw aanbod "gefaseerde" en dat u hebt de functionaliteit en marketinginhoud getest, kunt u doorgaan met de uiteindelijke publicatie fase, **stap 4**: [de aanbieding naar de marktplaats implementeren](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Zie ook
- [Aan de slag: een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md)
