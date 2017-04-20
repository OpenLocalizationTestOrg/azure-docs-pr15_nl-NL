<properties
   pageTitle="Instellen van uw ontwikkelomgeving op Linux | Microsoft Azure"
   description="Installeer de runtime en SDK en maak een cluster plaatselijke ontwikkeling op Linux. Na het voltooien van deze instelling, bent u klaar om u te maken van toepassingen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Uw ontwikkelomgeving op Linux voorbereiden


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Installeer de runtime en de algemene SDK om te implementeren en [stof van Azure-Service-toepassingen](service-fabric-application-model.md) worden uitgevoerd op de computer van de ontwikkeling Linux. U kunt ook optioneel SDK's voor Java en .NET Core installeren.

## <a name="prerequisites"></a>Vereisten voor
### <a name="supported-operating-system-versions"></a>Ondersteunde besturingssystemen
De volgende besturingssystemen worden ondersteund voor de ontwikkeling:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Bijwerken van uw huis bronnen

Als u wilt de SDK en de bijbehorende runtime-pakket via enz ontvang hebt geïnstalleerd, moet u eerst uw huis bronnen bijwerken.

1. Open een terminal.
2. De Service stof cessies‑retrocessies toevoegen aan uw lijst met bronnen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. De nieuwe GPG-sleutel toevoegen aan uw huis keyring.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Vernieuw uw pakket lijsten op basis van de toegevoegde opslagplaatsen.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Installeren en instellen van de SDK

Wanneer uw bronnen worden bijgewerkt, kunt u de SDK installeren.

1. Het pakket Service stof SDK installeren. U wordt gevraagd om te bevestigen van de installatie en om een gebruiksrechtovereenkomst te accepteren.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. De SDK setup-script uitvoeren.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>De Azure platforms CLI instellen

De [Azure platforms CLI] [ azure-xplat-cli-github] bevat opdrachten voor de communicatie met Service stof entiteiten, inclusief clusters en toepassingen. Dit is gebaseerd op Node.js dat het geval is [Zorg ervoor dat u knooppunt hebt geïnstalleerd] [ install-node] voordat u verder gaat met de onderstaande instructies.

1. De cessies‑retrocessies github naar uw computer ontwikkeling klonen.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Ga naar de gekloonde cessies‑retrocessies en installeren van de CLI afhankelijkheden met het knooppunt pakket Manager (npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Maken een symlink van de opslaglocatie/azure-map van de gekloonde cessies‑retrocessies naar /usr/bin/azure zodat deze wordt toegevoegd aan het pad en opdrachten beschikbaar vanuit een andere map zijn.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Tot slot inschakelen automatisch aanvullen Service stof opdrachten.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Een lokale cluster instellen

Als alles is geïnstalleerd, moet u mogelijk zijn een lokale cluster starten.

1. Het cluster setup-script uitvoeren.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Open een webbrowser en navigeer naar http://localhost:19080/Explorer. Als het cluster is gestart, ziet u het dashboard Service stof Explorer.

    ![Service stof Explorer op Linux][sfx-linux]

U bent nu kunt implementeren vooraf gedefinieerde Service stof toepassing-pakketten of nieuwe bestanden op basis van Gast containers of Gast uitvoerbare bestanden. Als u wilt maken van nieuwe services die via de Java of .NET Core SDK's, voer de volgende stappen uit de optionele installatie.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Installeren van de invoegtoepassing Java SDK en Eclips Neon (optioneel)

De Java-SDK biedt de bibliotheken en sjablonen die zijn vereist voor het maken van Service configuratieservices Java gebruiken.

1. Het pakket Java SDK installeren.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. De SDK setup-script uitvoeren.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

U kunt de Eclips-invoegtoepassing voor Service stof uit Eclips Neon IDE installeren.

1. Zorg ervoor dat u Buildship versie 1.0.17 of hoger is geïnstalleerd in Eclips. U kunt de versies van geïnstalleerde onderdelen controleren door te kiezen **Help > Installatiedetails**. U kunt bijwerken Buildship volgens de instructies [hier][buildship-update].

2. Als u wilt de Service stof-invoegtoepassing hebt geïnstalleerd, kiest u **Help > nieuwe Software installeren...**

3. Voer in het tekstvak 'Werken met': http://dl.windowsazure.com/eclipse/servicefabric

4. Klik op toevoegen.

    ![Eclips-invoegtoepassing][sf-eclipse-plugin]

5. Kies de Service stof-invoegtoepassing en klik op volgende.

6. Volg de stappen in de installatie en accepteer de gebruiksrechtovereenkomst.

## <a name="install-the-net-core-sdk-optional"></a>Installeer de SDK van .NET Core (optioneel)

De .NET Core SDK biedt de bibliotheken en sjablonen die zijn vereist voor het maken van Service configuratieservices met meerdere platforms .NET Core.

1. Het pakket .NET Core SDK installeren.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. De SDK setup-script uitvoeren.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Java-toepassing op Linux maken](service-fabric-create-your-first-linux-application-with-java.md)

- [Uw ontwikkelomgeving op OSX voorbereiden](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
