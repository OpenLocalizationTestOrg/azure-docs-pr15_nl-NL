<properties
    pageTitle="Aan de slag met Relay hybride verbindingen | Microsoft Azure"
    description="Een toepassing voor knooppunt console schrijven voor hybride verbindingen"
    services="service-bus"
    documentationCenter="node"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="node"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Aan de slag met Relay hybride-verbindingen

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Wat wordt worden uitgevoerd

Aangezien hybride verbindingen vereist is voor zowel een client als onderdeel van een server, maakt we twee consoletoepassingen in deze zelfstudie. Hier volgen de stappen:

1. Een naamruimte Relay, met behulp van de Azure portal maken.

2. De verbinding van een hybride, met behulp van de Azure portal maken.

3. Een server consoletoepassing voor het ontvangen van berichten schrijven.

4. Schrijf een client consoletoepassing om berichten te verzenden.

## <a name="prerequisites"></a>Vereisten voor

1. [Node.js](https://nodejs.org/en/) (in dit voorbeeld wordt knooppunt 7.0 gebruikt).

2. Een Azure-abonnement.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. een naamruimte met behulp van de Azure portal maken

Als u al een Relay naamruimte hebt gemaakt, gaat u naar de sectie [een hybride verbinding maken met behulp van de Azure portal](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. een hybride verbinding maken met de portal van Azure

Als u al een hybride verbinding hebt gemaakt, gaat u naar de sectie [maken een servertoepassing](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. een servertoepassing (luisteraar ervan af) maken

Als u wilt beluisteren en u ontvangt berichten van de Relay, schrijft we een Node.js console-programma.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. een clienttoepassing (afzender) maken

Als u wilt verzenden naar de Relay, schrijft we een Node.js console-programma.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. de toepassingen uitvoeren

1. Voer de servertoepassing.

2. De clienttoepassing uitvoeren en typ wat tekst.

3. Zorg ervoor dat de server-toepassing-console Hiermee kunt u de tekst die is ingevoerd in de clienttoepassing.

![voorlopig-toepassingen](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gefeliciteerd, u kunt een end-to-end-toepassing voor hybride verbindingen hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen:

- [Relay Veelgestelde vragen](relay-faq.md)
- [Een naamruimte maken](relay-create-namespace-portal.md)
- [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Aan de slag met knooppunt](relay-hybrid-connections-node-get-started.md)