De meeste van de tijd verificatiefouten resultaat zijn van onjuiste of inconsistente configuratie-instellingen. Hier volgen enkele specifieke suggesties voor wat u moet controleren.

* Zorg ervoor dat u de knop **Opslaan** ergens niet mist. Dit is vaak eenvoudig te doen, en het resultaat is dat u hebt voor het bekijken van de juiste waarden op een pagina in de portal, maar ze daadwerkelijk nog niet hebt opgeslagen in de Azure-omgeving of Azure AD-toepassing.
* Zorg ervoor dat de juiste API-app of WebApp is geselecteerd wanneer de instellingen zijn ingevoerd voor instellingen geconfigureerd in het blad **Toepassingsinstellingen** van de Azure-portal.  Ook voor zorgen dat de instellingen zijn ingevoerd als de **App-instellingen** en geen **tekenreeksen met de verbinding**, zoals de opmaak van de twee secties lijkt.
* Download het manifest opnieuw ter controle die voor verificatie van een front-end JavaScript, `oauth2AllowImplicitFlow` is gewijzigd naar `true`.
* Controleer of u HTTPS waar u URL's hebt geconfigureerd:

    * In projectcode
    * In CORS
    * In de App-instellingen Azure-omgeving voor elke API-app en WebApp
    * In de instellingen van Azure AD-toepassing.
    
    Houd er rekening mee dat als u een API-app-URL van de portal kopieert, vaak er `http://` en u moet deze handmatig wijzigen `https://`.

* Zorg ervoor dat de codewijzigingen zijn geïmplementeerd. In een veelvoud is van een project-oplossing is het bijvoorbeeld mogelijk is om te wijzigen van een project-code en kies een van de andere per ongeluk wanneer u wilt de wijziging implementeren.
* Zorg ervoor dat u HTTPS-URL's in uw browser niet HTTP-URL's wilt. Standaard Visual Studio maakt profielen met HTTP-URL's publiceren en dat dit is wat wordt geopend in de browser nadat u een project hebt geïmplementeerd.
* Zorg ervoor dat CORS correct is geconfigureerd op de API-app waarin de JavaScript-code wordt aangeroepen voor verificatie naar een front-end JavaScript. Als u twijfelt of het probleem is CORS-gerelateerde probeert "*" Als de toegestane origin-URL. 
* Voor een front-end JavaScript, opent u van uw browser ontwikkelaars hulpmiddelen voor Console tabblad om meer foutinformatie opvragen en HTTP-verzoeken in het netwerk onderzoeken. Console-foutberichten mogelijk misleidende. Als er een bericht waarin wordt aangegeven dat een fout CORS, mogelijk een probleem met de reële verificatie. U kunt controleren of dit het geval is door de app uit te voeren met verificatie tijdelijk tijdelijk uitgeschakeld.
* Zorg dat u op het punt zo veel informatie in foutberichten mogelijk door in te stellen [customErrors-modus op uit](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview)voor een .NET-API-app.
* Een [externe sessie voor foutopsporing](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)starten voor een .NET-API-app, en de waarde van de variabelen die worden doorgegeven aan de code waarmee wordt ADAL in het bezit van een dragertoken of code die vorderingen op de verwachte service principal ID. controleert controleren Houd er rekening mee dat uw code om de configuratiewaarden uit verschillende bronnen, kiezen kunt zodat het mogelijk om te zoeken verrassingen deze manier is. Als u verkeerd bijvoorbeeld `ida:ClientId` als `ida:ClientID` tijdens het configureren van omgevingsinstellingen Azure App-Service, de code kan worden de `ida:ClientId` waarde die wordt gezocht van het bestand Web.config, worden genegeerd de instelling Azure App-Service. 
* Als u dingen werken niet in een normale Internet Explorer-venster, een bestaande aanmelden mogelijk wordt verhinderd; InPrivate en probeer Chrome of Firefox.
