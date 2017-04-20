Met Azure resourcemanager u parameters definiëren voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters die alle van de parameterwaarden bevat.
U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u implementeert op definiëren. Parameters voor waarden die worden altijd hetzelfde blijft geen definieert. Elke parameterwaarde wordt gebruikt in de sjabloon definiëren van de resources die zijn implementeren. 

Bij het definiëren van parameters, moet u het **allowedValues** -veld gebruiken om te bepalen welke waarden die een gebruiker tijdens de implementatie bieden kan. Als u geen waarde is opgegeven tijdens de implementatie, kunt u het veld **defaultValue** gebruiken een waarde toewijzen aan de parameter.

Elke parameter in de sjabloon wordt beschreven.

### <a name="logicappname"></a>logicAppName

De naam van de logica-app maken.

    "logicAppName": {
        "type": "string"
    }