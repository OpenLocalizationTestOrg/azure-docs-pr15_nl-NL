U kunt een trigger hebt toegevoegd, wordt de tijd wilt hebben met de gegevens die worden gegenereerd door de trigger interessant. Als volgt te werk om toe te voegen een de actie **SFTP - uitpakken map** . Hiermee wordt de inhoud van een bestand uitgepakt als de gedefinieerde voorwaarden is voldaan. 

Voor het configureren van deze actie, moet u de onderstaande informatie opgeven. U ziet dat deze eenvoudig te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een deel van de eigenschappen voor het nieuwe bestand:

|SFTP - uitpakken folder, eigenschap|Beschrijving|
|---|---|
|Bestandspad voor bron archief|Dit is het pad voor het bestand uitgepakt. U kunt een van de tokens selecteren uit een eerdere actie of bladeren door de server SFTP om te zoeken van het bestandspad.|
|Bestemming mappad|Dit is het pad waar de uitgepakte bestanden wordt geplaatst. U kunt een van de tokens selecteren uit een eerdere actie als het pad naar de bestemming of bladeren door de server SFTP en selecteer een pad.|
|Overschrijven?|Geeft aan als een bestand met dezelfde naam als het uitgepakte bestand u in het pad van de map bestemming vindt als het bestaande bestand moet worden overschreven of niet.|

Laten we aan de slag de actie als u wilt extraheren van de bestanden als de voorwaarde die eerder hebt gedefinieerd in *waar resulteert*toe te voegen. 

1. Selecteer **een actie toevoegen**.        
![Afbeelding van de voorwaarde SFTP actie 6](./media/connectors-create-api-sftp/condition-6.png)   
- Selecteer de actie **SFTP - uitpakken map**      
![Afbeelding van de voorwaarde SFTP actie 7](./media/connectors-create-api-sftp/condition-7.png)   
- Selecteer **pad archiefbestand bron**              
![Afbeelding van de voorwaarde SFTP actie 9](./media/connectors-create-api-sftp/condition-9.png)   
- Selecteer het **bestandspad** token. Hiermee wordt aangegeven dat u het pad van het bestand dat de trigger gevonden als pad van het archiefbestand bron wilt gebruiken.           
![Afbeelding van de voorwaarde SFTP actie 10](./media/connectors-create-api-sftp/condition-10.png)   
- Selecteer de **bestemming mappad**           
![Afbeelding van de voorwaarde SFTP actie 11](./media/connectors-create-api-sftp/condition-11.png)   
- Selecteer het **bestandspad** token. Hiermee wordt aangegeven dat u het pad van het bestand dat de trigger gevonden als het bestemmingspad wilt gebruiken voor de opgehaalde bestanden.   
- Voer *\ExtractedFile* in het besturingselement **mappad bestemming** . Deze stap herhalen direct na de token voor het pad van bestand in het besturingselement bestemming map pad.         
![Afbeelding van de voorwaarde SFTP actie 12](./media/connectors-create-api-sftp/condition-12.png)   
- Geef *waar* in de **overschrijven?* besturingselement om aan te geven dat de bestaande bestanden moeten worden overschreven als ze dezelfde naam als de opgehaalde bestanden hebben.      
![Afbeelding van de voorwaarde SFTP actie 13](./media/connectors-create-api-sftp/condition-13.png)   
- Sla de wijzigingen aan uw werkstroom  
