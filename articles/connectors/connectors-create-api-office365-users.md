<properties
    pageTitle="De verbindingslijn Office 365-gebruikers in logica Apps toevoegen | Microsoft Azure"
    description="Overzicht van Office 365-gebruikers verbindingslijn met REST API parameters"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Aan de slag met de Office 365-gebruikers-connector

Verbinding maken met Office 365-gebruikers om profielen, gebruikers van de zoekfunctie en meer. 

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2015-08-01-schema voorbeeldversie.

Met Office 365-gebruikers, kunt u het volgende doen:

- De bedrijfswerkstroom van uw op basis van de gegevens die u van Office 365-gebruikers ontvangt maken. 
- Gebruik acties die directe ondergeschikten, krijgen van een manager-gebruikersprofiel en meer. Deze acties bent u een reactie en klikt u vervolgens de uitvoer beschikbaar maken voor andere acties. Bijvoorbeeld ophalen van een persoon directe ondergeschikten, nemen deze informatie en een SQL Azure-database bijwerken. 

Als u wilt een bewerking in logica apps toevoegen, raadpleegt u [een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Triggers en acties

De verbindingslijn Office 365-gebruikers heeft de volgende acties beschikbaar. Er zijn geen triggers.

| Triggers | Acties|
| --- | --- |
|Geen | <ul><li>Manager verkrijgen</li><li>Mijn profiel ophalen</li><li>Ondergeschikten ophalen</li><li>Gebruikersprofiel ophalen</li><li>Zoeken naar gebruikers</li></ul>|

Alle verbindingslijnen ondersteuning voor gegevens in JSON en XML-indelingen. 


## <a name="create-a-connection-to-office-365-users"></a>Een verbinding maken met Office 365-gebruikers

Wanneer u deze connector aan uw apps logica toevoegt, moet u aanmelden bij uw account voor Office 365-gebruikers en toestaan logica apps verbinding maken met uw account.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Nadat u de verbinding hebt gemaakt, voert u de eigenschappen in Office 365-gebruikers, zoals de gebruikers-ID. De **REST API verwijzing** in dit onderwerp worden deze eigenschappen.

>[AZURE.TIP] U kunt deze dezelfde verbinding met Office 365-gebruikers in andere apps logica.


## <a name="office-365-users-rest-api-reference"></a>Referentie voor Office 365-gebruikers REST API
Dit geldt voor versie: 1.0.

### <a name="get-my-profile"></a>Mijn profiel ophalen 
Hiermee haalt u het profiel voor de huidige gebruiker.  
```GET: /users/me``` 

Er zijn geen parameters voor dit gesprek.

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|202|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


### <a name="get-user-profile"></a>Gebruikersprofiel ophalen 
Hiermee haalt u een specifiek gebruikersprofiel.  
```GET: /users/{userId}``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen|Gebruikers principal naam of het e-id|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|202|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|


### <a name="get-manager"></a>Manager verkrijgen 
Gebruikersprofiel van de manager is van de opgegeven gebruiker worden opgehaald.  
```GET: /users/{userId}/manager``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen|Gebruikers principal naam of het e-id|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|202|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|



### <a name="get-direct-reports"></a>Ondergeschikten ophalen 
Ondergeschikten krijgen.  
```GET: /users/{userId}/directReports``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|gebruikers-id|tekenreeks|Ja|pad|geen|Gebruikers principal naam of het e-id|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|202|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|



### <a name="search-for-users"></a>Zoeken naar gebruikers 
Haalt de zoekresultaten van gebruikersprofielen.  
```GET: /users``` 

| Naam| Gegevenstype|Vereist|Zich bevindt In|Standaardwaarde|Beschrijving|
| ---|---|---|---|---|---|
|searchTerm|tekenreeks|geen|query|geen|Tekenreeks zoeken (van toepassing op: naam, voornaam, achternaam, mail, mail bijnaam en gebruiker UPN weergeven)|

#### <a name="response"></a>Antwoord

|Naam|Beschrijving|
|---|---|
|200|Bewerking is geslaagd|
|202|Bewerking is geslaagd|
|400|BadRequest|
|401|Onbevoegde|
|403|Verboden|
|500|Interne serverfout|
|Standaard|Bewerking is mislukt.|



## <a name="object-definitions"></a>Objectdefinities

#### <a name="user-user-model-class"></a>Gebruiker: Klasse User-model

|Naam van eigenschap | Gegevenstype |Vereist
|---|---|---|
|Weergavenaam|tekenreeks|geen|
|GivenName|tekenreeks|geen|
|Achternaam|tekenreeks|geen|
|E-mail|tekenreeks|geen|
|MailNickname|tekenreeks|geen|
|TelephoneNumber|tekenreeks|geen|
|AccountEnabled|Booleaanse waarde|geen|
|ID|tekenreeks|Ja
|UserPrincipalName|tekenreeks|geen|
|Afdeling|tekenreeks|geen|
|Functie|tekenreeks|geen|
|mobilePhone|tekenreeks|geen|


## <a name="next-steps"></a>Volgende stappen

[Een app logica maken](../app-service-logic/app-service-logic-create-a-logic-app.md).

Ga terug naar de [lijst API's](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
