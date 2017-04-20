<properties
   pageTitle="Beheer van uw afbeelding VM op Azure Marketplace | Microsoft Azure"
   description="Gedetailleerde handleiding voor het beheren van uw afbeelding VM op Azure Marketplace na de eerste publicatie."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Postproductie handleiding voor VM aanbiedingen in de Azure Marketplace

In dit artikel wordt uitgelegd hoe u een live VM aanbieding in de Azure Marketplace kunt bijwerken. Ook begeleidt u op het proces van het toevoegen van een of meer nieuwe SKU's aan een bestaande aanbod en verwijderen van een live VM aanbieding of SKU van Azure Marketplace.

Nadat een aanbieding/SKU is gefaseerde in de [Portal van Azure](http://portal.azure.com), kunt u de onderstaande velden niet wijzigen:

- **Bieden id:** [-Virtuele Machines portal publiceren >-uw aanbod -> selecteren > tabblad VM afbeeldingen bieden id ->]
- **SKU-id:** [-Virtuele Machines portal publiceren >-uw aanbod -> selecteren > SKU's tabblad -> toevoegen een SKU]
- **Publisher Namespace:** [-Virtuele Machines portal publiceren >-scenario uitleg ons over uw bedrijf (gevonden onder "Stap 2 hebt geregistreerd uw bedrijf") ->-tabblad > > Publisher Namespace -> Namespace]

Zodra de aanbieding/SKU wordt vermeld in het [Azure Marketplace](http://azure.microsoft.com/marketplace), kunt u de onderstaande velden niet wijzigen:

- **Bieden id:** [-Virtuele Machines portal publiceren >-uw aanbod -> selecteren > tabblad VM afbeeldingen bieden id ->]
- **SKU-id:** [-Virtuele Machines portal publiceren >-uw aanbod -> selecteren > SKU's tabblad -> toevoegen een SKU]
- **Publisher Namespace:** [-Virtuele Machines portal publiceren >-scenario -> tabblad > Vertel ons over uw bedrijf (gevonden onder stap 2 hebt geregistreerd) Publisher Namespace -> Namespace]
- **Poorten** [-Virtuele Machines portal publiceren >-uw aanbod -> selecteren > tabblad VM afbeeldingen-poorten openen >]
- **Wijzigen van vermelden SKU(s) prijzen**
- **Facturering van Model wijzigen van vermelden SKU(s)**
- **Verwijdering van gedeelten van vermelden SKU(s) facturering**
- **Het aantal van de schijf gegevens van vermelden SKU(s) wijzigen**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. het bijwerken van de technische details van een SKU

U kunt een nieuwe versie toevoegen aan de lijst SKU en opnieuw publiceren van uw voorstel door de onderstaande stappen te volgen:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op op het tabblad **VM afbeeldingen** in het menu linkerpagina kant.
4. Zoek in de sectie met **SKU's** van het tabblad **VM afbeeldingen** de SKU die u wilt bijwerken.
5. Daarna een nieuwe versienummer van de SKU toe te voegen en klik op de knop **'en'** . De nieuwe versie moet X.Y.Z opmaak waar X, Y-en Z gehele getallen zijn. Wijzigingen in versie moeten alleen oplopen.
6. In het vak **URL van de VHD OS** de gedeelde access-handtekening die URI voor het besturingssysteem VHD gemaakt toevoegen en de wijzigingen op te slaan.

    >[AZURE.IMPORTANT] U kunt geen verhoging/verlagen het aantal van de schijf gegevens van een lijst SKU. U moet een nieuwe SKU in dit geval maken. Raadpleeg de sectie [3. Het toevoegen van een nieuwe SKU onder een vermelden aanbieding](#3-how-to-add-a-new-sku-under-a-live-offer) voor gedetailleerde instructies.

7. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
8. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. het bijwerken van de niet-technische details van een aanbod of een SKU

U kunt de niet-technische bijwerken (marketing, juridische, ondersteunen, categorieën) details van uw live aanbieding of SKU in de Azure Marketplace.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 de aanbieding beschrijving en logo's bijwerken

U kunt de details van de aanbieding bijwerken en opnieuw publiceren van uw voorstel door de onderstaande stappen:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op de **MARKETINGACTIVITEIT** tab in het menu linkerpagina kant.
4. Klik op de knop **ENGELS (Verenigde Staten)** .
5. Klik op op het tabblad **DETAILS** in het menu linkerpagina kant. Onder de sectie *Beschrijving* van het tabblad **DETAILS** kunt u de titel van de aanbieding bijwerken, bieden overzicht, bieden lang overzicht en de wijzigingen op te slaan.

    >[AZURE.NOTE] Neem handelingen kunt verrichten van de volgende opties terwijl u de details SKU wilt bijwerken.
    **Voer geen dubbele tekst onder de beschrijving van de aanbieding en de beschrijving van de SKU. Voer geen dubbele tekst onder de titel van de SKU en de aanbieding lang samenvatting. Voer geen dubbele tekst onder de titel van de SKU en het overzicht van de aanbieding.**

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. Klik onder de sectie *logo's* van het tabblad **DETAILS** , kunt u het logo's bijwerken. Echter geldende de logo's de [richtlijnen van Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (verwijzen naar de sectie stap 1: de marketingactiviteit inhoud bieden Marketplace-Details > richtlijnen voor Logo van Azure Marketplace ->).

    >[AZURE.NOTE] Prominente afbeelding pictogram is optioneel. U kunt niet op het pictogram voor een prominente afbeelding uploaden. Zodra prominente pictogram is geüpload, klik er is echter geen voorziening om deze te verwijderen uit de publicatie portal. In dat geval moet u de [richtlijnen voor het pictogram van prominente](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) volgen (verwijzen naar de sectie stap 1: de marketingactiviteit inhoud bieden Marketplace-Details > aanvullende richtlijnen voor de prominente logo-banner ->).

7. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md).
8. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Pas de beschrijving SKU

U kunt de details van de SKU bijwerken en opnieuw publiceren van uw voorstel door de onderstaande stappen:

1. Meld u aan bij de [portal publiceren](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op de **MARKETINGACTIVITEIT** tab in het menu linkerpagina kant.
4. Klik op de knop **ENGELS (Verenigde Staten)** .
5. Klik op op het tabblad **ABONNEMENTEN** in het menu linkerpagina kant. U kunt de titel van de SKU, SKU samenvatting en SKU beschrijving details bijwerken en de wijzigingen op te slaan onder de sectie *SKU's* van het tabblad **ABONNEMENTEN** .

    >[AZURE.NOTE] Neem handelingen kunt verrichten van de volgende opties terwijl u de details SKU wilt bijwerken. **Voer geen dubbele tekst onder de beschrijving van de aanbieding en de beschrijving van de SKU. Voer geen dubbele tekst onder de titel van de SKU's en de aanbieding lang samenvatting. Voer geen dubbele tekst onder de titel van de SKU en het overzicht van de aanbieding.**

6. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze koppeling
7. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 wijzigen de bestaande koppelingen of nieuwe koppelingen toevoegen

U kunt wijzigen van de bestaande koppelingen of nieuwe koppelingen toevoegen en uw aanbod opnieuw door de onderstaande stappen te publiceren:

1. Meld u aan bij de [portal publiceren](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op de **MARKETINGACTIVITEIT** tab in het menu linkerpagina kant.
4. Klik op de knop **ENGELS (Verenigde Staten)** .
5. Klik op op het tabblad **LINKS** in het menu linkerpagina kant.
6. Als u wilt een nieuwe koppeling toevoegen, klik vervolgens onder de *koppelingen* sectie op op de knop **Koppeling toevoegen** . Het dialoogvenster *' koppeling toevoegen '* wordt geopend. In dit dialoogvenster kunt u de koppeling titel toevoegen en URL velden en de wijzigingen op te slaan. U kunt een koppeling met gegevens die de klanten helpen invoeren.
7. Als u wilt bijwerken of verwijderen van een bestaande koppeling, selecteer de gewenste koppeling op en klik op de knop bewerken of de knop verwijderen dienovereenkomstig gewijzigd.

    >[AZURE.NOTE] Zorg dat de koppelingen die u hebt ingevoerd in deze sectie goed, werken deze koppelingen ophalen gevalideerd tijdens uw aanvraag productieproces.

8. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md).
9. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2,4 een bestaande voorbeeldafbeelding wijzigen of een nieuwe voorbeeldafbeelding toevoegen

U kunt een bestaande voorbeeldafbeeldingen wijzigen of nieuwe voorbeeldafbeeldingen toevoegen en uw aanbod opnieuw door de onderstaande stappen te publiceren:

>[AZURE.NOTE] Van slechts één voorbeeldafbeelding wordt weergegeven in de [https://portal.azure.com](https://portal.azure.com).

1. Meld u aan bij de [portal publiceren](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op de **MARKETINGACTIVITEIT** tab in het menu linkerpagina kant.
4. Klik op de knop **ENGELS (Verenigde Staten)** .
5. Klik op op het tabblad **VOORBEELDAFBEELDINGEN** in het menu linkerpagina kant.
6. Als u een nieuwe voorbeeldafbeelding toevoegen wilt, klikt u vervolgens onder de sectie *Voorbeeldafbeeldingen* klikken op de knop **Nieuwe afbeelding uploaden** en de wijzigingen op te slaan.

    >[AZURE.NOTE] Een voorbeeldafbeelding inclusief is stap optioneel.

7. Als u wilt bijwerken of verwijderen van een bestaande voorbeeldafbeelding, zoek vervolgens het juiste voorbeeldafbeelding en klik vervolgens op de knop **Afbeelding vervangen** of de knop verwijderen dienovereenkomstig gewijzigd.

8. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md).
9. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2,5 de juridische inhoud bijwerken

U kunt de juridische inhoud bijwerken en opnieuw publiceren van uw voorstel door de onderstaande stappen:

1. Meld u aan bij de [portal publiceren](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op de **MARKETINGACTIVITEIT** tab in het menu linkerpagina kant.
4. Klik op de knop **ENGELS (Verenigde Staten)** .
5. Klik op de **juridische** tabblad in het menu linkerpagina kant. Klik onder de *juridische* sectie kunt u uw beleid/gebruiksvoorwaarden bijwerken. Voer of plak de beleidsregels/voorwaarden in het tekstvak *Gebruiksvoorwaarden* en de wijzigingen op te slaan.
6. Het aantal tekens voor de juridische gebruiksvoorwaarden is 1.000.000 tekens.
7. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
8. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2.6 de ondersteuningsinformatie bijwerken

U kunt de ondersteuningsinformatie bijwerken en opnieuw publiceren van uw voorstel door de onderstaande stappen:

1. Meld u aan bij de [portal publiceren](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik in het menu linkerpagina kant klikt u op op het tabblad **ondersteuning** .
4. Klik onder de sectie *Technische contactpersoon* van het tabblad **ondersteuning** kunt u gegevens van de contactpersoon bijwerken. Deze gegevens worden gebruikt voor interne communicatie tussen de partner en Microsoft alleen.
5. Klik onder de sectie *De klantenondersteuning* van het tabblad **ondersteuning** kunt u de contactgegevens voor ondersteuning, zoals **naam, e, telefoon** en **Ondersteuning URL**bijwerken. Deze gegevens worden gebruikt voor interne communicatie tussen de partner en Microsoft alleen.

    >[AZURE.NOTE] Als u alleen e-mail ondersteuning bieden wilt, vindt u een pop-telefoonnummer onder de sectie voor **Klantondersteuning** . In dit geval wordt de meegeleverde e-mail gebruikt in plaats daarvan.

6. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
7. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 de categorieën bijwerken

U kunt het gedeelte categorieën voor de aanbieding bijwerken en opnieuw publiceren uw aanbod door de onderstaande stappen:

1. Meld u aan bij de [portal publiceren](https://publish.windowsazure.com)
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op het tabblad **categorieën** in het menu linkerpagina kant.
4. Onder de sectie *categorieën* kunt u de categorieën voor de aanbieding bijwerken en de wijzigingen op te slaan. U kunt maximaal vijf categorieën voor de galerie met Azure Marketplace selecteren.
5. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
6. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. het toevoegen van een nieuwe SKU onder een vermelden aanbieding

U kunt een nieuwe SKU onder uw live aanbod toevoegen door de onderstaande stappen te volgen:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik in het menu linkerpagina kant klikt u op op het tabblad **SKU's** . Na deze Klik op de knop **ADD A SKU**.  Een nieuw dialoogvenster wordt geopend. Voer een SKU-id in kleine letters. Schakel het selectievakje voor de facturering model(BYOL) brengen zelf als u wilt publiceren van de nieuwe SKU met BYOL billing model. Schakel het selectievakje anders voor BYOL. Na deze Klik op de maatstreepjes in het dialoogvenster een nieuwe SKU maken. Als u niet voor de facturering model BYOL voor de nieuwe SKU, worden klikt u vervolgens het billing model automatisch ingesteld op per uur voor de nieuwe SKU. Als u de 30days gratis proefversie voor ieder uur billing model inschakelen wilt, klikt u vervolgens klik op de optie 'Één maand' voor 'Is een gratis proefversie beschikbaar?". Selecteer anders "Geen PROEFABONNEMENT". [Opmerking: de optie "Is een gratis proefversie beschikbaar?" wordt alleen weergegeven als u geen BYOL in het dialoogvenster hebt tijdens het maken van de nieuwe SKU.]

    >[AZURE.IMPORTANT] De optie "Dit SKU van Marketplace verbergen omdat deze altijd moet worden gekocht via een sjabloon oplossing" moeten worden gemarkeerd als "Ja" alleen als u voor het publiceren van een oplossing sjabloon aanbieding in de Azure Marketplace worden goedgekeurd. Deze optie moet anders altijd worden gemarkeerd als "Geen".

4. Nu de linkerpagina kant menu, klik op het tabblad **VM afbeeldingen** en ontdek de nieuwe SKU die u hebt gemaakt.
5. Als u de nieuwe SKU instelt, raadpleegt u de stap 5 van deze [koppeling](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) voor instructies.
6. Als u wilt de marketingactiviteit materiaal voor de nieuwe SKU toevoegen, raadpleegt u de sectie stap 1: de marketingactiviteit inhoud bieden Marketplace-Details > punt getallen 2 tot en met 5 van deze [koppeling](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content)->.
7. Als u wilt de prijsinformatie voor de nieuwe SKU toevoegen, raadpleegt u de sectie 2.1. Uw VM prijzen van deze [koppeling](marketplace-publishing-push-to-staging.md#step-2-set-your-prices) instellen
8. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad **publiceren** en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
9. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. het wijzigen van het aantal van de schijf gegevens voor een lijst SKU

U kunt geen verhoging/verlagen het aantal van de schijf gegevens van een lijst SKU. Moet u in dit geval een maken een nieuwe SKU. Raadpleeg de sectie [3. Het toevoegen van een nieuwe SKU onder een live aanbieding](#3-how-to-add-a-new-sku-under-a-live-offer) voor gedetailleerde instructies.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. het verwijderen van een lijst aanbieding van Azure Marketplace

Er zijn verschillende aspecten die worden afgehandeld voor het geval een verzoek moeten om het verwijderen van een live aanbieding. Volg de onderstaande stappen om hulp krijgen van het ondersteuningsteam verwijderen van een lijst aanbieding van Azure Marketplace:

1.  Een ondersteuningsticket via deze [koppeling](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) verhogen
2.  Type probleem selecteert als **"Beheren aanbiedingen"** en selecteer categorie als **"Voor het wijzigen van een aanbieding en/of SKU al in productie"**
3.  De aanvraag indienen

Het ondersteuningsteam begeleidt u bij de aanbieding/SKU verwijdering.

>[AZURE.NOTE] U kunt altijd de aanbieding verwijderen terwijl dit de status van een concept (dat wil zeggen niet in STAGING- of) door te klikken op de knop **Concept verwijderen** Klik op het tabblad **Geschiedenis** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. het verwijderen van een lijst SKU van Azure Marketplace

U kunt een vermelden SKU van Azure Marketplace verwijderen door de onderstaande stappen te volgen:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op op het tabblad **SKU's** uit het deelvenster aan de linkerpagina.
4. Selecteer de SKU die u wilt verwijderen en klik op de knop verwijderen tegen die SKU.
5. Als ingevuld, Ga naar het tabblad publiceren in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren de aanbieding in de Azure Marketplace.
6. Zodra de aanbieding wordt opnieuw zijn gepubliceerd in de Azure Marketplace, wordt de SKU worden verwijderd uit de Azure Marketplace en de Azure-Portal.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. het verwijderen van de huidige versie van een lijst SKU van Azure Marketplace

U kunt de huidige versie van een lijst SKU van Azure Marketplace verwijderen door de onderstaande stappen te volgen. Zodra het proces voltooid is, worden de SKU terug naar de vorige versie worden doorgevoerd.

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2.  Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3.  Klik op op het tabblad **VM afbeeldingen** uit het deelvenster aan de linkerpagina.
4.  Selecteer de SKU waarvan u wilt verwijderen en klik op de knop verwijderen met deze versie van de huidige versie.
5.  Als ingevuld, Ga naar het tabblad **publiceren** in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren de aanbieding in de Azure Marketplace.
6.  Zodra de aanbieding wordt opnieuw zijn gepubliceerd in de Azure Marketplace, wordt de huidige versie van de vermelde SKU worden verwijderd uit de Azure Marketplace en de Azure-Portal. De SKU worden teruggezet naar de vorige versie weergegeven.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. Hoe wilt terugdraaien vermelding prijs naar productiewaarden
Ik heb hebt gewijzigd de prijzen van een lijst SKU (of ik de facturering gebieden van een lijst SKU hebt verwijderd). Omdat de bewerking wordt niet ondersteund in de Azure Marketplace, wil ik mijn wijzigingen in de productiewaarden terugdraaien. Hoe bereikt die weergegeven?

Volg de onderstaande stappen:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op het tabblad **prijzen** in het menu linkerpagina kant.
4. Klik op het tabblad prijzen selecteert u een gebied waarvan prijzen die u wilt herstellen.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. Opnieuw instellen van de prijzen voor alle cores voor het geval SKU's met per uur billing model, zoals ze in de voor de geselecteerde regio zijn. Voor SKU's met BYOL billing model, de SKU beschikbaar maken in de regio door te controleren het selectievakje ten opzichte van de SKU onder de sectie EXTERNALLY-LICENSED (BYOL) SKU beschikbaarheid (Zie de onderstaande schermafbeelding).

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Klik nu op de knop **AUTOPRICE andere markten gebaseerd op prijzen IN de Verenigde Staten**.

    >[AZURE.NOTE] Het is mogelijk dat van de knop label verschillen afhankelijk van het gebied dat u hebt geselecteerd. Aangezien we Verenigde Staten hebt geselecteerd bij het maken van dit document, zodat de knop is gelabeld als 'Automatisch prijs andere markten op basis van de prijzen in de Verenigde Staten' in de onderstaande schermafbeelding.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. De wizard van de prijs automatisch wordt geopend. De eerste pagina wordt weergegeven de selectie voor grondtal market. Controleer uw sectie en naar de volgende pagina gaan door te klikken op de knop **"->"** .

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Optie voor het selecteren van de cores en abonnementen wordt weergegeven op de pagina 2. Selecteer de gewenste abonnementen en de boormonsters en klikt u op de knop-">".

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Pagina 3 weergeven het markten/regio 's Klik op de knop wisselknop alles om te selecteren van alle regio's of handmatig de selectievakjes in voor de regio. Klik op de knop "->" te verplaatsen naar de volgende pagina.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Pagina 4, geeft de exchange-tarieven. Klik op de knop Voltooien om de stappen te voltooien. De wizard worden opnieuw ingesteld voor de prijzen op basis van uw selectie.

11. Nu Ga naar het tabblad prijzen en klik op de knop 'Overzicht en wijzigingen weergeven'.
'Concept' selecteert in de sectie 'Weergave versie' en ' ' in "Vergelijken met" sectie (Zie de onderstaande schermafbeelding). Als u geen prijzen verschil ziet, betekent dit prijzen is teruggezet naar de productiewaarden succes.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Nadat u de wijzigingen aanbrengt, Ga naar het tabblad publiceren en klik op de knop **Tijdelijke PUSH**. Raadpleeg voor gedetailleerde informatie over het testen van uw voorstel in de testomgeving deze [koppeling](marketplace-publishing-vm-image-test-in-staging.md)
13. Zodra u uw aanbod in tijdelijke getest hebt, Ga naar het tabblad publiceren in de publicatie-portal en klik op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. hoe facturering model naar productiewaarden herstellen
Ik zijn het billing model van een lijst SKU gewijzigd. Omdat de bewerking wordt niet ondersteund in de Azure Marketplace, wil ik mijn wijzigingen in de productiewaarden terugdraaien. Hoe bereikt die weergegeven?

Volg de onderstaande stappen:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op het tabblad **SKU's** in het menu linkerpagina kant.
4. Klik op de knop bewerken als u wilt terugkeren het billing model. Een venster wordt geopend. Schakel het selectievakje **'facturering en licenties klaar is extern van Azure (ook bekend doen om uw eigen licentie)'** dienovereenkomstig gewijzigd of uit.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Eenmaal raadpleegt u het antwoord van de vraag 8 in dit document wilt terugdraaien weer de prijzen.
6. Nadat u die gaat u naar het tabblad **publiceren** in de publicatie-portal en -push de aanbieding naar tijdelijke om deze te testen. Wanneer u klaar bent met het testen van de aanbieding en klik op op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. hoe u de instelling voor zichtbaarheid van een lijst SKU naar de productiewaarde herstellen

Volg de onderstaande stappen:

1. Aanmelden bij de [portal publiceren](https://publish.windowsazure.com).
2. Ga naar het tabblad **virtuele MACHINES** en selecteert u uw aanbod.
3. Klik op het tabblad **SKU's** in het menu linkerpagina kant.
4. Selecteer uw SKU en de instelling voor zichtbaarheid van de SKU naar de productiewaarde terugkeren.

    ![tekenen](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Wanneer u klaar bent met de wijzigingen en klik op op de knop **Aanvragen goedkeuring naar PUSH naar productie** opnieuw publiceren van uw voorstel in de Azure Marketplace.

## <a name="see-also"></a>Zie ook
- [Aan de slag: Hoe u een aanbieding publiceren naar de Azure Marketplace](marketplace-publishing-getting-started.md)
- [Lidmaatschap verkoper inzichten rapportage](marketplace-publishing-report-seller-insights.md)
- [Informatie over uitbetaling rapportage](marketplace-publishing-report-payout.md)
- [Het wijzigen van uw zodat extra aantrekkelijk wederverkoper Cloud oplossingsprovider](marketplace-publishing-csp-incentive.md)
- [Problemen met algemene publicatie in de Marketplace](marketplace-publishing-support-common-issues.md)
- [Ondersteuning voor als een uitgever](marketplace-publishing-get-publisher-support.md)
- [Maken van een afbeelding VM on-premises implementatie](marketplace-publishing-vm-image-creation-on-premise.md)
- [Een virtuele machine waarop Windows wordt uitgevoerd in de portal Azure preview maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
