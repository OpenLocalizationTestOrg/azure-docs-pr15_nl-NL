<properties 
   pageTitle="Aan de slag met Azure gegevens Lake winkels met behulp van Azure SDK voor Node.js | Microsoft Azure"
   description="Leer hoe u Node.js gebruiken om te werken met Lake gegevensopslag accounts en het bestandssysteem." 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-sdk-for-nodejs"></a>Aan de slag met Azure Lake gegevensopslag met behulp van Azure SDK voor Node.js

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)


Informatie over het gebruik van de SDK Azure voor Node.js een Azure Lake gegevensopslag-account maken en uitvoeren van basisbewerkingen bijvoorbeeld maakt mappen, gegevensbestanden uploaden en downloaden verwijderen van uw account, enzovoort. Zie voor meer informatie over gegevensopslag Lake, [Overzicht van Lake gegevensopslag](data-lake-store-overview.md). Op dit moment ondersteunt de SDK

  *  **Node.js versie: 0.10.0 of hoger**
  *  **REST API-versie voor Account: 2015-10-01-preview**
  *  **REST API-versie voor bestandssysteem: 2015-10-01-preview**

## <a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Een Azure Active Directory-toepassing maken**. De Azure AD-toepassing kunt u de toepassing Lake gegevensopslag met Azure AD verifiëren. Er zijn verschillende manieren om te verifiëren met Azure AD, welke **eindgebruikers** of **service-naar-service-verificatie**. Zie voor instructies en meer informatie over het om te verifiëren, [verifiëren met Lake gegevensopslag Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="how-to-install"></a>Het installeren van

```bash
npm install azure-arm-datalake-store
```

## <a name="authenticate-using-azure-active-directory"></a>Verificatie met Azure Active Directory

De onderstaande fragmenten weergeven twee verschillende manieren met Lake gegevensopslag te verifiëren met Azure AD. Zie voor een gedetailleerde discussie op verschillende methoden voor verificatie met Lake gegevensopslag, [verifiëren met Lake gegevensopslag Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

Het codefragment van de onderstaande vereist ook invoeritems zoals Azure AD-domeinnaam, cliënt-ID voor een Azure AD-app, enzovoort. Alle deze gegevens kunnen worden opgehaald uit een Azure AD-toepassing die u moet hebt gemaakt, de details van die ook zijn opgenomen in de bovenstaande koppeling.

 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## <a name="create-the-data-lake-store-clients"></a>De Clients Lake gegevensopslag maken

```javascript
var adlsManagement = require("azure-arm-datalake-store");
var acccountClient = new adlsManagement.DataLakeStoreAccountClient(credentials, "your-subscription-id");
var filesystemClient = new adlsManagement.DataLakeStoreFileSystemClient(credentials);
```

## <a name="create-a-data-lake-store-account"></a>Een Account voor gegevensopslag Lake maken

```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlsacct';
var location = 'eastus2';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="create-a-file-with-content"></a>Een bestand met inhoud maken
```javascript
var util = require('util');
var accountName = 'testadlsacct';
var fileToCreate = '/myfolder/myfile.txt';
var options = {
  streamContents: new Buffer('some string content')
}

filesystemClient.fileSystem.listFileStatus(accountName, fileToCreate, options, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    // no result is returned, only a 201 response for success.
    console.log('response is: ' + util.inspect(response, {depth: null}));
  }
});
```

## <a name="get-a-list-of-files-and-folders"></a>Een lijst met bestanden en mappen

```javascript
var util = require('util');
var accountName = 'testadlsacct';
var pathToEnumerate = '/myfolder';
filesystemClient.fileSystem.listFileStatus(accountName, pathToEnumerate, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## <a name="see-also"></a>Zie ook

- [Microsoft Azure SDK voor Node.js](https://github.com/azure/azure-sdk-for-node)
- [Microsoft Azure SDK voor Node.js - Lake gebruiksanalyses gegevensbeheer](https://www.npmjs.com/package/azure-arm-datalake-analytics)
