# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Aanvraag mailetiquette en aanbevolen procedures voor inzenders van Microsoft Azure documentatie halen

Als u wijzigingen wilt documentatie publiceren, kunt u halen aanvragen indienen vanuit uw zich splitsen. Elke aanvraag halen heeft moet worden beoordeeld vóór worden samengevoegd. Lees dit artikel voor meer informatie over de manier waarop u moet werken met halen verzoek revisoren en hoe u kunt maken halen aanvragen die zijn eenvoudiger en sneller moet worden gereviseerd zodat de wachtrij voor het aanvragen van halen beter voor iedereen werkt.

## <a name="working-with-pull-request-reviewers"></a>Werken met halen verzoek revisoren

Hier ziet u de basisbeginselen die u weten moet over het werken met halen verzoek revisoren. 

- <b>Meer informatie over de rol van de revisor van de aanvraag halen. De revisor:</b>
  - Zorgt ervoor dat de eenvoudige kwaliteit van de inhoud
  - Hiermee voorkomt u dat behalen in de bibliotheek
  - Feedback bevat voor het samenvoegen

  Halen verzoek revisoren zijn opgeslagen in een rol inhoud beheermodel. De primaire bedoeling is niet gewoon samenvoegen datgene zo snel mogelijk is verzonden. Verwachten feedback waarvoor u updates, met name voor nieuwe en intensief gewijzigde artikelen uit te voeren.

- <b>Vooruit te plannen met uw revisor halen:</b>
  - Voor hoge prioriteit halen aanvragen
  - Halen verzoeken om timed/gedateerd is versies
  - Halen aanvragen die wijzigen of een groot aantal bestanden toe te voegen

- <b>SLA voor halen aanvragen controleren</b>

  In de persoonlijke bibliotheek, telkens wanneer die uw aanvraag halen de wachtrij voor het aanvragen van halen met het label bij de hand-naar-samenvoegen voert, het team wordt geprobeerd om te controleren van de aanvraag halen binnen 12 tijdens kantooruren (M-F, 08: 00 PM aan 5) en feedback geven of samenvoegen als u geen feedback is vereist. Deze SERVICEOVEREENKOMST is van toepassing op de act van controleren van de prijs, niet het samenvoegen. Wanneer ze voldoen aan [de criteria voor het samenvoegen van](contributor-guide-pr-criteria.md)PRs samengevoegd. 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Controleer de wachtrij voor het aanvragen van halen beter samen te werken voor iedereen

Er zijn twee eenvoudige situatie in de wachtrij prijs:

- Halen aanvragen die zijn klein in omvang en die vergelijkbaar wijzigingen bevatten duren minder tijd moet worden gereviseerd. 
- Halen aanvragen die grote in omvang of die andere, gemengde soorten wijzigingen bevatten duren langer om te controleren.

U kunt helpen de wachtrij voor het aanvragen van halen beter samen te werken aan de hand van deze aanbevolen procedures:

- Afzonderlijke secundaire updates naar bestaande artikelen van nieuwe artikelen of primaire herschreven. Werken met deze wijzigingen in afzonderlijke werken vertakkingen. 

- Wanneer u artikelen of afbeeldingen hebt verwijderd, niet de verwijderingen combineren met nieuwe inhoud toevoegingen of updates. De wijzigingen/nieuwe inhoud in een afzonderlijk werken tak verwerken.

- Voor de versies of refactoring van inhoud, vooruit te plannen met uw revisor prijs. Mogelijk moet u zijn of haar hulp bij het maken van een tak release of te coördineren samenvoegen tijden met publicerende tijden zodat uw inhoud wordt gepubliceerd op het juiste moment.

- Als u probeert te coördineren updates die zijn aangebracht in de ACOM cessies‑retrocessies (ie, wijzigingen in het linker navigatiegedeelte op pagina's, omleidingen of onderwijs kaarten lossen) met wijzigingen die u in de bibliotheek azure-inhoud-prijs maken wilt, moet u die kunnen worden geopend tijd vooraf afgestemd op uw revisor prijs. Anders kunnen met een groot aantal verbroken koppelingen.

## <a name="criteria-for-expedited-pull-requests"></a>Criteria voor versneld halen aanvragen

- Neem contact op met azdocprs PRs alleen als deze absoluut nodig versnellen. U kunt ook zelf aanvragen afgehandeld prijs verwerking voor rood Zone, privacy, legal en beveiligingskwesties; voor behoorlijk verbroken klantervaringen; en voor leidinggevenden doorverwijzingen. 
- Inhoud voor de functie releases komt niet in aanmerking voor het verwerken van versneld - functie release inhoud is vereist voorafgaande plannen of deze moet worden verwerkt door de prioriteitenwachtrij standaard.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>Hebt u haast? PRs die automatisch kunnen worden geaccepteerd indienen

Gebruik de PRMerger automatiseringsregels om meer van uw dagelijkse PRs automatisch samengevoegd.

PRMerger kunt accepteren uw prijs automatisch als:
* Dit van invloed is op 10 bestanden of minder.
* De presentatie bevat 15 doorvoeracties of minder.
* Minder dan 20% van wijzigingen in tekst.
* Selectors/switchers niet bijgewerkt.
* Geen bestanden zijn verwijderd of toegevoegd.
* Geen afbeeldingen zijn nieuw, gewijzigd of verwijderd.

Als uw aanvraag halen voldoet niet aan de volgende criteria, wordt het label "PRmerger kunnen niet worden samengevoegd" automatisch toegewezen zodat u weet dat deze moet worden gecontroleerd door een menselijke prijs revisor.

### <a name="need-to-make-a-lot-of-little-changes"></a>Moet u een groot aantal weinig wijzigingen aanbrengen?

Uw actiepunten uit de bovenstaande PRMerger automatiseringsregels maken en voer de volgende handelingen uit:
* Artikelen met light wijzigingen samen in een prijs met 10 of minder bestanden verzenden.
* Maak een aparte prijs voor artikelen in welke afbeeldingen of selectors wijzigen. U moet hiervoor menselijke controleren.
* Maak een aparte prijs voor nieuwe of verwijderde artikelen. U moet hiervoor menselijke controleren.

## <a name="related"></a>Gerelateerd

- [Kwaliteitscriteria voor halen aanvraag controleren](contributor-guide-pr-criteria.md)

- [Aanvraag Opmerking automatisering halen](contributor-guide-pull-request-comments.md)
