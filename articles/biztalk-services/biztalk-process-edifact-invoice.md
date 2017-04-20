<properties
   pageTitle="Zelfstudie: Verwerken EDIFACT facturen met behulp van Azure BizTalk Services | Microsoft Azure BizTalk-Services"
   description="Het maken en configureren van de app vak verbindingslijn of API en deze gebruiken in een app logica in Azure App-Service"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Zelfstudie: Proces EDIFACT facturen met behulp van Azure BizTalk-Services
U kunt de Portal van BizTalk-Services configureren en implementeren van X12 en EDIFACT overeenkomsten. In deze zelfstudie kijken we naar het maken van een overeenkomst EDIFACT voor de uitwisseling van facturen tussen werkbladen partners. Deze zelfstudie is geschreven rond een end-to-end-bedrijfsoplossing die betrekking hebben op personen twee werkbladen partners Noordenwind en Contoso die EDIFACT berichten uitwisselen.  

## <a name="sample-based-on-this-tutorial"></a>Voorbeeld op basis van deze zelfstudie
Deze zelfstudie is geschreven rond een steekproef, **EDIFACT facturen met BizTalk-Services verzenden**, die voor downloaden vanuit de [Galerie voor MSDN-Code](http://go.microsoft.com/fwlink/?LinkId=401005)beschikbaar is. U kunt de steekproef gebruiken en Ga door deze zelfstudie te begrijpen hoe de steekproef is gemaakt. Of u kunt deze zelfstudie om u te maken van uw eigen oplossing a t/m. Deze zelfstudie is gericht op de tweede aanpak, zodat u meer informatie over hoe deze oplossing is gemaakt. Bovendien zoveel mogelijk, de zelfstudie komt overeen met de steekproef en gebruikt u dezelfde namen voor onderdelen (bijvoorbeeld, schema's, transformaties) als gebruikte in de steekproef.  

>[AZURE.NOTE] Omdat deze oplossing heeft betrekking op het verzenden van een bericht van een brug EAI naar een brug bewerken, er wordt gebruikgemaakt van de steekproef [BizTalk Services brug steekproef koppelen](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) .  

## <a name="what-does-the-solution-do"></a>Wat doet de oplossing?

In deze oplossing ontvangt Noordenwind EDIFACT facturen van Contoso. Deze facturen zijn niet in een standaardindeling voor bewerken. Zo is, voordat u de factuur naar Noordenwind verzendt, deze moet worden omgezet naar een document met EDIFACT factuur (ook wel INVOIC genoemd). Hebben ontvangen, moet Noordenwind verwerken van de factuur EDIFACT en een besturingselement bericht (ook wel CONTRL genoemd) terugkeren naar Contoso.

![][1]  

Als u wilt bereiken deze bedrijfsscenario, gebruikt Contoso de functies die worden geleverd met Microsoft Azure BizTalk-Services.

*   Contoso wordt EAI bruggen om de oorspronkelijke factuur EDIFACT INVOIC transformeren.

*   De brug EAI vervolgens bericht het naar een bewerken verzenden brug geïmplementeerd als onderdeel van een overeenkomst geconfigureerd in de Portal van BizTalk-Services.

*   De bewerken verzenden brug de EDIFACT INVOIC verwerkt en doorgestuurd naar Northwind.

*   Nadat u de factuur ontvangt, ontvangen Noordenwind geeft als resultaat een CONTRL bericht naar de bewerken brug geïmplementeerd als onderdeel van de overeenkomst.  

> [AZURE.NOTE] Deze oplossing wordt desgewenst ook getoond hoe u de optie batchen de facturen verzenden in batches, in plaats van elke factuur afzonderlijk verzenden.  

Als u wilt het scenario hebt voltooid, gebruiken we Service Bus wachtrijen factuur van Contoso naar Noordenwind verzenden of ontvangen van bevestiging van Northwind. Deze wachtrijen kunnen worden gemaakt met behulp van een clienttoepassing, dat is gedownload en deel uitmaakt van de steekproef-pakket dat beschikbaar is als onderdeel van deze zelfstudie.  

## <a name="prerequisites"></a>Vereisten voor

*   U kunt een naamruimte Service Bus moet hebben. Zie voor instructies over het maken van een naamruimte [How To: maken of wijzigen van een Service Bus Service Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Laat ons wordt ervan uitgegaan dat u al een Service Bus naamruimte deze is ingericht, **edifactbts**genoemd.

*   U moet een BizTalk-Services-abonnement hebben. Zie [een BizTalk Service maken met behulp van Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=302280)voor instructies. Voor deze zelfstudie laat ons wordt ervan uitgegaan dat u hebt een abonnement BizTalk-Services, **contosowabs**genoemd.

*   Registreer uw abonnement BizTalk-Services op de BizTalk Services-Portal. Zie voor instructies voor het [registreren van een BizTalk-Service-implementatie op de BizTalk Services-Portal](https://msdn.microsoft.com/library/hh689837.aspx)

*   U moet Visual Studio is geïnstalleerd.

*   U moet de SDK van BizTalk Services is geïnstalleerd. U kunt de SDK downloaden van [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Stap 1: De Service Bus wachtrijen maken  
Deze oplossing wordt Service Bus wachtrijen gebruikt om te wisselen van berichten tussen werkbladen partners. Contoso en Noordenwind verzenden berichten aan de wachtrijen vanaf waar de bruggen EAI-en/of bewerken ze gebruiken. Voor deze oplossing moet u drie Service Bus wachtrijen:

*   **northwindreceive** – Noordenwind ontvangt de factuur van Contoso via deze wachtrij.

*   **contosoreceive** – Contoso de ontvangstbevestiging via ontvangt van Northwind deze wachtrij.

*   **geschorst** – geschorst alle berichten worden doorgestuurd naar deze wachtrij. Berichten worden onderbroken als ze tijdens het verwerken mislukt.

U kunt deze Service Bus wachtrijen maken met behulp van een clienttoepassing opgenomen in het Voorbeeldpakket.  

1.  Open vanaf de locatie waar u de steekproef hebt gedownload, **Zelfstudie verzenden facturen gebruiken BizTalk Services bewerken Bridges.sln**.

2.  Druk op **F5** om te maken en de clienttoepassing van **Zelfstudie** starten.

3.  Voer in het scherm de Service Bus ACS naamruimte, de naam van de uitgever en uitgever-toets.

    ![][2]  
4.  Een bericht waarin dat drie wachtrijen wordt gemaakt in de naamruimte van uw Service Bus. Klik op **OK**.

5.  Laat de zelfstudie-Client met. Open de, klik op **Service-Bus** > **_de naamruimte van uw Service Bus_** > **wachtrijen**, en controleer of de drie wachtrijen worden gemaakt.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Stap 2: Maken en implementeren van werkbladen partnerovereenkomst
Maak een handelen partnerovereenkomst tussen Contoso en Northwind. Een handelen partnerovereenkomst Hiermee definieert u een contract handel tussen de twee zakenpartners, zoals welke berichtschema dat u wilt gebruiken, welke SMS protocol kunt gebruiken, enzovoort. Een handelen partnerovereenkomst bevat twee bewerken bruggen, een berichten verzenden naar werkbladen partners (de **brug bewerken verzenden**genoemd) en een met berichten ontvangt van werkbladen partners (de **brug bewerken ontvangen**genoemd).

In de context van deze oplossing, de bewerken verzenden brug overeenkomt met de verzenden-kant van de overeenkomst en de factuur EDIFACT van Contoso verzenden naar Noordenwind wordt gebruikt. Op dezelfde manier de bewerken ontvangen brug overeenkomt met de ontvangen-kant van de overeenkomst en wordt gebruikt om te ontvangen bevestigingen van Northwind.  

### <a name="create-the-trading-partners"></a>Maken van de werkbladen partners

Beginnen met, werkbladen partners voor Contoso en Noordenwind maken.  

1.  Klik in de Portal BizTalk-Services op het tabblad **Partners** , klikt u op **toevoegen**.

2.  De pagina nieuwe partner **Contoso** invoeren als de Partnernaam van een en klik vervolgens op **Opslaan**.

3.  Herhaal de stap als u wilt maken van de tweede partner, **Northwind**.  

### <a name="create-the-agreement"></a>De overeenkomst maken
Werkbladen partner overeenkomsten worden tussen profielen voor zakelijke partners handelsdag gemaakt. Deze oplossing wordt gebruikt voor de standaard-partner-profielen die automatisch worden gemaakt als we de partners gemaakt.  

1.  Klik in de BizTalk Services-Portal op **overeenkomsten** > **toevoegen**.

2.  Pagina van de nieuwe overeenkomst **Algemene instellingen** opgeven van de waarden, zoals wordt weergegeven in de onderstaande afbeelding en klik vervolgens op **Doorgaan**.

    ![][3]  

    Nadat u op **Doorgaan**, komen twee tabbladen, **Ontvangen instellingen** en **Instellingen voor verzenden** beschikbaar.

3.  Maak de overeenkomst verzenden tussen Contoso en Northwind. Deze overeenkomst van toepassing hoe Contoso de factuur EDIFACT naar Noordenwind verzendt.

    1.  Klik op **Instellingen voor verzenden**.

    2.  De standaardwaarden op de tabbladen **Inkomende URL**, **Transformeren**en **Batching** behouden.

    3.  Klik op het tabblad **Protocol** onder de sectie **schema's maken** om het schema **EFACT_D93A_INVOIC.xsd** te uploaden. Dit schema is beschikbaar met de voorbeeld-pakket.

        ![][4]  
    4.  Klik op het tabblad **Transport** Geef de details in voor de Service Bus wachtrijen. We gebruiken voor de overeenkomst verzenden aan de clientzijde de wachtrij **northwindreceive** de factuur EDIFACT verzenden naar Noordenwind en de wachtrij **geschorst** om te leiden van alle berichten met mislukt tijdens het verwerken van en worden geblokkeerd. U hebt gemaakt met deze wachtrijen in **stap 1: de Service Bus wachtrijen maken** (in dit onderwerp).

        ![][5]  

        Klik onder **Transport-instellingen > Transport type** en **geschorst berichtinstellingen > Transport type**, selecteert u Azure Service Bus en geef de waarden zoals weergegeven in de afbeelding.

4.  Maak de overeenkomst ontvangen tussen Contoso en Northwind. Deze overeenkomst van toepassing hoe Contoso de bevestiging ontvangt van Northwind.

    1.  Klik op **Instellingen ontvangen**.

    2.  De standaardwaarden op de tabbladen **Transport** en **Transformeren** behouden.

    3.  Klik op het tabblad **Protocol** onder de sectie **schema's maken** om het schema **EFACT_4.1_CONTRL.xsd** te uploaden. Dit schema is beschikbaar met de voorbeeld-pakket.

    4.  Klik op het tabblad **Route** een filter om ervoor te zorgen dat alleen bevestigingen van Northwind worden doorgestuurd naar Contoso te maken. Klik onder **Instellingen voor doorsturen**, klikt u op **toevoegen** om te maken van het routeren filter.

        ![][6]  
        1.  Verstrek waarden voor **De naam van de regel**, **Route regel**en **Route bestemming** zoals wordt weergegeven in de afbeelding.

        2.  Klik op **Opslaan**.

    5.  Klik op het tabblad **Route** nogmaals opgeven waar geschorst bevestigingen (bevestigingen die tijdens het verwerken van mislukt) worden doorgestuurd naar. Het transporttype ingesteld op Azure Service Bus, type bestemming doorsturen naar **wachtrij**, verificatietype aan **Access-handtekening gedeeld** (SA's), de verbindingsreeks SA's bieden voor de Service Bus naamruimte en voer de naam van de wachtrij als **geschorst**.

5.  Klik tot slot op **Deploy** als u wilt de overeenkomst implementeren. Houd rekening met de eindpunten waar het verzenden en ontvangen overeenkomsten geïmplementeerd.

    *   Let op het tabblad **Instellingen voor verzenden** , klikt u onder **Inkomende URL**, het eindpunt. Als u wilt een bericht verzenden namens Contoso naar Noordenwind met het bewerken verzenden brug, moet u een bericht verzenden naar dit eindpunt.

    *   Let op het tabblad **Instellingen ontvangen** onder **Transport**, het eindpunt. Een bericht verzenden vanuit Noordenwind bij Contoso gebruik van de bewerken ontvangen brug, moet u een bericht verzenden naar dit eindpunt.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Stap 3: Maak en implementeer het BizTalk Services-project

In de vorige stap, u geïmplementeerd de bewerken verzenden en ontvangen van overeenkomsten EDIFACT facturen en bevestigingen verwerken. Deze overeenkomsten kunnen proces alleen berichten die aan het schema standaard EDIFACT bericht voldoen. Echter per het scenario voor deze oplossing stuurt Contoso een factuur naar Noordenwind in een interne, bedrijfseigen schema. Zo is, voordat het bericht wordt verzonden naar de bewerken verzenden brug, deze moet worden omgezet vanuit het interne schema aan het schema standaard EDIFACT factuur. De BizTalk Services EAI-project doet die.

Het project BizTalk-Services, **InvoiceProcessingBridge**, waarmee het bericht worden omgezet, is ook deel uit van de steekproef die u hebt gedownload. Het project bevat de volgende onderdelen:

*   **INHOUSEINVOICE. XSD** – Schema van de interne factuur die wordt verzonden naar Northwind.

*   **EFACT_D93A_INVOIC. XSD** – Schema van de standaard EDIFACT factuur.

*   **EFACT_4.1_CONTRL. XSD** – Schema van de ontvangstbevestiging EDIFACT die Noordenwind wordt verzonden naar Contoso.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – de transformatie die het schema interne factuur wordt toegewezen aan het schema standaard EDIFACT factuur.  

### <a name="create-the-biztalk-services-project"></a>Maak de BizTalk Services-project
1.  Vouw het project InvoiceProcessingBridge in de Visual Studio-oplossing, en open het bestand **MessageFlowItinerary.bcs** .

2.  Klik ergens op het canvas en de **URL van de Service BizTalk** instellen in het eigenschappenvak om op te geven van de naam van uw BizTalk Services-abonnement. Bijvoorbeeld `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Sleep vanuit de werkset, een **XML-One-Way brug** aan het canvas. Stel de eigenschappen van de **Entiteitsnaam** en het **Relatieve adres** van de brug op **ProcessInvoiceBridge**. Dubbelklik op **ProcessInvoiceBridge** als u wilt openen, het oppervlak van de configuratie van brug.

4.  Klik in het vak **Berichttypen** op het plusteken (**+**) knop kunt u het schema van het binnenkomende bericht opgeven. Aangezien de binnenkomende bericht voor de brug EAI altijd de interne factuur is, stelt u dit naar **INHOUSEINVOICE**.

    ![][8]  
5.  Klik op de vorm van de **XML-transformatie** en klik op de knop weglatingsteken (**...**) in het vak van de eigenschap voor de eigenschap **kaarten** . Selecteer het bestand dat **INHOUSEINVOICE_to_D93AINVOIC** transformeren in het dialoogvenster **Kaarten selectie** en klik vervolgens op **OK**.

    ![][9]  
6.  Ga terug naar **MessageFlowItinerary.bcs**en van de werkset, sleept u een **Externe Service-eindpunt Two-Way** aan de rechterkant van het **ProcessInvoiceBridge**. Stel de eigenschap **Entiteit Name** naar **EDIBridge**.

7.  In de Solution Explorer de **MessageFlowItinerary.bcs** uitvouwen en dubbelklik op het bestand **EDIBridge.config** . De inhoud van de **EDIBridge.config** vervangen door het volgende.

    > [AZURE.NOTE] Waarom heb ik nodig om de .config-bestand te bewerken? De externe service-eindpunt die we hebben toegevoegd aan het brug ontwerpfunctie canvas vertegenwoordigt de bruggen bewerken die we eerder geïmplementeerd. Bewerken bruggen zijn twee richtingen bruggen, met een verzenden en ontvangen van kant. De EAI-brug die we hebben toegevoegd aan de brug designer is echter een eenzijdige brug. Zo is, voor het verwerken van de verschillende bericht exchange patronen van de twee bruggen, gebruiken we het gedrag van een aangepaste brug door de configuratie in het. Het aangepaste gedrag verwerkt Daarnaast kunnen ook de verificatie naar het eindpunt bewerken verzenden brug. Dit aangepaste gedrag is beschikbaar als een afzonderlijke steekproef bij [BizTalk Services brug koppelen steekproef - EAI om te bewerken](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Deze oplossing wordt gebruikgemaakt van de steekproef.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Het bestand EDIBridge.config configuratiegegevens bijwerken

    *   Klik onder _<behaviors>_, geef het ACS naamruimte en de sleutel die is gekoppeld aan het abonnement BizTalk-Services.

    *   Klik onder _<client>_, geef het eindpunt waar de bewerken verzenden overeenkomst wordt geïmplementeerd.

    Wijzigingen opslaan en sluit het configuratiebestand.

9.  Klik op de **verbindingslijn** van de Toolbox, en deelnemen aan de **ProcessInvoiceBridge** en **EDIBridge** onderdelen. Selecteer de verbindingslijn en stel in het vak eigenschappen **Filtervoorwaarde** naar **Alle**. Dit zorgt ervoor dat alle berichten die door de brug EAI verwerkt worden doorgestuurd naar de brug bewerken.

    ![][10]  
10.  Wijzigingen opslaan op de oplossing.  

### <a name="deploy-the-project"></a>Implementeer het project

1.  Klik op de computer waarop u het BizTalk Services-project hebt gemaakt, downloadt en installeert u de SSL-certificaat voor uw abonnement BizTalk-Services. Uit, klik op **Dashboard**onder BizTalk-Services, en klik vervolgens op **SSL-certificaat downloaden**. Dubbelklik op het certificaat en volgt u de prompt om de installatie te voltooien. Zorg ervoor dat u het certificaat onder **Certificeringsinstanties die vertrouwde basiscertificaten** certificaat store installeren.

2.  Met de rechtermuisknop op het project **InvoiceProcessingBridge** in Visual Studio Solution Explorer, en klik vervolgens op **Deploy**.

3.  Geef de waarden zoals weergegeven in de afbeelding en klik op **Deploy**. U kunt de referenties ACS ophalen voor BizTalk Services door te klikken op **Verbindingsgegevens** vanuit het dashboard BizTalk-Services.

    ![][11]  

    Kopieer het eindpunt waar de brug EAI wordt geïmplementeerd, bijvoorbeeld vanuit het deelvenster uitvoer `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Moet u deze URL eindpunt later.  

## <a name="step-4-test-the-solution"></a>Stap 4: De oplossing testen


In dit onderwerp kijken we naar het testen van de oplossing met behulp van de **Zelfstudie** -clienttoepassing die is opgegeven als onderdeel van de steekproef.  

1.  Druk op F5 om de **Client van de zelfstudie**starten in Visual Studio.

2.  Het scherm moet de waarden die vooraf ingevuld van de stap waar we de Service Bus wachtrijen hebt gemaakt. Klik op **volgende**.

3.  Geef in het volgende venster ACS referenties voor BizTalk Services-abonnement en de eindpunten waar EAI- en bewerken (ontvangen) bruggen zijn geïmplementeerd.

    U hebt u er het eindpunt van de brug EAI in de vorige stap hebt gekopieerd. Ontvangen voor bewerken brug eindpunt, in de Portal van de Services BizTalk, gaat u naar de overeenkomst > instellingen voor ontvangen > Transport > eindpunt.

    ![][12]  
4.  Klik in het volgende venster onder Contoso, op de knop **Interne factuur verzenden** . Dialoogvenster openen, opent u het bestand INHOUSEINVOICE.txt in het bestand. De inhoud van het bestand en klik vervolgens op **OK** om de factuur te sturen.

    ![][13]  
5.  In een paar seconden wordt de factuur ontvangen bij Northwind. Klik op de koppeling van het **Bericht weergeven** als u wilt zien van de factuur is ontvangen door Northwind. Zoals u ziet hoe de factuur is ontvangen door Noordenwind in schema standaard EDIFACT terwijl het account dat is verzonden door Contoso een interne schema is.

    ![][14]  
6.  Selecteer de factuur en klik vervolgens op **Bevestiging verzenden**. In het dialoogvenster dat verschijnt, zoals u ziet dat de interchange-ID dezelfde in de ontvangen factuur en de ontvangstbevestiging is verzonden. Klik op OK in het dialoogvenster **Bevestiging verzenden** .

    ![][15]  
7.  In een paar seconden wordt de bevestiging ontvangen bij Contoso.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Stap 5 (optioneel): verzenden EDIFACT factuur in batches 
BizTalk Services bewerken bruggen ondersteunt ook batchen aan uitgaande berichten. Deze functie is handig voor het ontvangen van partners die een reeks berichten (vergadering bepaalde criterium) in plaats van afzonderlijke berichten ontvangt.

Het belangrijkste tijdens het werken met batches is de werkelijke versie van de batch, ook wel de release-criteria. De criteria release worden gebaseerd op hoe de ontvangst partner wil ontvangen van berichten. Als batchen is ingeschakeld, wordt de brug bewerken niet het uitgaande bericht verzenden naar de ontvangst partner totdat de release-criteria is voldaan. Bijvoorbeeld een batchen criteria op basis van bericht grootte verzendingen een batch alleen als n berichten batch zijn verwerkt. Een criterium batch zijn op basis van tijd, zodat een batch wordt verzonden op een vaste tijd elke dag. In deze oplossing kunt u de grootte van berichten op basis criteria.

1.  Klik op de overeenkomst die u eerder hebt gemaakt in de Portal BizTalk-Services. Klik op verzendinstellingen > batchen > Batch toevoegen.

2.  Batch in te voeren, voert u **InvoiceBatch**, Geef een beschrijving en klik vervolgens op **volgende**.

3.  Geef een criterium batch, waarmee wordt gedefinieerd welke berichten u batch moeten worden geplaatst. In deze oplossing batch we alle berichten. Zo is, selecteer de gebruiken die geavanceerde definities optie en voer **1 = 1**. Dit is een voorwaarde die altijd true en daarom alle berichten batch wordt verwerkt. Klik op **volgende**.

    ![][17]  
4.  Een batch release criteria opgeven. Selecteer **MessageCountBased**in de vervolgkeuzelijst en-klare, en opgeven voor **tellen**, **3**. Dit betekent dat een reeks drie berichten worden verzonden naar Northwind. Klik op **volgende**.

    ![][18]  
5.  Bekijk het overzicht en klik vervolgens op **Opslaan**. Klik op **Deploy** om de overeenkomst implementeren.

6.  Ga terug naar de **Zelfstudie Client**, klikt u op **Interne factuur verzenden**en volg de aanwijzingen voor het verzenden van de factuur. U ziet dat er geen factuur is ontvangen door Noordenwind omdat de batchgrootte niet is voldaan. Herhaal deze stap twee keer, zodat er drie factuur berichten verzonden naar Northwind. Dit voldoet aan de criteria van de release batch van 3 berichten en u ziet nu een factuur op Noordenwind.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

