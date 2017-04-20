<properties 
    pageTitle="Service-Bus met .NET en AMQP 1.0 | Microsoft Azure"
    description="Gebruik van de Service Bus van .NET met AMQP"
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>Gebruik van de Service Bus van .NET met AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>De Service-Bus SDK downloaden

AMQP 1.0-ondersteuning is beschikbaar in de Service Bus SDK versie 2.1 of hoger. U kunt ervoor zorgen dat u hebt de meest recente versie door de Service Bus bits van [NuGet][]downloaden.

## <a name="configuring-net-applications-to-use-amqp-10"></a>.NET toepassingen configureren voor gebruik AMQP 1.0

Standaard wordt de bibliotheek van de client Service Bus .NET communiceert met de Service Bus-service met een speciale op basis van SOAP-protocol. Als u wilt AMQP 1.0 gebruiken in plaats van de standaard vereist protocol expliciete configuratie op de verbindingsreeks van de Service Bus, zoals wordt beschreven in de volgende sectie. Dan deze wijziging ongewijzigd toepassingscode wanneer u een AMQP 1.0 gebruikt.

Er zijn een paar API-functies die niet worden ondersteund wanneer u een AMQP gebruikt in de huidige versie. Deze niet-ondersteunde functies zijn later weergegeven in de sectie [niet-ondersteunde functies, beperkingen, en doorgevoerd verschillen](#unsupported-features-restrictions-and-behavioral-differences). Enkele van de geavanceerde configuratie-instellingen hebt ook een andere betekenis wanneer u een AMQP gebruikt.

### <a name="configuration-using-appconfig"></a>Configuratie App.config gebruiken

Het is raadzaam voor toepassingen het configuratiebestand App.config gebruiken om op te slaan instellingen. Voor Bus Service-toepassingen, kunt u App.config voor de opslag van de instellingen voor de Service Bus **ConnectionString** -waarde. Een voorbeeld van een bestand App.config is als volgt:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

De waarde van de `Microsoft.ServiceBus.ConnectionString` instelling is de Service Bus-verbindingsreeks die wordt gebruikt voor het configureren van de verbinding met de Service Bus. De indeling is als volgt:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Waar `[namespace]` en `SharedAccessKey` van de [Azure portal][]worden opgehaald. Zie [aan de slag met Service Bus wachtrijen][]voor meer informatie.

Wanneer u met AMQP, toevoegen de verbindingsreeks met `;TransportType=Amqp`. Deze notatie wordt gemeld dat de clientbibliotheek als u wilt de verbinding maken met AMQP 1.0 bus voor Service.

## <a name="message-serialization"></a>Bericht serialisatie

Wanneer u met de standaardprotocol, is het standaardgedrag van de serialisatie van de bibliotheek van de client .NET gebruik van het type [DataContractSerializer][] serialiseren een exemplaar [BrokeredMessage][] tussen de clientbibliotheek en de Service Bus-service. Wanneer u met de AMQP transport-modus, wordt in de clientbibliotheek het AMQP typesysteem gebruikt voor serialisatie van het [bericht brokered][BrokeredMessage] in een AMQP-mailbericht. Deze serialisatie kunt het bericht wordt ontvangen en door een ontvangst-toepassing die mogelijk wordt uitgevoerd op een ander platform, bijvoorbeeld een Java-toepassing die de JMS-API voor toegang tot de Service Bus geïnterpreteerd.

Wanneer u een exemplaar [BrokeredMessage][] samenstellen, kunt u een .NET-object als een parameter voor de constructor als de hoofdtekst van het bericht moet fungeren opgeeft. Voor objecten die kunnen worden toegewezen aan AMQP primitieve typen, wordt de hoofdtekst omgezet in AMQP gegevenstypen. Als het object kan niet rechtstreeks worden toegewezen aan een AMQP primitieve type; dat wil zeggen een aangepast type gedefinieerd door de toepassing, klik met de [DataContractSerializer][]en de serienummer bytes van het object wordt omgezet in een bericht van de gegevens AMQP verzonden.

Om te vergemakkelijken interoperabiliteit met niet-.NET-clients, gebruikt u alleen .NET-gegevenstypen die kunnen worden omgezet rechtstreeks in AMQP typen voor de hoofdtekst van het bericht. De volgende tabel worden deze typen en de bijbehorende toewijzing aan het systeem van AMQP type.

| .NET hoofdtekst objecttype          | Toegewezen AMQP Type                          | AMQP hoofdtekst sectietype                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| BOOL                           | Booleaanse waarde                                   | AMQP waarde                                                                                                                                                |
| byte                           | ubyte                                     | AMQP waarde                                                                                                                                                |
| USHORT                         | USHORT                                    | AMQP waarde                                                                                                                                                |
| uint                           | uint                                      | AMQP waarde                                                                                                                                                |
| ULONG                          | ULONG                                     | AMQP waarde                                                                                                                                                |
| SByte                          | byte                                      | AMQP waarde                                                                                                                                                |
| korte                          | korte                                     | AMQP waarde                                                                                                                                                |
| int                            | int                                       | AMQP waarde                                                                                                                                                |
| lange                           | lange                                      | AMQP waarde                                                                                                                                                |
| toegestane achterstand                          | toegestane achterstand                                     | AMQP waarde                                                                                                                                                |
| dubbele                         | dubbele                                    | AMQP waarde                                                                                                                                                |
| decimaal                        | decimal128                                | AMQP waarde                                                                                                                                                |
| teken                           | teken                                      | AMQP waarde                                                                                                                                                |
| Datum /                       | tijdstempel                                 | AMQP waarde                                                                                                                                                |
| GUID                           | UUID                                      | AMQP waarde                                                                                                                                                |
| byte]                         | binair getal                                    | AMQP waarde                                                                                                                                                |
| tekenreeks                         | tekenreeks                                    | AMQP waarde                                                                                                                                                |
| System.Collections.IList       | lijst                                      | AMQP waarde: de items in de siteverzameling kunnen alleen worden items die zijn gedefinieerd in deze tabel.                                                             |
| System.Array                   | matrix                                     | AMQP waarde: de items in de siteverzameling kunnen alleen worden items die zijn gedefinieerd in deze tabel.                                                             |
| System.Collections.IDictionary | plattegrond                                       | AMQP waarde: de items in de siteverzameling kunnen alleen worden items die zijn gedefinieerd in deze tabel. Opmerking: alleen tekenreekssleutels worden ondersteund.                        |
| URI                            | Beschrijvingen van de tekenreeks (Zie de volgende tabel) | AMQP waarde                                                                                                                                                |
| DateTimeOffset                 | Lang beschreven (Zie de volgende tabel)   | AMQP waarde                                                                                                                                                |
| Tijdspanne                       | Lang beschreven (Zie de volgende)         | AMQP waarde                                                                                                                                                |
| Stream                         | binair getal                                    | AMQP-gegevens (mogelijk meerdere). De secties gegevens bevatten de onbewerkte bytes lezen van de Stream-object.                                                           |
| Ander Object                   | binair getal                                    | AMQP-gegevens (mogelijk meerdere). Bevat de serienummer binair getal van het object dat de DataContractSerializer of een serialisatiefunctie verstrekt door de toepassing gebruikt. |

