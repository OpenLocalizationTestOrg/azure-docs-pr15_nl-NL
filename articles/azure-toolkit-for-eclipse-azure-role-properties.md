<properties
    pageTitle="Eigenschappen van Azure"
    description="Informatie over het gebruik van de Azure Toolkit voor Eclips Azure rolinstellingen configureren."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Eigenschappen van Azure #

Verschillende configuratie-instellingen voor uw Azure rol kunnen worden ingesteld in de Toolkit Azure voor Eclips.

## <a name="configuring-azure-role-properties"></a>Eigenschappen van Azure configureren ##

De eigenschappen van de rol Azure configureert wordt gemaakt met behulp van de eigenschap dialoogvensters voor uw rol werknemer. Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster en selecteert u de opties voor AutoAanpassen **Azure** . (Als u de rol in de Eclips Projectverkenner niet ziet, vouwt u uw Azure project in Projectverkenner.)

![][ic789599]

De verschillende eigenschappen die kunnen worden ingesteld in de dialoogvensters **Eigenschappen** worden beschreven in dit onderwerp. Houd er rekening mee dat veel eigenschappen worden automatisch ingevuld wanneer u een nieuw project van Azure-implementatie maakt.

De volgende eigenschap-pagina's zijn beschikbaar voor Azure rollen.

