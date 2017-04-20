<properties
   pageTitle="Problemen met de rollen die niet worden gestart | Microsoft Azure"
   description="Hier volgen enkele veelvoorkomende redenen waarom een Cloudservice rol kan niet worden gestart. Oplossingen voor deze problemen zijn ook beschikbaar."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Problemen met Cloudservice rollen die niet worden gestart

Hier volgen enkele bekende problemen en oplossingen die zijn gerelateerd aan Azure-Cloudservices rollen die niet worden gestart.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Ontbrekende dll-bestanden of afhankelijkheden

Niet reageert rollen en functies die zijn functioneert tussen **Bezig met initialiseren**, **bezet**en **stoppen** Staten kunnen worden veroorzaakt door ontbrekende dll-bestanden of samenstellen.

Symptomen van het ontbrekende dll-bestanden of samenstellen kunnen zijn:

- Uw exemplaar van de rol is **Bezig met initialiseren**, **bezet**en **stoppen** Staten doorlopen.
- Uw exemplaar van de rol op **Gereed** is verplaatst, maar als u naar uw webtoepassing gaat, wordt de pagina wordt niet weergegeven.

Er zijn verschillende aanbevolen methoden voor deze problemen onderzoeken.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Een diagnose stellen bij ontbrekende DLL problemen in een Webrol

Wanneer u navigeren naar een website die wordt geïmplementeerd in een web rol en de browser een serverfout de volgende strekking weergegeven, is mogelijk dat een DLL ontbreekt.

