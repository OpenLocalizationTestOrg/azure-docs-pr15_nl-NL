<properties
   pageTitle="Hoe sla zoekopdrachten en gegevens activa vastmaken | Microsoft Azure"
   description="Hoe kan ik artikel mogelijkheden in de gegevenscatalogus Azure markeren voor het opslaan van gegevensbronnen en gegevens activa voor later opnieuw kunt gebruiken."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Hoe sla zoekopdrachten en gegevens activa vastmaken

## <a name="introduction"></a>Inleiding

Microsoft Azure-gegevenscatalogus biedt mogelijkheden voor gegevensdetectie bron. Gebruikers kunnen snel zoeken en filteren van de catalogus om te zoeken van gegevensbronnen en meer informatie over het beoogde gebruik, het gemakkelijker om de juiste gegevens voor de taak waaraan u bezig bent.

Maar hoe zit wanneer gebruikers moeten regelmatig samenwerken met dezelfde gegevens? Hoe zit het met wanneer gebruikers regelmatig hun kennis bijdragen aan de dezelfde gegevensbronnen in de catalogus? In deze situaties, hoeven herhaaldelijk actie het dezelfde zoekopdrachten kan niet erg efficiënt: dit is waar zoekactie opgeslagen en gegevens kunnen helpen bij het activa vastgemaakt.

## <a name="saved-searches"></a>Opgeslagen zoekopdrachten

Een opgeslagen zoekactie in de gegevenscatalogus Azure is een herbruikbare, per gebruiker zoeken definitie. Wanneer een gebruiker heeft een zoekopdracht, waaronder zoektermen, labels en andere filters, gedefinieerd kan hij deze opslaan voor later gebruik. De definitie opgeslagen zoekactie kan vervolgens opnieuw worden uitgevoerd op een later tijdstip om terug te keren gegevens elementen die overeenkomen met de zoekcriteria.

### <a name="creating-a-saved-search"></a>Een opgeslagen zoekactie maken

Als u wilt maken van een opgeslagen zoekactie, voert u eerst de zoekcriteria opnieuw worden gebruikt. Klik vervolgens op de koppeling 'Opslaan' in het vak 'Huidige Search' in de portal Azure-gegevenscatalogus.

 ![Selecteer 'opslaan' om op te slaan van de huidige zoekinstellingen voor het](./media/data-catalog-how-to-save-pin/01-save-option.png)

Voer een naam voor de opgeslagen zoekactie wanneer hierom wordt gevraagd. Kies een naam waarmee wordt duidelijke en beschrijvende van de activa van de gegevens die worden geretourneerd door de zoekopdracht.

 ![Geef een naam voor de opgeslagen zoekactie](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Beheer van opgeslagen zoekopdrachten

Wanneer een gebruiker heeft opgeslagen in een of meer zoekopdrachten, wordt de optie van een 'Opgeslagen zoekacties' wordt weergegeven in de portal Azure-gegevenscatalogus onder het vak 'Huidige Search'. Nadat dit is vergroot, wordt de volledige lijst van zoekopdrachten Hiermee slaat worden weergegeven.

 ![Lijst met opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/03-list.png)

Als u een opgeslagen zoekactie in de lijst selecteert, wordt de zoekopdracht moet worden uitgevoerd.

Het vervolgkeuzemenu te selecteren, vindt u een reeks opties:

 ![Opties voor het beheer van zoekacties](./media/data-catalog-how-to-save-pin/04-managing.png)

Selecteren "Naam" wordt de gebruiker gevraagd om in te voeren van een nieuwe naam voor de opgeslagen zoekopdracht. De definitie van zoeken wordt niet gewijzigd.

"Verwijderen" selecteren de gebruiker om bevestiging wordt gevraagd en vervolgens de opgeslagen zoekopdracht wordt verwijderd uit de lijst van de gebruiker.

Als u "Opslaan als standaard" selecteert, wordt de gekozen zoeken opgeslagen als de standaard-zoekopdracht voor de gebruiker gemarkeerd. Als de gebruiker wordt een 'lege' zoekopdracht uitgevoerd vanaf de startpagina van de gegevenscatalogus Azure, wordt van de gebruiker standaard zoeken uitgevoerd. Bovendien de zoekopdracht als standaard wordt weergegeven boven aan de lijst opgeslagen zoeken gemarkeerd.

### <a name="organizational-saved-searches"></a>Organisatie-opgeslagen zoekopdrachten

Elke gebruiker kunt zoekopdrachten voor eigen gebruik opslaan. Beheerders van de gegevenscatalogus kunnen ook opslaan voor zoekopdrachten voor alle gebruikers binnen de organisatie. Als u een zoekopdracht opslaat, worden beheerders van een optie voor het delen van de opgeslagen zoekopdracht binnen het bedrijf weergegeven. Als deze optie is geselecteerd, wordt het opgeslagen zoekactie worden opgenomen in de lijst met beschikbare wordt gezocht naar alle gebruikers.

 ![Organisatie-opgeslagen zoekopdrachten](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Vastgemaakte gegevens activa

Opgeslagen zoekopdrachten toestaan dat gebruikers kunnen opslaan en opnieuw gebruiken zoeken definities; de activa van de gegevens die het resultaat van de zoekopdrachten kunnen wijzigen als de inhoud van de catalogus wijzigen. Gegevens activa vast, kunnen gebruikers expliciet identificeren specifieke gegevens activa zodat ze beter toegankelijk zonder dat u nodig hebt voor het gebruik van een zoekopdracht.

Eenvoudig een activum gegevens vast is: gebruikers kunnen gewoon Klik op het pictogram "pincode" voor de activa gegevens toe te voegen aan de vastgemaakte lijst met. Dit pictogram wordt weergegeven in de hoek van de activa-tegel in de weergave naast elkaar en in de meest linkse kolom in de lijstweergave in de portal Azure-gegevenscatalogus.

![Een activum gegevens vast](./media/data-catalog-how-to-save-pin/05-pinning.png)

Activa unpinning gelijkmatig eenvoudig is: gebruikers klikt u op het pictogram "pincode" opnieuw als u wilt schakelen tussen de instelling voor de geselecteerde activa.

![Een activum gegevens unpinning](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Mijn activa"
De startpagina van de gegevenscatalogus van Azure-portal bevat een "Mijn activa" sectie die wordt weergegeven activa belangrijke voor de huidige gebruiker. In deze sectie bevat beide vastgemaakte activa en opgeslagen zoekopdrachten.

!["Mijn activa" op de startpagina](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Overzicht
Azure gegevenscatalogus biedt functies die het gemakkelijker voor gebruikers te vinden van de gegevensbronnen die zij nodig hebben, zodat u ze kunnen besteden minder tijd zoekt u gegevens en meer tijd ermee te werken. Opgeslagen zoekopdrachten en vastgemaakte gegevens activa deze mogelijkheden core combineren zodat gebruikers gegevensbronnen waarmee ze net zo vaak werken gemakkelijk kunnen herkennen.
