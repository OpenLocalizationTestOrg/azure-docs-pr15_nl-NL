<properties 
    pageTitle="Polling langdurige bewerkingen | Microsoft Azure" 
    description="Dit onderwerp wordt uitgelegd hoe u een poll uitvoeren onder langdurige bewerkingen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="delivering-live-streaming-with-azure-media-services"></a>Live Streaming met Azure mediaservices bieden

##<a name="overview"></a>Overzicht

Microsoft Azure Media Services biedt API's die aanvragen verzenden naar Media Services activiteiten te starten (bijvoorbeeld: maken, starten, stoppen of een kanaal verwijderen). Deze bewerkingen zijn langdurige.

De Media Services .NET SDK biedt API's die de aanvraag verzendt en wacht totdat de bewerking is voltooid (intern, de API's zijn polling voor de voortgang van bewerking op bepaalde momenten). Bijvoorbeeld wanneer u een kanaal belt. Start(), retourneert de methode nadat het kanaal wordt gestart. U kunt ook de asynchroon versie: wachten op een kanaal. StartAsync() (Zie voor informatie over asynchrone patroon op basis van een taak, [tikt u op](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). API's die een bewerking verzendt en vervolgens wordt gecontroleerd op de status totdat de bewerking voltooid is, worden 'polling methoden' genoemd. Deze methoden (met name de versie asynchrone) worden aanbevolen voor uitgebreide clienttoepassingen en/of statuscontrole services.

Zijn er scenario's waarin een toepassing geen tijd hebt om een langdurige http-aanvraag en wil handmatig controleren op de voortgang van de bewerking. Een typisch voorbeeld is een browser interactief werken met een stateless webservice: als de browser aanvraagt een kanaal maken, de webservice start van een langdurige bewerking en geeft als resultaat de bewerkings-ID naar de browser. De browser kunt vervolgens vragen voor de webservice om de status van de bewerking op basis van de-ID. De Media Services .NET SDK biedt API's die handig om dit scenario zijn. Deze API's heten "niet-peiling methoden".
De "niet-peiling methoden" hebben de volgende naming patroon:*OperationName*betrekking heeft (bijvoorbeeld SendCreateOperation) verzenden. Verzenden*OperationName*bewerking methoden retourneren de **IOperation** ; het geretourneerde object bevat informatie die kan worden gebruikt voor het bijhouden van de bewerking. De verzenden*OperationName*OperationAsync retourneert **taak<IOperation>**.

Op dit moment de niet-peiling methoden voor het ondersteunen van de volgende klassen: **kanaal**, **StreamingEndpoint**en **programma**.

Als u wilt controleren van de status betrekking heeft, gebruikt u de methode **GetOperation** voor de klasse **OperationBaseCollection** . Gebruik van de volgende intervallen om te controleren van de status van de bewerking: voor een **kanaal** en **StreamingEndpoint** bewerkingen, gebruikt u 30 seconden; voor bewerkingen, **programma** , gebruikt u 10 seconden.


##<a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt een klasse **ChannelOperations**genoemd gedefinieerd. De definitie van deze klasse mogelijk uitgangspunt voor de klassendefinitie van uw web-service. De volgende voorbeelden gebruikt voor eenvoudig, de niet-asynchrone-versies van methoden.

Het voorbeeld ziet ook hoe deze klasse door de client kunnen worden gebruikt.

###<a name="channeloperations-class-definition"></a>ChannelOperations class definitie

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>De clientcode

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
