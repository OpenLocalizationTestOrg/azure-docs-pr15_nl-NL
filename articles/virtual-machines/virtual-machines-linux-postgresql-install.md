<properties
    pageTitle="PostgreSQL op een VM Linux instellen | Microsoft Azure"
    description="Meer informatie over het installeren en configureren van PostgreSQL op een Linux virtuele machine in Azure wordt aangegeven"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


# <a name="install-and-configure-postgresql-on-azure"></a>Installeren en configureren van PostgreSQL op Azure

PostgreSQL is vergelijkbaar met Oracle en DB2 een geavanceerde open source-database. Het bevat gereed voor enterprise-functies zoals volledige ZURE naleving, betrouwbare transacties verwerking en meerdere versie gelijktijdigheid besturingselement. Ondersteunt ook standaarden zoals ANSI SQL- en SQL/m (inclusief refererende gegevens aanvullende inhoud voor Oracle, MySQL, MongoDB en dergelijke). Het is ten zeerste extensible met ondersteuning voor meer dan 12 procedurele talen, GIN en basisvertalingen indexen ruimte gegevensondersteuning en meerdere NoSQL-achtige functies voor JSON of toets-waarde-toepassingen.

In dit artikel leert u hoe installeren en configureren van PostgreSQL op een Azure virtuele machine waarop Linux.


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="install-postgresql"></a>PostgreSQL installeren

> [AZURE.NOTE] U moet al een Azure virtuele machines Linux uitvoeren om u te voltooien van deze zelfstudie hebt. Als u wilt maken en instellen op een VM Linux voordat u verder gaat, raadpleegt u de [zelfstudie Azure Linux VM](virtual-machines-linux-quick-create-cli.md).

In dit geval poort 1999 als de PostgreSQL-poort gebruiken.  

Verbinding maken met de Linux VM die u hebt gemaakt via stopverf. Als u de eerste keer dat u een Azure Linux VM gebruikt, ziet u [hoe u gebruik SSH met Linux op Azure](virtual-machines-linux-mac-create-ssh-keys.md) voor meer informatie over het gebruik van stopverf verbinding maken met een VM Linux.

1. Voer de volgende opdracht om te schakelen naar de hoofdsite (beheerders):

        # sudo su -

2. Sommige onderzoeken hebben afhankelijkheden die u installeren moet voordat u installeert PostgreSQL. Controleren op uw distro in deze lijst en voert u de juiste opdracht:

    - Rood rol grondtal Linux:

            # yum install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

    - Debian grondtal Linux:

            # apt-get install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam libxslt-devel tcl-devel python-devel -y  

    - SUSE Linux:

            # zypper install readline-devel gcc make zlib-devel openssl openssl-devel libxml2-devel pam-devel pam  libxslt-devel tcl-devel python-devel -y  

3. PostgreSQL downloaden in de hoofdmap en vervolgens Pak het pakket:

        # wget https://ftp.postgresql.org/pub/source/v9.3.5/postgresql-9.3.5.tar.bz2 -P /root/

        # tar jxvf  postgresql-9.3.5.tar.bz2

    De bovenstaande is een voorbeeld. U vindt het meer gedetailleerde downloaden adres in de [Index van/pub/bron /](https://ftp.postgresql.org/pub/source/).

4. Als u wilt het bouwen, moet u deze opdrachten uitvoeren:

        # cd postgresql-9.3.5

        # ./configure --prefix=/opt/postgresql-9.3.5

5. Als u wilt maken van alles die kunnen worden opgebouwd, inclusief de documentatie (HTML en man pagina's) en aanvullende modules (contrib), in plaats daarvan de volgende opdracht uitvoeren:

        # gmake install-world

    U kunt het volgende bevestigingsbericht moet ontvangen:

        PostgreSQL, contrib, and documentation successfully made. Ready to install.

## <a name="configure-postgresql"></a>PostgreSQL configureren

1. (Optioneel) Maak een symbolic link verkorten de PostgreSQL-verwijzing, zodat het niet inbegrepen bij het versienummer weergegeven:

        # ln -s /opt/pgsql9.3.5 /opt/pgsql

2. Een map voor de database maken:

        # mkdir -p /opt/pgsql_data

3. Een gebruiker niet-hoofdmap maken en wijzigen van het profiel van die gebruiker. Vervolgens kunt u overschakelen naar deze nieuwe gebruiker ( *postgres* in ons voorbeeld genoemd):

        # useradd postgres

        # chown -R postgres.postgres /opt/pgsql_data

        # su - postgres

   > [AZURE.NOTE] Vanwege de beveiliging PostgreSQL gebruikmaakt van een gebruiker niet-hoofdsite geïnitialiseerd, starten of afsluiten van de database.


4. Bewerk het bestand *bash_profile* door het invoeren van de onderstaande opdrachten. Deze regels worden toegevoegd aan het einde van het bestand *bash_profile* :

        cat >> ~/.bash_profile <<EOF
        export PGPORT=1999
        export PGDATA=/opt/pgsql_data
        export LANG=en_US.utf8
        export PGHOME=/opt/pgsql
        export PATH=\$PATH:\$PGHOME/bin
        export MANPATH=\$MANPATH:\$PGHOME/share/man
        export DATA=`date +"%Y%m%d%H%M"`
        export PGUSER=postgres
        alias rm='rm -i'
        alias ll='ls -lh'
        EOF

5. Het bestand *bash_profile* uitvoeren:

        $ source .bash_profile

6. Uw installatie valideren met behulp van de volgende opdracht uit:

        $ which psql

    Als de installatie voltooid is, ziet u het volgende antwoord:

        /opt/pgsql/bin/psql

7. U kunt ook de PostgreSQL-versie controleren:

        $ psql -V

8. Initialisatie van de database:

        $ initdb -D $PGDATA -E UTF8 --locale=C -U postgres -W

    U kunt de volgende uitvoer moet ontvangen:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no1.png)

## <a name="set-up-postgresql"></a>PostgreSQL instellen

<!--    [postgres@ test ~]$ exit -->

Voer de volgende opdrachten:

    # cd /root/postgresql-9.3.5/contrib/start-scripts

    # cp linux /etc/init.d/postgresql

Twee variabelen in het bestand /etc/init.d/postgresql wijzigen. Het voorvoegsel is ingesteld op het installatiepad van PostgreSQL: **/opt/pgsql**. PGDATA is ingesteld op het pad van de gegevens opslag van PostgreSQL: **/opt/pgsql_data**.

    # sed -i '32s#usr/local#opt#' /etc/init.d/postgresql

    # sed -i '35s#usr/local/pgsql/data#opt/pgsql_data#' /etc/init.d/postgresql

![afbeelding](./media/virtual-machines-linux-postgresql-install/no2.png)

Het bestand zodat het uitvoerbare wijzigen:

    # chmod +x /etc/init.d/postgresql

PostgreSQL starten:

    # /etc/init.d/postgresql start

Controleer of het eindpunt van PostgreSQL ingeschakeld is:

    # netstat -tunlp|grep 1999

Hier ziet u het volgende resultaat:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no3.png)

