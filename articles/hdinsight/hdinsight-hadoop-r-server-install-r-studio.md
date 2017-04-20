<properties
    pageTitle="RStudio met R-Server op HDInsight (preview) installeren | Microsoft Azure"
    description="Het installeren van RStudio met R-Server op HDInsight (preview)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Installatie van RStudio met R-Server op HDInsight (preview)

Er zijn meerdere geïntegreerde ontwikkelomgeving biedt (IDE) beschikbaar voor R vandaag, [R-hulpprogramma's voor Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), een reeks hulpmiddelen voor de bureaublad- en server uit [RStudio](https://www.rstudio.com/products/rstudio-server/)of Walware van Eclips gebaseerde [StatET](http://www.walware.de/goto/statet)Microsofts onlangs inclusief aangekondigd. Een van de populairste op Linux is het gebruik van [RStudio Server](https://www.rstudio.com/products/rstudio-server/) waarmee u een browser gebaseerde IDE door externe clients wordt gebruikt.  RStudio Server installeren op het randknooppunt van een cluster HDInsight Premium beschikt u over een volledige IDE ervaring voor de ontwikkeling en R scripts worden uitgevoerd met R-Server op het cluster en productiever aanzienlijk dan standaard gebruik van de R-Console.

In dit artikel leert u hoe u de (gratis) versie van de community van RStudio Server installeert op het randknooppunt van een cluster met behulp van een aangepast script. Als u liever de commercieel gelicentieerde Pro-versie van RStudio Server, moet u de installatie-instructies volgen van [RStudio-Server](https://www.rstudio.com/products/rstudio/download-server/).

> [AZURE.NOTE] De stappen in dit document een R-Server op HDInsight cluster vereisen en werkt niet goed als u een cluster HDInsight waarbij R is geïnstalleerd met de [Actie voor R Script installeren](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Vereisten voor

* Een Azure HDInsight cluster met R-Server is geïnstalleerd. Zie [aan de slag met R-Server op HDInsight clusters](hdinsight-hadoop-r-server-get-started.md)voor instructies.
* Een SSH-client. Voor Linux en Unix onderzoeken of Macintosh OS X, de `ssh` opdracht wordt geleverd bij het besturingssysteem. Voor Windows raden [Cygwin](http://www.redhat.com/services/custom/cygwin/) met de [optie OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU)of [stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>RStudio installeren op het cluster met een aangepast script

1. Geef aan dat het randknooppunt van het cluster. Hieronder volgt voor een HDInsight cluster met R-Server, de naamgevingsconventie op voor de kop en knooppunten van de rand.

    * Knooppunt van hoofd-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Randknooppunt-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH naar het randknooppunt van het gebruik van de bovenstaande naming patroon cluster. 
 
    * Als u verbinding van een client Linux maakt, raadpleegt u [verbinding maken met een cluster Linux gebaseerde HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Als u verbinding van een Windows-client maakt, raadpleegt u [verbinding maken met een HDInsight Linux gebaseerde cluster met stopverf](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Nadat u verbonden bent, worden de hoofdmap van een gebruiker op het cluster. Gebruik de volgende opdracht in de sessie SSH.

        sudo su -

4. Download de aangepast script om te kunnen installeren RStudio. Gebruik de volgende opdracht uit.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Wijzig de machtigingen voor het aangepast script-bestand en het script uitvoeren. Gebruik de volgende opdrachten.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Als u een wachtwoord SSH tijdens het maken van een HDInsight cluster met R-Server gebruikt, kunt u deze stap overslaan en doorgaan met de volgende. Als u een sleutel SSH in plaats daarvan de cluster maken gebruikt, moet u een wachtwoord instellen voor uw gebruikers-SSH. U moet dit wachtwoord bij verbinden met RStudio. Voer de volgende opdrachten. Wanneer u hierom wordt gevraagd om het **huidige Kerberos-wachtwoord**op te geven, druk op **ENTER**.  Opmerking die u moet vervangen `USERNAME` met een gebruiker SSH voor uw cluster HDInsight.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Als uw wachtwoord is ingesteld, ziet u een bericht als volgt.

        passwd: password updated successfully


    De sessie SSH afsluiten.

7. Een tunnel SSH aan het cluster maken door toe te wijzen `localhost:8787` op het cluster HDInsight naar de clientcomputer. U moet een tunnel SSH maken voordat u een nieuwe browsersessie opent.

    * Op een Linux-client of een Windows-client met [Cygwin](http://www.redhat.com/services/custom/cygwin/) vervolgens sessie een open en gebruik van de volgende opdracht uit.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        **Gebruikersnaam** vervangen door een gebruiker SSH voor uw cluster HDInsight en **CLUSTERNAAM** vervangen door de naam van uw cluster HDInsight ook kunt u een sleutel SSH in plaats van met een wachtwoord door toe te voegen`-i id_rsa_key`     

    * Als met een Windows-client stopverf vervolgens

        1.  Open stopverf en voert u de verbindingsgegevens van uw. Als u niet bekend met stopverf bent, raadpleegt u [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight vanuit Windows](hdinsight-hadoop-linux-use-ssh-windows.md) voor meer informatie over hoe u dit product gebruiken met HDInsight.
        2.  Klik in de sectie **categorie** aan de linkerkant van het dialoogvenster **verbinding**uitvouwen, **SSH**uitvouwen en selecteer vervolgens **Tunnels**.
        3.  De onderstaande informatie opgeven in het formulier **Opties SSH poort doorschakelen bepalen** :

            * **Bronpoort** - de poort op de client die u wilt doorsturen. Bijvoorbeeld **8787**.
            * **Bestemming** - de bestemming die moet worden toegewezen aan de lokale clientcomputer. Bijvoorbeeld: **localhost:8787**.

            ![Een tunnel SSH maken] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Een tunnel SSH maken")

        4. Klik op **toevoegen** om de instellingen en klik vervolgens op **openen** om te openen van een verbinding SSH.
        5. Wanneer u wordt gevraagd, meld u aan bij de server. Hiermee wordt een sessie SSH tot stand brengen en de tunnel inschakelen.

8. Open een webbrowser en voer de volgende URL op basis van de poort die u hebt opgegeven voor de tunnel.

        http://localhost:8787/ 

9. U wordt gevraagd om in te voeren SSH gebruikersnaam en wachtwoord verbinding maken met de cluster. Als u een sleutel SSH tijdens het maken van het cluster gebruikt, moet u het wachtwoord die u hebt gemaakt in stap 5 hierboven.

    ![Verbinding maken met R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Een tunnel SSH maken")

10. Als u wilt testen of de RStudio installatie voltooid is, kunt u een test uitvoeren script die wordt uitgevoerd R MapReduce en een taken gebaseerd op het cluster. Ga terug naar de SSH-console en voer de volgende opdrachten voor het downloaden van de testscript uitvoeren in RStudio.

    * Als u een Hadoop-cluster met R hebt gemaakt, gebruikt u deze opdracht.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Als u een een-cluster met R hebt gemaakt, gebruikt u deze opdracht.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. In RStudio ziet u de testscript die u hebt gedownload. Klik met de Dubbelklik op het bestand als u wilt openen, selecteert u de inhoud van het bestand en klik vervolgens op **uitvoeren**. Hier ziet u de uitvoer in het deelvenster **Console** .
 
    ![Test de installatie] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Test de installatie")

Een andere optie zou typen `source(testhdi.r)` of `source(testhdi_spark.r)` het script uitvoeren.

## <a name="see-also"></a>Zie ook

- [Context-opties voor R-Server op HDInsight clusters berekenen](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure opslagopties voor R-Server op HDInsight premium](hdinsight-hadoop-r-server-storage.md)


 
