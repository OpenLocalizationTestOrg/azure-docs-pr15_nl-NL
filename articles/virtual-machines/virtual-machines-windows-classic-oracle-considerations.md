<properties
pageTitle="Overwegingen bij het gebruik van Oracle VM afbeeldingen | Microsoft Azure"
description="Meer informatie over ondersteunde configuraties en beperkingen voor een Oracle-VM op Windows Server in Azure wordt aangegeven voordat u de implementatie uitvoert."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Diverse overwegingen voor Oracle VM afbeeldingen



In dit artikel behandelt overwegingen voor Oracle virtuele machines in Azure wordt aangegeven, die zijn gebaseerd op Oracle software afbeeldingen is verstrekt door Oracle.  

-  Oracle-Database VM afbeeldingen
-  Oracle WebLogic Server VM afbeeldingen
-  Oracle JDK VM afbeeldingen

##<a name="oracle-database-virtual-machine-images"></a>Oracle-Database VM afbeeldingen
### <a name="clustering-rac-is-not-supported"></a>Clustering (RAC) wordt niet ondersteund.

Azure worden momenteel niet ondersteund voor Oracle Real Application Clusters (RAC) van de Oracle-Database. Alleen zelfstandige exemplaren van Oracle-Database zijn mogelijk. Dit is omdat Azure momenteel niet virtuele schijf delen van een alleen-lezen/schrijven, en over verschillende VM ondersteunt. Multicast-UDP wordt ook niet ondersteund.

### <a name="no-static-internal-ip"></a>Geen statische interne IP

Azure toegewezen elke virtuele machine een interne IP-adres. Tenzij de virtuele machine deel van een virtueel netwerk uitmaakt, is het IP-adres van de virtuele machine dynamische en kan worden gewijzigd nadat de virtuele machine opnieuw is opgestart. Dit kan problemen veroorzaken, omdat de Oracle-Database het IP-adres statisch verwacht. Als u wilt voorkomen dat het probleem, kunt u overwegen de virtuele machine met een virtueel Azure-netwerk. Zie [Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) en [maken een virtueel netwerk in Azure wordt aangegeven](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) voor meer informatie.

### <a name="attached-disk-configuration-options"></a>Configuratieopties aangesloten schijf

U kunt gegevensbestanden plaatsen op de schijf besturingssysteem van de virtuele machine of op bijgevoegde schijven, ook bekend als gegevensschijven. Gekoppelde schijven mogelijk betere prestaties en grootte flexibiliteit dan de schijf besturingssysteem bieden. Het is mogelijk dat de schijf besturingssysteem voorkeur alleen voor databases onder 10 GB (Gigabytes).

