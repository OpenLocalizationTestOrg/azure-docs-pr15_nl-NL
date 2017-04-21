Volg deze stappen als u wilt installeren en uitvoeren van MongoDB op een virtuele machine waarop CentOS Linux.

> [AZURE.WARNING] MongoDB beveiligingsfuncties, zoals verificatie en IP-adresbinding, worden niet al dan niet standaard ingeschakeld. Beveiligingsfuncties moeten worden ingeschakeld voordat MongoDB naar een productieomgeving wordt geïmplementeerd.  Zie [beveiligings- en verificatie](http://www.mongodb.org/display/DOCS/Security+and+Authentication) voor meer informatie.

1. Het pakket Management systeem (YUM) zodanig configureren dat u MongoDB kunt installeren. */Etc/yum.repos.d/10gen.repo* bestand houdt u informatie over uw bibliotheek en voer de volgende maken:

        [10gen]
        name=10gen Repository
        baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
        gpgcheck=0
        enabled=1

2. Sla het bestand cessies‑retrocessies en voer de volgende opdracht bijwerken van de lokale pakket-database:

        $ sudo yum update

3. Als u wilt het pakket hebt geïnstalleerd, voert u de volgende opdracht uit om de meest recente stabiele versie MongoDB en de bijbehorende hulpmiddelen te installeren:

        $ sudo yum install mongo-10gen mongo-10gen-server

    Wacht terwijl MongoDB downloadt en installeert.

4. Maak een gegevensmap. MongoDB worden standaard gegevens opgeslagen in de adreslijst */data/db* , maar moet u die map maken. Als u deze hebt gemaakt, wilt uitvoeren:

        $ sudo mkdir -p /srv/datadrive/data
        $ sudo chown `id -u` /srv/datadrive/data

    Zie voor meer informatie over het installeren van MongoDB op Linux [Quickstart Unix][QuickstartUnix].

5. Als u wilt de database, voert u het:

        $ mongod --dbpath /srv/datadrive/data --logpath /srv/datadrive/data/mongod.log

    Alle berichten in het logboek doorgestuurd naar het bestand */srv/datadrive/data/mongod.log* zoals MongoDB server wordt gestart, en preallocates logboek bestanden. Het kan enkele minuten duren voor MongoDB vooraf toewijzen de logboek-bestanden en begin te luisteren voor verbindingen.

6. Als u wilt de administratieve shell MongoDB, opent u een afzonderlijk venster voor SSH of stopverf en uitvoeren:

        $ mongo
        > db.foo.save ( { a:1 } )
        > db.foo.find()
        { _id : ..., a : 1 }
        > show dbs  
        ...
        > show collections  
        ...  
        > help  

    De database is gemaakt door de invoegen.

7. Wanneer MongoDB is geïnstalleerd moet u een eindpunt configureren zodat MongoDB toegankelijk is op afstand. Klik in de Portal Management **virtuele Machines**, en vervolgens klikt u op de naam van uw nieuwe virtuele machine, klik op **eindpunten**.
    
    ![Eindpunten][Image7]

8. Klik op **Eindpunt toevoegen** onder aan de pagina.
    
    ![Eindpunten][Image8]

9. Een eindpunt toevoegen met de volgende instellingen:

 - **Naam**: Mongo
 - **Protocol**: TCP
 - **Openbare poort**: 27017
 - **Privé poort**: 27017
 
 Hierdoor kunt MongoDB externe toegang tot.



[QuickStartUnix]: http://www.mongodb.org/display/DOCS/Quickstart+Unix


[Image7]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint.png
[Image8]: ./media/install-and-run-mongo-on-centos-vm/LinuxVmAddEndpoint2.png
