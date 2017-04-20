<properties 
    pageTitle="Een certificaat toevoegen aan de store Java CA | Microsoft Azure" 
    description="Informatie over het toevoegen van een certificaat-certificeringsinstantie (CA) naar de store Java CA certificaat (cacerts) voor Twilio service of Azure Service Bus." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Een certificaat toevoegen aan de certificaten van Java CA
De volgende stappen hoe u een certificaat certificeringsinstantie (CA)-certificaat toevoegen aan de store Java CA-certificaat (cacerts). Het voorbeeld gebruikt is voor de CA-certificaat dat is vereist door de service Twilio. Informatie die later in het onderwerp wordt beschreven hoe de CA-certificaat installeren voor de Bus Azure-Service. 

U kunt het certificaat CA voordat u uw JDK comprimeren en toevoegen aan uw Azure project **approot** map toevoegen hulpprogramma Keytool gebruikt, of u de taak in een Azure opstart waarin hulpprogramma Keytool gebruikt voor het toevoegen van het certificaat kan uitvoeren. In dit voorbeeld wordt ervan uitgegaan dat u een certificeringsinstantie vóór de JDK wordt ingepakte wordt toegevoegd. Ook een specifieke certificeringsinstantie wordt gebruikt in het voorbeeld, maar de stappen van een andere CA-certificaat aanvragen en u deze importeert in de winkel cacerts zou hetzelfde moeten zijn.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Een certificaat toevoegen aan de store cacerts

1. Bij een opdrachtprompt die is ingesteld op van uw JDK **jdk\jre\lib\security** map, voert u de volgende handelingen uit om te zien welke certificaten zijn geïnstalleerd:

    `keytool -list -keystore cacerts`

    U wordt gevraagd het wachtwoord store. Het standaardwachtwoord is **changeit**. (Als u wijzigen van het wachtwoord wilt, raadpleegt u de documentatie hulpprogramma Keytool gebruikt op <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) In dit voorbeeld wordt ervan uitgegaan dat het certificaat met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 niet wordt vermeld en die u wilt importeren (dit certificaat is door de Twilio API-service nodig).
2. Het certificaat aanvragen in de lijst met certificaten die bij [GeoTrust basiscertificaten](http://www.geotrust.com/resources/root-certificates/)worden vermeld. Met de rechtermuisknop op de koppeling voor het certificaat met serienummer 35:DE:F4:CF en sla deze op de map **jdk\jre\lib\security** . Voor toepassing van dit voorbeeld wordt deze zijn opgeslagen in een bestand met de naam **Equifax\_Secure\_certificaat\_Authority.cer**.
3. Importeer het certificaat via de volgende opdracht uit:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Wanneer u wordt gevraagd om te vertrouwen dit certificaat, als het certificaat MD5 vingerafdruk 67:CB:9 D heeft: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, reageren door te typen **y**.
4. Voer de volgende opdracht uit om ervoor te zorgen dat het CA-certificaat is geïmporteerd:

    `keytool -list -keystore cacerts`

5. De JDK ZIP en deze toevoegen aan uw Azure project **approot** map.

Zie voor informatie over het hulpprogramma Keytool gebruikt, <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure basiscertificaten

Uw toepassingen die gebruikmaken van Azure services (zoals Azure Service Bus) moeten het certificaat Baltimore CyberTrust Root vertrouwt. (15 April 2013 begin Azure van start is gegaan migreren vanaf de GTE CyberTrust globale Root in de hoofdmap van de CyberTrust Baltimore. Deze migratie heeft verschillende maanden om te voltooien.)

Het certificaat mogelijk is geïnstalleerd in de winkel cacerts Baltimore onthouden zodat om uit te voeren de **hulpprogramma Keytool gebruikt-lijst** opdracht eerst om te zien als deze al bestaat.

Als u toevoegen naar de hoofdsite van de CyberTrust Baltimore wilt, heeft het seriële getal 02:00:00:b9 en SHA1 vingerafdruk d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Dit kan worden gedownload van <https://cacert.omniroot.com/bc2025.crt>, opgeslagen in een lokaal bestand met de extensie **.cer**en vervolgens importeert met **hulpprogramma Keytool gebruikt** , zoals hierboven.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de basiscertificaten door Azure gebruikt, raadpleegt u [Azure hoofdsite certificaat migratie](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Zie voor meer informatie over Java, het [Java Developer Center](/develop/java/).
