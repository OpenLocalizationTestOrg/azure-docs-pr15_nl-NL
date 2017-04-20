<properties
    pageTitle="Richtlijnen voor de resource van de groepen | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren richtlijnen voor het implementeren van resourcegroepen in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="azure-resource-group-guidelines"></a>Azure resource groep richtlijnen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

In dit artikel ligt de nadruk op het logisch bouwen van uw omgeving en de onderdelen in resourcegroepen groeperen.


## <a name="implementation-guidelines-for-resource-groups"></a>Implementatie van richtlijnen voor resourcegroepen

Beslissingen:

- Gaat u resourcegroepen bouwen door de onderdelen van de infrastructuur core of volledige toepassingsimplementatie?
- Moet u toegang tot resourcegroepen met Access-besturingselementen op basis van rollen beperken?

Taken:

- Definiëren wat core onderdelen van de infrastructuur en specifiek resourcegroepen die u nodig hebt.
- Lees hoe u implementeert resourcemanager sjablonen voor consistente, gereproduceerd implementaties.
- Definiëren wat gebruikersrollen access u nodig hebt voor toegang tot resourcegroepen beheren.
- Maak de set resourcegroepen uw naamgevingsconventie gebruiken. U kunt de Azure CLI of de portal.


## <a name="resource-groups"></a>Resourcegroepen

In Azure wordt aangegeven, moet u logisch verwante resources zoals opslagruimte accounts, virtuele netwerken en virtuele machines (VMs) kunt implementeren, beheren en onderhouden ze als één eenheid groeperen. Deze methode gemakkelijker om te implementeren toepassingen terwijl de verwante bronnen samen blijft beheren vanuit het perspectief van of anderen om toegang te verlenen aan die groep van resources. Voor een uitgebreidere begrip van resourcegroepen, kunt u het [overzicht van de Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)lezen.

Een belangrijke functie resourcegroepen is de mogelijkheid om te bouwen van uw omgeving met een JSON-bestand met de opslag, netwerken, en berekenen van resources. U kunt ook alle gerelateerde aangepaste scripts of configuraties om toe te passen definiëren. Met behulp van deze JSON-sjablonen, kunt u consistente, gereproduceerd implementaties maken voor uw toepassingen. Deze methode kunt u bij het maken van een omgeving in de ontwikkelingsfase bevindt en gebruikt u dezelfde sjabloon voor het maken van een productie-implementatie, of vice versa. Lees [de instructies van de sjabloon](../resource-manager-template-walkthrough.md) die u bij elke stap bouwen van een sjabloon JSON begeleidt voor een beter begrip over het gebruik van sjablonen.

Er zijn twee verschillende methoden die u bij het ontwerpen van uw omgeving met resourcegroepen kunt uitvoeren:

- Resourcegroepen voor elke toepassingsimplementatie waarin de opslag-accounts, virtuele netwerken en subnetten, VMs, zijn gecombineerd geladen balancers, enzovoort.
- Gecentraliseerde resourcegroepen die uw core virtuele netwerken en subnetten of opslag accounts bevatten. Uw toepassingen zijn vervolgens in hun eigen resourcegroepen die alleen VMs, netwerktaakverdelers, netwerkinterfaces, enzovoort bevatten.

Als u schaal, maken van gecentraliseerde resourcegroepen voor uw virtuele netwerken en subnetten het vergemakkelijkt maken van netwerkverbindingen cross-premises voor hybride connectiviteitsopties. De alternatieve benadering is voor elke toepassing om hun eigen virtueel netwerk waarvoor configuratie en onderhoud. [Toegang tot besturingselementen voor rollen gebaseerde](../active-directory/role-based-access-control-what-is.md) om er zelf een gedetailleerde toe toegang tot resourcegroepen te beheren. U kunt de gebruikers die toegang bronnen tot mogelijk bepalen voor productietoepassingen, of voor de core infrastructuur resources kunt u alleen infrastructuur engineers u ermee beperken. De toepassing-eigenaars alleen hebben toegang tot de toepassingsonderdelen binnen hun resourcegroep en niet de core Azure-infrastructuur van uw omgeving. Als u uw omgeving ontwerpt, kunt u de gebruikers die toegang tot de bronnen nodig hebben en ontwerpen van uw groepen Resource dienovereenkomstig gewijzigd. 


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 