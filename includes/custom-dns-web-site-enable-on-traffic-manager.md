Nadat de records voor uw domeinnaam hebt doorgegeven, moet u mogelijk zijn uw browser gebruiken om te bevestigen dat de naam van uw aangepaste domein kan worden gebruikt voor toegang tot uw web-app in Azure App-Service.

> [AZURE.NOTE] Het kan even duren voordat uw CNAME-record met doorvoeren via het DNS-systeem. U kunt een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> gebruiken om te bevestigen dat de CNAME beschikbaar is.

Als u uw web-app hebt niet als een eindpunt verkeer Manager hebt toegevoegd, moet u dit doen voordat naamresolutie, als het aangepaste domein naam routes naar verkeer Manager werkt. Verkeer Manager stuurt vervolgens door naar uw web-app. Gebruik de informatie in [toevoegen of verwijderen eindpunten](../articles/traffic-manager/traffic-manager-endpoints.md) toe te voegen van uw web-app als een eindpunt in uw profiel verkeer Manager.

> [AZURE.NOTE] Als uw web-app niet wordt vermeld bij het toevoegen van een eindpunt, controleert u of deze is geconfigureerd voor **Standard** App-Service plannen modus. U moet **standaardmodus voor uw web-app** gebruiken om te kunnen werken met verkeer Manager.

1. Open de [Portal van Azure](https://portal.azure.com)in uw browser.

1. Klik op de naam van uw web-app op het tabblad **Web Apps** , selecteer **Instellingen**en selecteer **aangepaste domeinen**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. Klik in het blad **aangepaste domeinen** op **hostname toevoegen**.
    
1. Typt de naam van het verkeer Manager domein koppelen aan deze WebApp via de tekstvakken **Hostname** .

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Klik op **valideren** om op te slaan de configuratie domain name.

7.  Na het klikken op **valideren** Azure wordt starten domeinverificatie werkstroom. Hiermee wordt gecontroleerd op eigendom van het domein, evenals Hostname beschikbaarheid en rapport bewerking is geslaagd of gedetailleerde fout opgetreden bij het beschrijvende guidence over hoe u de fout op te lossen.    

8.  Na succesvolle validatie **toevoegen hostname** van de knop kracht en u kunnen de hostnaam toewijzen. Ga naar de naam van uw aangepaste domein in een browser. U ziet nu het uitvoeren van uw app met uw aangepaste domeinnaam. 

    Wanneer de configuratie is voltooid, wordt de aangepaste domeinnaam worden vermeld in de sectie **domain names** van uw web-app.

Nu moet u mogelijk voert de naam van het verkeer Manager-domein in de browser en ziet u dat deze met succes u naar uw web-app gaat.
