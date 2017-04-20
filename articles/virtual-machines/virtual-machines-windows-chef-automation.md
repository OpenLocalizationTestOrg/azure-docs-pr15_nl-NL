<properties
   pageTitle="Implementatie van de Azure virtuele machines met Chef | Microsoft Azure"
   description="Informatie over het gebruik van Chef moet geautomatiseerde VM installatie en configuratie van Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Implementatie van de Azure virtuele machines met Chef automatiseren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Chef is een prima hulpmiddel om automation te bieden en staat configuraties gewenst.

Met onze meest recente versie van de cloud-api maakt Chef naadloze integratie met Azure, waardoor u de mogelijkheid voor inrichten en implementeren van lidstaten van de configuratie door één opdracht.

In dit artikel ziet ik u hoe u uw omgeving Chef inrichten van Azure virtuele machines en leest u een beleid of "Kookboek" maken en deze vervolgens implementeert dit kookboek met een Azure virtuele machine instellen.

U kunt nu beginnen!

## <a name="chef-basics"></a>Basisbeginselen van chef

Voordat u begint, stel ik dat u de basisconcepten van Chef bekijken. Er is handig materiaalresource <a href="http://www.chef.io/chef" target="_blank">hier</a> en ik raden u hebt een snelle lezen voordat u deze procedure. Ik zal de basisbeginselen echter samenvatting voordat we aan de slag.

In het volgende diagram ziet u de geavanceerde Chef architectuur.

![][2]

Chef heeft drie hoofdonderdelen architectuur: Chef Server, Chef-Client (knooppunt) en Chef Workstation.

De Chef Server onze punt management en er zijn twee opties voor de Chef-Server: een gehoste oplossing of een on-premises implementatie-oplossing. We gebruiken een gehoste oplossing.

De Chef-Client (knooppunt) is de agent die wordt weergegeven op de servers die u beheert.

Het werkstation Chef is onze beheerstation waar we ons beleid maken en uitvoeren van onze opdrachten voor het beheer. We voert u de opdracht **mes** vanaf het werkstation Chef voor het beheren van onze infrastructuur.

Er is ook het concept van het 'Cookbooks' en 'Recepten'. Hierna ziet u effectief het beleid we definiëren en toepassen op onze servers.

## <a name="preparing-the-workstation"></a>Het werkstation voorbereiden

U kunt eerst het werkstation voorbereiden. Ik gebruik een standaard Windows-werkstation. Moeten we een map als u wilt bewaren van onze configuratiebestanden en cookbooks maken.

Maak eerst een map met de naam van C:\chef.

De tweede map c:\chef\cookbooks maken.

Nu moeten we onze Azure instellingenbestand downloaden zodat Chef met onze Azure-abonnement communiceren kan.