Gekoppelde schijven, is afhankelijk van de Azure Blob storage-service. Elke schijf zijn theoretische maximaal 500 invoer/uitvoer-bewerkingen per seconde (IO's / s). De prestaties van gekoppelde schijven mogelijk geen optimale in eerste instantie en IO's / s prestaties mogelijk aanzienlijk na een periode 'branden-in' van ongeveer 60-90 minuten van continue bewerking. Als een schijf later nog steeds niet actief geweest, IO's / s prestaties mogelijk neemt na af tot een andere branden in periode van continue bewerking. Kortom, het meer actieve per schijf, de vaker verloopt aanpak optimale IO's / s prestaties.

Hoewel de eenvoudigste benadering is één schijf toevoegen aan de virtuele machine en databasebestanden op de schijf wilt plaatsen, werkt deze methode ook de meest beperken met prestaties. U kunt in plaats daarvan vaak de effectieve IO's / s-prestaties verbeteren als u meerdere gekoppelde schijven gebruiken, verdeeld over databasegegevens ze en gebruik vervolgens Oracle automatische Storage Management (ASM). Zie [overzicht van de automatische Oracle-opslag](http://www.oracle.com/technetwork/database/index-100339.html) voor meer informatie. Hoewel deze mogelijk gesegmenteerd te verdelen van meerdere schijven gebruiken op het niveau van het besturingssysteem is, wordt dat afgeraden omdat deze niet bekend IO's / s prestaties te verbeteren.

Houd rekening met twee verschillende methoden voor het koppelen van meerdere schijven op basis van of u wilt de prioriteit te bepalen de prestaties van gelezen bewerkingen of schrijven bewerkingen voor uw database:

- **Oracle ASM op een eigen** is waarschijnlijk leiden tot betere prestaties schrijven, maar erger IO's / s voor meer bewerkingen ten opzichte van de methode schijf matrices. De volgende afbeelding ziet u logisch deze optie kiest.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] De verhouding tussen de prestaties van schrijven en gelezen op basis van de door geval evalueren. De werkelijke resultaten kunnen variëren wanneer u dit gebruikt.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Hoge beschikbaarheid herstel overwegingen

Wanneer u Oracle-Database in Azure virtuele machines, bent u bent verantwoordelijk zijn voor de uitvoering van een hoge beschikbaarheid hersteloplossing om te voorkomen dat een downtime. Bent u ook verantwoordelijk voor het back-ups van uw eigen gegevens en toepassingen.

Hoge beschikbaarheid herstel voor Oracle-Database Enterprise Edition (zonder RAC) op Azure kan worden bereikt met [gegevens beveiliging actieve gegevens beveiliging](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)of [Oracle Golden heeft bereikt](http://www.oracle.com/technetwork/middleware/goldengate), met twee databases in twee afzonderlijke virtuele machines. Beide virtuele machines moet in dezelfde [cloudservice](virtual-machines-linux-classic-connect-vms.md) en hetzelfde [virtueel netwerk](https://azure.microsoft.com/documentation/services/virtual-network/) om ervoor te zorgen dat ze toegang tot elkaar via het privé permanente IP-adres.  Bovendien is het raadzaam de virtuele machines plaatsen in het dezelfde [beschikbaarheid instellen](virtual-machines-windows-manage-availability.md) mogen Azure voor deze plaatsen in afzonderlijke foutenstructuuranalyse domeinen en het upgraden van domeinen. Alleen virtuele machines in dezelfde cloudservice kunt deelnemen aan dezelfde beschikbaarheid set. Elke virtuele machine moet ten minste 2 GB geheugen en 5 GB schijfruimte hebben.

Met de beveiliging van Oracle-gegevens, beschikbaarheid met een primaire database in één virtuele machine, een secundaire (stand-by) database in een andere virtuele computer kan worden bereikt en één richting replicatie ertussen instellen. Het resultaat is alleen toegang tot de kopie van de database. Met Oracle GoldenGate, kunt u bidirectionele replicatie tussen de twee databases configureren. Zie informatie over het instellen van een oplossing voor hoge beschikbaarheid voor uw databases die met deze hulpmiddelen, [Actieve gegevens beveiliging](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) en [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) documentatie bij de Oracle-website. Als u nodig hebt lees-schrijftoegang tot de kopie van de database, kunt u [Het actieve gegevens beveiliging Oracle](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server VM afbeeldingen

-  **Cluster wordt ondersteund op Enterprise Edition alleen.** U mag u WebLogic cluster alleen bij gebruik van de Enterprise Edition van WebLogic Server. Gebruik geen cluster met WebLogic Server Standard Edition.

-  **Verbinding time-out:** Als uw toepassing, is afhankelijk van verbindingen met openbare eindpunten van een andere Azure cloudservice (bijvoorbeeld een database laag), mogelijk Azure sluit u deze open verbindingen na vier minuten inactiviteit. Dit kan invloed zijn op functies en toepassingen te vertrouwen op verbinding van toepassingen, omdat verbindingen die inactief voor meer dan deze limiet zijn mogelijk niet langer geldig blijven. Als dit van invloed is op uw toepassing, kunt u "permanente" logica op uw verbinding van toepassingen inschakelen.

    Als een eindpunt *interne* aan uw implementatie van Azure cloud-service (zoals een zelfstandige database virtuele machine binnen de *dezelfde* cloudservice als uw WebLogic virtuele machines) is, klikt u vervolgens de verbinding is directe niet afhankelijk is van de Azure taakverdeling en dus niet onderhevig aan een time-out van de verbinding.

-  **UDP multicast wordt niet ondersteund.** Azure ondersteunt UDP unicasting, maar niet multicasting of uitzenden. WebLogic Server kan aanmelding Azure UDP unicast mogelijkheden. Voor de beste resultaten te vertrouwen op UDP unicast, het is raadzaam dat de grootte van de cluster WebLogic statische, worden gehouden met niet meer dan 10 beheerde servers opgenomen in het cluster worden bewaard.

-  **WebLogic Server verwacht openbare en persoonlijke poorten moet hetzelfde voor T3 toegang (bijvoorbeeld wanneer u een Enterprise JavaBeans gebruikt).** Houd rekening met een scenario voor meerdere niveaus waarop een servicetoepassing voor de laag (EJB) wordt uitgevoerd op een WebLogic Server cluster die bestaat uit twee of meer beheerde-servers in een cloudservice **SLWLS**met de naam. De clientlaag bevindt zich in een andere cloudservice, een eenvoudige Java-programma probeert te bellen EJB in de service-laag. Omdat deze nodig zijn voor het verdelen de laag service, moet een openbare taakverdeling eindpunt worden gemaakt voor de virtuele Machines in het cluster WebLogic-Server. Als de privé poort die u voor dat eindpunt opgeeft verschilt van de openbare poort (bijvoorbeeld 7006:7008), wordt een foutbericht zoals de volgende weergegeven:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    Dit is omdat voor externe toegang T3, WebLogic Server verwacht de poort van de verdeling van laden en de serverpoort WebLogic beheerd moeten overeenkomen. Klik in het bovenstaande geval is, de client toegang heeft tot poort 7006 (de belasting voor gelijkmatige verdeling poort) en de beheerde server luistert op 7008 (de privé poort). Deze beperking geldt alleen voor T3 toegang moet niet HTTP.

    Als u wilt voorkomen dat dit probleem, gebruikt u een van de volgende oplossingen:

    -  Gebruik dezelfde persoonlijke en openbare poortnummers voor de eindpunten van taakverdeling specifiek voor T3 access.

    -  Zijn de volgende JVM parameter bij het starten van WebLogic Server:

            -Dweblogic.rjvm.enableprotocolswitch=true

Zie KB-artikel **860340.1** bij <http://support.oracle.com>voor meer informatie.

-  **Dynamische cluster en beperkingen van taakverdeling.** Stel dat u wilt gebruiken van een dynamische cluster in WebLogic Server en deze weer te geven via een enkele, openbare taakverdeling eindpunt in Azure wordt aangegeven. Dit kan gebeuren zo lang maken als u een vaste poortnummer voor elk van de beheerde servers gebruiken (niet dynamisch toegewezen uit een bereik) en meer beheerde servers dan machines het bijhouden van de beheerder zijn er niet worden gestart (dat wil zeggen niet meer dan één beheerde server per virtuele machine). Als uw configuratie in meer WebLogic servers wordt gestart resulteert dan er virtuele machines zijn (dat wil zeggen waar meerdere exemplaren van WebLogic Server delen dezelfde virtuele machine), en vervolgens het is niet mogelijk om meer dan een van de instanties van WebLogic Server servers binding maken met een opgegeven poortnummer – anderen op die virtuele machine, mislukt.

    Aan de andere kant, als u de beheerder-server automatisch unieke poortnummers toewijzen aan de beheerde servers is geconfigureerd, is klikt u vervolgens taakverdeling niet mogelijk omdat Azure biedt geen ondersteuning voor toewijzing van één openbare poort aan meerdere privé poorten, zoals vereist voor deze configuratie is.

-  **Meerdere exemplaren van Weblogic Server op een virtuele machine.** Afhankelijk van de vereisten van uw implementatie, kunt u overwegen de optie van het uitvoeren van meerdere exemplaren van WebLogic Server op dezelfde virtuele machine, als de virtuele machine groot genoeg is. Klik op een middelgrote virtuele machine, waarin twee cores, kunt u bijvoorbeeld twee instanties van WebLogic Server uitvoeren. Opmerking echter dat nog steeds het is raadzaam dat u Kennismaking met potentiële risico in uw architectuur, die de hoofdletters/kleine letters vermijden als u slechts één virtuele machine met meerdere exemplaren van WebLogic Server gebruikt. Gebruik ten minste twee virtuele machines mogelijk een betere benadering en elk van deze virtuele machines vervolgens meerdere exemplaren van WebLogic Server kan uitvoeren. Elk van deze exemplaren van WebLogic Server kan nog steeds deel uitmaken van hetzelfde cluster. Opmerking, echter dit is momenteel niet mogelijk te gebruiken Azure Verdeel de werklast eindpunten die worden gebruikt door de implementaties van dergelijke WebLogic Server binnen dezelfde virtuele machine, omdat Azure taakverdeling is vereist voor de servers verdeeld moet worden verdeeld over unieke virtuele machines.

##<a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK VM afbeeldingen

-  **JDK 6 en 7 meest recente updates.** Terwijl we raden u aan de meest recente openbare, ondersteunde versie van Java (momenteel Java 8) gebruiken, wordt Azure ook JDK 6 en 7 afbeeldingen beschikbaar. Dit is bedoeld voor de oudere toepassingen die nog niet gereed om te worden bijgewerkt naar JDK 8. Terwijl de updates naar vorige JDK afbeeldingen mogelijk niet meer beschikbaar zijn voor het publiek, gegeven de Microsoft-verbinding met Oracle, zijn de JDK 6 en 7 afbeeldingen verstrekt door Azure bedoeld om een meer recente update van niet-openbare die normaal wordt aangeboden door Oracle naar alleen een bepaalde groep van Oracle ondersteunde klanten bevatten. Nieuwe versies van de afbeeldingen JDK komen beschikbaar na verloop van tijd met bijgewerkte versies van JDK 6 en 7.

    De JDK beschikbaar in deze JDK 6 en 7 afbeeldingen, en de virtuele machines en afbeeldingen die zijn afgeleid van deze kan alleen worden gebruikt in Azure.

-  **64-bits JDK.** De Oracle WebLogic Server VM afbeeldingen en de Oracle JDK VM afbeeldingen is verstrekt door Azure bevatten de 64-bits versies van Windows Server zowel de JDK.

##<a name="additional-resources"></a>Aanvullende informatie
[Oracle VM afbeeldingen voor Azure](virtual-machines-linux-classic-oracle-images.md)
