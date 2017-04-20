<properties 
   pageTitle="Weergeven en het wijzigen van hostnamen | Microsoft Azure"
   description="Het weergeven en wijzigen van hostnamen voor Azure virtuele machines met webonderdelen en werknemer rollen voor mailnamen omzetten"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Weergeven en hostnamen wijzigen

Als u wilt dat uw rol exemplaren u wilt verwijzen op hostnaam, moet u de waarde voor de hostnaam instellen in het configuratiebestand service voor elke rol. U doet u dat door de gewenste hostnaam toevoegen aan het kenmerk **vmName** van de **rol** -element. De waarde van het kenmerk **vmName** wordt gebruikt als basis voor de hostnaam van elk exemplaar van de rol. Bijvoorbeeld als **vmName** *webrole* is en er drie exemplaren van die rol zijn, zijn de hostnamen van de exemplaren *webrole0*, *webrole1*en *webrole2*. U hoeft niet te geven een naam voor de virtuele machines in het configuratiebestand, omdat de hostnaam voor een virtuele machine is ingevuld op basis van de naam van de virtuele machine. Zie [Azure Service configuratieschema (.cscfg bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx) voor meer informatie over het configureren van een Microsoft Azure-service

## <a name="viewing-hostnames"></a>Hostnamen weergeven

U kunt de hostnamen van virtuele machines en exemplaren van de rol in een cloudservice weergeven met behulp van een van de onderstaande hulpmiddelen.

### <a name="azure-portal"></a>Azure-Portal

U kunt de [portal van Azure](http://portal.azure.com) hostnamen voor virtuele machines weergeven op het blad Overzicht voor een virtuele machine. Houd er rekening mee dat het blad ziet u een waarde voor de **naam** en **Host Name**. Hoewel ze zijn in eerste instantie hetzelfde, wordt er bij het wijzigen van de hostnaam niet de naam van de virtuele machine of het exemplaar van de rol gewijzigd.

Rol exemplaren kunnen ook worden weergegeven in de portal van Azure, maar wanneer u de exemplaren in een cloudservice, de hostnaam niet wordt weergegeven. Ziet u een naam voor elk exemplaar, maar die naam geeft geen de hostnaam.

### <a name="service-configuration-file"></a>Service-configuratiebestand

U kunt het configuratiebestand service voor een uitgevouwen service downloaden van het blad **configureren** van de service in de portal van Azure. Vervolgens kunt u zoeken voor het kenmerk **vmName** van het element **de naam van de rol** om de hostnaam van de weer te geven. Houd er rekening mee dat deze hostnaam wordt gebruikt als basis voor de hostnaam van elk exemplaar van de rol. Bijvoorbeeld als **vmName** *webrole* is en er drie exemplaren van die rol zijn, zijn de hostnamen van de exemplaren *webrole0*, *webrole1*en *webrole2*.

### <a name="remote-desktop"></a>Extern bureaublad

Nadat u extern bureaublad (Windows), Windows PowerShell externe (Windows) of RAS-SSH (Linux en Windows) naar uw virtuele machines of rol exemplaren hebt ingeschakeld, kunt u de hostnaam van een actieve verbinding met extern bureaublad kunt op verschillende manieren weergeven:

- Typ hostname bij de opdrachtprompt of SSH terminal.

- Typ ipconfig/alle bij de opdrachtprompt (alleen Windows).

- De naam van de computer weergeven in de systeeminstellingen (alleen Windows).

### <a name="azure-service-management-rest-api"></a>Azure servicebeheer REST API

Volg deze instructies in een REST-client:

1. Zorg ervoor dat er een clientcertificaat verbinding maken met de portal van Azure. Als u een clientcertificaat, volgt u de stappen in gepresenteerd [hoe: downloaden en publiceren importinstellingen en informatie over abonnement](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Stel de invoer in een koptekst met de naam met een waarde van 2013-01-11 x-ms-versie.

1. Een vergaderverzoek verzenden in de volgende indeling: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<servicenaam\>?embed details = true

1. Zoek naar het element **HostName** voor elk element **RoleInstance** .

>[AZURE.WARNING] U kunt ook het achtervoegsel interne domeinnaam voor uw cloudservice van het antwoord van de oproep REST door te schakelen van het element **InternalDnsSuffix** , of door ipconfig/alles vanaf een opdrachtprompt weergeven in een extern bureaublad-sessie (Windows) of door lopende kat /etc/resolv.conf vanaf een terminal SSH (Linux).

## <a name="modifying-a-hostname"></a>Een hostname wijzigen

U kunt de hostnaam VM of rol exemplaar wijzigen door het uploaden van een bestand van de configuratie gewijzigde service of door de naam van de computer van een extern bureaublad-sessie te.

## <a name="next-steps"></a>Volgende stappen

[Naamresolutie (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure-Service configuratieschema (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093)

[DNS-instellingen voor gebruik van netwerk configuratiebestanden opgeven](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
