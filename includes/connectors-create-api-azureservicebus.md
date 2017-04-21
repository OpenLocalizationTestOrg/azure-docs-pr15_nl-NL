U kunt een trigger hebt toegevoegd, wordt de tijd wilt hebben met de gegevens die worden gegenereerd door de trigger interessant. Als volgt te werk om toe te voegen een actie in de **SharePoint Online - bestand maken** . Deze actie maakt een bestand in SharePoint Online telkens wanneer het nieuwe item trigger wordt uitgevoerd. 

Voor het configureren van deze actie, moet u de onderstaande informatie opgeven. U ziet dat deze eenvoudig te gebruiken gegevens die zijn gegenereerd door de trigger als invoer voor een deel van de eigenschappen voor het nieuwe bestand:

|Bestandseigenschap maken|Beschrijving|
|---|---|
|Site-URL|Dit is de URL van de SharePoint Online-site waar u wilt maken van het nieuwe bestand. Selecteer de site in de lijst die wordt gepresenteerd.|
|Pad naar de map|Dit is de map (op de Site-URL) waar het nieuwe bestand wordt geplaatst. Blader naar en selecteer de map.|
|Bestandsnaam|Dit is de naam van het bestand wordt gemaakt.|
|De inhoud van bestand|De inhoud die worden geschreven naar het bestand.|

1. Selecteer **+ nieuwe stap** de actie toevoegen.  
![](./media/connectors-create-api-sharepointonline/action-1.png)  
- Selecteer de koppeling **toevoegen een actie** . Dit wordt geopend zodra het zoekvak waarin u voor een actie u zoeken kunt wilt uitvoeren. In dit voorbeeld bent SharePoint acties geïnteresseerd.    
![](./media/connectors-create-api-sharepointonline/action-2.png)    
- Voer in *sharepoint* om te zoeken naar acties die betrekking hebben op SharePoint.
- Selecteer **SharePoint Online - bestand maken** als de actie moet worden uitgevoerd.   **Opmerking**: u wordt gevraagd uw app logica voor toegang tot uw SharePoint-account als u niet hebt gedaan eerder toe te staan.    
![](./media/connectors-create-api-sharepointonline/action-3.png)    
- Het besturingselement **maken bestand** wordt geopend.   
![](./media/connectors-create-api-sharepointonline/action-4.png)     
- Selecteer **Site-URL** en blader naar de site waar u naartoe wilt maken van het bestand.     
![](./media/connectors-create-api-sharepointonline/action-5.png)  
- Selecteer **mappad** en blader naar de map waar het nieuwe bestand wordt geplaatst.  
![](./media/connectors-create-api-sharepointonline/action-6.png)  
- Selecteer het besturingselement **de naam van het bestand** en voer de naam van het bestand dat u wilt maken. Voor de bestandsnaam, zoals u ziet dat u de eigenschappen van de trigger die u eerder hebt gemaakt door deze te selecteren in de lijst gepresenteerd kunt gebruiken.     
![](./media/connectors-create-api-sharepointonline/action-7.png)  
- Selecteer het besturingselement **bestandsinhoud** en voert u de inhoud die worden geschreven naar het bestand dat wordt gemaakt. Voor de inhoud van het bestand, zoals u ziet dat u kunt de eigenschappen van de trigger die u eerder hebt gemaakt. Selecteert u de eigenschappen in de lijst gepresenteerd. U kunt ook de **bestandsinhoud** tekst rechtstreeks in het besturingselement invoeren. In dit voorbeeld ik bepaalde eigenschappen geselecteerd en spaties en een streepje tussen elke eigenschap toegevoegd.        
![](./media/connectors-create-api-sharepointonline/action-8.png)  
- Sla de wijzigingen aan uw werkstroom  
