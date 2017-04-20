<properties
    pageTitle="Het gebruik van Azure bestand Vgx. Cache met Node.js | Microsoft Azure"
    description="Aan de slag met Azure bestand Vgx. Cache Node.js en node_redis gebruiken."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Het gebruik van Azure bestand Vgx. Cache met Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure bestand Vgx. Cache hebt u toegang tot een beveiligde, speciale bestand Vgx. cache, beheerd door Microsoft. De cache is toegankelijk zijn vanuit een Microsoft Azure-toepassing.

In dit onderwerp ziet u hoe u aan de slag met Azure bestand Vgx. Cache Node.js gebruiken. Zie [een toepassing Node.js Chat met Socket.IO op de Website van een Azure maakt](../app-service-web/web-sites-nodejs-chat-app-socketio.md)voor een ander voorbeeld van het gebruik van Azure bestand Vgx. Cache met Node.js.


## <a name="prerequisites"></a>Vereisten voor

[Node_redis](https://github.com/mranney/node_redis)installeren:

    npm install redis

In deze zelfstudie wordt [node_redis](https://github.com/mranney/node_redis). Zie de afzonderlijke documentatie voor de Node.js-clients die bij [Node.js bestand Vgx. clients](http://redis.io/clients#nodejs)worden vermeld voor voorbeelden van het gebruik van andere Node.js-clients.

## <a name="create-a-redis-cache-on-azure"></a>Een bestand Vgx. cache op Azure maken

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De host-toets voor de naam van en toegang ophalen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Verbinding maken met de cache veilig via SSL

De meest recente versies van [node_redis](https://github.com/mranney/node_redis) bieden ondersteuning voor Azure bestand Vgx. Cache verbinding via SSL. Het volgende voorbeeld ziet hoe u verbinding maken met Azure bestand Vgx. Cache met het eindpunt SSL van 6380. Vervang `<name>` met de naam van de cache en `<key>` met een uw primaire of secundaire sleutel als die worden beschreven in de vorige sectie voor het [ophalen van de host-toets voor de naam en access](#retrieve-the-host-name-and-access-keys) .

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Een ander nummer toevoegen aan de cache en deze op te halen

Het volgende voorbeeld ziet u hoe u verbinding maken met een exemplaar Azure bestand Vgx. Cache en opslaan en ophalen van een item uit de cache. Zie voor meer voorbeelden van het gebruik van het bestand Vgx. met de client [node_redis](https://github.com/mranney/node_redis) [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Uitvoer:

    OK
    value


## <a name="next-steps"></a>Volgende stappen

- [Diagnostische hulpprogramma's cache inschakelen](cache-how-to-monitor.md#enable-cache-diagnostics) zodat u [monitor](cache-how-to-monitor.md) de status van de cache kunt.
- Lees de officiële [bestand Vgx. documentatie](http://redis.io/documentation).



