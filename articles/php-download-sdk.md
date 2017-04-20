<properties
    pageTitle="Downloaden van de Azure SDK voor PHP"
    description="Informatie over het downloaden en installeren van de Azure-SDK voor PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Downloaden van de Azure SDK voor PHP

## <a name="overview"></a>Overzicht

De SDK Azure voor PHP bevat onderdelen waarmee u kunt de ontwikkeling, implementatie en PHP-toepassingen voor Azure beheren. De SDK Azure voor PHP bevat de volgende gegevens op:

* **De PHP clientbibliotheken voor Azure**. Deze bibliotheken class biedt een interface voor toegang tot Azure functies, zoals data management-services en cloud services.  
* **De Interface van de Azure opdrachtregel voor Mac, Linux, en Windows (Azure CLI)**. Dit is een reeks opdrachten voor implementatie en het beheer van Azure services, zoals Azure Websites en Azure virtuele Machines. De werkzaamheden Azure CLI voor elk platform, inclusief Mac, Linux en Windows.
* **Azure PowerShell (alleen Windows)**. Dit is een verzameling PowerShell-cmdlets voor implementatie en het beheer van Azure-Services, zoals Cloudservices en virtuele Machines.
* **De Azure Emulators (alleen Windows)**. De opslag en rekenkracht emulators zijn lokale emulators van cloudservices en data management-services waarmee u kunt een toepassing lokaal testen. De Emulators Azure alleen in Windows worden uitgevoerd.

De secties hierna wordt beschreven hoe downloaden en installeren van de onderdelen die hierboven is beschreven.

De instructies in dit onderwerp wordt ervan uitgegaan dat er [PHP] [ install-php] is geïnstalleerd.

> [AZURE.NOTE] U moet PHP 5,5 of hoger voor het gebruik van de bibliotheken van de client PHP voor Azure hebben.

##<a name="php-client-libraries-for-azure"></a>PHP clientbibliotheken voor Azure

De bibliotheken van de Client PHP voor Azure biedt een interface voor toegang tot Azure functies, zoals data management-services en -services, op een ander besturingssysteem cloud. Deze bibliotheken kunnen worden geïnstalleerd via de Composer.

Zie voor informatie over het gebruik van de bibliotheken van de Client PHP voor Azure [het gebruik van de Service Blob][blob-service], [het gebruik van de Service van de tabel] [ table-service] en [het gebruik van de Service wachtrij][queue-service].

###<a name="install-via-composer"></a>Installeren via Composer

1. [Cijfer installeren][install-git].


    > [AZURE.NOTE] In Windows moet u ook het cijfer uitvoerbare toevoegen aan uw omgevingsvariabele PATH.

2. Een bestand met de naam **composer.json** in de hoofdmap van uw project maken en voeg de volgende code toe:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Download ** [composer.phar] [ composer-phar] ** in de hoofdmap van uw project.

4. Open een opdrachtprompt en dit in de hoofdmap van uw project uitvoeren

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell en Azure Emulators

Azure PowerShell is een verzameling PowerShell-cmdlets voor implementatie en het beheer van Azure-Services (zoals Cloudservices en virtuele Machines). De Emulators Azure zijn emulators van cloudservices en data management-services waarmee u kunt een toepassing lokaal testen. Deze onderdelen worden ondersteund alleen Windows.

De aanbevolen wijze voor de installatie Azure PowerShell en de Emulators Azure is met het [Installatieprogramma van Microsoft Web Platform][download-wpi]. Houd er rekening mee dat u kunt ook andere onderdelen voor ontwikkelaars, zoals PHP, SQL Server, de Microsoft-Drivers voor SQL Server voor PHP en WebMatrix installeren.

Zie [hoe u Azure PowerShell gebruiken]voor informatie over het gebruik van Azure PowerShell[powershell-tools].

##<a name="azure-cli"></a>Azure CLI

De CLI Azure is een verzameling opdrachten voor implementatie en het beheer van Azure services, zoals Azure Websites en Azure virtuele Machines. Zie [de CLI Azure installeren](xplat-cli-install.md)voor informatie over het installeren van Azure CLI.

## <a name="next-steps"></a>Volgende stappen

Zie het [PHP Developer Center](/develop/php/)voor meer informatie.


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
