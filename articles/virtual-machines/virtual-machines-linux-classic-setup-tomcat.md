<properties
    pageTitle="Apache Tomcat op een VM Linux instellen | Microsoft Azure"
    description="Meer informatie over het instellen van Apache Tomcat7 met een Azure virtuele machine (VM) waarop Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Het instellen van Tomcat7 op Linux met Microsoft Azure virtuele machines

Apache Tomcat (of gewoon Tomcat, voorheen ook ondersteuning Tomcat) is een webserver bron openen en de servlet container ontwikkeld door de Apache Software Foundation (AVP). Tomcat implementeert de Servlet Java en de specificaties Java Server Pages (JSP) van zo Microsystems en biedt een puur Java HTTP web server-omgeving waarin Java-code uit te voeren. De eenvoudigste configuratie en Tomcat wordt in een proces één besturingssysteem worden uitgevoerd. Dit proces wordt uitgevoerd Java virtuele machines (JVM). Elke HTTP-aanvraag via een browser naar Tomcat wordt verwerkt als een afzonderlijke thread in het proces Tomcat.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In deze handleiding, wordt u tomcat7 op de afbeelding van een Linux installeren en deze implementeert in Microsoft Azure.  

U leert:  

-   Het maken van een virtuele machine in Azure wordt aangegeven.
-   Hoe u de virtuele machine voorbereiden voor tomcat7.
-   Het installeren van tomcat7.

