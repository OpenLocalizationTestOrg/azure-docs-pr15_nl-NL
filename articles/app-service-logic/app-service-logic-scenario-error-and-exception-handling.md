<properties
    pageTitle="Logboekregistratie en foutverwerking in logica Apps | Microsoft Azure"
    description="Een praktijk use-case van geavanceerde fout afhandelen en aan te melden met logica Apps weergeven"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Logboekregistratie en foutverwerking in logica-Apps

In dit artikel wordt beschreven hoe u een app logica om betere ondersteuning afhandeling van uitzonderingen kunt uitbreiden. Dit is een praktijk use-case en onze antwoord op de vraag van "Logica Apps ondersteunt uitzondering en foutafhandeling?"

>[AZURE.NOTE]De huidige versie van de functie logica Apps van Microsoft Azure-App-Service biedt een standaardsjabloon voor actie antwoorden.
>Dit geldt ook voor zowel de interne validatie als de fout antwoorden geretourneerd uit een API-app.

## <a name="overview-of-the-use-case-and-scenario"></a>Overzicht van de use-case en scenario

Het volgende artikel is de use-case voor in dit artikel.
Een bekende gezondheidszorg organisatie betrokken blijft ons voor het ontwikkelen van een Azure oplossing die een patiënten portal met behulp van Microsoft Dynamics CRM Online wilt maken. Ze nodig voor het verzenden van afspraakrecords tussen de Dynamics CRM Online patiënten portal en -televergaderingen.  We zijn gevraagd naar de standaard [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) gebruiken voor alle patiënten records.

Het project hebt u er twee belangrijke vereisten:  

 -  Een methode aan te melden records die zijn verzonden vanuit de Dynamics CRM Online-portal
 -  Een manier om weer te geven van fouten die zijn aangebracht in de werkstroom


## <a name="how-we-solved-the-problem"></a>Hoe wij het probleem opgelost

