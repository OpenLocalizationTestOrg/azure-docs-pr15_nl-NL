<properties
   pageTitle="Service-tolerantie richtlijnen | Microsoft Azure"
   description="Koppelingen naar problemen oplossen en proactief tolerantie en beschikbaarheid van de richtlijnen voor het Microsoft Azure-services."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Richtlijnen voor tolerantie van Microsoft Azure-Services
Microsoft Azure is bedoeld om u te geven met de resources die u nodig hebt, wanneer u deze nodig hebt. Als onderdeel van goed ontwerp en werkwijzen, moet u zowel het bouwen van het gebruik van Azure services om te bereiken beschikbaarheid als wat u moet doen als uw toepassing wordt beïnvloed door een service-onderbreking weten. Om u te helpen bij dit proces, bevat dit document koppelingen naar richtlijnen voor noodgevallen-herstel, evenals de richtlijnen van ontwerpen voor verschillende Azure services.

##<a name="disaster-recovery-guidance"></a>Richtlijnen voor noodgevallen herstel
De noodgevallen herstel richtlijnen zijn van de onderstaande koppelingen kunt u met de gegevens die u nodig hebt om u aan uw toepassing weer online snel als u worden beïnvloed door een Azure service-onderbreking helpen bieden. Deze koppelingen zijn gemaakt waarmee u de vraag, "Ik ben wordt beïnvloed door een Azure service-onderbreking, wat kan ik doen?"

##<a name="design-guidance"></a>Richtlijnen
De onderstaande ontwerp richtlijnen koppelingen zijn ontwerp en architectuur richtlijnen die laat zien hoe het beste elke Azure service gebruiken op een manier die gemaximaliseerd beschikbaarheid van de toepassing is gemaakt. Deze koppelingen zijn gemaakt waarmee u de vraag "Hoe zorg ik ervoor dat een fout, hardware mislukt, service-onderbreking of andere storing niet van invloed op de algehele beschikbaarheid van mijn toepassing?" Als er geen specifieke richtlijnen voor de service die u momenteel zoekt, kan de [beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](./resiliency-high-availability-azure-applications.md) -artikel aanvullende informatie die u bij het ontwerpen van helpen kan hebben. 

##<a name="service-guidance"></a>Richtlijnen voor services
| Service  | Richtlijnen voor noodgevallen herstel | Richtlijnen |
|:---------|:--------------------------:|:------------------:|
| [Cloudservices] (https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services")       | [koppeling] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure-Cloudservices noodgevallen herstel richtlijnen")   | Niet beschikbaar |
| [Belangrijke kluis] (https://azure.microsoft.com/services/key-vault/ "Azure belangrijke kluis")                      | [koppeling] (../key-vault/key-vault-disaster-recovery-guidance.md "Azure toets kluis noodgevallen herstel richtlijnen")        | Niet beschikbaar |
| [Opslag] (https://azure.microsoft.com/services/storage/ "Azure-opslag")                            | [koppeling] (../storage/storage-disaster-recovery-guidance.md "Azure Storage noodgevallen herstel richtlijnen")          | Niet beschikbaar |
| [SQL-Databases] (https://azure.microsoft.com/services/sql-database/ "Azure SQL-Databases")           | [koppeling] (../sql-database/sql-database-disaster-recovery.md  "Azure SQL-Database noodgevallen herstel richtlijnen")    | [koppeling] (../sql-database/sql-database-business-continuity.md "Overzicht van bedrijfscontinuïteit met Azure SQL-Database") |
| [Virtuele Machines] (https://azure.microsoft.com/services/virtual-machines/ "Azure virtuele Machines") | [koppeling] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azure virtuele Machines noodgevallen herstel richtlijnen") | Niet beschikbaar |
| [Virtual Network] (https://azure.microsoft.com/services/virtual-network/ "Azure Virtual Network")    | [koppeling] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure Virtual Network noodgevallen herstel richtlijnen")  | Niet beschikbaar |

##<a name="next-steps"></a>Volgende stappen
Als u voor instructies die breder ligt de nadruk op systemen en oplossingen zoeken wilt, lees dan [herstel en betere beschikbaarheid voor toepassingen gebaseerd op Microsoft Azure](https://aka.ms/drtechguide).
