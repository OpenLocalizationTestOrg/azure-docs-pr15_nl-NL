Met Azure resourcemanager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters die alle van de parameterwaarden bevat.
U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die worden altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn geïmplementeerd. 

Bij het definiëren van parameters, moet u het **allowedValues** -veld gebruiken om te bepalen welke waarden die een gebruiker tijdens de implementatie bieden kan. Als u geen waarde is opgegeven tijdens de implementatie, kunt u het veld **defaultValue** gebruiken een waarde toewijzen aan de parameter.

Elke parameter in de sjabloon wordt beschreven.

### <a name="sitename"></a>Sitenaam

De naam van de web-app die u wilt maken.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

De naam van het abonnement dat App gebruiken voor het hosten van de web-app.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU

De prijzen laag voor de hosting-abonnement.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

De sjabloon definieert de waarden die zijn toegestaan voor deze parameter en wijst u een standaardwaarde (S1) als u geen waarde is opgegeven.

### <a name="workersize"></a>workerSize

De grootte van het exemplaar van het hostingprovider plan (klein, normaal of groot).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
De sjabloon definieert de waarden die zijn toegestaan voor deze parameter (0, 1 of 2) en wijst u een standaardwaarde (0) als u geen waarde is opgegeven. De waarden komen overeen met kleine, middelgrote en grote.