>[AZURE.TIP] U kunt een video op hoog niveau van het project de [Integratie van gebruiker groep](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integratie gebruiker groep")bekijken.

We hebben gekozen [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") als opslagplaats voor de logboeken en fout records (DocumentDB verwijst naar de records als documenten). Omdat logica Apps een standaardsjabloon voor alle antwoorden heeft, zou doen wat u niet hoeft te maken van een aangepast schema. We kan een API-app in te **voegen** en de **Query** voor zowel fout als log records maken. We kan ook een schema voor elk binnen de app API definiëren.  

Er is een andere vereiste records na een bepaalde datum verwijderen. DocumentDB heeft een eigenschap met de naam van [Time to Live](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "Time to Live") (TTL), die toegestaan ons voor het instellen van een **Time to Live** -waarde voor elke record of de siteverzameling. Dit was het niet nodig handmatig om records te verwijderen in DocumentDB.

### <a name="creation-of-the-logic-app"></a>Het maken van de logica-app

De eerste stap is de logica-app maken en laadt u deze in de ontwerpfunctie. In dit voorbeeld gebruiken we bovenliggende en onderliggende logica apps. Stel dat we de bovenliggende al hebt gemaakt en wilt maken van een onderliggende logica-app.

Omdat we worden de record die afkomstig zijn uit Dynamics CRM Online registratie gaat, laten we beginnen aan de bovenkant. Moeten we een aanvraag-trigger gebruiken omdat de bovenliggende logica app deze kind activeert.

> [AZURE.IMPORTANT] Als u wilt deze zelfstudie hebt voltooid, moet u om een database DocumentDB en twee verzamelingen (logboekregistratie en fouten) te maken.

### <a name="logic-app-trigger"></a>Logica app-trigger

We gebruiken een aanvraag-trigger zoals wordt weergegeven in het volgende voorbeeld.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Stappen

Moeten we Meld u aan de bron (aanvraag) van de patiënten record uit de Dynamics CRM Online-portal.

1. Moeten we een nieuwe afspraakrecord ophalen uit Dynamics CRM Online.
    De trigger die afkomstig zijn uit CRM wij beschikken over de **CRM PatentId**, **recordtype**, **Nieuw of bijgewerkt-Record** (nieuwe of Booleaanse waarde bijwerken), en **SalesforceId**. De **SalesforceId** kan niet null zijn, omdat het alleen om een update gebruikt wordt.
    We krijgt de CRM-record met behulp van de CRM **PatientID** en het **Record Type**.
1. Vervolgens moeten we onze DocumentDB API-app **InsertLogEntry** bewerking toevoegen, zoals wordt weergegeven in de volgende cijfers.


#### <a name="insert-log-entry-designer-view"></a>Log vermelding ontwerpfunctie weergave invoegen

![Log fragmenten invoegen](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Fout-fragment ontwerpfunctie weergave invoegen
![Log fragmenten invoegen](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Controleren op record fout bij maken

![Voorwaarde](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Logica app-broncode

>[AZURE.NOTE]  Hier volgen enkele voorbeelden alleen. Omdat deze zelfstudie is gebaseerd op een implementatie die momenteel in productie, is het mogelijk dat de waarde van een **Bronknooppunt** niet het gewenste weergegeven eigenschappen die zijn gerelateerd aan een afspraak plannen.

### <a name="logging"></a>Logboekregistratie
De volgende logica app voorbeeld ziet u hoe u omgaat met logboekregistratie.

#### <a name="log-entry"></a>Vermelding
Dit is de logica app-broncode voor het invoegen van een logboek.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Log aanvraag

Dit is het logboek request-bericht geplaatst bij de API-app.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Log antwoord

Dit is het logboek antwoordbericht vanuit de API-app.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Nu eens de stappen voor foutafhandeling.


### <a name="error-handling"></a>Foutafhandeling

Het volgende voorbeeld logica Apps ziet u hoe u foutafhandeling kunt implementeren.

#### <a name="create-error-record"></a>Foutrecord maken

Dit is de logica Apps-broncode voor het maken van een foutrecord.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Fout bij invoegen in DocumentDB--aanvragen

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Fout in DocumentDB--antwoord invoegen


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>SalesForce-foutmelding

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>De reactie terug naar de bovenliggende logica app retourneren

Nadat u het antwoord hebt, kunt u deze doorgeven naar de bovenliggende logica-app.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Success antwoord naar de bovenliggende logica app retourneren

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Fout antwoord naar de bovenliggende logica app retourneren

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>DocumentDB opslagplaats en -portal

Onze oplossing toegevoegd extra mogelijkheden met [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Fout-beheerportal

Als u wilt de fouten weergeven, kunt u een MVC web-app om DocumentDB van de foutrecords weer te geven. **Lijst**, **Details**, **bewerken**en **verwijderen** bewerkingen worden opgenomen in de huidige versie.

> [AZURE.NOTE]Bewerking bewerken: DocumentDB heeft een vervangen van het hele document.
> De records die wordt weergegeven in de **lijst** en **gedetailleerde** weergaven zijn alleen voorbeelden. Ze zijn niet werkelijke patiënten afspraakrecords.

Hieronder vindt u voorbeelden van onze MVC appdetails die zijn gemaakt met de hierboven beschreven aanpak.

#### <a name="error-management-list"></a>Fout management lijst

![Lijst met fouten](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Fout management detailweergave

![Meer informatie over fout](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Log management portal

Als u wilt weergeven in de logboeken, ook die u hebt gemaakt een MVC web-app.  Hieronder vindt u voorbeelden van onze MVC appdetails die zijn gemaakt met de hierboven beschreven aanpak.

#### <a name="sample-log-detail-view"></a>Voorbeeld log detailweergave

![Log detailweergave](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API-appdetails

#### <a name="logic-apps-exception-management-api"></a>Logica Apps uitzondering management API

Onze open source logica Apps uitzondering management API-app biedt de volgende functionaliteit.

Er zijn twee controllers:

- **ErrorController** Hiermee voegt u een foutrecord (document) in een verzameling DocumentDB.
- **LogController** Hiermee voegt u een logboekrecord (document) in een verzameling DocumentDB.

> [AZURE.TIP] Beide controllers gebruiken `async Task<dynamic>` bewerkingen. Hiermee kunt bewerkingen die moeten worden opgelost gedurende runtime, zodat we het schema DocumentDB in de hoofdtekst van de bewerking maken kunnen.

Elk document in DocumentDB moet hebben een unieke ID. We gebruiken `PatientId` en het toevoegen van een tijdstempel dat is geconverteerd naar een Unix tijdstempel waarde (double). We afkappen als u wilt verwijderen van de decimale waarde.

U kunt de broncode van onze controller fout API [van GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)weergeven.

We bellen de API vanuit een logica-app met behulp van de volgende syntaxis.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

De expressie in het voorgaande voorbeeld wordt gecontroleerd of de status *Create_NewPatientRecord* **mislukt**.

## <a name="summary"></a>Overzicht

- U kunt op eenvoudige wijze logboekregistratie en foutverwerking in een app logica.
- U kunt DocumentDB gebruiken als opslagplaats voor logboeken en fout records (documenten).
- U kunt MVC gebruiken om te maken van een portal om Logboeken en fout records te geven.

### <a name="source-code"></a>Broncode
De broncode voor het beheer van logica Apps uitzondering API-toepassing is beschikbaar in deze [bibliotheek GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logica App uitzondering Management API").


## <a name="next-steps"></a>Volgende stappen
- [Meer voorbeelden van de logica Apps en scenario's weergeven](app-service-logic-examples-and-scenarios.md)
- [Meer informatie over de logica Apps hulpprogramma's voor controle](app-service-logic-monitor-your-logic-apps.md)
- [Een sjabloon voor een geautomatiseerde implementatie logica-App maken](app-service-logic-create-deploy-template.md)
