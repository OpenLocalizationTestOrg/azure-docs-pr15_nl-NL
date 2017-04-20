<properties
    pageTitle="Releaseopmerkingen voor Azure BizTalk Services | Microsoft Azure BizTalk-Services"
    description="Lijsten de bekende problemen voor Azure BizTalk-Services" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Releaseopmerkingen voor Azure BizTalk-Services

De releaseopmerkingen voor de Services van Microsoft Azure BizTalk bevatten de bekende problemen in deze release.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Wat is er nieuw in de update November van BizTalk-Services
* Versleuteling in rust kan worden ingeschakeld in de Portal van BizTalk-Services. Zie [versleuteling in rust in BizTalk Services-Portal inschakelen](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Geschiedenis van updates

### <a name="october-update"></a>Oktober bijwerken

* Organisatie-accounts voorwaarden wordt voldaan:  
 * **Scenario**: U een BizTalk Service-implementatie via een Microsoft-account hebt geregistreerd (zoals user@live.com). In dit scenario kunnen alleen Microsoft-Account gebruikers de BizTalk-Service met behulp van de portal BizTalk Services beheren. Een organisatieaccount kan niet worden gebruikt.  
 * **Scenario**: U een BizTalk Service-implementatie met een organisatie-account van een Azure Active Directory geregistreerd (zoals user@fabrikam.com of user@contoso.com). In dit scenario kunnen alleen Azure Active Directory gebruikers binnen dezelfde organisatie de BizTalk-Service met behulp van de portal BizTalk Services beheren. Een Microsoft-account kan niet worden gebruikt.  
* Wanneer u een BizTalk-Service in de portal van Azure klassieke maakt, wordt u automatisch geregistreerd in de Portal van BizTalk-Services.
 * **Scenario**: U zich aanmelden bij de portal voor Azure klassieke maken van een BizTalk Service en selecteer vervolgens **beheren** voor de eerste keer. Wanneer de portal BizTalk Services wordt geopend, wordt de BizTalk-Service automatisch wordt geregistreerd en gereed is voor uw implementaties.  
 Zie [registreren en bijwerken van een BizTalk-Service-implementatie op de BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>14 augustus bijwerken
* Overeenkomst en brug ontkoppeling – handelsdag partner overeenkomsten en bruggen zijn nu ontkoppelde in de Portal van BizTalk-Services. Nu u overeenkomsten en bruggen afzonderlijk en gedurende runtime bruggen omzetten in een overeenkomst op basis van de waarden in het bericht bewerken. Zie [Overeenkomsten maken in Azure BizTalk-Services](https://msdn.microsoft.com/library/azure/hh689908.aspx), [maken de brug van een bewerken met behulp van BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn793986.aspx), [maken de brug van een AS2 met behulp van BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn793993.aspx), en [hoe bruggen los overeenkomsten gedurende runtime?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* De optie voor het maken van sjablonen voor overeenkomsten is gestopt.  
* U kunt nu verschillende scheidingsteken sets voor elk schema opgeven voor de overeenkomst verzenden aan de clientzijde. Deze configuratie is opgegeven onder instellingen voor verzenden kant overeenkomst protocol. Zie voor meer informatie [maken een X12 overeenkomst in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx) en [maken een overeenkomst EDIFACT in Azure BizTalk-Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Twee nieuwe entiteiten worden ook toegevoegd aan de TPM OM-API voor hetzelfde doel. Zie [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) en [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standaard XSD constructies, waaronder kleurencombinaties typen, worden nu ondersteund. Zie [standaard XSD gebruik wordt gemaakt in uw kaarten](https://msdn.microsoft.com/library/azure/dn793987.aspx) en [Gebruik kleurencombinaties typen in de toewijzing scenario's en voorbeelden](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 ondersteunt nieuwe Microfoon algoritmen voor het ondertekenen van berichten en versleutelingsalgoritmen voor nieuwe. Zie [een overeenkomst AS2 in Azure BizTalk-Services te maken](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Bekende problemen

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Verbindingsproblemen nadat BizTalk Services bijwerken Portal

  Als u de BizTalk Services-Portal openen terwijl BizTalk Services wordt bijgewerkt als u wilt implementeren wijzigingen in de service nodig hebt, kunt u mogelijk verbindingsproblemen met de Portal van de Services BizTalk betrokken.  
  Als een tijdelijke oplossing kan u de browser opnieuw starten, de browsercache verwijderen of starten van de portal in privé-modus.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE kan het onderdeel niet vinden als u klikt op een fout of waarschuwing in een project BizTalk-Services
Installeer Visual Studio 2012 Update 3 RC 1 u kunt het probleem oplossen.  

### <a name="custom-binding-project-reference"></a>Naslaggids voor het project van aangepaste binding
Houd rekening met de volgende situaties met een project BizTalk-Services in een Visual Studio-oplossing:  
* In dezelfde oplossing Visual Studio, moet u er een project BizTalk-Services en een aangepaste binding-project is. Het project BizTalk Service heeft een verwijzing naar dit projectbestand aangepaste binding.
* Het project BizTalk Service heeft een verwijzing naar een aangepaste binding/gedrag DLL.

U maken' ' de oplossing in Visual Studio is. Vervolgens u 'Opnieuw opbouwen' of 'Schonen' de oplossing. Daarna wanneer u opnieuw opbouwen of nogmaals schonen het volgende foutbericht weergegeven:  
  Kan bestand niet kopiëren <Path to DLL> naar "bin\Debug\FileName.dll". Het proces heeft geen toegang tot het bestand 'bin\Debug\FileName.dll' omdat deze door een ander proces wordt gebruikt.  

#### <a name="workaround"></a>Tijdelijke oplossing
* Als [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) is geïnstalleerd, hebt u de volgende twee opties:

  * Start opnieuw Visual Studio, of

  * Start opnieuw op de oplossing. Voer alleen een opbouwen op de oplossing.  

* Als [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) niet is geïnstalleerd, Taakbeheer openen, klikt u op het tabblad processen, klik op het proces MSBuild.exe en klik vervolgens op de knop proces beëindigen.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Zoekresultaten omleiden naar een BasicHttpRelay eindpunten wordt niet ondersteund van bruggen en BizTalk Services-Portal als niet-afdrukbare tekens zijn gepromoveerd tot HTTP-headers

Als u niet-afdrukbare tekens als onderdeel van gepromoveerde eigenschappen voor berichten gebruikt, kunnen niet die berichten worden gerouteerd naar relay bestemmingen die de binding BasicHttpRelay gebruiken. Bovendien de gepromoveerde eigenschappen die beschikbaar zijn als onderdeel van bijhouden, worden URL-codering voor BLOB's en niet gecodeerd voor bestemmingen.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN wordt asynchroon verzonden, zelfs als de optie verzenden asynchrone MDN uitgeschakeld is  
Houd rekening met dit scenario – als u het selectievakje voor het **verzenden van asynchrone MDN** selecteert en een URL als u wilt verzenden, het asynchrone MDN opgeeft tot, en schakel het selectievakje voor het **verzenden van asynchrone MDN** nogmaals de MDN is nog steeds verzonden naar de opgegeven URL, hoewel de optie voor het verzenden van asynchrone MDNs niet is ingeschakeld.  
Als een tijdelijke oplossing, moet u schakelt u de opgegeven URL voordat u het selectievakje voor het **verzenden van asynchrone MDN** uitschakelt en vervolgens implementeren de overeenkomst AS2.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Spaties buiten een geldige interchange ertoe leiden dat een leeg bericht worden verzonden naar het eindpunt onderbreken  
Als er whitespaces buiten een IEA segment, wordt de disassembler wordt deze verwerkt als einde van de huidige interchange en kijkt naar de volgende set whitespaces als een volgende bericht. Aangezien dit geen geldige interchange is, kunt u mogelijk dat een bericht is voltooid wordt verzonden naar de bestemming van de route en één lege bericht wordt verzonden het eindpunt onderbreking zien.  
### <a name="tracking-in-biztalk-services-portal"></a>Bijhouden in BizTalk Services-Portal  
Bijhouden gebeurtenissen worden vastgelegd snel aan de verwerking van het bericht bewerken en eventuele correlatie. Als een bericht niet buiten het podium Protocol, kunnen bijhouden wordt weergegeven als geslaagd. In dit geval verwijzen naar de sectie LOG onder de kolom **Details** in **bijhouden** voor meer informatie over fout.
De X12 instellingen voor verzenden en ontvangen ([maken een X12 overeenkomst in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx)) vindt u informatie over het podium Protocol.  

### <a name="update-agreement"></a>Overeenkomst bijwerken  
Met behulp van de Portal van BizTalk-Services kunt u de kwalificatie van een identiteit wijzigen wanneer een overeenkomst is geconfigureerd. Dit kan resulteren in is inconsistent eigenschappen. Er is bijvoorbeeld een overeenkomst met ZZ:1234567 en ZZ:7654321 de kwalificatie. In de profielinstellingen BizTalk Services-Portal wijzigt u ZZ:1234567 om te worden 01:ChangedValue. U opent de overeenkomst en 01:ChangedValue wordt weergegeven in plaats van ZZ:1234567.
Als u wilt de kwalificatie van een identiteit wijzigen, verwijderen van de overeenkomst, **identiteiten** -update in het Partnerprofiel en de overeenkomst vervolgens opnieuw te maken.  
> AZURE. Waarschuwing over dit probleem invloed op X12 en AS2.  

### <a name="as2-attachments"></a>Bijlagen AS2  
Bijlagen voor AS2 berichten worden niet ondersteund in verzenden of ontvangen. Name bijlagen worden genegeerd en wordt de hoofdtekst van het bericht wordt verwerkt als een gewone AS2-bericht.  
### <a name="resources-remembering-path"></a>Bronnen: Onthouden pad  
Bij het toevoegen van **Resources**, kan het pad dat eerder hebt gebruikt voor het toevoegen van een resource niet in het dialoogvenster onthouden. Als u wilt onthouden het pad eerder hebt gebruikt, kunt u het toevoegen van de website BizTalk Services-Portal op **Vertrouwde websites** in Internet Explorer.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Als u de naam van de naam van de entiteit van een brug wijzigen en het project sluiten zonder wijzigingen worden opgeslagen, resulteert vervolgens opnieuw opent de entity in een fout
Houd rekening met een scenario in de volgende volgorde:  
* Een brug (bijvoorbeeld een XML-One-Way brug) toevoegen aan een project BizTalk-Service  

* De naam van de brug wijzigen door een waarde voor de eigenschap entiteitsnaam te geven. Hiermee wijzigt u het bijbehorende .bridgeconfig-bestand met de naam die u hebt opgegeven.  

* Sluit het .bcs-bestand (door het sluiten van het tabblad in Visual Studio) zonder de wijzigingen op te slaan.  

* Open het bestand .bcs opnieuw vanuit de Solution Explorer.  
U ziet dat terwijl het bijbehorende .bridgeconfig-bestand de nieuwe naam die u hebt opgegeven bevat, de naam van de entiteit op het ontwerpoppervlak nog steeds de oude naam. Als u probeert te openen van de configuratie van de brug door te dubbelklikken op de component brug, krijgt u het volgende foutbericht weergegeven:  
  '<old name>'Entiteit hoort bestand'<old name>.bridgeconfig' niet bestaat  
Verandert nadat u de naam van de entiteiten in een project BizTalk Service kunt u voorkomen dat in dit scenario, moet dat u opslaan.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk-Service automatisch samengesteld is zelfs als een onderdeel is uitgesloten uit een project Visual Studio
Houd rekening met een scenario waarin u een onderdeel (bijvoorbeeld een XSD-bestand) toevoegen aan een project BizTalk Service, dat onderdeel opnemen in de configuratie van brug (bijvoorbeeld door op te geven deze als een bericht verzoek) en deze vervolgens uitsluiten van het project Visual Studio. In dat geval krijgt het samenstellen van het project niet elke fout bij het zo lang maken als de verwijderde onderdeel beschikbaar op de schijf op dezelfde locatie is vanaf waar deze is opgenomen in de Visual Studio-project.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>Het project BizTalk Service wordt niet gecontroleerd voor het schema beschikbaarheid tijdens het configureren van de bruggen
Klik in een project BizTalk Service als een schema die is toegevoegd aan het project importeert in een ander schema, wordt het project BizTalk Service niet gecontroleerd of het geïmporteerde schema wordt toegevoegd aan het project. Als u probeert te maken, zoals een project, er opbouwfouten.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Het antwoordbericht voor een XML-verzoek beantwoorden brug is altijd van tekenset UTF-8
Voor deze release, is altijd de tekenset van het antwoordbericht van een XML-verzoek beantwoorden brug ingesteld op UTF-8.
### <a name="user-defined-datatypes"></a>Aangepaste gegevenstypen
De adapters BizTalk Adapter Pack binnen de functie BizTalk Adapter Service kunnen maken met het gebruik van de gebruiker gedefinieerde gegevenstypen voor adapter bewerkingen.
Wanneer u met de gebruiker gedefinieerde gegevenstypen, de bestanden (dll) kopiëren naar station: \Program Files\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\ of naar de globale constructie Cache (GAC) op de server met de BizTalk Adapter-service. Het volgende foutbericht mogelijk anders weergegeven op de client:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Het verdient gebruik GACUtil.exe om een bestand in de Cache van de globale constructie installeren. GACUtil.exe documenten met dit hulpmiddel en de Visual Studio-opdrachtregelopties gebruiken.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Opnieuw starten van de website van BizTalk Adapter Service
Installatie van de **BizTalk Adapter Service Runtime** * Hiermee maakt u de * *BizTalk Adapter Service* * website in IIS met de * *BAService* * toepassing.* *BAService** relay binding toepassing intern gebruikt voor het uitbreiden van het bereik van on-premises service-eindpunt in de cloud. Een service die worden gehost on-premises, wordt het bijbehorende relay eindpunt op Service bevinden worden geregistreerd alleen wanneer de on-premises implementatie-service wordt gestart.  

Als u stoppen en start u een toepassing, wordt de configuratie voor het automatisch starten van een toepassing niet uitgevoerd. Dus wanneer **BAService** is gestopt, moet u altijd opnieuw de **BizTalk Adapter Service** -website in plaats daarvan. Geen starten en stoppen de toepassing **BAService** .
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Speciale tekens mogen niet worden gebruikt voor het adres en entiteit namen van LOB-onderdelen
U moet de speciale tekens niet gebruiken voor het adres en entiteit namen van LOB-onderdelen. Als u dit doet, wordt er een fout tijdens het implementeren van het project BizTalk-Service. Voor bepaalde tekens zoals '%', de website BizTalk Adapter Service mogelijk gaat u naar een gestopt staat en u moet deze handmatig te starten.
### <a name="test-map-with-get-context-property"></a>Test-Map met de eigenschap Context ophalen
Als een transformatie een **Eigenschap Context krijgen** kaart betrekking heeft bevat, mislukt **Test kaart** . Als tijdelijke oplossing, kunt u de **Eigenschap Context krijgen** kaart bewerking vervangen door een tekenreeks Concatenate kaart bewerking met POP gegevens. Hiermee wordt het doelschema vullen en toestaan dat u andere functionaliteit transformeren testen.
### <a name="test-map-property-does-not-display"></a>De eigenschap toewijzen testen worden niet weergegeven
De eigenschappen van de **Test-toewijzing** kunnen niet worden weergegeven in Visual Studio. Dit kan gebeuren wanneer het venster **Eigenschappen** en de **Oplossingverkenner** -venster niet tegelijk zijn gekoppeld. U lost dit op door dokken de **Eigenschappen** en de **Oplossing Explorer** -vensters.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Vervolgkeuzelijst DateTime-notatie grijs wordt weergegeven
Wanneer een bewerking DateTime opnieuw indelen kaart is toegevoegd aan het ontwerpvlak en hebt geconfigureerd, kan de vervolgkeuzelijst indeling grijs worden. Dit kan gebeuren als de computer weergave **Normaal-125%** of **groter – 150%**is ingesteld. Om op te lossen, stelt u de weergave op **kleinere – 100% (standaard)** volgens de onderstaande stappen:  
1. Open het **Configuratiescherm** en klik op **Vormgeving en persoonlijke instellingen**.
2. Klik op **weergeven**.
3. Klik op **kleinere – 100% (standaard)** en klikt u op **toepassen**.

De vervolgkeuzelijst **notatie** moet nu werkt zoals verwacht.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Dubbele overeenkomsten in de Portal BizTalk diensten
Bekijk de volgende scenario:
1. Maak een overeenkomst de handel Partner Management OM-API gebruiken.
2. Open de overeenkomst in de Portal van de Services BizTalk in twee andere tabbladen.
3. De overeenkomst uit beide de tabbladen implementeren.
4. Hierdoor beide de overeenkomsten met dubbele items in de Portal van de Services BizTalk resultaat geïmplementeerd

**Tijdelijke oplossing**. Open een van de dubbele overeenkomsten in de Portal van de Services BizTalk- en uitgeschakeld.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Bruggen gebruik geen bijgewerkte certificaat, zelfs nadat een certificaat in de winkel onderdeel is bijgewerkt
Houd rekening met de volgende scenario's:  

**Scenario 1: Vingerafdruk gebaseerde certificaten gebruiken voor het beveiligen van bericht doorverbinden van een brug met een service-eindpunt**  
Bekijk een scenario waar u vingerafdruk gebaseerde certificaten in uw project BizTalk-Service gebruiken. U het certificaat in de Portal van de Services BizTalk bijwerken met dezelfde naam, maar een ander vingerafdruk, maar niet worden bijgewerkt het project BizTalk Service dienovereenkomstig gewijzigd. In een dergelijke situatie blijven de brug mogelijk de berichten worden verwerkt, omdat de oudere certificaatgegevens mogelijk nog steeds in de cache kanaal. Daarna de berichtverwerking van het is mislukt.  

**Tijdelijke oplossing**: werk het certificaat in het project BizTalk Service en implementeer deze opnieuw het project.  

**Scenario 2: Gedrag op basis van naam gebruiken om aan te geven van certificaten voor het beveiligen van bericht doorverbinden van een brug met een service-eindpunt**

Bekijk een scenario waarbij u gedrag op basis van naam gebruiken om te identificeren certificaten in uw project BizTalk-Service. U het certificaat in de Portal van de Services BizTalk bijwerken maar werk het project BizTalk Service niet op dienovereenkomstig gewijzigd. In een dergelijke situatie blijven de brug mogelijk de berichten worden verwerkt, omdat de oudere certificaatgegevens mogelijk nog steeds in de cache kanaal. Daarna de berichtverwerking van het is mislukt.  

**Tijdelijke oplossing**: werk het certificaat in het project BizTalk Service en implementeer deze opnieuw het project.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Bruggen gaat u verder met het verwerken van berichten, zelfs wanneer de SQL-database offline is
De bruggen BizTalk Services gaat u verder met het verwerken van berichten voor verloop van tijd zelfs als de Microsoft Azure SQL-Database (dat slaat de actieve informatie, zoals geïmplementeerd onderdelen en pijpleidingen), is offline. Dit komt doordat BizTalk Services wordt gebruikt voor de in de cache onderdelen en configuratie van brug.
Als u niet dat de bruggen alle berichten te verwerken wilt wanneer de SQL-Database offline is, kunt u de BizTalk Services PowerShell-cmdlets gebruiken om te stoppen of de BizTalk Service uitstellen. Zie [Azure BizTalk Service Management voorbeeld](http://go.microsoft.com/fwlink/p/?LinkID=329019) op de Windows PowerShell-cmdlets voor het beheren van bewerkingen.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Een extra stuklijst teken bevat lezen van het XML-bericht binnen een brug van aangepaste codecomponent
Bekijk een scenario waarbij u wilt lezen van een XML-bericht van een brug aangepaste code. Als u de .NET-API System.Text.Encoding.UTF8.GetString(bytes) een extra stuklijst teken in de uitvoer aan het begin van het bericht is opgenomen. Ja, als u niet dat de uitvoer om op te nemen het extra stuklijst-teken wilt, moet u ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Verzenden van berichten naar een brug met WCF schaal niet
De schaal van berichten die zijn verzonden naar een met WCF-brug bevat niet aanpassen. Als u wilt dat een scalable client, moet u in plaats daarvan HttpWebRequest gebruiken.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>UPGRADE: Token providerfout na een upgrade van BizTalk Services Preview naar algemene beschikbaarheid (GA)
Er is een bewerken of AS2 overeenkomst met actieve batches. Wanneer de BizTalk-Service wordt bijgewerkt van voorbeeldversie naar GA, doet zich voor de volgende handelingen uit:
* Fout: De token-provider is niet geen beveiligingstoken opgeven. Token provider geretourneerd bericht: de naam van de externe kan niet worden omgezet.

* Batchtaken worden geannuleerd.

**Tijdelijke oplossing**: na de BizTalk-Service wordt bijgewerkt naar algemene beschikbaarheid (GA), implementeer deze opnieuw de overeenkomst.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UPGRADE: Werkset toont de oude brug pictogrammen na een upgrade van de SDK van de Services BizTalk
Nadat u een upgrade uitvoert van een eerdere versie van de SDK van de Services BizTalk, die oude pictogrammen dat staat voor het bruggen waren, blijft de werkset weergeven van de oude pictogrammen voor de bruggen. Als u een brug aan BizTalk Service project ontwerpfunctie oppervlak toevoegt, ziet het oppervlak het nieuwe pictogram.  

**Tijdelijke oplossing**. U kunt dit probleem omzeilen door de bestanden .tbd onder te verwijderen <system drive>: \Users\<gebruiker > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UPGRADE: BizTalk Portal update van voorbeeldversie GA mogelijk een foutbericht dat aangeeft dat de mogelijkheid voor bewerken niet beschikbaar is weergegeven
Als u bent aangemeld bij de Portal van de Services BizTalk terwijl de Services BizTalk wordt bijgewerkt van voorbeeldversie naar GA, krijgt u mogelijk het volgende foutbericht weergegeven op de portal:  

Deze functie is niet beschikbaar als onderdeel van deze editie van Microsoft Azure BizTalk-Services. Als u wilt gebruiken wordt deze mogelijkheden overschakelen naar een juiste editie.  

**Resolutie**: Log af van de portal, sluiten en open de browser en klik vervolgens Meld u aan bij de portal.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UPGRADE: Nieuwe bijhouden gegevens niet wordt weergegeven nadat BizTalk Services wordt bijgewerkt naar GA
Wordt ervan uitgegaan dat u een scenario waarin u een XML-brug geïmplementeerd op BizTalk Services Preview-abonnement hebt. U berichten verzenden naar de brug en de bijbehorende voor het bijhouden van gegevens is beschikbaar op de BizTalk Services-Portal. Nu, als de bits van de runtime BizTalk Services-Portal en BizTalk Services worden bijgewerkt naar GA en u een bericht naar hetzelfde brug eindpunt eerder geïmplementeerd verzenden, de gegevens bijhouden wordt niet weergegeven voor berichten die zijn verzonden, na de upgrade.  

### <a name="pipelines-vs-bridges"></a>Pijpleidingen v/s bruggen
In dit document, worden de term 'pijpleidingen' en 'bruggen' door elkaar gebruikt. Beide in principe betekenis dezelfde, dat wil zeggen, de indeling van een bericht verwerkingseenheid geïmplementeerd op BizTalk-Services.  

### <a name="concepts"></a>Concepten  

[BizTalk-Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