## <a name="connect-to-the-postgres-database"></a>Verbinding maken met de database Postgres

Overschakelen naar de gebruiker postgres weer:

    # su - postgres

Een database Postgres maken:

    $ createdb events

Verbinding maken met de gebeurtenissen-database die u zojuist hebt gemaakt:

    $ psql -d events

## <a name="create-and-delete-a-postgres-table"></a>Maken en verwijderen van een tabel Postgres

Nu u hebt verbonden met de database, kunt u tabellen maken in deze.

Een nieuwe tabel van de voorbeeld-Postgres bijvoorbeeld maken met behulp van de volgende opdracht uit:

    CREATE TABLE potluck (name VARCHAR(20), food VARCHAR(30),   confirmed CHAR(1), signup_date DATE);

U hebt nu een tabel met vier kolommen met de volgende kolomnamen en beperkingen instellen:

1. De kolom "naam" is door de opdracht VARCHAR onder 20 tekens lang zijn beperkt.
2. De kolom "eten" geeft het levensmiddelitem dat elke persoon worden gebracht. VARCHAR bestandsgrootten deze tekst onder 30 tekens.
3. De kolom 'bevestigd' records of de persoon aan de Amerikaans heeft RSVP'd. De waarden die zijn "Y" en "N".
4. De "date" kolom ziet u wanneer ze zich voor de gebeurtenis aanmeldde. Postgres is vereist dat datums jjjj-mm-dd worden geschreven

Zie de volgende als u de tabel is gemaakt:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no4.png)

U kunt ook de tabelstructuur controleren met behulp van de volgende opdracht uit:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no5.png)

### <a name="add-data-to-a-table"></a>Gegevens toevoegen aan een tabel

Eerst de informatie in een rij invoegen:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('John', 'Casserole', 'Y', '2012-04-11');

Hier ziet u deze uitvoer:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no6.png)

U kunt een paar meer personen toevoegen aan de tabel als u ook. Hier volgen enkele opties of u kunt uw eigen maken:

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Sandy', 'Key Lime Tarts', 'N', '2012-04-14');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES ('Tom', 'BBQ','Y', '2012-04-18');

    INSERT INTO potluck (name, food, confirmed, signup_date) VALUES('Tina', 'Salad', 'Y', '2012-04-18');

### <a name="show-tables"></a>Tabellen weergeven

Gebruik de volgende opdracht uit om weer te geven van een tabel:

    select * from potluck;

De uitvoer is:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no7.png)

### <a name="delete-data-in-a-table"></a>Gegevens in een tabel verwijderen

Gebruik de volgende opdracht uit om gegevens in een tabel te verwijderen:

    delete from potluck where name=’John’;

Hiermee verwijdert u alle gegevens in de rij "John". De uitvoer is:

![afbeelding](./media/virtual-machines-linux-postgresql-install/no8.png)

### <a name="update-data-in-a-table"></a>Gegevens in een tabel bijwerken

Gebruik de volgende opdracht uit om gegevens in een tabel te werken. Voor dit item, heeft Sandy bevestigd dat zij bijwoont is, zodat we haar RSVP van 'NB' naar 'Y' wijzigen:

    UPDATE potluck set confirmed = 'Y' WHERE name = 'Sandy';


##<a name="get-more-information-about-postgresql"></a>Meer informatie over PostgreSQL
Nu dat u de installatie van PostgreSQL in een Azure Linux VM hebt voltooid, kunt u profiteren van deze gebruiken in Azure wordt aangegeven. Meer informatie over PostgreSQL, gaat u naar de [PostgreSQL-website](http://www.postgresql.org/).
