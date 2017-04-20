<properties
  pageTitle="Azure DNS gebruiken met andere services Azure | Microsoft Azure"
  description="Informatie over het gebruik van Azure DNS voor het oplossen van naam voor andere Azure services"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Azure DNS gebruiken met andere services van Azure

Azure DNS is een gehoste DNS-beheer en naam resolutie service. Hiermee kunt u het maken van openbare DNS-namen voor de andere toepassingen en services die u hebt geïmplementeerd in Azure wordt aangegeven. Maken van een naam voor een Azure-service in uw aangepaste domein is net zo eenvoudig als het toevoegen van een record van het juiste type voor uw service.

* Voor dynamisch toegewezen IP-adressen, moet u een DNS CNAME-record die is toegewezen aan de naam van de DNS-die Azure voor uw service gemaakt. DNS-standaarden voorkomen dat u met behulp van een CNAME-record voor de top zone.
* Voor statisch toegewezen IP-adressen, kunt u een DNS-A-record met elke naam, zoals een _Open_ domeinnaam op de top zone maken.

De volgende tabel vindt u een overzicht van de ondersteunde recordtypen die kunnen worden gebruikt voor verschillende Azure services. Zoals u in deze tabel zien kunt, ondersteunt Azure DNS alleen DNS-records voor internetgerichte netwerk resources. Azure DNS kan niet worden gebruikt voor naamresolutie interne, persoonlijke adressen.

| Azure-Service | Netwerkinterface | Beschrijving |
|---------------|-------------------|-------------|
| Toepassingsgateway | Front openbare IP | U kunt een DNS-A of CNAME-record maken. |
| De belasting voor documentconversies | Front openbare IP | U kunt een DNS-A of CNAME-record maken. Taakverdeling kan een IPv6 openbare IP-adres dat is dynamisch toegewezen hebben. Daarom moet u een CNAME-record voor een IPv6-adres. |
| Verkeer Manager | De naam van de openbare | U kunt alleen een CNAME die is toegewezen aan de naam van de trafficmanager.net die zijn toegewezen aan uw profiel verkeer Manager maken. Zie [hoe verkeer Manager werkt](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example)voor meer informatie. |
| Cloudservice | Openbare IP | Voor statisch toegewezen IP-adressen, kunt u een DNS-A-record maken. Voor dynamisch toegewezen IP-adressen, moet u een CNAME-record die is toegewezen aan de naam van de _cloudapp.net_ maken. Deze regel geldt voor VMs gemaakt in de klassieke-portal, omdat ze zijn geïmplementeerd als een cloudservice. Zie [een aangepaste domeinnaam in de Cloud Services configureren](../cloud-services/cloud-services-custom-domain-name-portal.md)voor meer informatie. |
| App-Service | Externe IP-adres | Voor externe IP-adressen, kunt u een DNS-A-record maken. Anders moet u een CNAME-record die is toegewezen aan de naam van de azurewebsites.net maken. Zie voor meer informatie [toewijzen van een aangepaste domeinnaam aan een Azure-app](../app-service-web/web-sites-custom-domain-name.md) |
| Resourcemanager VMs | Openbare IP | Resourcemanager VMs kunt openbare IP-adressen hebben. Een VM met een openbare IP-adres kan ook worden achter een taakverdeling. U kunt een DNS A of CNAME-record voor het adres van uw openbare maken. Deze aangepaste naam kan worden gebruikt, worden omzeild de VIP op de taakverdeling. |
| Klassieke VMs | Openbare IP | Klassieke VMs gemaakt via PowerShell of CLI kan worden geconfigureerd met een dynamische of statische (gereserveerd) virtuele adres. U kunt een DNS CNAME of een record, respectievelijk maken. |
