<properties
    pageTitle="Azure CDN realtime waarschuwingen | Microsoft Azure"
    description="Realtime waarschuwingen in Microsoft Azure CDN. Realtime waarschuwingen bieden meldingen over de prestaties van de eindpunten in uw profiel CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Realtime waarschuwingen in Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Overzicht

In dit document wordt uitgelegd realtime waarschuwingen in Microsoft Azure CDN. Deze functionaliteit biedt realtime meldingen over de prestaties van de eindpunten in uw profiel CDN.  U kunt e-mail of HTTP waarschuwingen op basis van instellen:

* Bandbreedte
* Statuscodes
* Statussen die cache
* Verbindingen

## <a name="creating-a-real-time-alert"></a>Een melding voor een realtime maken

1. Blader in de [Portal van Azure](https://portal.azure.com)aan uw profiel CDN.

    ![CDN profiel blade](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Klik in het blad CDN-profiel op de knop **beheren** .

    ![CDN profiel blade knop beheren](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    De beheerportal CDN wordt geopend.

3. Plaats de muisaanwijzer boven de tab **Analytics** en klik vervolgens muis boven de **Real-Time stat** : flyout.  Klik op **realtime waarschuwingen**.

    ![CDN-beheerportal](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    De lijst met bestaande waarschuwing configuraties (indien aanwezig) wordt weergegeven.

4. Klik op de knop **Waarschuwing toevoegen** .

    ![Knop Waarschuwing toevoegen](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Een formulier voor het maken van een nieuwe waarschuwing wordt weergegeven.

    ![Een nieuw formulier met waarschuwing](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Als u deze melding wilt naar actief zijn wanneer u op **Opslaan**klikt, schakelt u het selectievakje **Waarschuwing ingeschakeld** .

6. Voer een beschrijvende naam voor de waarschuwing in het veld **naam** .

7. In de vervolgkeuzelijst **Mediatype** door **Grote HTTP-Object**te selecteren.

    ![Mediatype met grote HTTP-Object geselecteerd](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] U moet **HTTP grote Object** selecteren als het **Mediatype**.  De andere opties worden niet gebruikt door **Azure CDN van Verizon**.  Fout om **Grote HTTP-Object** te selecteren, wordt de waarschuwing nooit worden geactiveerd.

8. Maak een **expressie** om te controleren door een **Metrisch**, de **Operator**en de **inwerkingtreding waarde**te selecteren.

    - Selecteer het type voorwaarde die u wilt dat gecontroleerde voor **Metrisch**.  **Bandbreedte Mbps** is het bedrag van bandbreedtegebruik in megabits per seconde.  **Totaal aantal verbindingen** is het aantal gelijktijdige HTTP-verbindingen met onze edge-servers.  Zie [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx) en [Azure CDN HTTP-Status Codes](https://msdn.microsoft.com/library/mt759238.aspx) voor definities van de verschillende statussen van de cache en statuscodes,
    - **Operator** is de rekenkundige operator die tot stand de relatie tussen de meetwaarde en de inwerkingtreding-waarde brengt.
    - **Trigger waarde** is de drempelwaarde die moet worden voldaan voordat een melding wordt verzonden.

    Klik in het onderstaande voorbeeld de expressie die ik heb gemaakt wordt aangegeven dat ik wil een melding ontvangen wanneer het aantal 404 statuscodes groter dan 25 is.

    ![Realtime waarschuwing voorbeeldexpressie](./media/cdn-real-time-alerts/cdn-expression.png)

9. Voer de hoe vaak u wilt de expression voor **Interval**.

10. Selecteer in de vervolgkeuzelijst **Waarschuwen op** wanneer u een melding ontvangen wilt wanneer de expressie waar is.
    
    - **Voorwaarde starten** wordt aangegeven dat een melding wordt verzonden wanneer de opgegeven voorwaarde voor het eerst wordt aangetroffen.
    - **Einde van de voorwaarde** geeft aan dat een melding wordt verzonden wanneer de opgegeven voorwaarde niet meer wordt aangetroffen. Deze melding kan alleen worden geactiveerd nadat ons netwerk monitoring systeem gedetecteerd dat de opgegeven voorwaarde is opgetreden.
    - **Doorlopend** wordt aangegeven dat een melding elke keer ontvangt dat het netwerk systeem voor controle wordt gedetecteerd voor de opgegeven voorwaarde. Houd er rekening mee dat het netwerk systeem voor controle slechts één keer per interval voor de opgegeven voorwaarde controleren wordt.
    - Geeft aan dat een melding de eerste keer ontvangt dat de opgegeven voorwaarde is gedetecteerd en opnieuw als de voorwaarde is niet langer gedetecteerd **voorwaarde begin en einde** .

11. Als u meldingen per e-mail ontvangen wilt, controleert u het selectievakje **Waarschuwen per E-mail** .  

    ![Hoogte door e-mailformulier](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    Voer in het veld **aan** het e-mailadres dat u de gewenste positie voor meldingen verzonden. U mogelijk de standaardwaarde laten staan voor **onderwerp** en de **hoofdtekst**, of u mogelijk het bericht met de lijst **beschikbare trefwoorden** dynamisch waarschuwing om gegevens te voegen wanneer het bericht is verzonden aanpassen.

    > [AZURE.NOTE] U kunt de e-mailmelding testen door te klikken op de knop **Melding testen** , maar alleen nadat de waarschuwing configuratie is opgeslagen.

12. Als u meldingen moeten worden geboekt tot een webserver wilt, schakelt u het selectievakje **Waarschuwen door HTTP Post** .

    ![Hoogte door HTTP Post-formulier](./media/cdn-real-time-alerts/cdn-notify-http.png)

    Voer in het veld **Url** de URL die u waar u het HTTP-bericht geplaatst. Voer in het tekstvak **kopteksten** de HTTP-headers in de uitnodiging wordt verzonden.  U kunt de berichten in de lijst **beschikbare trefwoorden** dynamisch waarschuwing om gegevens te voegen wanneer het bericht is verzonden aanpassen voor **hoofdtekst** .  **Kop** - en **hoofdtekst** standaard naar een XML-achter die vergelijkbaar is met het onderstaande voorbeeld.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] U kunt de melding HTTP Post testen door te klikken op de knop **Melding testen** , maar alleen nadat de waarschuwing configuratie is opgeslagen.

13. Klik op de knop **Opslaan** om op te slaan uw waarschuwing configuratie.  Als u **Een melding ingeschakeld** in stap 5 hebt gecontroleerd, is de waarschuwing nu actief.

## <a name="next-steps"></a>Volgende stappen

- [Realtime stat in Azure CDN](cdn-real-time-stats.md) analyseren
- De afdruk met [Geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
- [Gebruikspatronen](cdn-analyze-usage-patterns.md) analyseren

