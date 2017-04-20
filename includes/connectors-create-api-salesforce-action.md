U kunt een voorwaarde hebt toegevoegd, wordt de tijd wilt hebben met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen als de actie **Salesforce - Get-object** wilt toevoegen. Deze actie krijgt de gegevens telkens wanneer die een nieuwe potentiële klant is gemaakt. U wordt ook een tweede actie die wordt gebruikt de gegevens uit de Salesforce - om een actie bij object naar een e-mail verzenden via de Office 365-verbindingslijn toevoegen.  

Voor het configureren van deze actie, moet u de onderstaande informatie opgeven. U ziet dat deze eenvoudig te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een deel van de eigenschappen voor het nieuwe bestand:

|Bestandseigenschap maken|Beschrijving|
|---|---|
|Objecttype|Dit is het type Salesforce-object waarin u geïnteresseerd bent. Voorbeelden zijn potentiële klant, Account, enzovoort.|
|Object-ID|Hiermee wordt een id voor het object.|


1. Selecteer de koppeling **toevoegen een actie** . Dit wordt geopend zodra het zoekvak waarin u voor een actie u zoeken kunt wilt uitvoeren. In dit voorbeeld bent Salesforce-acties geïnteresseerd.      
![Afbeelding van de actie SalesForce 1](./media/connectors-create-api-salesforce/action-1.png)  
- Voer *salesforce* om te zoeken naar acties die betrekking hebben op salesforce.
- Selecteer **Salesforce - Get-object** als de actie moet worden uitgevoerd.   **Opmerking**: u wordt gevraagd uw app logica voor toegang tot uw Salesforce-account als u niet hebt gedaan eerder toe te staan.    
![Afbeelding van de actie SalesForce 2](./media/connectors-create-api-salesforce/action-2.png)    
- Het besturingselement voor het **ophalen van object** wordt geopend.  
- Selecteer de *potentiële klant* als het objecttype.
- Selecteer het **Object-ID** -besturingselement.
- Selecteer **…** om uit te vouwen van de lijst met tokens die kan worden gebruikt als invoer voor acties.       
![Afbeelding van de actie SalesForce 3](./media/connectors-create-api-salesforce/action-3.png)    
- Selecteer besturingselement van **Potentiële klant-ID** wordt geopend.   
![Afbeelding van de actie SalesForce 4](./media/connectors-create-api-salesforce/action-4.png)     
- U ziet dat de potentiële klant-ID-token bevindt zich nu in het besturingselement van Object-ID, dat aangeeft dat de actie bij object Get wordt gezocht naar een potentiële klant met een ID die gelijk is aan de potentiële klant-ID van potentiële klant die deze app logica geactiveerd.  
![Afbeelding van de actie SalesForce 5](./media/connectors-create-api-salesforce/action-5.png)  
- Sla uw werk. Dat is het, u kunt de actie bij object ophalen hebt toegevoegd aan uw logica-app. Uw controle object ziet er als volgt:    
![Afbeelding van de actie SalesForce 6](./media/connectors-create-api-salesforce/action-6.png)  

U kunt een actie om een potentiële klant hebt toegevoegd, wilt u mogelijk doen iets interessants met de zojuist gemaakte potentiële klant. In een in het bedrijf wilt u mogelijk een e-mail sturen naar een distributielijst een melding dat er een nieuwe potentiële klant is gemaakt. Laten we de Office 365-connector gebruiken om een e-mailbericht met een deel van de relevante informatie uit het nieuwe potentiële klant-object in Salesforce.  

1. Selecteer **een actie toevoegen** en voer de *e-mailadres* in het besturingselement zoeken. Hiermee filtert de acties die betrekking hebben op e-mail verzenden en ontvangen.  
- Selecteer het item van de lijst met **Office 365 Outlook - een e-mailbericht verzenden** . Als u al een *verbinding* met uw Office 365-account hebt gemaakt, wordt u gevraagd om uw Office 365 als u wilt deze nu hebt gemaakt. Nadat u klaar bent, wordt het besturingselement voor het **verzenden van een e-mailbericht** geopend.        
![Afbeelding van de actie SalesForce 7](./media/connectors-create-api-salesforce/action-7.png)  
- Voer het e-mailadres dat u wilt een e-mail sturen naar in het besturingselement **aan** .
-  In het besturingselement **onderwerp** , typ *nieuwe potentiële klant gemaakt* - Klik op het *bedrijf* token. Het veld van het *bedrijf* van de nieuwe potentiële klant die is gemaakt in Salesforce wordt weergegeven.  
-  U kunt een van de tokens selecteren in het nieuwe potentiële klant-object in de **hoofdtekst** , en u kunt ook de tekst die u wilt weergeven in de hoofdtekst van het e-mailbericht invoeren. Hier volgt een voorbeeld:  
![Afbeelding van de actie SalesForce 8](./media/connectors-create-api-salesforce/action-8.png)   
- Sla uw werkstroom.  

Dat is. Uw app logica is voltooid.  

Nu kunt u uw app logica testen: in Salesforce, maakt u een nieuwe potentiële klant die voldoet aan de voorwaarde die u hebt gemaakt.  Als u dit overzicht volledig hebt uitgevoerd, kunt u alleen een potentiële klant met een e-mailadres met *amazon.com* erin maken. Na een paar seconden uw app logica moet worden gestart en de resultaten kunnen er ongeveer als volgt uitzien:  
![Afbeelding van de actie SalesForce 9](./media/connectors-create-api-salesforce/action-9.png)  