* [VM eigenschappen](#virtual_machine_properties)
* [In de cache eigenschappen](#caching_properties)
* [Eigenschappen van certificaten](#certificates_properties)
* [Eigenschappen van onderdelen](#components_properties)
* [Voor foutopsporing in eigenschappen](#debugging_properties)
* [Eigenschappen van de eindpunten](#endpoints_properties)
* [Eigenschappen van de variabelen omgeving](#environment_variables_properties)
* [Taakverdeling / sessie affiniteit (a.k.a "Plaktoetsen sessies")-eigenschappen](#session_affinity_properties)
* [Eigenschappen van de lokale opslag](#local_storage_properties)
* [Configuratie-servereigenschappen](#server_configuration_properties)
* [SSL offloading eigenschappen](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>VM eigenschappen ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster, klik op **Azure**en klik vervolgens op **Eigenschappen**, en u de mogelijkheid om de VM formaat te wijzigen, en ook wijzigen het aantal exemplaren, zoals wordt weergegeven in de volgende afbeelding.

![][ic719499]

>[AZURE.NOTE] Alleen Windows: wanneer u het aantal exemplaren op een waarde groter is dan 1 en u ook een toepassingsserver configureren, de toolkit slechts 1 rol exemplaar om uit te voeren in de emulator, ongeacht deze instelling wordt toestaan. Dit is om te voorkomen dat poort binding conflicten tussen de verschillende server-exemplaren (bijvoorbeeld alle probeert te maken met poort 8080) wanneer ze op dezelfde computer worden uitgevoerd. De instelling voor uw exemplaar van het gewenste aantal blijft behouden, maar deze wordt uitgevoerd op alleen wanneer u dashboard naar de cloud implementeren.

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>In de cache eigenschappen ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **opslaan in cache**. In dit dialoogvenster kunt u benoemde reserveren memcache-compatibele cache inschakelen zodat u kunt versnellen uw webtoepassingen.

![][ic719483]

U kunt binnen de pagina van de eigenschap **cache** globale instellingen voor het volgende opgeven:

* of reserveren caching is ingeschakeld.
* de grootte van de cache als een percentage van geheugen.
* de naam van de account opslag voor het opslaan van de cache staat wanneer uw toepassing wordt uitgevoerd als een cloudservice of geen als u niet wilt dat de status van de cache opslaan. (De naam van het opslag-account wordt niet gebruikt wanneer u uw toepassing in de emulator berekeningscluster uitvoert.) Als u de naam van het opslag-account instelt op **(automatisch)** (dit is de standaardinstelling), worden uw in de cache configuratie automatisch hetzelfde opslag-account gebruikt als de relatie die u in het dialoogvenster **publiceren op Azure selecteert** .

>[AZURE.NOTE] De instelling **(automatisch)** heeft het gewenste effect alleen als u uw implementatie met de Eclips-toolkit publiceert wizard publiceren. Als u in plaats daarvan het publiceren van het .cspkg-bestand handmatig met behulp van een externe om, zoals de [Beheerportal van Azure][], de implementatie niet naar behoren.

Het volgende dialoogvenster ziet u de eigenschappen voor een cache.

![][ic719501]

* **Naam:** De naam van de cache voor samen bevindt.
* **Poortnummer:** Het poortnummer voor de cache wilt gebruiken.
* **Verloopbeleid:** Een van de volgende waarden waarmee wordt bepaald wanneer een sleutel in de cache verloopt.
    * **Absolute:** De sleutel verloopt wanneer de tijd die is opgegeven door **minuten live** is bereikt.
    * **NeverExpires:** De toets heeft geen een verlooptijd.
    * **SlidingWindow:** De sleutel verloopt als dit niet is geopend voor de hoeveelheid tijd die is opgegeven door **minuten live**. Telkens wanneer die dit wordt geopend, de klok van de verlooptijd is opnieuw instellen.
* **Minuten live:** Maximum aantal minuten voor een sleutel memcached naar live, onderhevig aan het verloopbeleid.
* **Beschikbaarheid met andere rol exemplaren gerepliceerde back-ups:** Als ingeschakeld, vindt u helpt beschikbaarheid met behulp van back-ups op andere rol exemplaren gerepliceerd. Houd er rekening mee dat ten minste twee instanties van de rol van kracht moeten zijn voor uw implementatie van deze functie om te werken.

Als u wilt een nieuwe cache hebt toegevoegd, klikt u op de knop **toevoegen** in het eigenschappenvenster van de **cache** en een dialoogvenster **Met de naam Cache configureren** wordt geopend. Waarden opgeven voor de eigenschappen die hierboven is beschreven.

Als u wilt een benoemde cache wijzigen, selecteert u de cache en klik op de knop **bewerken** in het eigenschappenvenster van de **cache** . Een dialoogvenster wordt geopend waarin u de eigenschappen van de cache wijzigen. Druk op **OK** om de cachewaarden opslaan.

Als u wilt een cache verwijderen, selecteert u de cache en klik op de knop **verwijderen** in het eigenschappenvenster van de **cache** en klik vervolgens op **Ja** om de verwijdering te bevestigen.

Zie voor meer informatie over het gebruik van de cache opslaan, [Caching van Co-located gebruiken][].

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>Eigenschappen van certificaten ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **certificaten**.

![][ic710964]

In dit dialoogvenster kunt u toevoegen of verwijderen van certificaten waarnaar wordt verwezen door uw project Eclips. Houd er rekening mee dat de certificaten hier vermeld niet automatisch in een keystore Java opgeslagen worden en kunnen daarom niet automatisch beschikbaar voor gebruik van binnen een Java-toepassing zijn. Ze zijn alleen geregistreerd met Azure, zodat ze kunnen worden vooraf geïnstalleerd bij de Windows certificaat worden opgeslagen op de virtuele machines uitgevoerd van uw implementatie en vervolgens wordt gebruikt door andere software van Windows. De enige functie van de toolkit waarin de certificaten waarnaar wordt verwezen deze manier in het dialoogvenster **certificaten** is momenteel [SSL Offloading][], vanwege gebruikmaakt van Internet Information Services (IIS) en toepassing aanvragen routering (ARR), waarvoor het juiste certificaat beschikbaar zijn op deze manier.

Wanneer u uw project met de wizard Publiceren Azure implementeert, wordt u gevraagd Wijs met de persoonlijke informatie Exchange (PFX)-bestanden die overeenkomt met deze certificaten, samen met hun wachtwoorden vermeld, om te kunnen ze automatisch uploaden naar de Azure-service, maar alleen als ze niet zijn geüpload er eerder.

<a name="components_properties"></a> 
### <a name="components-properties"></a>Eigenschappen van onderdelen ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **onderdelen**. In dit dialoogvenster hebt u de mogelijkheid om te toevoegen, wijzigen, of verwijder de onderdelen van uw rol, evenals de volgorde waarin ze worden verwerkt wijzigen.

![][ic719502]

De functie onderdelen kunt u afhankelijkheden toevoegen aan uw project Azure-implementatie, zoals Java toepassing projecten, speciale bestanden en uitvoerbare opdrachtregel-instructies die nodig zijn voor uw implementatie.

Voor elk onderdeel, kunt u het volgende opgeven:

* De stap te nemen bij het importeren van het onderdeel in uw project Azure-implementatie wanneer deze is gebaseerd.
* De stap moeten worden genomen wanneer dat onderdeel in de cloud Azure implementeren.

>[AZURE.NOTE] Wanneer u opgeeft onderdeelbestanden of opdracht lijnen, houd er rekening mee dat uw implementatie wordt gepubliceerd naar een virtuele Windows-computer, zodat uw aangepaste stappen geldig voor een Windows-besturingssysteem zijn moeten. 

Onderdelen bestaan uit de volgende eigenschappen:

* **Importeren:** Methode die wordt aangegeven hoe het onderdeel worden geïmporteerd in het project wanneer het project is gebaseerd. Dit is een van de volgende waarden:
    * **kopiëren:** Het onderdeel is gekopieerd van het lokale pad dat is opgegeven door de eigenschap **From** in van de functie **approot** directory.
    * **OOR:** Het onderdeel is een Java enterprise archief (OOR) geïmporteerd uit een ondernemingsproject van toepassing op het opgegeven door de eigenschap **uit** lokale pad. (Dit wordt automatisch gedetecteerd door de toolkit op basis van de aard van het project op deze locatie).
    * **JAMPOT:** Het onderdeel is van een Java-archief (oppervlak) en zijn geïmporteerd uit een project Java op het opgegeven door de eigenschap **uit** lokale pad. (Dit wordt automatisch gedetecteerd door de toolkit op basis van de aard van het project op deze locatie).
    * **geen:** Geen actie is, wordt het onderdeel importeren. Dit is van toepassing wanneer de component wordt uitgegaan van al aanwezig zijn in van de functie **approot** directory, of wanneer het onderdeel is alleen een overzicht van het uitvoerbare opdrachtregel, zoals opgegeven in de eigenschap **als** wanneer de methode **Deploy** **uitvoeren**.
    * **WAR:** Het onderdeel is van een Java web application archief (WAR) en zijn geïmporteerd uit een dynamische Web Project op het opgegeven door de eigenschap **uit** lokale pad. (Dit wordt automatisch gedetecteerd door de toolkit op basis van de aard van het project op deze locatie).
    * **zip:** Het onderdeel is van een zip-bestand en wordt geïmporteerd door het comprimeren van de map of bestand opgegeven met de eigenschap **uit** .
* **Van:** Bronpad op uw lokale computer naar de map of bestand met de items importeren in uw implementatie. Windows-omgevingsvariabelen kunnen worden gebruikt in deze eigenschap. Alle importeerbare onderdelen worden geïmporteerd in van de functie **approot** directory wanneer het project is gebaseerd.
    
    Houd er rekening mee dat u de mogelijkheid om te implementeren van een onderdeel van het downloaden van een hebt wanneer u de implementatie in de cloud (niet de emulator berekeningscluster). Zie de onderstaande verwante informatie over het toevoegen van een onderdeel.    
    
* **As:** Een bestandsnaam, waaronder het onderdeel worden geïmporteerd in van de functie **approot** directory en uiteindelijk geïmplementeerd in de cloud Azure. Laat deze eigenschap leeg als de naam houden hetzelfde als ze op de lokale computer. (Voor uitvoerbare onderdelen, dat wil zeggen de waarvan methode **Deploy** is ingesteld op **uitvoeren**, dit is een willekeurige Windows opdrachtregel-instructie.)

    >[AZURE.IMPORTANT] Als u spaties voor deze waarde gebruikt, worden ze anders worden verwerkt afhankelijk van de methode deploy. Als de methode **Raad is**, worden spaties beschouwd als scheidingstekens voor de opdrachtregel-argumenten en niet als onderdeel van de bestandsnaam. Voor alle andere implementeren methoden, spaties worden beschouwd als onderdeel van de bestandsnaam.
    
* **Implementeren:** Methode die wordt aangegeven welke actie op de component wordt toegepast wanneer de implementatie wordt gestart. Dit is een van de volgende waarden:
    * **kopiëren:** Het onderdeel wordt gekopieerd naar het bestemmingspad is opgegeven door de eigenschap **aan** .
    * **Raad:** Het onderdeel is uitvoerbare Windows opdrachtregel instructie uitgevoerd in de context van het pad dat is opgegeven door de eigenschap **aan** , op het moment dat de implementatie wordt gestart.
    * **geen:** Geen actie wordt toegepast op de component wanneer de implementatie wordt gestart.
    * **zip:** Het onderdeel is uitgepakt naar het opgegeven door de eigenschap **To** bestemmingspad. Deze methode is alleen beschikbaar wanneer de eigenschap **importeren** **zip is**.
* **To:** Bestemmingspad op de virtuele machine waarin het onderdeel wordt geïmplementeerd. Windows-omgevingsvariabelen kunnen worden gebruikt in deze eigenschap en bestandspaden zijn ten opzichte van **approot**.
    
Een nieuwe als onderdeel wilt toevoegen, klikt u op de knop **toevoegen** in het eigenschappenvenster **onderdelen** en een dialoogvenster **Azure rol Component** wordt geopend. Waarden opgeven voor de eigenschappen die hierboven is beschreven. 

Hieronder ziet u een voorbeeld voor het toevoegen van een nieuw WAR-onderdeel.

![][ic719503]

Wanneer u implementeert in de cloud (niet de berekeningscluster emulator), als u wilt het onderdeel van het downloaden van een implementeren, moet u ervoor zorgen dat **Wanneer u zich in de cloud, in plaats van op te nemen in het pakket implementeren van** is ingeschakeld. Als u downloaden van uw account Azure opslag, selecteer de opslag-account in de vervolgkeuzelijst **opslag account** lijst wilt (u kunt de koppeling **Accounts** als u wilt wijzigen wat staat er in de lijst op), die gedeeltelijk in het veld **URL** invullen en vult u het resterende deel van de URL. Als u niet Azure opslagruimte gebruiken wilt, selecteer **(geen)** in de vervolgkeuzelijst **opslag-account** en voert u de URL naar het onderdeel in het veld **URL** . Geef een van de volgende manieren:

* **kopiëren:** Het onderdeel downloaden wordt gekopieerd naar door **Naar** het pad van de opgegeven bestemmingspad.
* **dezelfde:** Dezelfde methode gebruikt voor het **distribueren van downloaden** voor **Deploy uit pakket**.
* **zip:** Het onderdeel van het downloaden is uitgepakt aan door **Naar** het pad van de opgegeven bestemmingspad.

Als u wilt wijzigen van een onderdeel, selecteer het onderdeel en klik op de knop **bewerken** in het eigenschappenvenster **onderdelen** . Een dialoogvenster wordt geopend waarin u de onderdeeleigenschappen van het wijzigen. Klik op **OK** om op te slaan van het onderdeelwaarden.

Als u wilt verwijderen van een onderdeel, selecteer het onderdeel en klik op de knop **verwijderen** in het eigenschappenvenster **onderdelen** en klik vervolgens op **Ja** om de verwijdering te bevestigen.

Onderdelen worden in de weergegeven volgorde verwerkt. Gebruik de knoppen **Omhoog** en **Omlaag** om de volgorde te bepalen.

>[AZURE.NOTE] De functie server configuratie, is afhankelijk van onderdelen ook. De onderdelen die kunnen worden verwijderd of gewijzigd zonder de bijbehorende serverconfiguratie verwijderen. U wordt gevraagd over die tijdens een poging om deze onderdelen te wijzigen.

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>Voor foutopsporing in eigenschappen ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **Foutopsporing**. In dit dialoogvenster, u kunt in- of uitschakelen foutopsporing op afstand, evenals foutopsporing configuraties maken, zoals weergegeven in de volgende afbeelding.

![][ic719504]

Zie voor meer informatie over foutopsporing [Azure-toepassingen foutopsporing in Eclips][].

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>Eigenschappen van de eindpunten ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **eindpunten**. In dit dialoogvenster hebt u de mogelijkheid om te maken van een eindpunt, evenals bewerken of verwijderen van een eindpunt, zoals wordt weergegeven in de volgende afbeelding.

![][ic719505]

Een eindpunt toevoegen, klikt u op de knop **toevoegen** in het eigenschappenvenster van de **eindpunten** , en een dialoogvenster **Eindpunt toevoegen** wordt geopend.

![][ic710897]

Voer een naam voor het eindpunt, selecteer het type ( **invoer**, **interne**of **InstanceInput**) en geef de openbare en persoonlijke poort. Klik op **OK** om op te slaan van de nieuwe eindpunt waarden.

Afhankelijk van het type eindpunt, mag u bereikwaarden als volgt:

* De openbare poort kan een reeks poorten (bijvoorbeeld **2000-2010**) zijn en de privé poort is een vaste waarde voor een eindpunt invoer exemplaar.
* Voor een interne eindpunt, kan de openbare poort niet wordt gebruikt, en de privé poort een bereik, of u leeg of is ingesteld op een sterretje om aan te geven dat deze wordt automatisch ingesteld door Azure.
* Voor invoer eindpunten, de openbare poort kan alleen worden voor een vaste waarde en de privé poort kan een vaste waarde, of u leeg of ingesteld als een sterretje om aan te geven dat het automatisch door Azure is geconfigureerd.

Als u een enkel poortnummer gebruiken in plaats van een bereik wilt, laat u het tekstvak voor het einde van het bereik geen.

Voor poorten die zijn ingesteld op automatisch, als u nodig hebt om te bepalen welke poort werkelijk wordt gebruikt tijdens runtime, kunnen toepassingen gebruikmaken van de Azure-Service Runtime API, die wordt beschreven in het [com.microsoft.windowsazure.serviceruntime pakket samenvatting][].

Hoe exemplaar invoer eindpunten kunnen worden gebruikt om te helpen met voor foutopsporing in een implementatie meerdere exemplaren, raadpleegt u [voor foutopsporing in een exemplaar van de specifieke rol in een meerdere exemplaren-implementatie][].

Als u wilt een eindpunt wijzigen, selecteert u het eindpunt en klik op de knop **bewerken** in het eigenschappenvenster van de **eindpunten** . Een dialoogvenster wordt geopend waarmee u kunt de naam van het eindpunt, type en openbare en persoonlijke poorten wijzigen. Klik op **OK** om op te slaan de gewijzigde eindpunt waarden.

Als u wilt een eindpunt verwijderen, selecteert u het eindpunt en klik op de knop **verwijderen** in het eigenschappenvenster van de **eindpunten** en klik vervolgens op **Ja** om de verwijdering te bevestigen.

Voordat u goed configureren enkele van de functies (zoals cache, foutopsporing op afstand, sessie affiniteit of SSL offloading) ingeschakeld door de gebruiker op een rol, kan de toolkit speciale eindpunten die samen met de gebruiker gedefinieerde eindpunten worden weergegeven automatisch configureren. De toolkit wordt voorkomen dat de gebruiker bewerken of verwijderen zoals automatisch gegenereerde eindpunten zo lang maken als de bijbehorende functie is ingeschakeld.

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>Eigenschappen van de variabelen omgeving ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **Omgevingsvariabelen**. In dit dialoogvenster hebt u de mogelijkheid om te maken van een omgevingsvariabele, evenals wijzigen of verwijderen van een omgevingsvariabele, zoals wordt weergegeven in de volgende afbeelding.

![][ic719506]

Omgevingsvariabelen zijn beschikbaar voor uw opstartscript wanneer de rol wordt gestart.

>[AZURE.NOTE] Wanneer u opgeeft omgevingsvariabelen, houd er rekening mee dat uw implementatie wordt gepubliceerd naar een virtuele Windows-computer, zodat de omgevingsvariabelen van uw geldig voor een Windows-besturingssysteem zijn moeten.

Als een voorbeeld van een omgevingsvariabele wordt beschikbaar wanneer de rol wordt gestart, moet u een nieuwe omgevingsvariabele maken door te klikken op de knop **toevoegen** . Hieronder ziet u omgevingsvariabele **MyRoleVersion** wordt gemaakt en de waarde **1.0**toegewezen.

![][ic659268]

U kunt binnen uw jsp-code weergeven het gebruik van de waarde de `System.getenv` methode:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Resulteert in deze uitvoer wanneer uw toepassing wordt uitgevoerd:

![][ic552233]

Als u wilt een omgevingsvariabele wijzigen, selecteert u de omgevingsvariabele en klik op de knop **bewerken** in het eigenschappenvenster van de **Omgevingsvariabelen** . Een dialoogvenster wordt geopend waarin u de omgeving variabele eigenschappen wijzigen. Klik op **OK** om op te slaan de omgeving variabele waarden.

Als u wilt een omgevingsvariabele verwijderen, selecteert u de omgevingsvariabele en klik op de knop **verwijderen** in het eigenschappenvenster van de **Omgevingsvariabelen** en klik vervolgens op **Ja** om de verwijdering te bevestigen.

Voordat u goed configureren enkele van de functies (zoals serverconfiguratie, foutopsporing op afstand of lokale opslag) ingeschakeld door de gebruiker op een rol, kan de toolkit speciale omgevingsvariabelen die wordt weergegeven samen met de gebruiker gedefinieerde omgevingsvariabelen automatisch configureren. De toolkit wordt voorkomen dat de gebruiker bewerken of verwijderen zoals automatisch gegenereerde omgevingsvariabelen zo lang maken als de bijbehorende functie is ingeschakeld.

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Taakverdeling / sessie affiniteit (a.k.a "Plaktoetsen sessies")-eigenschappen ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **Taakverdeling**. In dit dialoogvenster hebt u de mogelijkheid om te in- of uitschakelen van sessie affiniteit, zoals wordt weergegeven in de volgende afbeelding.

![][ic719492]

Zie voor meer informatie kunt [Sessie affiniteit][]. Noteer ook de werking van deze functie in de context van SSL offloading, zoals beschreven op [SSL Offloading][].

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>Eigenschappen van de lokale opslag ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **Lokale opslag**. In dit dialoogvenster hebt u de mogelijkheid om te maken, wijzigen of verwijderen van tijdelijke lokale opslag voor de virtuele machine die uw toepassing wordt uitgevoerd. Specifieke waarden kunnen worden ingesteld voor de grootte van de lokale opslag, alsmede of de inhoud behouden blijven wanneer de rol wordt herhaald, zoals wordt weergegeven in de volgende afbeelding.

![][ic719508]

U kunt desgewenst ook een omgevingsvariabele die met de lokale opslag overeenkomt opgeven.

Standaard is alles wat u in Azure implementeren geplaatst (en bestanden) in de map **approot** van het exemplaar van de rol. Terwijl de meest eenvoudige implementaties wordt is passend ook nadat ritsen, de ruimte die is toegewezen aan de map **approot** beperkt en niet goed gedefinieerde (minder dan 1 GB een redelijk vuistregel). Daarom dat Azure wordt voldoende schijfruimte toegewezen voor grotere implementaties die mogelijk niet in de map **approot** past, zodat u moet een lokale opslag resource instellen via het dialoogvenster **Lokale opslag** . Zie voor een eenvoudige manier u dit wilt doen, [Grote implementaties implementeren][].

U kunt eenvoudig verwijzen naar de resource opslag van opstartscripts (bijvoorbeeld uw **startup.cmd**) met de omgevingsvariabele automatisch door de toolkit Eclips die bij de resource horen, zoals wordt weergegeven in het dialoogvenster **Lokale opslag** . Die omgevingsvariabele bevat het volledige pad van de lokale resource die u hebt geconfigureerd op het moment dat uw opstartscript wordt uitgevoerd. 

Als u wilt wijzigen van een resource lokale opslag, selecteert u de resource lokale opslag en klik op de knop **bewerken** in het eigenschappenvenster van de **Lokale opslag** . Een dialoogvenster wordt geopend waarin u de eigenschappen van de resource lokale opslag wijzigen. Klik op **OK** om op te slaan van de bronwaarden lokale opslag.

Als u wilt verwijderen van een resource lokale opslag, selecteert u de resource lokale opslag en klik op de knop **verwijderen** in het eigenschappenvenster van de **Lokale opslag** en klik vervolgens op **Ja** om de verwijdering te bevestigen.

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>Configuratie-servereigenschappen ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **Serverconfiguratie**. In dit dialoogvenster u hebt de mogelijkheid om te toevoegen, verwijderen en wijzigen van de JDK en Java toepassingsserver die worden gebruikt door de implementatie, evenals toevoegen of verwijderen van de toepassingen (zoals WAR-, oppervlak- of OOR bestanden) die wordt gebruikt door uw implementatie.

### <a name="jdk-configuration"></a>JDK configuratie ###

In dit dialoogvenster kunt u opgeven van het pakket JDK wilt gebruiken voor uw implementatie. Als u Eclips op Windows gebruikt, kunt u het pakket JDK lokaal gebruiken wanneer u zich in de Azure emulator en u de optie voor het implementeren van die lokale installatie naar Azure hebt. De instelling van de JDK emulator is niet van toepassing op niet-Windows-besturingssystemen en u niet de lokaal geïnstalleerde JDK implementeren gezien het feit niet compatibel is met Windows is. Ongeacht het besturingssysteem dat u gebruikt, kunt u echter altijd kiezen tussen de 3e JDK-pakketten van derden implementeren naar Azure of wijst u uw eigen JDK compatibel is met Windows-pakket van een alternatieve downloadlocatie.

Hier volgt een voorbeeld van hoe u een JDK in Windows opgeven kunt:

![][ic780647]

Als u Eclips op Windows gebruikt, kunt u een JDK voor gebruik met de emulator berekeningscluster; Controleer of **dat gebruik de JDK uit dit bestandspad voor het testen van lokaal** is ingeschakeld in de sectie **Emulator implementatie** hiervoor. Geef het lokale pad naar uw JDK; u kunt naar andere JDK Bladeren als degene die u wilt gebruiken niet automatisch is geselecteerd. U hebt ook de mogelijkheid om te implementeren van uw JDK naar uw Azure cloudservice; hiervoor, selecteert u de optie **Deploy mijn lokale JDK (auto-uploaden naar cloudopslag)** in de sectie **Cloud-implementatie** .

Opmerking: Op niet-Windows-besturingssystemen, de **Emulator implementatie** -instellingen en de optie **Deploy mijn lokale JDK** zijn niet beschikbaar. Het volgende voorbeeld ziet u een JDK geven op een Mac of andere ondersteund niet-Windows-besturingssysteem:

![][ic789643]

Ongeacht het besturingssysteem dat u zich op, moet u de volgende twee **Cloud-implementatie** -opties voor de bron- en type uw JDK-pakket hebt:

* **Een beschikbaar op Azure 3e partijen JDK pakket implementeren** 
* **Implementeren van het downloaden van een aangepaste** 

Als u de optie **Deploy een 3e derden JDK pakket verkrijgbaar via Azure** gebruikt:

1. Schakel het selectievakje benoemde **Deploy een 3e derden JDK pakket verkrijgbaar via Azure**.
1. Selecteer in de vervolgkeuzelijst het 3e JDK pakket van derden die beschikbaar is op Azure.
1. Uw tabblad **JDK** ziet er ongeveer als volgt te werk in Windows:  ![][ic780648] 
    en deze ziet er ongeveer als volgt te werk op Mac OS of andere niet-Windows-besturingssystemen worden ondersteund: ![][ic789643]
1. Klik op **OK** om uw wijzigingen op te slaan.
1. Wanneer u wordt gevraagd naar accepteer de licentieovereenkomst uit de 3e JDK Pakketprovider, moet u de licentievoorwaarden bekijken. Als dat u akkoord met de voorwaarden, klik op **Ja** om het dialoogvenster **accepteren licentieovereenkomst** te sluiten.
    Houd er rekening mee dat de onderliggende logica voor welke items worden weergegeven in de vervolgkeuzelijst voor de optie **Deploy een 3e derden JDK pakket verkrijgbaar via Azure** kan zo worden aangepast. Klik op de koppeling **aanpassen** om aan te passen van items uit, klik in het dialoogvenster **JDK** . Hiermee wordt het eigenschappenblad **JDK** sluiten en open het bestand **componentsets.xml** in Eclips, waarin u vervolgens naar wens kunt aanpassen. Documentatie voor **componentsets.xml** is opgenomen in het bestand **componentsets.xml** zelf.

Als u de optie **Deploy een JDK van het downloaden van een aangepaste** gebruikt:

1. Maken van een ZIP van uw JDK-installatiemap, ervoor te zorgen dat het knooppunt directory zelf is de subtitel van de ZIP-structuur en niet de inhoud ervan. Let op de naam van de map, zoals u deze later nodig hebt en houd er rekening mee dat deze installatie JDK wordt geïmplementeerd in een virtuele Windows-computer.
1. Upload de ZIP in uw account Azure opslag als een blob. U kunt doen om deze met een extern beschikbare hulpmiddel voor uploaden BLOB's met Azure storage. Het wordt aanbevolen een privé blob gebruiken. Let op de blob-URL van de ZIP-inhoud.
1. Schakel het selectievakje **Deploy een JDK van het downloaden van een aangepaste**naam.
    Als u downloaden van uw account Azure opslag, selecteer de opslag-account in de vervolgkeuzelijst **opslag account** lijst wilt (u kunt de koppeling **Accounts** als u wilt wijzigen wat staat er in de lijst op), die gedeeltelijk in het veld **URL** invullen en vult u het resterende deel van de URL. Als u niet Azure opslagruimte gebruiken wilt, selecteer **(geen)** in de vervolgkeuzelijst **opslag-account** en voert u de URL naar uw JDK downloaden in het veld **URL** . Als Azure opslag gebruikt, is blob namen in de URL moeten kleine letters.
1. Zorg ervoor dat het tekstvak **JAVA_HOME** naar de naam van de juiste map verwijst. Standaard deze verwijst naar de naam van de dezelfde JDK map als de waarde die u hebt gekozen voor uw lokaal gebruik. Maar als de map in het ZIP heeft een andere naam (bijvoorbeeld vanwege via een andere versie), de naam van de map in het tekstvak **JAVA_HOME** dienovereenkomstig bijwerken, omdat deze instelling wordt gebruikt in de cloud (niet in de emulator berekeningscluster).
1. Klik op **OK** om uw wijzigingen op te slaan.

Dat is. Nu, als u voor de cloud samenstelt, zult u grootte van het pakket wordt niet veel groter, de opbouwen moet meestal duren minder tijd en de implementatie zelf wanneer u naar de cloud publiceert moet ook aandacht minder tijd. Houd er rekening mee dat de opties **Deploy mijn lokale JDK (auto-uploaden naar cloudopslag)** of **een JDK van het downloaden van een aangepaste Deploy** van kracht zijn alleen wanneer de toepassing wordt geïmplementeerd in de cloud. Ze hebben geen invloed op de functionaliteit van de berekeningscluster emulator; de lokale versie van de onderdelen worden nog steeds worden gebruikt wanneer u de emulator berekeningscluster implementeert. 

### <a name="server-configuration"></a>Serverconfiguratie ###

Hier volgt een voorbeeld van hoe u een toepassingsserver kunt opgeven.

![][ic796926]

Controleer of het selectievakje **Deploy een server van dit type** is geselecteerd, en kies vervolgens het type toepassingsserver die u wilt gebruiken.

Om aan te geven van een server wilt gebruiken voor de cloud-implementatie, kunt u profiteren van de volgende opties:

1. **Deploy een 3e derden server beschikbaar op Azure** - dit is met name van toepassing in ontwikkelaar/testen scenario's waarbij implementatie en eenvoudig een prioriteit en de server is geen een aangepaste configuratie vereist. Of wanneer u wilt een van deze servers als uitgangspunt gebruiken, maar u opnemen aanpassing van de juiste server stappen in de implementatie van opstarten logica.
1. **Deploy van het downloaden van een aangepaste** - dit is met name van toepassing in productie scenario's als er een speciaal voorbereid en geconfigureerde server die u wilt gebruiken in de cloud.
1. **Deploy mijn lokale server-installatie** - dit is met name van toepassing in als de installatie van uw lokale server nog is aangepast geconfigureerd voor gebruik. Als u deze optie kiest, moet u ook uw lokale server pad opgeven in het tekstvak **pad van de lokale server** .

Als u de optie **Deploy een 3e derden server beschikbaar op Azure** gebruikt:

1. Schakel het selectievakje benoemde **Deploy een 3e derden server beschikbaar op Azure**.
1. Selecteer de gewenste server-software voor gebruik met uw implementatie in de cloud in de vervolgkeuzelijst. Opmerking, als u al een type server gebruik van eerder hebt opgegeven, u worden beperkt tot een cloud-server die zich in dezelfde familie als dat servertype kiezen. Maar als u een servertype niet hebt gekozen, kunt u kiezen uit een van de servers die momenteel beschikbaar op Azure zijn en het servertype wordt automatisch voor u zijn geselecteerd.
1. Klik op **OK** om uw wijzigingen op te slaan.

Als de optie **Deploy van het downloaden van een aangepaste** gebruikt:

1. Zorg ervoor dat u al een servertype op basis van de voorgaande stappen hebt geselecteerd. Hiermee worden vermeld de invoegtoepassing voor het implementeren van de server van uw aangepaste downloaden, zoals deze afkomstig van dezelfde familie terwijl u typt geselecteerde server zijn moet.
1. Schakel het selectievakje **Deploy van het downloaden van een aangepaste**naam.
    Als u downloaden van uw account Azure opslag wilt, downloaden Selecteer de opslag-account in de vervolgkeuzelijst **opslag account** lijst (u kunt de koppeling **Accounts** als u wilt wijzigen wat staat er in de lijst op), die gedeeltelijk in het veld **URL** invullen en vult u het resterende deel van de URL naar uw server ZIP (bij het gebruik van Azure opslag, blob namen in de URL moet kleine letters) Als u niet Azure opslagruimte gebruiken wilt, selecteer **(geen)** in de vervolgkeuzelijst **opslag-account** en voert u de URL naar uw server downloaden ZIP in het veld **URL** . De POSTCODES bevat een onderliggende map dat staat voor uw toepassing server-installatiemap. Bijvoorbeeld als u gebruikmaakt van een zip voor zou Apache Tomcat 7.0.35, in het zip de onderliggende map dat staat voor de installatiemap, zoals **apache-tomcat-7.0.35**. 
1. De waarde voor de omgevingsvariabele basismap opgeven. De waarde die wordt gebruikt voor de server lokale toepassing, indien van toepassing wordt standaard, maar u kunt een andere waarde opgeven als uw toepassingsserver cloud van uw lokale toepassingsserver verschilt. Echter u nodig hebt om ervoor te zorgen dat uw toepassingsserver cloud van dezelfde familie als de server is type eerder hebt geselecteerd.
    Als u uw zip cloud-toepassing server in de toekomst bijwerkt, kunt u handmatig de instelling van de basismap, wijzigen of, zodat deze opnieuw overeenkomen met uw lokale instellen (als u uw lokale toepassingsserver te gewijzigd).
1. Klik op **OK** om uw wijzigingen op te slaan.

De onderliggende logica voor welke items worden weergegeven in het tabblad **Server** van de pagina van de eigenschap **Serverconfiguratie** kan worden aangepast. Dit is een geavanceerde functie die u moet mogelijk als uw behoeften voldoet de standaardwaarden reiken of als u wilt toevoegen van andere servers bevinden. Klik op de koppeling **aanpassen** als u wilt aanpassen van de logica, klik in het dialoogvenster **Server** . Hiermee wordt het eigenschappenblad **Serverconfiguratie** te sluiten en open het bestand **componentsets.xml** in Eclips, waarin u kunt vervolgens zo nodig voor het uitbreiden van de server configuratiesjabloon wijzigen. Documentatie voor **componentsets.xml** is opgenomen in het bestand **componentsets.xml** zelf.

Als u de optie **Deploy mijn lokale server (auto-uploaden naar cloudopslag)** gebruikt:

1. Schakel het selectievakje benoemde **Deploy mijn lokale server (auto-uploaden naar cloudopslag)**.
1. Selecteer met de vervolgkeuzelijst **opslag-account** **(automatisch)**. Als u **(auto)** hier opgeeft, wordt de toolkit Eclips hetzelfde account opslagruimte voor uw server gebruikt als geselecteerd voor de implementatie in het dialoogvenster **publiceren op Azure** .
1. Klik op **OK** om uw wijzigingen op te slaan.

Selecteer in het tekstvak **lokale serverpad** installatiepad van een server op uw computer als een van de volgende voorwaarden voldaan wordt:

* U wilt testen van uw implementatie in de emulator (geldt voor Windows alleen).
* U wilt uw lokaal geïnstalleerde server implementeren in de cloud.
* U wilt gebruiken het downloaden van een aangepaste server van uw eigen in de cloud, waarin hoofdletters/kleine letters, ook Controleer of dat de optie **Deploy mijn lokale server (auto-uploaden naar cloudopslag)** hierboven is ingeschakeld.

Als geen van de voorgaande opties voor uw situatie toepassen, is de instelling van de lokale server is optioneel.

### <a name="applications-configuration"></a>Configuratie van toepassingen ###

Hier volgt een voorbeeld van hoe u een toepassing kunt opgeven.

![][ic719512]

Klik op **toevoegen** aan een andere toepassing toevoegen of **verwijderen** om een toepassing te verwijderen. Voor efficiency doeleinden, als u downloaden van een gebruiken voor de bron van een toepassing wilt wanneer u de implementatie in de cloud, gebruikt u de [Eigenschappen van onderdelen](#components_properties) om op te geven van een URL, opslag-account, enzovoort. 

Beginnen met de April 2014-versie, worden uw toepassingen automatisch geüpload in hetzelfde opslag-account (onder de container **eclipsedeploy** ) als de relatie die is geselecteerd om uw implementatie. De logica opstarten van uw implementatie bevat een stap die u eerst die toepassingen is gedownload vanaf het betreffende account opslag. Dit betekent dat u mogelijk een upgrade uitvoert uw toepassingen in uw implementatie zonder opnieuw opbouwen en implementeren van het volledige pakket, door te uploaden handmatig nieuwere versies van de toepassing rechtstreeks in dat opslag-account (via de portal van Azure bijvoorbeeld), worden vervangen door de bestanden WAR oorspronkelijk er geüpload door de toolkit. Klik net initiëren de hergebruik van deze rol overal met behulp van de Azure management portal opnieuw of via de opdrachtregel hulpprogramma's. (Rol hergebruik rechtstreeks vanuit binnen de toolkit Eclips activeert is momenteel niet ondersteund.)

### <a name="notes-about-server-configuration"></a>Opmerkingen over serverconfiguratie ###

Via de pagina van de eigenschap **Serverconfiguratie** wijzigingen worden doorgevoerd in de `<component>` elementen van het bestand Package.xml heeft.

Wanneer u het **automatisch uploaden...** gebruiken of **Deploy uit downloaden...** opties voor de JDK of toepassingsserver en u voor de cloud (niet de emulator berekeningscluster samenstelt) en u met het netwerk verbonden bent, ziet u berichten zoals de volgende opties in de uitvoer Console maken zoals de Ant opbouwfunctie voor wordt gecontroleerd met de beschikbaarheid van het downloaden:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Als u de optie **Deploy uit downloaden...** hebt geselecteerd, wordt de volgende waarschuwing kan worden weergegeven, maar het bouwen blijft:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Deze waarschuwing is de enige aanwijzing dat het downloaden van beschikbaarheid nog niet is geverifieerd. Dus als een implementatie is mislukt in de cloud om welke reden, controleert u of u deze waarschuwing ontvangen.

Als u wilt uitschakelen van de download-verificatie (bijvoorbeeld als u denkt dat deze onnodig het bouwen vertraagd), stelt u de `verifydownloads` kenmerk naar `false` in de `<windowsazurepackage>` element van Package.xml heeft: 

`<windowsazurepackage verifydownloads="false" ...>` 

Als u de optie **automatisch uploaden...** , klikt u vervolgens hebt geselecteerd in het consolevenster ziet u de opbouwen berichten rapportage van de voortgang van de upload elke 5 seconden, wanneer het uploaden nodig is.

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL offloading eigenschappen ###

Open het contextmenu voor de rol in de Eclips Projectverkenner deelvenster **Azure**op en klik vervolgens op **SSL Offloading**. 

![][ic719481]

In dit dialoogvenster kunt u SSL offloading, zodat u eenvoudig Hypertext Transfer Protocol Secure (HTTPS)-ondersteuning in uw implementatie Java op Azure, zonder dat u SSL configureren in uw Java-toepassingsserver inschakelen. Zie [SSL Offloading][] en [hoe u gebruik SSL Offloading][]voor meer informatie.

## <a name="see-also"></a>Zie ook ##

[Azure Toolkit voor Eclips][]

[Installatie van de Azure Toolkit voor Eclips][]

[Maken van een toepassing van de wereld Hallo voor Azure in Eclips][]

[Azure Projecteigenschappen][]

[Lijst met accounts Azure opslag][]

Zie voor meer informatie over het gebruik van Azure met Java [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Projecteigenschappen]: http://go.microsoft.com/fwlink/?LinkID=699524
[Lijst met accounts Azure opslag]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.Microsoft.windowsazure.serviceruntime pakket samenvatting]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Maken van een toepassing van de wereld Hallo voor Azure in Eclips]: http://go.microsoft.com/fwlink/?LinkID=699533
[Voor foutopsporing in een exemplaar van de specifieke rol in een meerdere exemplaren-implementatie]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Azure-toepassingen in Eclips foutopsporing]: http://go.microsoft.com/fwlink/?LinkID=699535
[Grote implementaties implementeren]: http://go.microsoft.com/fwlink/?LinkID=699536
[Het gebruik van Caching van samen bevindt]: http://go.microsoft.com/fwlink/?LinkID=699542
[Het gebruik van SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installatie van de Azure Toolkit voor Eclips]: http://go.microsoft.com/fwlink/?LinkId=699546
[Sessie affiniteit]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-Offloading]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png
