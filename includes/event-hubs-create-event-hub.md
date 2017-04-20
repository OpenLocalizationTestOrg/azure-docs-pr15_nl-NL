## <a name="create-an-event-hub"></a>De Hub van een gebeurtenis maken

1. Meld u aan bij de [portal van Azure][]en klikt u op **Nieuw** aan de bovenkant van het scherm naar links.

2. Klik op **gegevens + Analytics**en klik op **Gebeurtenis Hubs**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. Typ de naamruimtenaam van een in het blad **naamruimte maken** . Het systeem onmiddellijk wordt gecontroleerd of de naam beschikbaar is.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Wanneer de naamruimtenaam van de ervoor te zorgen dat beschikbaar is, kiest u de prijzen laag (Basic of standaard). Kies ook een Azure-abonnement, resourcegroep en locatie voor het maken van de resource. 

2. Klik op **maken** om te maken van de naamruimte.

6. Klik in de lijst van de naamruimte gebeurtenis Hubs op de naamruimte gemaakte.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. Klik in het blad naamruimte op **Gebeurtenis Hubs**.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. Klik op **Gebeurtenis Hub toevoegen**aan de bovenkant van het blad.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Typ een naam voor de Hub van de gebeurtenis en klik op **maken**.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. In de lijst met gebeurtenis Hubs, klikt u op de naam van de zojuist gemaakte gebeurtenis Hub. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Terug in het blad naamruimte (niet het specifieke blad gebeurtenis Hub) **clienttoegangsbeleid gedeeld**op en klik vervolgens op **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. Klik op de knop kopiëren om de verbindingsreeks **RootManageSharedAccessKey** naar het Klembord kopiëren. Sla deze verbindingsreeks gebruiken verderop in deze zelfstudie.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Uw Hub gebeurtenis is nu gemaakt en hebt u de verbindingstekenreeksen die u wilt verzenden en ontvangen van gebeurtenissen.

[Azure-portal]: https://portal.azure.com/