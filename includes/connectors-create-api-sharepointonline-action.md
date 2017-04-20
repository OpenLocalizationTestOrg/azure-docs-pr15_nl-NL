U kunt een trigger hebt toegevoegd, wordt de tijd wilt hebben met de gegevens die worden gegenereerd door de trigger interessant. Volg deze stappen om de **SharePoint Online - bestand maken** actie toevoegen. Deze actie maakt een bestand in SharePoint Online telkens wanneer het nieuwe item trigger wordt uitgevoerd. 

Voor het configureren van deze actie, moet u de onderstaande informatie opgeven. Als u deze gegevens opgeeft, ziet u dat dit eenvoudig te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een deel van de eigenschappen voor het nieuwe bestand is:

|Bestandseigenschap maken|Beschrijving|
|---|---|
|Site-URL|Dit is de URL van de SharePoint Online-site waar u wilt maken van het nieuwe bestand. Selecteer de site in de lijst die wordt gepresenteerd.|
|Pad naar de map|Dit is de map (op de Site-URL geselecteerd in de vorige stap) waar het nieuwe bestand wordt geplaatst. Blader naar en selecteer de map.|
|Bestandsnaam|Dit is de naam van het bestand wordt gemaakt.|
|De inhoud van bestand|De inhoud die worden geschreven naar het bestand.|

1. Selecteer **+ nieuwe stap** de actie toevoegen.  
![Afbeelding van SharePoint online actie 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Selecteer de koppeling **toevoegen een actie** . Dit wordt geopend zodra het zoekvak waarin u voor een actie u zoeken kunt wilt uitvoeren. In dit voorbeeld worden SharePoint-acties van belang zijn.    
![Afbeelding van SharePoint online actie 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- Voer in *sharepoint* om te zoeken naar acties die betrekking hebben op SharePoint.
- Selecteer **SharePoint Online - bestand maken** als de actie moet worden uitgevoerd.   **Opmerking**: u wordt gevraagd om te machtigen uw app logica voor toegang tot uw SharePoint-account als u nog geen verbinding maken met SharePoint Online eerder hebt gemaakt.    
![Afbeelding van SharePoint online actie 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Het besturingselement **maken bestand** wordt geopend.   
![Afbeelding van SharePoint online actie 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Selecteer **Site-URL** en blader naar de site waar u naartoe wilt maken van het bestand.     
![SharePoint online actie afbeelding 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Selecteer **mappad** en blader naar de map waar het nieuwe bestand wordt geplaatst.  
![SharePoint online actie afbeelding 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Selecteer het besturingselement **de naam van het bestand** en voer de naam van het bestand dat u wilt maken. Hier, u kunt de bestandsnaam rechtstreeks invoeren of u kunt de eigenschappen van de trigger die u eerder hebt gemaakt. U doet dit door eigenschappen te selecteren in de lijst met **uitvoer wanneer een nieuw item wordt gemaakt**. Deze lijst is alleen-weergeven nadat u het besturingselement **de naam van het bestand** hebt geselecteerd. In dit walkthough, ik-ID (de ID van het nieuwe lijstitem) hebt geselecteerd als de naam van het bestand wordt gemaakt door de **SharePoint Online - bestand maken** actie.    
![Afbeelding van SharePoint online actie 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Selecteer het besturingselement **bestandsinhoud** en voert u de inhoud die worden geschreven naar het bestand dat wordt gemaakt. Voor de inhoud van het bestand, zoals u ziet dat u kunt de eigenschappen van de trigger die u eerder hebt gemaakt. Selecteert u de eigenschappen in de lijst gepresenteerd. U kunt ook de **bestandsinhoud** tekst rechtstreeks in het besturingselement invoeren. In dit voorbeeld ik bepaalde eigenschappen geselecteerd en spaties en een streepje tussen elke eigenschap toegevoegd.        
![SharePoint online actie afbeelding 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Sla de wijzigingen aan uw werkstroom  
- Gefeliciteerd, u hebt nu een volledig functioneel logica-app die wordt uitgevoerd wanneer een nieuw item wordt toegevoegd aan een SharePoint Online-lijst. De app maakt vervolgens een bestand, met enkele van de eigenschappen van het nieuwe lijstitem.  U kunt deze nu testen door te maken van een nieuw item in de SharePoint-lijst. 
