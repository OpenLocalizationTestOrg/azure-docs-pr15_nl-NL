<properties
    pageTitle="MongoDB installeren op een Windows-VM | Microsoft Azure"
    description="Leer hoe u MongoDB installeren op een Azure VM met Windows Server 2012 R2 die zijn gemaakt met het implementatiemodel resourcemanager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Installeren en configureren van MongoDB op een Windows-VM in Azure wordt aangegeven
[MongoDB](http://www.mongodb.org) is een populaire open source, krachtige NoSQL database. In dit artikel helpt u bij het installeren en configureren van MongoDB op een Windows Server 2012 R2 virtuele machine (VM) in Azure wordt aangegeven. U kunt ook [installeren MongoDB op een VM Linux in Azure wordt aangegeven](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Vereisten voor

Voordat u installeren en configureren van MongoDB, moet u een VM maken en, in het ideale geval een gegevensschijf toevoegen aan deze. Zie de volgende artikelen een VM maken en toevoegen van een gegevensschijf:

- [Een Windows Server VM maken met behulp van de Azure portal](virtual-machines-windows-hero-tutorial.md) of [Maak een Windows Server VM via Azure PowerShell](virtual-machines-windows-ps-create.md)
- [Bijvoegen een gegevensschijf voor een Windows Server VM met behulp van de Azure portal](virtual-machines-windows-attach-disk-portal.md) of [bijvoegen een gegevensschijf voor een Windows Server VM via Azure PowerShell](https://msdn.microsoft.com/library/mt603673.aspx)
    
Om te beginnen installeren en configureren MongoDB, [Meld u aan bij uw Windows Server VM](virtual-machines-windows-connect-logon.md) met behulp van extern bureaublad.


## <a name="install-mongodb"></a>MongoDB installeren

> [AZURE.IMPORTANT] MongoDB beveiligingsfuncties, zoals verificatie en IP-adresbinding, worden niet al dan niet standaard ingeschakeld. Beveiligingsfuncties moeten worden ingeschakeld voordat MongoDB naar een productieomgeving wordt geïmplementeerd. Zie voor meer informatie [MongoDB beveiligings- en verificatie](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Nadat u hebt verbonden met uw VM met extern bureaublad, moet u Internet Explorer openen vanuit het menu **Start** op de VM.

2. Selecteer **Gebruik aanbevolen beveiliging, privacy en compatibiliteitsinstellingen** wanneer Internet Explorer voor het eerst wordt geopend en klik op **OK**.

3. Internet Explorer Verbeterde beveiliging is standaard ingeschakeld. De website MongoDB toevoegen aan de lijst met toegestane sites:

    - Selecteer het pictogram **Extra** in de rechterbovenhoek.
    - Selecteer het tabblad **beveiliging** voor **Internet-opties**en selecteer het pictogram **Vertrouwde websites** .
    - Klik op de knop **Sites** . Toevoegen _https://\*. mongodb.org_ aan de lijst met vertrouwde websites en sluit het dialoogvenster.

    ![Beveiligingsinstellingen van Internet Explorer configureren](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Blader naar de pagina [MongoDB - Downloads](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads).

5. Standaard moet het de **Community Server** -editie en de meest recente huidige stabiele versie voor Windows Server 2008 R2 64-bits en hoger selecteren. Als u wilt het installatieprogramma downloaden, klikt u op **(msi) downloadt**.

    ![MongoDB installatieprogramma downloaden](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Nadat het downloaden voltooid is, voert u het installatieprogramma.

6. Lees en accepteer de licentieovereenkomst. Wanneer u wordt gevraagd, selecteert u **voltooid** installeren.

7. Klik op het laatste scherm, klikt u op **installeren**.


## <a name="configure-the-vm-and-mongodb"></a>De VM en MongoDB configureren

1. De padvariabelen worden niet bijgewerkt door het installatieprogramma van MongoDB. Zonder de MongoDB `bin` locatie in uw padvariabele, moet u het volledige pad opgeven telkens wanneer u een MongoDB uitvoerbaar bestand gebruiken. De locatie toevoegen aan uw padvariabele:

    - Met de rechtermuisknop op het menu **Start** en selecteer **systeem**.
    - Klik op **Geavanceerde systeeminstellingen**en klik op **Omgevingsvariabelen**.
    - Klik onder **Systeemvariabelen**, selecteer **pad**en klik vervolgens op **bewerken**.

    ![Padvariabelen configureren](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Het pad toevoegen aan uw MongoDB `bin` map. MongoDB is meestal geïnstalleerd `C:\Program Files\MongoDB`. Controleer of het installatiepad op uw VM. Het volgende voorbeeld wordt de standaard MongoDB installeren locatie waarop u wilt de `PATH` variabele:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Zorg ervoor dat u het begin van de regel toevoegen (`;`) om aan te geven dat u wilt toevoegen aan een locatie voor uw `PATH` variabele.

2. Maak MongoDB gegevens- en logbestanden mappen op de gegevensschijf. Selecteer in het menu **Start** **opdrachtprompt**. De volgende voorbeelden maken de mappen op station F:

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. Open een MongoDB-instantie met de volgende opdracht, het pad naar uw gegevens aanpassen en meld u mappen dienovereenkomstig gewijzigd:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Het kan enkele minuten duren voor MongoDB toewijzen de logboek-bestanden en begin te luisteren voor verbindingen. Alle berichten in het logboek worden doorgestuurd naar het bestand *F:\MongoLogs\mongolog.log* als `mongod.exe` server wordt gestart en logboek bestanden worden toegewezen.

    > [AZURE.NOTE] De opdrachtprompt blijft richten op deze taak terwijl uw MongoDB is gestart. Sluit het opdrachtpromptvenster kunnen blijven MongoDB uitvoeren. Of, MongoDB installeren als service, zoals wordt beschreven in de volgende stap.

4. Voor een krachtiger MongoDB-ervaring, installeert u de `mongod.exe` als een service. Maken van een service betekent dat u niet hoeft te verlaten een opdrachtprompt elke keer dat u wilt gebruiken, MongoDB. De service als volgt maken, past het pad naar de adreslijsten van uw gegevens- en logbestanden aan:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    De voorgaande opdracht Hiermee maakt u een service met MongoDB, de naam en een beschrijving van 'Mongo DB'. De volgende parameters worden ook opgegeven:

    - De `--dbpath` optie wordt de locatie van de gegevensmap.
    - De `--logpath` optie moet worden gebruikt om op te geven van een logboekbestand, omdat de actieve service geen een opdrachtvenster om uitvoer weer te geven.
    - De `--logappend` optie geeft aan dat een herstart van de service veroorzaakt uitvoer toe te voegen aan het bestaande bestand.

  Als u wilt de MongoDB-service starten, voert u de volgende opdracht uit:

    ```
    net start MongoDB
    ```

    Zie voor meer informatie over het maken van de MongoDB-service [configureren een Windows-Service voor MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Het exemplaar MongoDB testen

Met MongoDB wordt uitgevoerd met één exemplaar of als een service die u hebt geïnstalleerd, kunt u nu maken en gebruiken van uw databases starten. Start de administratieve shell MongoDB door een andere opdrachtvenster openen vanuit het menu **Start** en voer de volgende opdracht:

```
mongo  
```

U kunt een lijst met de databases met de `db` opdracht. Sommige gegevens bijvoegen:

```
db.foo.insert( { a : 1 } )
```

Zoeken naar gegevens als volgt:

```
db.foo.find()
```

De uitvoer is vergelijkbaar met het volgende voorbeeld:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Afsluiten de `mongo` console als volgt:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Firewall en regels voor netwerk-beveiligingsgroep configureren
Nu dat MongoDB geïnstalleerd en wordt uitgevoerd is, moet u een poort in Windows Firewall openen zodat u extern verbinding met MongoDB maken kunt. Als u wilt een nieuwe binnenkomende regel voor het toestaan van TCP-poorten 27017 maken, opent u een administratieve PowerShell-prompt en voer de volgende opdracht:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

U kunt ook de regel maken met behulp van de grafische Beheerhulpmiddel **Windows Firewall met geavanceerde beveiliging** . Maak een nieuwe binnenkomende regel TCP-poorten 27017 toestaan.

Indien nodig kunt u een regel netwerk beveiligingsgroep toegang geeft tot MongoDB van buiten de bestaande Azure virtuele netwerk subnet maken. U kunt de beveiligingsgroep van netwerk-regels maken met behulp van de [Azure beheerportal](virtual-machines-windows-nsg-quickstart-portal.md) of [Azure PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md). Als u met regels voor de Windows Firewall toestaan TCP-poorten 27017 aan de interface virtueel netwerk van uw MongoDB VM.

> [AZURE.NOTE] TCP-poorten 27017 is de standaardpoort naar die worden gebruikt door MongoDB. U kunt deze poort wijzigen met behulp van de `--port` parameter bij het starten van `mongod.exe` handmatig of van een service. Als u de poort verandert, zorg er dan voor dat bijwerken van de Windows Firewall uit en netwerk-beveiligingsgroep regels in de voorgaande stappen.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe installeren en configureren van MongoDB op uw Windows-VM. U kunt nu toegang tot MongoDB op uw Windows-VM, volgens de geavanceerde onderwerpen in de [documentatie van MongoDB](https://docs.mongodb.com/manual/).
