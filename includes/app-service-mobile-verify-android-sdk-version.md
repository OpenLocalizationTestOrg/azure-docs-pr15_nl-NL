Vanwege lopende ontwikkeling, kan de Android SDK versie geïnstalleerd in Android Studio niet overeenkomen met de versie in de code. De Android SDK waarnaar wordt verwezen in deze zelfstudie is versie 23, de meest recente op het moment van schrijven. Het versienummer toenemen naarmate er nieuwe versies van de SDK worden weergegeven en het is raadzaam de meest recente versie beschikbaar.

Er zijn twee symptomen van niet-overeenkomende versie:

1. Bij het maken of opnieuw opbouwen van het project, krijgt u mogelijk de foutberichten Gradle zoals '**kan doel Google Inc.:Google APIs:n vinden**'.

2. Standaard Android objecten in de code die moeten worden opgelost op basis van `import` instructies kunnen foutberichten genereren.

Als een van deze wordt weergegeven, de versie van de Android SDK geïnstalleerd in Android Studio mogelijk niet overeenkomen met het doel van de SDK van het gedownloade project.  Als u wilt controleren of de versie, moet u de volgende wijzigingen aanbrengen:


1. Klik op **Extra**Studio Android, => **Android** => **SDK Manager**. Als u de nieuwste versie van het Platform SDK niet hebt geïnstalleerd, klikt u op om het te installeren. Noteer het versienummer weergegeven.

2. Open het bestand in het tabblad Projectverkenner onder **Gradle Scripts**, **build.gradle (modeule: app)**. Zorg ervoor dat de **compileSdkVersion** en **buildToolsVersion** zijn ingesteld op de meest recente SDK versie is geïnstalleerd. De labels uitzien als volgt:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. In de Android Studio Projectverkenner met de rechtermuisknop op het projectknooppunt, kiest u **Eigenschappen**en kies in de linkerkolom **Android**. Zorg ervoor dat het **Project bouwen doel** is ingesteld op dezelfde versie als die SDK als de **targetSdkVersion**.

4. Android Studio, wordt het manifest bestand niet langer gebruikt voor de doeltoepassing SDK en de minimale SDK-versie, in tegenstelling tot de hoofdletters/kleine letters met Eclips opgeven.
