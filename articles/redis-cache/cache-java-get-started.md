<properties
   pageTitle="Het gebruik van Azure bestand Vgx. Cache met Java | Microsoft Azure"
    description="Aan de slag met Azure bestand Vgx. Cache Java gebruiken"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-java"></a>Het gebruik van Azure bestand Vgx. Cache met Java

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure bestand Vgx. Cache hebt die u toegang tot een specifieke bestand Vgx. cache, beheerd door Microsoft. De cache is toegankelijk zijn vanuit een Microsoft Azure-toepassing.

In dit onderwerp ziet u hoe u aan de slag met Azure bestand Vgx. Cache Java gebruiken.

## <a name="prerequisites"></a>Vereisten voor

[Jedis](https://github.com/xetorthio/jedis) - Java-client voor het bestand Vgx.

Deze zelfstudie Jedis gebruikt, maar u kunt elke Java-client die bij [http://redis.io/clients](http://redis.io/clients)worden vermeld.

## <a name="create-a-redis-cache-on-azure"></a>Een bestand Vgx. cache op Azure maken

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De host-toets voor de naam van en toegang ophalen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>De niet-SSL-eindpunt inschakelen

Bij sommige clients bestand Vgx. geen ondersteuning voor SSL en standaard de [niet-SSL-poort is uitgeschakeld voor nieuwe exemplaren van Azure bestand Vgx. Cache](cache-configure.md#access-ports). Op het moment van deze schrijven ondersteunen niet de client [Jedis](https://github.com/xetorthio/jedis) SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]




## <a name="add-something-to-the-cache-and-retrieve-it"></a>Een ander nummer toevoegen aan de cache en deze op te halen

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Volgende stappen

- [Diagnostische hulpprogramma's cache inschakelen](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) zodat u [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) de status van de cache kunt.
- Lees de officiële [bestand Vgx. documentatie](http://redis.io/documentation).

