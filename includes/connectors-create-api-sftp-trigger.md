Laten we een trigger toevoegen.

1. *Sftp* invoeren in het zoekvak op de logica apps ontwerpfunctie en selecteert u de trigger **SFTP - wanneer een bestand wordt toegevoegd of gewijzigd**   
![Afbeelding van de trigger SFTP 1](./media/connectors-create-api-sftp/trigger-1.png)  
- Het besturingselement **wanneer een bestand wordt toegevoegd of gewijzigd** wordt geopend  
![Afbeelding van de trigger SFTP 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Selecteer de **…** zich aan de rechterkant van het besturingselement. Hiermee opent u de map het besturingselement Datumkiezer  
![Afbeelding van de trigger SFTP 3](./media/connectors-create-api-sftp/action-1.png)  
- Selecteer de **SFTP** naar de hoofdmap selecteert als de map om te controleren op nieuwe of gewijzigde bestanden. Zoals u ziet dat de hoofdmap wordt nu weergegeven in het besturingselement **map** .  
![Afbeelding van de trigger SFTP 4](./media/connectors-create-api-sftp/action-2.png)   

Nu is uw app logica geconfigureerd met een trigger die een uitvoeren van de andere triggers en acties in de werkstroom wordt gestart wanneer een bestand wordt gewijzigd of in de specifieke SFTP-map gemaakt. 

>[AZURE.NOTE]Voor een logica-app wilt gebruiken, moet ten minste één trigger en een actie bevatten. Volg de stappen in de volgende sectie voor het toevoegen van een actie.  