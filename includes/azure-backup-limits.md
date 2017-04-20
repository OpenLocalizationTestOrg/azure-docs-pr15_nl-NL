 (back-up kluizen<properties
   Paginatitel = "Azure Backup limits table"
   Beschrijving = "systeemlimieten voor wordt beschreven Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


De volgende limieten gelden voor Azure back-up.

| Limiet-ID | Standaardbeperking |
|---|---|
|Aantal servers/machines die kunnen worden geregistreerd ten opzichte van elke kluis|50 voor Windows Server/Client/SCDPM <br/> 200 voor IaaS VMs|
|Grootte van een gegevensbron voor de gegevens die zijn opgeslagen in Azure kluis opslag|54400 GB max<sup>1</sup>|
|Aantal back-up kluizen die in elk Azure abonnement kan worden gemaakt|25 (back-up kluizen) <br/> 25 herstel Services kluis per regio|
|Aantal keren back-up kan worden gepland per dag|3 per dag voor Windows Server-Client <br/> 2 per dag voor SCDPM <br/> Één keer per dag voor IaaS VMs|
|Gegevensschijven die zijn bijgevoegd bij een Azure virtuele machines voor back-up|16|

- <sup>1</sup> De limiet van 54400 GB geldt niet voor IaaS VM back-up.