Ervan wordt uitgegaan dat de lezer al een Azure-abonnement heeft.  Als dit niet kunt u zich voor een gratis proefversie bij [http://azure.microsoft.com](https://azure.microsoft.com/). Als u een MSDN-abonnement hebt, raadpleegt u [speciale prijzen van Microsoft Azure: MSDN, MPN en Bizspark voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Zie voor meer informatie over Azure [Wat Azure is?](https://azure.microsoft.com/overview/what-is-azure/).

In dit onderwerp wordt ervan uitgegaan dat er basiskennis werken van tomcat en Linux.  

##<a name="phase-1-create-an-image"></a>Fase 1: Een afbeelding maken
In deze fase maakt u een virtuele machine door een Linux-afbeelding in Azure wordt aangegeven.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Stap 1: Een SSH verificatie-sleutel genereren
SSH is een belangrijk hulpmiddel voor systeembeheerders. Beveiliging in access op basis van een wachtwoord human bepaald configureren is echter niet een goede gewoonte. Kwaadwillende gebruikers kunnen in uw systeem op basis van een gebruikersnaam en wachtwoord hebben zwakke verbreken.

Het goede nieuws is dat er is een manier om te verlaten RAS openen en geen zorgen hoeft te wachtwoorden. De methode bestaat uit verificatie met asymmetrische cryptografische. Persoonlijke sleutel van de gebruiker is de waarmee de verificatie wordt verleend. U kunt zelfs account van de gebruiker om te weigeren wachtwoordverificatie volledig vergrendelen.

Een ander voordeel van deze methode is dat u verschillende wachtwoorden aanmelden bij verschillende servers niet nodig hebt. U kunt verifiëren met de persoonlijke persoonlijke sleutel op alle servers bevinden, waardoor u niet hoeft te weten verschillende wachtwoorden.

Het is ook mogelijk aanmelden met een wachtwoord in te stellen met deze methode.

Volg deze stappen om de verificatie-toets SSH genereren.

1.  Download en installeer puttygen van de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  PUTTYGEN uitvoeren. EXE.
3.  Klik op **genereren** om te genereren van de toetsen. U kunt in het proces onzekerheid vergroten door de muis boven het lege gebied in het venster te bewegen.  
![][1]
4.  Na het proces genereren, wordt de Puttygen.exe uw gegenereerde sleutel weergegeven. Bijvoorbeeld:  
![][2]
5.  Selecteer en kopieer de openbare sleutel in **sleutel** en opslaan in een bestand met de naam publicKey.pem. Niet op **Opslaan openbare sleutel**, omdat de bestandsindeling van de opgeslagen openbare sleutel verschilt van de openbare sleutel die we horen.
6.  Klik op **persoonlijke sleutel opslaan** en opslaan in een bestand met de naam privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Stap 2: Maak de afbeelding in de portal van Azure.
Klik op **Nieuw** in de taakbalk te maken van een afbeelding, de Linux-afbeelding op basis van uw behoeften kiezen in de [portal van Azure](https://portal.azure.com/). Het volgende voorbeeld wordt de afbeelding Ubuntu 14.04.
![][3]

Voor **Host Name** de naam voor de URL die u en Internet-clients gebruiken voor toegang tot deze virtuele machine opgeven. Het laatste onderdeel van de DNS-naam, bijvoorbeeld tomcatdemo, definiëren en Azure de URL als tomcatdemo.cloudapp.net wordt gegenereerd.  

Voor **SSH verificatie-sleutel**, de sleutelwaarde van het bestand **publicKey.pem** , waarin de openbare sleutel gegenereerd door puttygen te kopiëren.  
![][4]

Andere instellingen configureren desgewenst en klik vervolgens op maken.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fase 2: Uw VM voorbereiden voor Tomcat7
In deze fase, wordt u een eindpunt configureert voor tomcat verkeer en maak verbinding met uw nieuwe virtuele machine.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Stap 1: Open de HTTP-poort zodat web access
Eindpunten in Azure bestaan uit een protocol (TCP of UDP), samen met een openbare en persoonlijke poort. De privé wordt de poort die de service op de virtuele machine luistert. De openbare wordt de poort die de Azure cloudservice op extern voor binnenkomende, op Internet gebaseerde verkeer luistert.  

TCP-poort 8080 is het standaardnummer poort op welke luistert tomcat. Deze poort openen met een Azure eindpunt kunt u en andere clients internettoegang tomcat pagina's.  

1.  Klik op **Bladeren**in de portal Azure -> **VM**, en klik vervolgens op de virtuele machine die u hebt gemaakt.  
![][5]
2.  Als u wilt een eindpunt hebt toegevoegd aan uw VM, klik in het vak **eindpunten** .
![][6]
3.  Klik op **toevoegen**.  
    1.  Voor het **eindpunt**, typ een naam voor het eindpunt in eindpunt en typt u in **Openbare poort**80.  

        Als u deze op 80 instelt, niet nodig hebt om op te nemen het poortnummer in de URL die u hebt u toegang tot tomcat. Bijvoorbeeld http://tomcatdemo.cloudapp.net.    

        Als u deze op een andere waarde, zoals 81 instelt, moet u het poortnummer toevoegen aan de URL voor toegang tot tomcat. Bijvoorbeeld http://tomcatdemo.cloudapp.net:81 /.
    2.  Typ 8080 in privé poort. Tomcat luistert standaard op TCP-poort 8080. Als u gewijzigd in de standaard poort van tomcat beluisteren, moet u privé poort zijn dat hetzelfde als de tomcat poort luisteren bijwerken.  
    ![][7]

4.  Klik op **OK** om het eindpunt toevoegen aan uw virtuele computer.



###<a name="step-2-connect-to-the-image-you-created"></a>Stap 2: Koppelen aan de afbeelding die u hebt gemaakt.
U kunt een hulpmiddel SSH verbinding maken met uw computer virtuele kiezen. In dit voorbeeld gebruiken we stopverf.  

Eerst, krijgen de naam van de DNS van uw virtuele computer van de Azure-portal. **Klik op Bladeren** -> **virtuele machines** -> de naam van uw VM -> **Eigenschappen**en kijkt u in het veld **Domeinnaam** van de tegel **Eigenschappen** .  

Het poortnummer voor SSH verbindingen vanuit het veld **SSH** ophalen. Hier volgt een voorbeeld.  
![][8]

Download stopverf vanuit [hier](http://www.putty.org/) .  

Nadat u hebt gedownload, klikt u op het uitvoerbare bestand STOPVERF. EXE. De eenvoudige opties met de hostnaam configureren en het poortnummer uit de eigenschappen van uw VM opgehaald. Hier volgt een voorbeeld:  
![][9]

Klik in het linkerdeelvenster op **verbinding** -> **SSH** -> **Auth** en klik vervolgens op **Bladeren** om op te geven van de locatie van het bestand **privateKey.ppk** , waarin de persoonlijke sleutel gegenereerd door puttygen in fase 1: een afbeelding maken. Hier volgt een voorbeeld:  
![][10]

Klik op **openen**. U mogelijk worden gewaarschuwd per een berichtvak. Als u de DNS-naam hebt geconfigureerd en poortnummer correct, klikt u op **Ja**.
![][11]  


Zie de volgende:  
![][12]

Typ de naam van de gebruiker die is opgegeven als u de virtuele machine in fase 1 gemaakt: een afbeelding maken. Worden er ongeveer als volgt te werk:  
![][13]





##<a name="phase-3-install-software"></a>Fase 3: Installeer software
In deze fase installeert u de Java runtime-omgeving, tomcat en andere onderdelen tomcat.  

###<a name="java-runtime-environment"></a>Java runtime-omgeving
Tomcat is geschreven in Java. Er zijn twee soorten Java Development Kit (JDKs) (OpenJDK en Oracle JDK), en kunt u de gewenste kiezen.  

>AZURE. Opmerking: Beide JDKs hebt vrijwel dezelfde code voor de categorieën in de Java-API, maar de code voor de virtuele machine daadwerkelijk verschilt. Wanneer u naar bibliotheken gaat, vaak OpenJDK openen bibliotheken gebruiken terwijl Oracle doorgaans gesloten bestanden gebruiken. Maar Oracle JDK heeft meer klassen en enkele vaste bugs bij te werken en Oracle JDK stabiele dan OpenJDK is.

De volgende opdrachten downloadt u de verschillende JDKs.  

Open-jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle-jdk  

-   De JDK downloaden via de Oracle-website:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   Een map waarop u de JDK-upbestanden maken:  

        sudo mkdir /usr/lib/jvm  

-   De JDK-bestanden naar de map/usr/bibliotheek/jvm/extraheren:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Oracle JDK als het standaard JVM instellen:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
U kunt een opdracht als volgt te werk om te testen of de Java runtime-omgeving correct is geïnstalleerd:  

    java -version  

Als u openen-jdk hebt geïnstalleerd, ziet u een bericht als volgt uit:![][14]

Als u oracle-jdk hebt geïnstalleerd, ziet u een bericht als volgt uit:![][15]

###<a name="tomcat7"></a>Tomcat7
Gebruik de volgende opdracht voor het installeren van tomcat7:  

    sudo apt-get install tomcat7  

Als u niet tomcat7 gebruikt, gebruikt u de gewenste variatie van deze opdracht.  

####<a name="test"></a>Test:

Als u wilt controleren of er tomcat7 is geïnstalleerd, bladert u naar uw tomcat DNS-servernaam (http://tomcatexample.cloudapp.net/ is de voorbeeld-URL in dit artikel). Als u een pagina als volgt te werk ziet, hebt u tomcat7 geïnstalleerd juiste.
![][16]


###<a name="install-other-tomcat-components"></a>Andere Tomcat-onderdelen installeren
Er zijn andere optionele tomcat-onderdelen die u kunt ook installeren.  

Gebruik de opdracht **sudo enz-cache zoeken tomcat7** om alle beschikbare onderdelen weer te geven. De volgende opdrachten zijn voorbeelden voor het installeren van bepaalde handige delen.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Fase 4: Tomcat configureren
In deze fase beheren u tomcat.
###<a name="start-and-stop-tomcat7"></a>Starten en stoppen tomcat7
De tomcat7-server automatisch gestart wanneer u deze installeren. U kunt ook deze starten zelf met de volgende opdracht uit:   

    sudo /etc/init.d/tomcat7 start

Tomcat7 beëindigen:  

    sudo /etc/init.d/tomcat7 stop

De status van tomcat7 weergeven:  

    sudo /etc/init.d/tomcat7 status

Tomcat-services opnieuw starten:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Beheer van de Tomcat
U kunt het configuratiebestand van Tomcat gebruiker om het instellen van uw beheerdersreferenties met de volgende opdracht bewerken:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Hier volgt een voorbeeld:  
![][17]  

>AZURE. Opmerking: Maak een sterk wachtwoord voor de gebruikersnaam van de beheerder.  

Nadat dit bestand zijn bewerkt, moet u tomcat7 services met de volgende opdracht uit om ervoor te zorgen dat de wijzigingen van kracht opnieuw starten:  

    sudo /etc/init.d/tomcat7 restart  

Open uw browser en typ de URL **http://<your tomcat server DNS name>/manager/html**. Het voorbeeld in dit artikel is de URL http://tomcatexample.cloudapp.net/manager/html.  

Nadat u verbinding maakt, worden er iets ongeveer als volgt uit:  
![][18]

##<a name="common-issues"></a>Veelvoorkomende problemen

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Geen toegang tot de virtuele machine met Tomcat en Moodle van Internet

-   **Symptoom**  
Tomcat wordt uitgevoerd, maar u kunt de standaardpagina Tomcat zien met de browser.
-   **Mogelijke hoofdsite hoofdletters/kleine letters**   
    1.  De tomcat luisteren poort is niet hetzelfde als de privé-poort van uw VM eindpunt voor tomcat-verkeer is toegestaan.  

        Controleer de instellingen van uw openbare poort en privé poort eindpunt en controleer of dat de poort privé is dat hetzelfde als de tomcat poort beluisteren. Zie fase 1: Een afbeelding voor instructies over het configureren van de eindpunten voor uw virtuele machine maken.  

        Om te bepalen de tomcat luisteren poort, open /etc/httpd/conf/httpd.conf (rood rol definitieve versie) of /etc/tomcat7/server.xml (Debian definitieve versie). Standaard is de tomcat luisteren poort 8080. Hier volgt een voorbeeld:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Als u een virtuele machine zoals Debian of Ubuntu en dat u wijzigen van de standaard poort wilt van Tomcat beluisteren (bijvoorbeeld 8081) gebruikt, moet u ook de poort voor de OS openen. Open eerst het profiel:  

            sudo vi /etc/default/tomcat7  

        Vervolgens Verwijder de opmerkingen bij de laatste regel en wijzigt u "geen' naar 'Ja'.  

            AUTHBIND=yes

    2.  De firewall heeft de poort luisteren van tomcat uitgeschakeld.

        Als u alleen de standaardpagina Tomcat van de lokale host ziet kunt, klikt u vervolgens is het probleem meestal dat de poort die is geluisterd door tomcat is geblokkeerd door de firewall. U kunt de w3m gebruiken om te bladeren van de pagina met webonderdelen. De volgende opdrachten w3m installeren en bladert u naar de standaardpagina Tomcat:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Oplossing**
    1. Als de tomcat beluisteren poort is niet hetzelfde als de privé-poort van het eindpunt van verkeer naar de virtuele machine, hoeft u de poort privé als u dat hetzelfde als de tomcat poort luisteren wijzigen.   

    2.  Als het probleem wordt veroorzaakt door de firewall/iptables, moet u de volgende regels toevoegen aan /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Houd er rekening mee dat de tweede regel alleen nodig is voor https-verkeer is toegestaan.  

        Controleer of dat deze zich boven alle regels die globaal van toegang, zoals de volgende beperken zou:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Als u wilt laden de iptables, voert u de volgende opdracht uit:  

            service iptables restart  

        Dit is getest op CentOS 6.3.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>U hebt geen machtiging wanneer u bestanden met /var/lib/tomcat7/webapps projecteren upload /  

-   **Symptoom**  
Wanneer u een SFTP-client (zoals FileZilla) gebruiken om verbinding maken met uw VM en navigeer naar /var/lib/tomcat7/webapps/publiceren van uw site, krijgt u een foutbericht vergelijkbaar met het volgende:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Mogelijke hoofdsite hoofdletters/kleine letters** U hebt geen machtigingen voor toegang tot de map /var/lib/tomcat7/webapps.  
-   **Oplossing**  
U moet toestemming krijgen van de hoofdsite-account. U kunt het eigendom van deze map hoofdsite wijzigen in de gebruikersnaam die u hebt gebruikt bij de inrichting van de computer. Hier volgt een voorbeeld met de naam van het azureuser-account:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    Gebruik de optie -R te de machtigingen voor alle bestanden in een map te voorzien.  

    Houd er rekening mee dat deze opdracht ook voor mappen werkt. De optie -R verandert de machtigingen voor alle bestanden en mappen binnen de adreslijst. Hier volgt een voorbeeld:  

        sudo chown -R username:group directory  

    Deze opdracht wijzigt eigendom (gebruikers en groepen) voor alle bestanden en mappen in de map en map zelf.  

    De volgende opdracht uit, alleen de machtiging van de map map wordt gewijzigd maar de bestanden en mappen in de map alleen blijft.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
