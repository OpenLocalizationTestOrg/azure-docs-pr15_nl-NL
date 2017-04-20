### <a name="prerequisites"></a>Vereisten voor
- Een Azure-account; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een [Azure-blobopslag account](../articles/storage/storage-create-storage-account.md) waaronder de naam van het opslag-account en de access-sleutel. Deze informatie wordt weergegeven in de eigenschappen van het account opslag in de portal van Azure. Meer informatie over de [Opslag van Azure](../articles/storage/storage-introduction.md).

Voordat u uw account Azure-blobopslag gebruikt in een app logica, verbinding maken met uw account Azure-blobopslag. U kunt dit eenvoudig doen binnen uw app logica op de Azure-portal.  

Verbinding maken met uw Azure-blobopslag account met de volgende stappen:  

1. Een app logica maken. Klik in de ontwerpfunctie logica Apps een trigger toevoegen en vervolgens een actie toevoegen. Selecteer **Microsoft weergeven beheerde API's** in de vervolgkeuzelijst en voer 'blob' in het zoekvak. Selecteer een van de acties:  

    ![Azure-blobopslag verbinding maken stap](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Als u geen verbindingen met Azure opslag nog niet eerder hebt gemaakt, kunt u wordt gevraagd om de verbindingsgegevens:   

    ![Azure-blobopslag verbinding maken stap](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Geef de details van de account opslag. Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Meer informatie |
|---|---|
| Verbindingsnaam * | Voer een naam voor de verbinding. |
| Azure-Opslagaccountnaam * | Voer de naam van het opslag-account. De naam van het opslag-account wordt weergegeven in de eigenschappen van opslag in de portal van Azure. |
| Toegangstoets Azure opslag-Account * | Voer de accountsleutel opslag. De toegangstoetsen worden weergegeven in de eigenschappen van opslag in de portal van Azure. |

    Deze referenties worden gebruikt voor het toestaan van uw app logica verbinding maakt en toegang tot uw gegevens. 

4. Selecteer **maken**.

5. Zoals u ziet dat de verbinding is gemaakt. Nu kunt u doorgaan met de overige stappen in uw app logica: 

    ![Azure-blobopslag verbinding maken stap](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
