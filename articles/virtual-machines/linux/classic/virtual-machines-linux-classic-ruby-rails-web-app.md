<properties
    pageTitle="Een Ruby op Rails website op een VM Linux hosten | Microsoft Azure"
    description="Instellen en een Ruby op Rails gebaseerde website op Azure met een Linux virtuele machine hosten."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby op Rails webtoepassing op een Azure-VM

Deze zelfstudie wordt getoond hoe een Ruby op Rails website op Azure hosten met een VM Linux.  

Deze zelfstudie is gevalideerd met Ubuntu Server 14.04 LTS. Als u een ander Linux-verdeling gebruikt, moet u mogelijk de stappen om te kunnen installeren Rails wijzigen.

> [AZURE.IMPORTANT] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [resourcemanager en klassiek](../../../resource-manager-deployment-model.md).  In dit artikel beschreven hoe u met het implementatiemodel klassieke. Microsoft raadt meest nieuwe implementaties het model resourcemanager gebruiken.

## <a name="create-an-azure-vm"></a>Een Azure VM maken

Beginnen met het maken van een VM Azure met een afbeelding Linux.

Als u wilt de VM hebt gemaakt, kunt u de portal van Azure klassieke of de Azure-Interface met opdrachtregel (CLI).

### <a name="azure-management-portal"></a>Azure Management Portal

1. Meld u aan bij de [portal van Azure klassieke](http://manage.windowsazure.com)
2. Klik op **nieuwe** > **berekenen** > **VM** > **snel tabellen maken**. Selecteer de afbeelding van een Linux.
3. Een wachtwoord invoeren.

Nadat de VM is ingericht, klikt u op de naam van de VM en klik op **Dashboard**. Zoek het eindpunt SSH, vermeld onder **SSH Details**.

### <a name="azure-cli"></a>Azure CLI

Volg de stappen in [maken een virtuele Machine uitgevoerd Linux][vm-instructions].

Nadat de VM is ingericht, kunt u het eindpunt SSH verkrijgen door de volgende opdracht uit te voeren:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Ruby op Rails installeren

1. Met SSH verbinding maken met de VM.

2. Uit de sessie SSH, gebruikt u de volgende opdrachten Ruby installeren op de VM:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    De installatie kan een paar minuten duren. Wanneer deze is voltooid, gebruikt u de volgende opdracht uit om te bevestigen dat Ruby is geïnstalleerd:

        ruby -v

    Hiermee wordt de versie van Ruby die is geïnstalleerd.

3. Gebruik de volgende opdracht uit om te Rails installeren:

        sudo gem install rails --no-rdoc --no-ri -V

    Gebruik de--geen-rdoc en--geen ri vlaggen om over te slaan met het installeren van de documentatie, dat wil sneller zeggen.
    Deze opdracht duurt waarschijnlijk veel tijd moet worden uitgevoerd, zodat de -V toevoegt, wordt informatie over de voortgang van de installatie weergegeven.

## <a name="create-and-run-an-app"></a>Maken en uitvoeren van een app

Terwijl u nog steeds aangemeld via SSH, voert u de volgende opdrachten:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

De opdracht voor [nieuwe](http://guides.rubyonrails.org/command_line.html#rails-new) Hiermee maakt u een nieuwe Rails-app. De opdracht [server](http://guides.rubyonrails.org/command_line.html#rails-server) Start de webserver WEBrick die wordt geleverd met Rails. (Voor gebruik in productieomgeving, u zou waarschijnlijk wilt gebruiken een andere server, zoals Eenhoorn of personen.)

Hier ziet u de volgende strekking uitvoer.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Een eindpunt toevoegen

1. Ga naar de [portal van Azure klassieke] [ management-portal] en selecteert u uw VM.

    ![VM-lijst][vmlist]

2. Selecteer **EINDPUNTEN** boven aan de pagina en klik vervolgens op **+ EINDPUNT toevoegen** onder aan de pagina.

    ![pagina eindpunten][endpoints]

3. Selecteer 'Toevoegen een zelfstandig product-eindpunt' in het dialoogvenster **EINDPUNT toevoegen** en klik op **de pijl-rechts** .

    ![dialoogvenster voor de nieuwe eindpunt][new-endpoint1]

3. Voer de volgende gegevens in de volgende pagina van dialoogvenster:

    * **Naam**: HTTP

    * **PROTOCOL**: TCP

    * **Openbare poort**: 80

    * **Privé poort**: 3000

    Hiermee maakt u een openbare poort 80 die verkeer naar de persoonlijke poort van 3000 doorsturen, waar de server Rails luistert.

    ![dialoogvenster voor de nieuwe eindpunt][new-endpoint]

4. Klik op het vinkje om op te slaan van het eindpunt.

5. Een bericht ziet waarin wordt aangegeven **UPDATE IN uitvoering**. Wanneer dit bericht is verdwenen, wordt het eindpunt actief is. U mag de toepassing nu testen door te gaan naar de naam van de DNS van uw virtuele computer. De website ziet er ongeveer als volgt uit:

    ![standaardpagina rails][default-rails-cloud]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u meestal de stappen handmatig. In een productieomgeving, zou u schrijven van uw app op een ontwikkelcomputer en het dashboard implementeren naar de VM Azure. De meeste productieomgevingen hosten ook de toepassing Rails in combinatie met een ander serverproces zoals Apache of NginX, welke ingangen aanvragen zoekresultaten omleiden naar meerdere exemplaren van de toepassing Rails en op statische resources. Zie http://rubyonrails.org/deploy/ voor meer informatie.

Meer informatie over Ruby op Rails, gaat u naar de [Ruby op Rails hulplijnen][rails-guides].

Als u wilt gebruiken Azure services van uw Ruby-toepassing, raadpleegt u:

* [Opslag van ongestructureerde gegevens BLOB's gebruiken][blobs]

* [Store sleutel/waardeparen met behulp van tabellen][tables]

* [Dienen hoge bandbreedte inhoud met het netwerk van de bezorging van inhoud][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