| .NET-type      | Toegewezen AMQP beschreven Type                                                                                              | Notities                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Tijdspanne       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Niet-ondersteunde functies en beperkingen doorgevoerd verschillen

De volgende functies van de Service Bus .NET-API worden momenteel niet ondersteund wanneer u een AMQP gebruikt:

-   Transacties

-   Via de bestemming van de bestandsoverdracht verzenden

Er zijn ook enkele kleine verschillen in het gedrag van de Service Bus .NET-API wanneer u een AMQP, vergeleken met het standaard-protocol gebruikt:

-   De eigenschap [OperationTimeout][] wordt genegeerd.

-   `MessageReceiver.Receive(TimeSpan.Zero)`wordt geïmplementeerd als `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Voltooiing van berichten door vergrendelen tokens kan alleen worden uitgevoerd door de ontvangers van berichten die in eerste instantie berichten ontvangen.

## <a name="controlling-amqp-protocol-settings"></a>Beheert AMQP protocolinstellingen

De .NET-API's worden verschillende instellingen om het gedrag van het protocol AMQP te bepalen:

-   **MessageReceiver.PrefetchCount**: besturingselementen voor de eerste lof toegepast op een koppeling. De standaardinstelling is aan 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: besturingselementen voor de maximumgrootte van een AMQP frame tijdens de onderhandelingen bij verbinding aangeboden opnieuw wordt geopend. De standaardinstelling is 65.536 bytes.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: als overdrachten batchable, deze waarde bepaalt de maximale vertraging voor het verzenden van dispositions. Overgenomen door afzenders/ontvangers al dan niet standaard. Afzonderlijke afzender/ontvanger kunt overschreven door de standaard, dat wil 20 milliseconden zeggen.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: besturingselementen of AMQP verbindingen tot stand worden gebracht via een SSL-verbinding. De standaardinstelling is **voldaan**.

## <a name="next-steps"></a>Volgende stappen

Klaar voor meer informatie? Ga naar de volgende koppelingen:

- [Overzicht van de service Bus AMQP]
- [AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen]
- [AMQP in Service Bus voor Windows Server]

  [Aan de slag met Service Bus wachtrijen]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure-portal]: https://portal.azure.com
[Overzicht van de service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Service Bus voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
