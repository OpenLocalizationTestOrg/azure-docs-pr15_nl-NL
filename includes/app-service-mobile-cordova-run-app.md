
1. Bezoek het [Azure-Portal]. Klik op **Door alles bladeren** > **Mobile-Apps** > de backend die u zojuist hebt gemaakt. In de mobiele app-instellingen, klikt u op **Quickstart** > **Cordova**. Onder **de clienttoepassing configureren**, selecteer **een nieuwe App maken**, klik op **downloaden**. Dit een voltooid project Cordova voor een app vooraf is geconfigureerd voor verbinding met uw backend-downloads.

2. Het gedownloade ZIP-bestand naar een map op uw harde schijf uitpakken, Ga naar het oplossingsbestand (.sln) en open het gebruik van Visual Studio.

5. Visual Studio, het platform oplossing (Android, iOS- of Windows) in de vervolgkeuzelijst naast de begin-pijl en selecteer een specifieke Implementatieapparaat of emulator door te klikken op de vervolgkeuzelijst op de groene pijl. Houd er rekening mee dat u de standaard Android platform en Rimpel emulator kunt gebruiken. Meer geavanceerde zelfstudies, moet u een ondersteund apparaat of emulator selecteren. 

6. Druk op F5 of klik op de groene pijl om te maken en en uw app Cordova uitvoeren. Als u een beveiligingsdialoogvenster in de emulator vragen van toegang tot het netwerk wordt weergegeven, dit accepteren.   

7. Na het de app wordt gestart op het apparaat of emulator, typ duidelijke tekst in **de nieuwe tekst invoeren**, zoals _de zelfstudie voltooid_ en klik vervolgens op de knop **toevoegen** .  
Hiermee wordt een POST-aanvraag verzonden naar de Azure backend die u eerder hebt geïmplementeerd. De backend voegt gegevens uit de aanvraag is in de tabel TodoItem in de SQL-Database en geeft als resultaat informatie over de zojuist opgeslagen items terug naar de mobiele app. De mobiele app van weergegeven deze gegevens in de lijst.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Herhaal de vorige drie stappen voor elk apparaatplatform die u van plan bent om te ondersteunen.

[Azure-Portal]: https://portal.azure.com/
