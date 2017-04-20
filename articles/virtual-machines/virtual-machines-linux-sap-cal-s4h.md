<properties 
pageTitle="S/4 HANA of zwart/wit/4 HANA op een Azure VM implementeren | Microsoft Azure" 
description="S/4 HANA of zwart/wit/4 HANA op een Azure VM implementeren" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>S/4 HANA of zwart/wit/4 HANA op Microsoft Azure implementeren 

In dit artikel wordt beschreven hoe S/4 HANA op Microsoft Azure via SAP Cloud toestel bibliotheek 3.0 implementeren.
De schermafbeeldingen weergeven stap voor stap het proces. Andere oplossingen SAP HANA gebaseerde implementeert zoals zwart/wit/4 HANA werkt op dezelfde manier vanuit het perspectief van een proces. Alleen hebben tot het selecteren van een andere oplossing.

Beginnen met SAP Cloud toestel bibliotheek (SAP EENCAL) Ga [hier](https://cal.sap.com/). Er is een blog van SAP over de nieuwe [SAP Cloud toestel bibliotheek 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


De volgende schermafbeeldingen hoe stapsgewijze S/4 HANA op Microsoft Azure implementeren. Het proces werkt op dezelfde manier voor andere oplossingen likeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

De eerste afbeelding ziet u alle SAP ROEP HANA gebaseerde oplossingen die beschikbaar op Microsoft Azure zijn.
Exemplarily de "SAP S/4 HANA on-premises editie" (oplossing onderaan op de schermafbeelding) is gekozen doorlopen het proces.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Een nieuw account voor SAP-ROEP moet eerst worden gemaakt. Er zijn twee mogelijkheden voor Azure - standaard Azure- en Azure op China via dat wordt beheerd door 21Vianet van de partner.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Vervolgens hebben tot de ID van de Azure abonnement die u kunt vinden op de Azure-portal - ook Zie verder omlaag hoe om dit te voeren. Een certificaat Azure management moet achteraf worden gedownload.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

De nieuwe Azure zoekt portal één u in het item "Abonnementen" aan de linkerkant. Klik op om weer te geven van alle actieve abonnementen voor de gebruiker.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Een van de abonnementen selecteren en vervolgens kiezen "Management certificaten" dit artikel wordt uitgelegd dat er wordt een nieuw concept "service principes" gebruiken voor het nieuwe resourcemanager Azure-model.
SAP ROEP nog niet is aangepast voor deze nieuwe model en nog steeds vereist het model 'klassieke' en de voormalige Azure portal voor gebruik met management certificaten.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Hier ziet een de voormalige Azure-portal. Het uploaden van een certificaat management biedt SAP ROEP de machtigingen voor het maken van virtuele machines in een klant-abonnement. Klik onder de "ABONNEMENTEN" vindt tabblad een de abonnements-ID die moet worden vermeld in de portal SAP ROEP.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Klik op het tabblad tweede kan vervolgens het management-certificaat dat is gedownload voordat u van SAP ROEP uploaden.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Een klein dialoogvenster verschijnt de gedownloade certificaatbestand te selecteren.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Zodra het certificaat is geüpload de verbinding tussen SAP-ROEP en de klant Azure abonnement binnen SAP Roep kan worden getest. Een klein bericht wordt weergegeven waarin wordt aangegeven dat de verbinding geldig is.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Na de installatie van een account hebben tot een oplossing die moet worden geïmplementeerd en maken van een exemplaar te selecteren.
Met 'eenvoudige'-modus is heel eenvoudig. Voer de exemplaarnaam van een, kiest u een Azure regio en definieer de basispagina wachtwoord voor de oplossing.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Na het enige tijd afhankelijk van de grootte en complexiteit van de oplossing (een schatting wordt gegeven door SAP ROEP) wordt weergegeven als 'actieve' en klaar voor gebruik. Het is heel eenvoudig.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Sommige details van de oplossing een kijken, kunt u zien welk VMs zijn geïmplementeerd. In dit geval zijn drie Azure VMs van verschillende grootte en doel gemaakt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

In de portal Azure, de virtuele machines vindt u beginnen met dezelfde naam van het exemplaar dat u hebt opgegeven in SAP ROEP.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Nu is het mogelijk te verbinding maken met de oplossing via de knop koppelen in de portal SAP ROEP. Het dialoogvenster weinig bevat een koppeling naar een gebruikershandleiding waarmee de standaardreferenties voor gebruik met de oplossing wordt beschreven.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Er is een andere optie naar aanmelden bij de Windows-VM-client en begint u bijvoorbeeld de vooraf geconfigureerde SAP-grafische gebruikersinterface.







