<properties
    pageTitle="Push-meldingen met Azure melding Hubs en Node.js verzenden"
    description="Informatie over het gebruik van de melding Hubs push-meldingen verzenden vanuit een Node.js-toepassing."
    keywords="push-bericht, push notifications,node.js push, ios push"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Push-meldingen met Azure melding Hubs en Node.js verzenden
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Overzicht

> [AZURE.IMPORTANT] Als u wilt deze zelfstudie hebt voltooid, moet u een actieve Azure-account hebt. Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs)voor meer informatie.

Deze handleiding wordt uitgelegd hoe u verzenden van push-meldingen met behulp van Azure melding Hubs rechtstreeks vanuit een Node.js-toepassing. 

De scenario's waarvoor zijn push-meldingen verzenden naar toepassingen voor de volgende besturingssystemen:

* Android-apparaat
* iOS
* Windows Phone
* Universele Windows-Platform 

Zie voor meer informatie over melding hubs, de sectie van de [Volgende stappen](#next) .

##<a name="what-are-notification-hubs"></a>Wat zijn melding Hubs?

Azure melding Hubs beschikt over een eenvoudig te gebruiken, meerdere platforms, scalable infrastructuur voor het verzenden van push-meldingen voor mobiele apparaten. Zie voor meer informatie over de service-infrastructuur, de pagina [Azure melding Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) .

##<a name="create-a-nodejs-application"></a>Een Node.js-servicetoepassing maken

De eerste stap in deze zelfstudie is een nieuwe, lege Node.js-toepassing maken. Zie voor instructies over het maken van een toepassing voor de Node.js [maken en implementeren van een toepassing voor de Node.js naar Azure website][nodejswebsite], [Node.js Cloudservice] [ Node.js Cloud Service] met behulp van Windows PowerShell of [websites onderhouden met WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Uw toepassing melding Hubs configureren

Als u wilt gebruiken Azure melding Hubs, die u wilt downloaden en gebruiken van het Node.js [azure-pakket](https://www.npmjs.com/package/azure), waaronder een ingebouwde reeks helper bibliotheken die met de push notification REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Gebruik knooppunt pakket Manager (NPM) het pakket verkrijgen

1.  Gebruik een opdrachtregel-interface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Linux) en navigeer naar de map waar u uw lege toepassing hebt gemaakt.

2.  Typ **npm azure-sb installeren** in het opdrachtvenster.

3.  U kunt handmatig uitvoeren met de opdracht **ls** of **dir** om te controleren of die een **knooppunt\_modules** map is gemaakt. Zoek het **azure** -pakket waarin de bibliotheken die u nodig hebt voor toegang tot de Hub melding in deze map.

>[AZURE.NOTE] U kunt meer informatie over het installeren van NPM op de officiële [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>De module importeren

Met een teksteditor, voeg de volgende naar het begin van het bestand **server.js** van de toepassing:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Een Hub voor melding van Azure-verbinding instellen

Het object **NotificationHubService** kunt u werken met melding hubs. De volgende code maakt een object **NotificationHubService** voor de nofication hub met de naam **hubname**. Toevoegen aan de bovenkant van het bestand **server.js** na de instructie om te importeren van de azure-module.

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

De waarde van de **connectionstring** verbinding kan worden opgehaald uit de [Azure-Portal] door de volgende stappen uit te voeren:

1. Klik in het linkernavigatiedeelvenster op **Bladeren**.

2. Selecteer **Melding Hubs**en zoek de hub die u wilt gebruiken voor de steekproef. U kunt verwijzen naar de [Windows Store aan de slag zelfstudie](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) als u hulp bij het maken van een nieuwe melding Hub nodig hebt.

3. Selecteer **Instellingen**.

4. Klik op **Clienttoegangsbeleid**. Hier ziet u beide tekenreeksen met de gedeelde en volledige toegang verbinding.

![Azure-Portal - melding Hubs](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] U kunt ook de verbindingsreeks met de cmdlet **Get-AzureSbNamespace** is verstrekt door [Azure PowerShell](../powershell-install-configure.md) of de opdracht **azure sb naamruimte weergeven** met de [Interface van Azure-opdrachtregel (Azure CLI)](../xplat-cli-install.md)ophalen.

##<a name="general-architecture"></a>Algemene architectuur

Het object **NotificationHubService** bevat de volgende gevallen object voor het verzenden van push-meldingen op bepaalde apparaten en toepassingen:

* **Android** - gebruik van het object **GcmService** , dat beschikbaar binnen **notificationHubService.gcm is**
* **iOS** - gebruik van het object **ApnsService** , dat wil toegankelijk is bij **notificationHubService.apns zeggen**
* **Windows Phone** - gebruik van het object **MpnsService** , dat beschikbaar binnen **notificationHubService.mpns is**
* **Universele Windows-Platform** - gebruik van het object **WnsService** , dat beschikbaar binnen **notificationHubService.wns is**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Hoe u: push-meldingen verzenden naar Android toepassingen

Het object **GcmService** biedt een methode **send** die kan worden gebruikt om de push-meldingen verzenden naar Android toepassingen. De methode **verzenden** accepteert de volgende parameters:

* **Labels** - de tag-id. Als er geen argument is opgegeven, wordt de melding naar al clients worden verzonden.
* **Nettolading** - de JSON of onbewerkte tekenreeks nettolading van het bericht.
* **Terugbellen** - de functie terugbellen.

Zie voor meer informatie over de nettolading-indeling, de sectie **nettolading** van het document [GCM Server implementeren](http://developer.android.com/google/gcm/server.html#payload) .

De volgende code wordt het **GcmService** exemplaar die worden aangeboden door de **NotificationHubService** om een push-bericht verzenden naar alle geregistreerde clients.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Hoe u: push-meldingen verzenden naar iOS-toepassingen

Hetzelfde als met Android toepassingen hierboven beschreven, het **ApnsService** -object biedt een methode **send** die kan worden gebruikt voor het verzenden van push-meldingen voor iOS-toepassingen. De methode **verzenden** accepteert de volgende parameters:

* **Labels** - de tag-id. Als er geen argument is opgegeven, wordt de melding naar alle clients worden verzonden.
* **Nettolading** - JSON van het bericht of tekenreeks nettolading.
* **Terugbellen** - de functie terugbellen.

Zie het gedeelte **Melding nettolading** van het document [lokaal en Push-meldingen Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) voor meer informatie de nettolading-indeling.

De volgende code wordt het **ApnsService** exemplaar die worden aangeboden door de **NotificationHubService** een waarschuwingsbericht verzenden naar alle clients:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Hoe u: push-meldingen verzenden naar Windows Phone-toepassingen

Het object **MpnsService** biedt een methode **send** die kan worden gebruikt voor het verzenden van push-meldingen voor Windows Phone-toepassingen. De methode **verzenden** accepteert de volgende parameters:

* **Labels** - de tag-id. Als er geen argument is opgegeven, wordt de melding naar alle clients worden verzonden.
* **Nettolading** - XML-nettolading van het bericht.
* **Doelnaam**  -  `toast` voor mailpop meldingen. `token`voor tegelmeldingen.
* **NotificationClass** - de prioriteit van de melding. Zie het gedeelte **HTTP koptekstelementen** van het document [Push-meldingen van een server](http://msdn.microsoft.com/library/hh221551.aspx) voor geldige waarden.
* **Opties** - optionele kopteksten.
* **Terugbellen** - de functie terugbellen.

Voor een lijst met geldige opties voor **doelnaam**, **NotificationClass** en koptekst, raadpleegt u de pagina [Push-meldingen van een server](http://msdn.microsoft.com/library/hh221551.aspx) .

De volgende code wordt het **MpnsService** exemplaar die worden aangeboden door de **NotificationHubService** een mailpop push-bericht verzenden:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Hoe u: push-meldingen verzenden naar toepassingen universele Windows-Platform (UWP)

Het object **WnsService** biedt een methode **send** die kan worden gebruikt voor het verzenden van push-meldingen naar toepassingen universele Windows-platforms.  De methode **verzenden** accepteert de volgende parameters:

* **Labels** - de tag-id. Als er geen argument is opgegeven, wordt de melding worden verzonden naar alle geregistreerde clients.
* **Nettolading** - de XML-bericht nettolading.
* **Type** - het meldingstype.
* **Opties** - optionele kopteksten.
* **Terugbellen** - de functie terugbellen.

Zie de [Push notification-service vergaderverzoeken en antwoorden kopteksten](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx)voor een lijst met geldige typen en de kop van de aanvraag.

De volgende code wordt het **WnsService** exemplaar die worden aangeboden door de **NotificationHubService** een mailpop push-bericht verzenden naar een UWP-app:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Volgende stappen

Het bovenstaande voorbeeld-fragmenten kunnen u gemakkelijk maken voor service-infrastructuur voor het geven van push-meldingen voor allerlei apparaten. U kunt de basisbeginselen van het gebruik van de melding Hubs met node.js hebt geleerd, volgt u deze koppelingen voor meer informatie over hoe u deze mogelijkheden verder kunt uitbreiden.

-   Zie de verwijzing MSDN voor [Azure melding Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
-   Ga naar de bibliotheek [Azure SDK voor knooppunt] op GitHub voor meer voorbeelden en implementatiedetails.

  [Azure SDK voor knooppunt]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Websites onderhouden met WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure-Portal]: https://portal.azure.com
