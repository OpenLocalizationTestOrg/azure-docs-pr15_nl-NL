
Standaard kunnen API's in een mobiele App backend anoniem worden aangeroepen. Vervolgens moet u toegang beperken tot alleen geverifieerde clients.  

+ **Node.js backend (via de portal)** :  
    
    In de Mobile-App- **Instellingen**, klik op **Eenvoudige tabellen** en selecteer de tabel. Klik op **machtigingen wijzigen**, selecteert u **alleen geverifieerde toegang** voor alle machtigingen en klik op **Opslaan**. 

+ **.NET backend (C#)**:  

    Ga in het serverproject, **controller** > **TodoItemController.cs**. Toevoegen de `[Authorize]` als volgt aan de klasse **TodoItemController** kenmerk. Als u wilt toegang beperken tot specifieke methoden, kunt u ook dit kenmerk toepassen alleen op deze methoden in plaats van de klas. Het serverproject publiceren.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js backend (via Node.js code)** :  
    
    Als u wilt verificatie vereist voor toegang tot tabellen, voeg de volgende regel aan het Node.js server-script:


        table.access = 'authenticated';

    Raadpleeg voor meer informatie [hoe: verificatie vereisen voor toegang tot tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Zie informatie over het downloaden van het CodeProject quickstart van uw site, [hoe: downloaden van het Node.js backend quickstart CodeProject cijfer met](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

