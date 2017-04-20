Nadat de records voor uw domeinnaam hebt doorgegeven, moet u deze koppelen aan uw Web-App. Gebruik de volgende stappen uit om in te schakelen van de domeinnamen via uw webbrowser.

> [AZURE.NOTE] Het kan even duren voordat TXT-records die zijn gemaakt in de vorige stappen voor het doorvoeren via het DNS-systeem. U kunt de domeinnaam van niet toevoegen aan uw web-app, totdat de TXT-record heeft doorgegeven. Als u een A-record gebruikt, kunt u de naam van het A-record domein niet naar uw web-app toevoegen totdat de TXT-record in de vorige stap hebt gemaakt heeft doorgegeven.
>
> U kunt een service zoals <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> gebruiken om te bevestigen dat de TXT-record beschikbaar is.

1. Open de [Portal van Azure](https://portal.azure.com)in uw browser.

2. Klik op de naam van uw web-app op het tabblad **Web Apps** en selecteer vervolgens **aangepaste domeinen**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. Klik in het blad **aangepaste domeinen** op **hostname toevoegen**.
    
4. Gebruik de tekstvakken **Hostname** om in te voeren van de domeinnamen aan deze WebApp wilt koppelen.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Klik op **valideren**.

7.  Na het klikken op **valideren** Azure wordt starten domeinverificatie werkstroom. Hiermee wordt gecontroleerd op eigendom van het domein, evenals Hostname beschikbaarheid en rapport bewerking is geslaagd of gedetailleerde fout opgetreden bij het beschrijvende guidence over hoe u de fout op te lossen.    

Nu moet u mogelijk voert de aangepaste domeinnaam in de browser en ziet u dat deze met succes u naar uw web-app gaat.
