<properties
   pageTitle="Microsoft Power BI-ingesloten - verbinding maken met een gegevensbron"
   description="Power BI ingesloten, verbinding maken met gegevensbronnen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Verbinding maken met een gegevensbron

Met **Power BI ingesloten**, kunt u rapporten insluiten in uw eigen app. Wanneer u een Power BI-rapport in uw app insluit, het rapport maakt verbinding met de onderliggende gegevens door deze te **importeren** een kopie van de gegevens of door **rechtstreeks verbinding maken** met de gegevensbron met behulp van **DirectQuery**.

Dit zijn de verschillen tussen het gebruik van **importeren** en **DirectQuery**.

|Importeren | DirectQuery
|---|---
|Tabellen, kolommen, *en gegevens* zijn geïmporteerd of gekopieerd naar van het rapport gegevensset. Als u wilt zien wijzigingen die zijn aangebracht in de onderliggende gegevens, moet u vernieuwen of gegevensset op te halen, een volledige, huidige opnieuw.|Alleen de *tabellen en kolommen* zijn geïmporteerd of gekopieerd naar van het rapport gegevensset. U weergeven altijd de meest recente gegevens.
Met Power BI ingesloten, u kunt DirectQuery gebruiken met cloud-gegevensbronnen, maar niet on-premises gegevensbronnen op dit moment.

## <a name="benefits-of-using-directquery"></a>Voordelen van het gebruik van DirectQuery

Er zijn twee belangrijke voordelen wanneer u een **DirectQuery**gebruikt:

   -    **DirectQuery** kunt u visualisaties maken via zeer grote gegevenssets, waar deze anders zou unfeasible naar eerste import alle gegevens.
   -    Onderliggende gegevenswijzigingen kunt vereisen vernieuwen van gegevens en voor sommige rapporten grote gegevens overdrachten, waardoor u opnieuw importeren gegevens unfeasible kunt vragen om de nodig recente gegevens weergeven. **DirectQuery** rapporten daarentegen altijd de huidige gegevens gebruiken.

## <a name="limitations-of-directquery"></a>Beperkingen van DirectQuery

   Er gelden enkele beperkingen voor het gebruik van de **DirectQuery**:

   -    Alle tabellen moeten afkomstig zijn uit één database.
   -    Als de query te complex is, wordt een foutbericht weergegeven. U lost de fout moet u de query refactoring zodat u minder complex. Als de quuery complexe worden moet, moet u de gegevens in plaats van **DirectQuery**importeren.
   -    Het filteren van relatie is beperkt tot een één richting, in plaats van beide richtingen.
   -    U kunt het gegevenstype van een kolom niet wijzigen.
   -    Standaard beperkingen voor DAX-expressies die zijn toegestaan in eenheden. Zie [DirectQuery en maateenheden](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery en metingen

Om ervoor te zorgen query's die zijn verzonden naar de onderliggende gegevensbron acceptabel, zijn de beperkingen van maatregelen waarin. Wanneer gebruik van **Power BI Desktop**, ervaren gebruikers kunt kiezen, worden deze beperking omzeild door te kiezen **Bestand > Opties en instellingen > Opties**. Kies **DirectQuery**in het dialoogvenster **Opties** en selecteer de optie **toestaan onbeperkte maateenheden in DirectQuery-modus**. Als deze optie is geselecteerd, kan een DAX-expressie die geldig is voor een eenheid kan worden gebruikt. Gebruikers rekening moet houden; echter dat sommige expressies die kunnen worden uitgevoerd uitstekend wanneer de gegevens zijn geïmporteerd kunnen dit leiden tot traag query's naar de backend-bron in de **DirectQuery** -modus. 

## <a name="see-also"></a>Zie ook
- [Aan de slag met Microsoft Power BI ingesloten](power-bi-embedded-get-started.md)
- [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
