<properties 
    pageTitle="Profielen van een Cloudservice lokaal in de Emulator berekeningscluster | Microsoft Azure" 
    services="cloud-services"
    description="Onderzoek prestatieproblemen met de in de cloudservices met de profiler Visual Studio" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>De prestaties van een Cloudservice testen lokaal in de Azure berekeningscluster Emulator met de Profiler Visual Studio

Een groot aantal hulpprogramma's en technieken zijn beschikbaar voor het testen van de prestaties van cloudservices.
Wanneer u een cloudservice naar Azure publiceert, kunt u beschikken over Visual Studio profileringsgegevens verzamelen en vervolgens lokaal analyseren als omschreven in de [profielen van een toepassing Azure][1].
U kunt ook diagnostische gegevens gebruiken voor het bijhouden van tal van prestatiemeteritems, zoals wordt beschreven in [werken meteritems in Azure wordt aangegeven][2].
U kunt ook naar het profiel van uw toepassing lokaal in de emulator berekeningscluster voordat u deze implementeert in de cloud.

In dit artikel worden de CPU steekproeven methode van profielen, die lokaal kan worden uitgevoerd in de emulator. CPU steekproeven is een methode die een beoordeling is niet erg storende. Op een interval aangewezen steekproeven neemt de profiler een momentopname van de stapel gesprek. De gegevens worden verzameld gedurende een periode, en wordt weergegeven in een rapport. Deze methode van het profiel vaak om aan te geven waar in een toepassing voor rekenkracht meeste CPU werk wordt klaar is.  Hierdoor kunt u het kunt richten op het 'warm pad' waar uw toepassing de meeste tijd besteedt.



## <a name="1-configure-visual-studio-for-profiling"></a>1: Visual Studio voor profielen configureren

Er zijn eerst een paar opties van de Visual Studio configuratie die nuttig zijn kunnen wanneer u een beoordeling. Als u de profielbeleid rapporten interpreteren, moet u symbolen (.pdb-bestanden) voor uw toepassing en ook symbolen voor systeembibliotheken. Wilt u Zorg ervoor dat u verwijzen naar de beschikbare symbool-servers. Klik hiervoor in het menu **Extra** in Visual Studio, kiest u **Opties**en kies vervolgens **Foutopsporing**, klikt u vervolgens op **symbolen**. Zorg ervoor dat Microsoft symbool-Servers staat vermeld onder **symbool bestandslocaties (.pdb)**.  U kunt ook verwijzen naar http://referencesource.microsoft.com/symbols, die mogelijk aanvullende symboolbestanden.

![Symboolopties][4]

Desgewenst kunt u de rapporten die de profiler wordt gegenereerd door in te stellen alleen mijn Code vereenvoudigen. Functie gesprek stapels zijn met alleen mijn Code is ingeschakeld, vereenvoudigd zodat de rapporten oproepen geheel interne bibliotheken en .NET Framework zijn verborgen. Kies op het menu **Extra** **Opties**. Vervolgens uitvouwen van het knooppunt **Hulpprogramma's voor prestaties** en kies **Algemeen**. Schakel het selectievakje voor **Inschakelen alleen mijn Code voor profiler rapporten**.

![Alleen opties voor mijn Code][17]

U kunt deze instructies gebruiken met een bestaand project of met een nieuw project.  Als u een nieuw project om te proberen de hieronder beschreven technieken maakt, kiest u een project C# **Azure-Cloudservice** en selecteer een **Rol Web** en een **Werknemer rol**.

![Azure Cloudservice Projectrollen][5]

Doeleinden, wordt bijvoorbeeld code toevoegen aan uw project die veel tijd en enkele duidelijke prestatieprobleem demonstreert. Bijvoorbeeld de volgende code aan een project van de rol werknemer toevoegen:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Belt u deze code van de methode RunAsync in van de rol van de werknemer RoleEntryPoint afgeleide klasse. (De waarschuwing over de methode synchrone uitvoering negeren).

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Maken en uw cloudservice lokaal zonder foutopsporing (Ctrl + F5), met de oplossingsconfiguratie ingesteld op **Release**. Dit zorgt ervoor dat alle bestanden en mappen worden gemaakt voor het uitvoeren van de toepassing lokaal en zorgt ervoor dat alle emulators worden gestart. De gebruikersinterface Emulator berekenen starten vanaf de taakbalk om te controleren of uw rol werknemer wordt uitgevoerd.

## <a name="2-attach-to-a-process"></a>2: koppelen aan een proces

In plaats van de toepassing vanaf de Visual Studio 2010 IDE profiel, moet u de profiler koppelen aan een actieve proces. 

Kies de profiler koppelen aan een proces, in het menu **analyseren** **Profiler** en **Bijvoegen/ontkoppelen**.

![Profiel optie bijvoegen][6]

Zoek het proces WaWorkerHost.exe voor een rol werknemer.

![WaWorkerHost proces][7]

Als uw projectmap op een netwerkstation, vraagt de profiler u een andere locatie om op te slaan de profielbeleid rapporten op te geven.

 U kunt ook koppelen aan een Webrol door aan WaIISHost.exe te koppelen.
