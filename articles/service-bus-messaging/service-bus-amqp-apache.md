<properties 
    pageTitle="Hoe u Apache Qpid Proton-C installeert op een VM Linux | Microsoft Azure"
    description="Het maken van een CentOS Linux VM met Azure virtuele Machines en bouwen en installeren van de Apache Qpid Proton-C-bibliotheek."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Apache Qpid Proton-C installeren op een Azure Linux VM

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

In deze sectie ziet u hoe u een CentOS Linux VM met Azure virtuele Machines maken en hoe downloaden, maken en installeren van de bibliotheek Apache Qpid Proton-C samen met de taal Python en PHP bindingen. Nadat u deze stappen uitvoert, is mogelijk om uit te voeren in de Python en PHP voorbeelden wordt geleverd bij deze handleiding.

De eerste stap is uitgevoerd met behulp van de [Azure klassieke portal][]. De volgende schermafbeelding ziet u hoe een CentOS VM met de naam "scott-centos" is gemaakt:

![Proton op een Azure Linux VM][0]

De portal wordt weergegeven na het inrichten het volgende:

![Proton op een Azure Linux VM][1]

Om te kunnen aanmelden bij de computer, moet u de eindpuntpoort voor SSH weten. U kunt deze waarde in de [portal van Azure klassieke][] verkrijgen door de zojuist gemaakte VM selecteren en te klikken op het tabblad **eindpunten** . De volgende schermafbeelding ziet u dat de openbare SSH poort voor deze computer 57146 is.

![Proton op een Azure Linux VM][2]

U kunt nu verbinding maken met SSH bij de computer. In dit voorbeeld wordt de stopverf hulpmiddel, zoals in de volgende schermafbeelding die u gebruikt:

![Proton op een Azure Linux VM][3]

Dit voorbeeld wordt de Python en PHP-apps voor de bibliotheken van de client Proton van Apache. Deze bibliotheken zijn beschikbaar voor downloaden van [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Het Leesmij-bestand in het pakket verdeling wordt uitgelegd dat de benodigde stappen voor het installeren van de afhankelijkheden en Proton maken. Hier volgt een overzicht van de stappen uit:

1.  Het configuratiebestand yum bewerken (/ etc/yum.conf) en commentaar uitsluiting op updates kernel kolomkoppen (\# uitsluiten = kernel\*). Dit is nodig om de compileerprogramma gcc installeren.

2.  De vereiste pakketten installeren:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  De bibliotheek Proton downloaden:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  De code Proton uit het archief verdeling halen:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Maak en installeren van de code die met de volgende stappen, die u uit het Leesmij-bestand hebt gemaakt:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Na deze stappen uit te voeren, is Proton geïnstalleerd op de computer en klaar voor gebruik.

## <a name="next-steps"></a>Volgende stappen

Klaar voor meer informatie? Ga naar de volgende koppeling:

- [Overzicht van de service Bus AMQP][]

[Overzicht van de service Bus AMQP]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure klassieke portal]: http://manage.windowsazure.com


