
1. Bezoek het [Azure-Portal]. Klik op **Door alles bladeren** > **Mobile-Apps** > de backend die u zojuist hebt gemaakt. In de mobiele app-instellingen, klikt u op **Quickstart** > **Android)**. Klik onder **configureren de clienttoepassing**, klikt u op **downloaden**. Dit een volledig Android project voor een app vooraf is geconfigureerd voor verbinding met uw backend-downloads. 

2. Open het project met behulp van **Android Studio**, met behulp van **project importeren (Eclips ADT, Gradle, enzovoort)**. Zorg ervoor dat u deze selectie importeren om te voorkomen dat eventuele fouten JDK maakt.

3. Druk op de knop **'app' uitvoeren** voor het bouwen van het project en start de app in de Android simulator.

4. Typ duidelijke tekst, zoals _de zelfstudie voltooid_ in de app en klik op de knop 'Toevoegen'. Hiermee wordt een POST-aanvraag verzonden naar de Azure backend die u eerder hebt geïmplementeerd. De backend voegt gegevens uit de aanvraag is in de tabel TodoItem SQL en geeft als resultaat informatie over de zojuist opgeslagen items terug naar de mobiele app. De mobiele app van weergegeven deze gegevens in de lijst. 

    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[Azure-Portal]: https://portal.azure.com/
