<properties
   pageTitle="Omgekeerde DNS-records voor uw Azure-mailservices via Azure CLI beheert | Microsoft Azure"
   description="Hoe u het omgekeerde DNS-records of PTR-records voor Azure-mailservices via de CLI Azure in resourcemanager beheren"
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-using-the-azure-cli"></a>Het omgekeerde DNS-records voor uw Azure-mailservices via de CLI Azure beheren

[AZURE.INCLUDE [DNS-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](dns-reverse-dns-record-operations-classic-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Validatie van omgekeerde DNS-records
Azure kunt dat een derde partij omgekeerde DNS-records toe te wijzen aan uw DNS-domeinen niet kunt maken, zodat alleen het maken van een omgekeerde DNS-record waarvoor het volgende geldt:

- De "ReverseFqdn" is hetzelfde als de "Fqdn' voor de resource van het openbare IP-adres waarvan deze is opgegeven, of de"Fqdn' voor een openbare IP-adres binnen hetzelfde abonnement bijvoorbeeld, "ReverseFqdn" is "contosoapp1.northus.cloudapp.azure.com.".

- De "ReverseFqdn" doorsturen wordt omgezet in de naam of het IP-het openbare IP-adres voor waarop deze is opgegeven, of naar een openbare IP-adres 'Fqdn' of IP-binnen hetzelfde abonnement verplaatsen, bijvoorbeeld "ReverseFqdn" is "app1.contoso.com." welke is een CName alias voor 'contosoapp1.northus.cloudapp.azure.com.'

Gegevensvalidatie controles worden alleen worden uitgevoerd wanneer de omgekeerde DNS-eigenschap voor een openbare IP-adres is ingesteld of gewijzigd. Periodiek opnieuw validatie wordt niet uitgevoerd.

## <a name="add-reverse-dns-to-existing-public-ip-addresses"></a>Omgekeerde DNS toevoegen aan bestaande openbare IP-adressen
U kunt omgekeerde DNS toevoegen aan een bestaande openbare IP-adres dat is met het instellen van de openbare ip-azure netwerk:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -f contosoapp1.westus.cloudapp.azure.com.

Als u omgekeerde DNS toevoegen aan een bestaande openbare IP-adres dat een DNS-naam nog niet heeft wilt, moet u ook een DNS-naam. U kunt toevoegen dit met behulp van de set van azure netwerk openbare ip-bereiken:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.

## <a name="create-a-public-ip-address-with-reverse-dns"></a>Een openbare IP-adres met omgekeerde DNS maken
U kunt toevoegen om een nieuwe openbare IP-adres met het omgekeerde DNS-eigenschap opgegeven met behulp van de azure netwerk openbare ip-maken:

    azure network public-ip create -n PublicIp3 -g NRP-DemoRG-PS -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.

## <a name="view-reverse-dns-for-existing-public-ip-addresses"></a>Weergave omgekeerde DNS-records voor bestaande openbare IP-adressen
U kunt de geconfigureerde waarde voor een bestaande openbare IP-adres met het weergeven van de openbare ip-azure netwerk bekijken:

    azure network public-ip show -n PublicIp3 -g NRP-DemoRG-PS

## <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Omgekeerde DNS verwijderen uit bestaande openbare IP-adressen
U kunt een omgekeerde DNS-eigenschap verwijderen uit een bestaande openbare IP-adres met behulp van azure netwerk openbare ip-set. Dit is gedaan door de waarde van de eigenschap ReverseFqdn In blank:

    azure network public-ip set -n PublicIp3 -g NRP-DemoRG-PS –f “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]