Download uw instellingen uit [hier](https://manage.windowsazure.com/publishsettings/)publiceren.

Sla het bestand van de instellingen publiceren in C:\chef.

##<a name="creating-a-managed-chef-account"></a>Een beheerde Chef-account maken

Registreren voor een gehoste Chef account [hier](https://manage.chef.io/signup).

Tijdens het aanmeldingsproces, wordt u gevraagd om te maken van een nieuwe organisatie.

![][3]

Nadat uw organisatie is gemaakt, kunt u de starterskit downloaden.

![][4]

> [AZURE.NOTE] Als u gevraagd waarschuwing wordt dat uw sleutels worden opnieuw ingesteld, is het ok om door te gaan als er geen bestaande infrastructuur nog geconfigureerd.

Dit starter kit zip-bestand bevat uw organisatie configuratiebestanden en de toetsen.

##<a name="configuring-the-chef-workstation"></a>Het werkstation Chef configureren

Pak de inhoud van de chef-starter.zip naar C:\chef.

Kopieer alle bestanden onder chef-starter\chef-cessies‑retrocessies\.chef naar de map c:\chef.

Uw adreslijst ziet er nu ongeveer zo in het volgende voorbeeld.

![][5]

U hebt nu vier bestanden, met inbegrip van de Azure publishing-bestand in de hoofdmap van c:\chef.

De PEM-bestanden bevatten uw organisatie en persoonlijke sleutels van de beheerder voor communicatie terwijl het bestand knife.rb uw configuratie mes bevat. Moeten we het knife.rb-bestand bewerken.

Open het bestand in de editor van uw keuze en wijzigen van de 'cookbook_path' doordat de /... / van het pad, zodat deze wordt weergegeven zoals volgende.

    cookbook_path  ["#{current_dir}/cookbooks"]

Ook toevoegen de volgende regel de naam van uw Azure na te denken instellingenbestand publiceren.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Uw bestand knife.rb ziet er nu vergelijkbaar met het volgende voorbeeld.

![][6]

Deze regels zorgt ervoor dat dat mes verwijst naar de map cookbooks onder c:\chef\cookbooks en onze instellingenbestand Azure publiceren ook worden gebruikt tijdens Azure bewerkingen.

## <a name="installing-the-chef-development-kit"></a>Installatie van de Chef Development Kit

Volgende [downloadt en installeert u](http://downloads.getchef.com/chef-dk/windows) de ChefDK (Chef Development Kit) voor het instellen van uw werkstation Chef.

![][7]

In de standaardlocatie van c:\opscode installeren. Deze installatie duurt ongeveer 10 minuten.

Bevestig dat uw variabele pad bevat items voor C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Als dat niet er, zorg ervoor dat u deze paden toevoegt.

*OPMERKING DE VOLGORDE VAN HET PAD IS BELANGRIJK!* Als uw opscode paden in de juiste volgorde staan dan hebt u problemen.

Start opnieuw op uw werkstation voordat u verdergaat.

Vervolgens installeert we de extensie mes Azure. Hierdoor mes met de 'Azure-invoegtoepassing'.

Voer de volgende opdracht.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Het argument – pre zorgt ervoor dat u de nieuwste RC versie van de Azure-invoegtoepassing voor mes die toegang tot de laatste set API's biedt ontvangt.

Is het mogelijk dat een aantal afhankelijkheden ook worden geïnstalleerd op hetzelfde moment.

![][8]


Om ervoor te zorgen dat alles correct is geconfigureerd, moet u de volgende opdracht uitvoeren.

    knife azure image list

Als alles correct is geconfigureerd, ziet u een lijst met beschikbare Azure afbeeldingen door te schuiven.

Gefeliciteerd. Het werkstation is ingesteld!

##<a name="creating-a-cookbook"></a>Een kookboek maken

Een kookboek wordt gebruikt door Chef definiëren van een reeks opdrachten die u wilt uitvoeren op uw beheerde client. Maken van een kookboek eenvoudig is en we de opdracht **chef genereren kookboek** gebruiken om te genereren van onze kookboek-sjabloon. Ik zullen contact met mijn webserver kookboek als ik een beleid dat automatisch IIS geïmplementeerd zou willen.

Voer de volgende opdracht onder het telefoonboek van uw C:\Chef.

    chef generate cookbook webserver

Hiermee wordt een set bestanden onder de map C:\Chef\cookbooks\webserver genereren. Nu moeten we de reeks opdrachten die we onze Chef-client wilt uitvoeren op onze beheerde virtuele machine wilt definiëren.

De opdrachten zijn opgeslagen in het bestand default.rb. In dit bestand moet ik een reeks opdrachten die IIS is geïnstalleerd, wordt gestart IIS en een sjabloonbestand kopiëren naar de wwwroot-map definiëren.

Het bestand C:\chef\cookbooks\webserver\recipes\default.rb wijzigen en voeg de volgende regels.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Sla het bestand als u klaar bent.

## <a name="creating-a-template"></a>Een sjabloon maken

Zoals we eerder is vermeld, moeten we een sjabloonbestand die worden gebruikt als onze pagina default.html genereren.

Voer de volgende opdracht voor het genereren van de sjabloon.

    chef generate template webserver Default.htm

Nu kunt u navigeren naar het bestand C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Bewerk het bestand door enkele eenvoudige 'Hallo wereld"HTML-code toe te voegen en sla het bestand.



## <a name="upload-the-cookbook-to-the-chef-server"></a>De kookboek uploaden naar de Server Chef

In deze stap zijn er een kopie van de kookboek die we hebben gemaakt op de lokale computer en deze naar de Chef Hosted Server wordt geüpload. Zodra geüpload, wordt de kookboek verschijnt onder het tabblad **beleid** .

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Een virtuele machine met Mes Azure implementeren

Er wordt nu een Azure virtuele machines implementeren en toepassen van de 'Webserver' kookboek die wordt onze IIS web service en de standaard webpagina installeren.

Gebruik hiervoor de opdracht **mes azure server maken** .

Ben voorbeeld van de opdracht volgende weergegeven.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

De parameters zijn geen uitleg. Vervangen door uw bepaalde variabelen en uitvoeren.

> [AZURE.NOTE] Via de de opdrachtregel ik ben ook automatiseren mijn eindpunt netwerk filterregels met behulp van de parameter – tcp-eindpunten. Ik heb geopend omhoog poort 80 en 3389 voor toegang tot mijn webpagina en RDP-sessie.

Nadat u de opdracht uitvoert, gaat u naar de Azure-portal en ziet u de computer begint inrichten.

![][13]

De opdrachtprompt verschijnt het volgende.

![][10]

Zodra de implementatie voltooid is, moeten we kunnen zijn via verbinding maken met de webservice poort 80 als we de poort had geopend als we de virtuele machine met de opdracht mes Azure deze is ingericht. Als deze virtuele machine de enige virtuele machine in mijn-cloudservice is, moet ik het verbinding maken met de url van de cloud.

![][11]

Zoals u ziet, krijg ik creative met mijn HTML-code.

Vergeet niet dat we kunnen ook verbinding maakt via een RDP-sessie vanuit de Azure klassieke portal via poort 3389.

Ik hopen dat dit is handig. Gaat en start u de infrastructuur van uw als code reis met Azure op vandaag nog!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