Als er meerdere werknemer rol processen in uw toepassing, moet u het proces-id gebruiken om ze te onderscheiden. U kunt het proces-id via programmacode zoeken via het object proces. Als u deze code met de methode uitvoeren van de klasse RoleEntryPoint kleurencombinaties in een rol hebt toegevoegd, kunt u kunt bijvoorbeeld voor zoeken in het logboek in de gebruikersinterface van de Emulator berekenen welke proces verbinding maken met informatie.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Als u wilt de weergeeft, start de gebruikersinterface Emulator berekenen.

![Start de berekeningscluster Emulator UI][8]

Open het venster werknemer rol logboek console in de gebruikersinterface van de Emulator berekenen door te klikken op de titelbalk van het consolevenster. Hier ziet u de proces-ID in het logboek.

![Weergave proces-ID][9]

Een u hebt bijgevoegd, voer de stappen in de gebruikersinterface van de toepassing (indien nodig) om te reproduceren het scenario.

Wanneer u stoppen met het profiel wilt, kiest u de koppeling **Stoppen profiel** .

![Stoppen met het profiel van de optie][10]

## <a name="3-view-performance-reports"></a>3: prestatierapporten weergeven

Het prestatierapport voor uw toepassing wordt weergegeven.

Nu de profiler stopt uitvoeren, worden gegevens opgeslagen in een bestand .vsp en wordt een rapport met een analyse van deze gegevens.

![Profiler rapport][11]


Als u String.wstrcpy in het warm pad ziet, klikt u op alleen mijn Code wilt wijzigen van de weergave om weer te geven van de gebruikerscode alleen.  Als u String.Concat ziet, drukt u op de knop alle Code weergeven.

U ziet de tekst.samenvoegen methode en String.Concat een groot deel van de tijd in beslag.

![Analyse van rapport][12]

Als u de code van de samenvoeging tekenreeks in dit artikel hebt toegevoegd, ziet u een waarschuwing in de takenlijst hiervoor. Ziet u mogelijk ook een waarschuwing dat er is veel opschonen is vanwege het aantal tekenreeksen die zijn gemaakt en verwijderd.

![Prestaties waarschuwingen][14]

## <a name="4-make-changes-and-compare-performance"></a>4: Wijzig en prestaties vergelijken

U kunt ook de prestaties voor en na het wijzigen van een code vergelijken.  Het actieve proces beëindigen en de code om de bewerking van de samenvoeging tekenreeks vervangen door het gebruik van StringBuilder bewerken:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Voer een andere prestaties uitvoeren en vergelijk de prestaties. In de Verkenner prestaties als de uitgevoerd zich in dezelfde sessie, kunt u alleen beide rapporten selecteren, opent u het snelmenu te openen en kies **Voortgangsrapporten vergelijken**. Als u vergelijken met een uitvoeren in een andere prestatiesessie wilt, open het menu **analyseren** en kiest u **Rapporten over de Werkstroomprestaties vergelijken**. Beide bestanden opgeven in het dialoogvenster dat wordt weergegeven.

![De optie rapporten prestaties vergelijken][15]

De rapporten markeren verschillen tussen de twee wordt uitgevoerd.

![Vergelijkend rapport][16]

Gefeliciteerd! U hebt opgehaald gaan met de profiler.

## <a name="troubleshooting"></a>Problemen oplossen

- Controleer of u een opbouwen Release zijn profiel- en starten zonder foutopsporing.

- Als de optie bijvoegen/loskoppelen niet is ingeschakeld in het menu Profiler, voert u de Wizard prestaties.

- Gebruik de Emulator berekenen gebruikersinterface de status van uw toepassing weergeven. 

- Als u zich problemen voordoen tijdens het starten van toepassingen in de emulator of koppelen van de profiler, sluit de emulator berekeningscluster omlaag en start u het opnieuw. Als dat het probleem niet is opgelost, probeert u opnieuw op te starten. Dit probleem kan optreden als u de Emulator berekenen gebruiken om te onderbreken en actieve implementaties verwijderen.

- Als u een van de profielbeleid opdrachten vanaf de opdrachtregel, met name de globale instellingen hebt gebruikt voor zorgen dat VSPerfClrEnv /globaloff is aangeroepen en dat VsPerfMon.exe is afgesloten.

- Als steekproef, u het bericht ziet ' PRF0025: geen gegevens zijn verzameld, "Controleer of het proces dat u hebt gekoppeld aan CPU activiteit heeft. Toepassingen die niet mee bezig zijn eventuele rekenkundige wijzigingen kunnen niet alle gegevens steekproeven opleveren.  Het is ook mogelijk dat het proces is afgesloten voordat een steekproeven werden geëffend. Controleer of de methode Run voor een rol die aan u zijn profielen niet beëindigd.

## <a name="next-steps"></a>Volgende stappen

Het opzetten van Azure binaire bestanden in de emulator wordt niet ondersteund in de Visual Studio-profiler, maar als u testen geheugentoewijzing wilt, kunt u deze optie kiezen wanneer u een beoordeling. U kunt ook gelijktijdigheid profielen, waarin kunt u bepalen of threads tijdverspilling dat voor vergrendelingen of laag interactie profielen, waarmee u achterhalen prestatieproblemen wanneer de interactie tussen lagen van een toepassing, meest tussen de gegevenslaag en een rol werknemer zijn.  U kunt de database-query's die uw app genereert bekijken en de profileringsgegevens gebruiken om te verbeteren van het gebruik van de database. Zie voor informatie over het samenstellen van laag interactie profiel, het blogbericht [Stapsgewijze instructies: gebruik van de laag interactie Profiler in Visual Studio Team systeem 2010][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 