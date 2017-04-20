<properties
   pageTitle="Omgekeerde DNS-records voor uw Azure (klassieke)-services via PowerShell beheert | Microsoft Azure"
   description="Hoe u het omgekeerde DNS-records of PTR-records voor Azure-services via PowerShell in het implementatiemodel klassieke beheren. "
   services="DNS"
   documentationCenter="na"
   authors="s-malone"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="DNS"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/28/2016"
   ms.author="smalone" />

# <a name="how-to-manage-reverse-dns-records-for-your-azure-services-classic-using-azure-powershell"></a>Het omgekeerde DNS-records voor uw Azure services (klassieke) via Azure PowerShell beheren

[AZURE.INCLUDE [dns-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](dns-reverse-dns-record-operations-ps.md).

## <a name="validation-of-reverse-dns-records"></a>Validatie van omgekeerde DNS-records
Azure kunt dat een derde partij omgekeerde DNS-records toe te wijzen aan uw DNS-domeinen niet kunt maken, zodat alleen het maken van een omgekeerde DNS-record waarvoor het volgende geldt:

- Het omgekeerde DNS FQDN is de naam van de Cloudservice waarvan deze is opgegeven of de naam van een Cloudservice binnen hetzelfde abonnement bijvoorbeeld, omgekeerde DNS 'contosoapp1.cloudapp.net.' is.
- Doorschakelen de omgekeerde DNS FQDN wordt omgezet in de naam of IP van de Cloud-Service voor waarop deze is opgegeven of naar een Cloudservice naam of IP-binnen hetzelfde abonnement bijvoorbeeld omgekeerde DNS is "app1.contoso.com." Dit is een CName alias voor contosoapp1.cloudapp.net.

Gegevensvalidatie controles worden alleen worden uitgevoerd wanneer de omgekeerde DNS-eigenschap voor een Cloudservice is ingesteld of gewijzigd. Periodiek opnieuw validatie wordt niet uitgevoerd.

## <a name="add-reverse-dns-to-existing-cloud-services"></a>Omgekeerde DNS toevoegen aan bestaande Cloudservices
U kunt een omgekeerde DNS-record toevoegen aan een bestaande Cloud-Service met de cmdlet 'Set-AzureService':

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="create-a-cloud-service-with-reverse-dns"></a>Een Cloudservice met omgekeerde DNS maken
U kunt een nieuwe Cloudservice toevoegen met het omgekeerde DNS-eigenschap die is opgegeven met de cmdlet 'Set-AzureService':

    PS C:\> New-AzureService –ServiceName “contosoapp1” –Location “West US” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “contosoapp1.cloudapp.net.”

## <a name="view-reverse-dns-for-existing-cloud-services"></a>Weergave omgekeerde DNS-records voor bestaande Cloudservices
U kunt de geconfigureerde waarde voor een bestaande Cloud-Service met de cmdlet "Get-AzureService" bekijken:

    PS C:\> Get-AzureService "contosoapp1"

## <a name="remove-reverse-dns-from-existing-cloud-services"></a>Omgekeerde DNS van bestaande Cloudservices verwijderen
U kunt een omgekeerde DNS-eigenschap verwijderen uit een bestaande Cloud-Service met de cmdlet "Set-AzureService". Dit is gedaan door het omgekeerde DNS-eigenschapswaarde leeg:

    PS C:\> Set-AzureService –ServiceName “contosoapp1” –Description “App1 with Reverse DNS” –ReverseDnsFqdn “”

[AZURE.INCLUDE [FAQ1](../../includes/dns-reverse-dns-record-operations-faq-host-own-arpa-zone-include.md)]

[AZURE.INCLUDE [FAQ2](../../includes/dns-reverse-dns-record-operations-faq-asm-include.md)]
