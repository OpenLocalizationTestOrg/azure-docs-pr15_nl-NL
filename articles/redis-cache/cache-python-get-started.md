<properties
    pageTitle="Het gebruik van Azure bestand Vgx. Cache met Python | Microsoft Azure"
    description="Aan de slag met Azure bestand Vgx. Cache Python gebruiken"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-python"></a>Het gebruik van Azure bestand Vgx. Cache met Python

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

In dit onderwerp ziet u hoe u aan de slag met Azure bestand Vgx. Cache Python gebruiken.


## <a name="prerequisites"></a>Vereisten voor

Installeer [bestand Vgx. kopiëren](https://github.com/andymccurdy/redis-py).


## <a name="create-a-redis-cache-on-azure"></a>Een bestand Vgx. cache op Azure maken

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>De host-toets voor de naam van en toegang ophalen

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-the-non-ssl-endpoint"></a>De niet-SSL-eindpunt inschakelen

Bij sommige clients bestand Vgx. geen ondersteuning voor SSL en standaard de [niet-SSL-poort is uitgeschakeld voor nieuwe exemplaren van Azure bestand Vgx. Cache](cache-configure.md#access-ports). Op het moment van deze schrijven ondersteunen niet de client [bestand Vgx. kopiëren](https://github.com/andymccurdy/redis-py) SSL. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Een ander nummer toevoegen aan de cache en deze op te halen


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Vervang `<name>` met de Cachenaam van de en `key` met uw access-toets.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