![Serverfout in '/' Application.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Een diagnose stellen bij problemen met het uitschakelen van aangepaste fouten

Gedetailleerde foutinformatie kan worden bekeken door de web.config voor de Webrol om in te stellen van de foutmodus aangepaste op uit configureren en opnieuw te distribueren de service.

Gedetailleerde fouten zonder extern bureaublad weergeven:

1. Open de oplossing in Microsoft Visual Studio.

2. Zoek het bestand web.config in de **Oplossing Explorer**en open dit.

3. Ga naar de sectie system.web en voeg de volgende regel in het bestand web.config:

    ```xml
    <customErrors mode="Off" />
    ```

4. Sla het bestand.

5. Opnieuw inpakken en implementeer deze opnieuw de service.

Zodra de service is geïmplementeerd, ziet u een foutbericht weergegeven met de naam van de ontbrekende constructie of DLL.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Een diagnose stellen bij problemen door te bekijken van de fout op afstand

U kunt Extern bureaublad voor toegang tot de rol en gedetailleerde foutgegevens extern weergeven. Gebruik de volgende stappen uit om weer te geven van de fouten met behulp van extern bureaublad:

1. Zorg ervoor dat Azure SDK 1.3 of hoger is geïnstalleerd.

2. Tijdens het implementeren van de oplossing met behulp van Visual Studio, wilt u 'Configureren verbindingen met extern bureaublad...'. Zie [Extern bureaublad met Azure rollen](../vs-azure-tools-remote-desktop-roles.md)voor meer informatie over het configureren van de verbinding met extern bureaublad.

3. Klik in de klassieke portal Microsoft Azure zodra het exemplaar ziet u de status **Gereed**, klikt u op een van de rol exemplaren.

4. Klik op het pictogram **verbinding maken met** in het gebied **RAS** van het lint.

5. Meld u aan bij de virtuele machine met behulp van de referenties die zijn opgegeven tijdens de configuratie Extern bureaublad.

6. Open een opdrachtvenster.

7. Type `IPconfig`.

8. Houd rekening met de waarde IPv4-adres.

9. Open Internet Explorer.

10. Typ het adres en de naam van de webtoepassing. Bijvoorbeeld `http://<IPV4 Address>/default.aspx`.

Navigeren naar de website resulteert vervolgens meer expliciete foutberichten:

* Serverfout in '/' Application.

* Beschrijving: Er is een onverwerkte uitzondering opgetreden tijdens de uitvoering van de huidige webaanvraag. Raadpleeg de tracering stapel voor meer informatie over de fout en bron in de code.

* Details van uitzondering: System.IO.FIleNotFoundException: kan niet worden geladen, bestand of constructie ' Microsoft.WindowsAzure.StorageClient, versie = 1.1.0.0, cultuur neutraal, PublicKeyToken = 31bf856ad364e35 =' of op een van de bijbehorende afhankelijkheden. Het opgegeven bestand vinden niet.

Bijvoorbeeld:

![Expliciete Serverfout in '/' Application](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Een diagnose stellen bij problemen met behulp van de emulator berekenen

U kunt de Microsoft Azure berekeningscluster emulator vaststellen en oplossen van problemen met ontbrekende afhankelijkheden en web.config fouten.

U moet een computer of virtuele machines waarvoor een schone installatie van Windows gebruiken voor de beste resultaten bij het gebruik van deze methode van diagnose. Gebruik deze best de vorm van de Azure-omgeving, Windows Server 2008 R2 x64.

1. Installeer de zelfstandige versie van de [Azure SDK](https://azure.microsoft.com/downloads/).

2. Op de computer ontwikkeling, maakt u het project cloud-service.

3. Navigeer naar de map bin\debug van het project cloud-service in Windows Verkenner.

4. Kopieer het .csx-map en .cscfg-bestand naar de computer die u gebruikt voor foutopsporing van de problemen.

5. Op de computer wissen.Control, opent u een venster opdrachtprompt van Azure SDK en type `csrun.exe /devstore:start`.

6. Typ bij de opdrachtprompt `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Wanneer de rol wordt gestart, ziet u gedetailleerde gegevens over fouten in Internet Explorer. U kunt ook standaard Windows hulpprogramma's voor probleemoplossing het probleem verder te analyseren.

## <a name="diagnose-issues-by-using-intellitrace"></a>Een diagnose stellen bij problemen met behulp van IntelliTrace

Voor de werknemer en web rollen die .NET Framework 4 gebruiken, kunt u [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), die beschikbaar is in [Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Volg deze stappen om het implementeren van de service met IntelliTrace ingeschakeld:

1. Bevestig dat Azure SDK 1.3 of hoger is geïnstalleerd.

2. De oplossing met behulp van Visual Studio implementeren. Tijdens de implementatie, moet u het selectievakje **IntelliTrace inschakelen voor .NET-4-rollen** in.

3. Zodra het exemplaar wordt gestart, opent u de **Server Explorer**.

4. Vouw de **Azure\\Cloudservices** knooppunt en zoek naar de implementatie.

5. Vouw de implementatie totdat u de rol exemplaren ziet. Klik met de rechtermuisknop op een van de exemplaren.

6. Kies **weergave IntelliTrace logboeken**. Het **Overzicht van de IntelliTrace** wordt geopend.

7. Zoek de sectie uitzonderingen van het overzicht. Als er uitzonderingen, wordt de sectie gelabeld **Uitzonderingsgegevens**.

8. Vouw de **Uitzonderingsgegevens** en zoekt u **System.IO.FileNotFoundException** fouten ongeveer als volgt uit:

![Uitzonderingsgegevens, ontbrekende bestand of een constructie](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adres ontbrekende dll-bestanden en samenstellen

Voer de volgende stappen uit om ontbrekende DLL en constructie fouten op te lossen:

1. Open de oplossing in Visual Studio.

2. Open de map **verwijzingen** in **Solution Explorer**.

3. Klik op de vergadering die zijn opgegeven in de fout.

4. Klik in het deelvenster **Eigenschappen** **lokale kopie eigenschap** te zoeken en de waarde ingesteld op **waar**.

5. Implementeer deze opnieuw in de cloudservice.

Nadat u hebt gecontroleerd dat alle fouten hebt gecorrigeerd, kunt u de service implementeren zonder te controleren het selectievakje **IntelliTrace inschakelen voor .NET-4-rollen** .

## <a name="next-steps"></a>Volgende stappen

Meer [artikelen probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloudservices weergeven

Als u wilt weten hoe u problemen met de rol serviceproblemen cloud met behulp van Azure PaaS computer naar diagnostische gegevens, raadpleegt u [de reeks blogartikelen van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
