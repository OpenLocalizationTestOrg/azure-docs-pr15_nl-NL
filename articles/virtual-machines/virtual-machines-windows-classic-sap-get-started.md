<properties
   pageTitle="Gebruik van SAP op Windows virtuele machines | Microsoft Azure"
   description="Wissen over het gebruik van SAP op Windows virtuele machines (VMs) in Microsoft Azure"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Gebruik van SAP op Windows virtuele machines in Azure wordt aangegeven

Cloud Computing is een veelgebruikte term die meer urgentie binnen de IT-branche, van kleine bedrijven snel aan de grote en multinationals ondernemingen krijgen is. Microsoft Azure is de Cloud Services-Platform van Microsoft die een groot aantal nieuwe mogelijkheden biedt. Nu kunnen klanten snel inrichten en maak inrichten van toepassingen als Cloud-Services, zodat ze niet beperkt tot technische of budgettering beperkingen. In plaats van de tijd en geld investeren in hardware infrastructuur, bedrijven zich kunnen concentreren op de toepassing, bedrijfsprocessen en de voordelen voor klanten en gebruikers.

Met Microsoft Azure virtuele machines biedt Microsoft een uitgebreide infrastructuur nodig als een Service (IaaS)-platform. SAP NetWeaver op basis-toepassingen worden ondersteund op Azure virtuele Machines (IaaS). De onderstaande artikelen wordt uitgelegd hoe u plannen en implementeren van SAP NetWeaver op basis van toepassingen op Windows virtuele machines in Azure wordt aangegeven. U kunt ook SAP NetWeaver op basis van toepassingen op [Linux virtuele machines](virtual-machines-linux-classic-sap-get-started.md)implementeren.

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver op Azure - HA

Titel: SAP NetWeaver op Azure - cluster SAP ASC's / SCS exemplaren met Windows Server failovercluster op Azure met SIOS DataKeeper

Overzicht: ' dit document wordt beschreven hoe SIOS DataKeeper gebruiken voor het instellen van een zeer beschikbaar SAP ASC's / SCS-configuratie op Azure. SAP beschermen hun potentieel mislukt onderdelen zoals SAP ASC's / SCS of plaatsen herhaling Services met Windows Server failovercluster configuraties waarvoor gedeelde schijven. Deze SAP-onderdelen zijn essentieel voor de functionaliteit van een SAP-systeem. Daarom moet hoge beschikbaarheid functionaliteit opgezet om ervoor te zorgen dat de onderdelen die een fout van een server of een VM als voltooid met Windows-Cluster configuraties voor de voorziening en Hyper-V omgevingen kunnen vangen. Azure op zichzelf kan geen gedeelde schijven die nodig zijn voor de Windows op basis van zeer beschikbare configuraties die zijn vereist voor deze kritieke SAP-onderdelen vanaf augustus 2015. Met behulp van het product DataKeeper door SIOS, kunnen echter failovercluster van Windows Server configuraties zo nodig voor SAP ASC's / SCS worden gebaseerd op de Azure IaaS-platform. Dit artikel wordt beschreven in een aanpak stap-voor-stap het installeren van een failovercluster van Windows Server-configuratie met gedeelde schijf verstrekt door SIOS Datakeeper in Azure wordt aangegeven. Het papier wordt details in configuraties aan de kant Azure, Windows en SAP, waardoor de beschikbaarheid configuratie werken in een optimale wijze uitgelegd. Het papier is een aanvulling op de documentatie van SAP-installatie en SAP-notities die staan voor de primaire bronnen voor installaties en implementaties van SAP-software op de opgegeven platforms.

Bijgewerkt: Augustus 2015

[Deze handleiding nu downloaden](http://go.microsoft.com/fwlink/?LinkId=613056)
