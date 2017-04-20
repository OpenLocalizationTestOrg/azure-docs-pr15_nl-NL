<properties
   pageTitle="Uw ontwikkelomgeving in Mac OS X instellen | Microsoft Azure"
   description="Installeer de runtime, SDK en hulpprogramma's en maken van een cluster plaatselijke ontwikkeling. Na het voltooien van deze instelling, bent u klaar om u te maken van toepassingen op Mac OS X."
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
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Uw ontwikkelomgeving in Mac OS X instellen

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

U kunt maken op stof Service-toepassingen om uit te voeren op Linux clusters met Mac OS X. In dit artikel wordt uitgelegd hoe u uw Mac voor de ontwikkeling van instellen.

## <a name="prerequisites"></a>Vereisten voor

Service stof wordt niet standaard uitgevoerd op OS X. Wij bieden een vooraf geconfigureerde Ubuntu virtuele machine met Vagrant en VirtualBox wilt uitvoeren door een lokale Service stof cluster. Voordat u begint, moet u het volgende nodig:

- [Vagrant (v1.8.4 of hoger)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>De lokale VM maken

Ga als volgt te werk als u wilt maken van de lokale VM met een cluster van de Service stof 5 knooppunten:

1. De cessies‑retrocessies Vagrantfile klonen

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Navigeer naar de lokale kopie van de cessies‑retrocessies

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Optioneel) De standaardinstellingen voor VM wijzigen

    Standaard is de lokale VM geconfigureerd als volgt:

    - 3 GB geheugen toegewezen
    - Persoonlijke host netwerk geconfigureerd IP-192.168.50.50 indirecte verkeer uit de Mac-host inschakelen

    U kunt een van deze instellingen wijzigen of andere configuratie toevoegen aan de VM in de Vagrantfile. Zie de [documentatie van Vagrant](http://www.vagrantup.com/docs) voor de volledige lijst met configuratieopties.

4. De VM maken

    ```bash
    vagrant up
    ```

    Deze stap de vooraf geconfigureerde VM-afbeelding, het lokaal en stel vervolgens een lokale Service stof cluster in deze opstarten gedownload. U moet onverwacht enkele minuten duren. Als de installatie is voltooid, ziet u een bericht in de uitvoer die aangeeft dat het cluster wordt gestart.

    ![Cluster setup starten na VM inrichting][cluster-setup-script]

5. Test of het cluster heeft correct zijn ingesteld door het navigeren naar de Service stof Explorer bij http://192.168.50.50:19080/Explorer (ervan uitgaande dat u de standaard privé netwerk IP bewaard).

    ![Service stof Explorer worden weergegeven op de Mac-host][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>De Service stof-invoegtoepassing installeren voor Eclips Neon (optioneel)

Service stof vindt u een invoegtoepassing voor de Eclips Neon IDE die het proces van het maken en implementeren van Java-services kunt vereenvoudigen.

1. Zorg ervoor dat u Buildship versie 1.0.17 of hoger is geïnstalleerd in Eclips. U kunt de versies van geïnstalleerde onderdelen controleren door te kiezen **Help > Installatiedetails**. U kunt bijwerken Buildship volgens de instructies [hier][buildship-update].

2. Als u wilt de Service stof-invoegtoepassing hebt geïnstalleerd, kiest u **Help > nieuwe Software installeren...**

3. Voer in het tekstvak 'Werken met': http://dl.windowsazure.com/eclipse/servicefabric.

4. Klik op toevoegen.

    ![Eclips Neon Plug Service stof][sf-eclipse-plugin-install]

5. Kies de Service stof-invoegtoepassing en klik op volgende.

6. Volg de stappen in de installatie en accepteer de gebruiksrechtovereenkomst.

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste stof Service-toepassing voor Linux maken](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Een Service stof cluster maken in de portal van Azure](service-fabric-cluster-creation-via-portal.md)
- [Een Service stof cluster met bronbeheer Azure maken](service-fabric-cluster-creation-via-arm.md)
- [Meer informatie over het model van de toepassing Service stof](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
