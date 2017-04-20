<properties
    pageTitle="Bouwen en implementeren van een Java-API-app in Azure App-Service"
    description="Leer hoe u een Java-API app-pakket maken en het dashboard implementeren naar Azure App-Service."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Bouwen en implementeren van een Java-API-app in Azure App-Service

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Deze zelfstudie leert hoe u een Java-toepassing maken en het dashboard implementeren naar Azure App Service API Apps [cijfer]gebruiken. De instructies in deze zelfstudie kunnen op elk besturingssysteem dat Java waarop kan worden gevolgd. De code in deze zelfstudie is ingebouwd [Maven]gebruiken. [Jax-RS] gebruikt voor het maken van de RESTful Service en is gegenereerd op basis van de metagegevens [Swagger] specificatie met de [Editor Swagger].

## <a name="prerequisites"></a>Vereisten voor

1. [Java Developer's Kit 8] \(of hoger)
1. [Maven] geïnstalleerd op uw computer ontwikkeling
1. [Cijfer] geïnstalleerd op uw computer ontwikkeling
1. Een betaalde of [gratis proefversie] abonnement op [Microsoft Azure]
1. Een HTTP-test-toepassing zoals [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>De API Swagger.IO met scaffold

Met de online swagger.io-editor, kunt u JSON Swagger of YAML code dat staat voor de structuur van uw API invoeren. Nadat u het API-oppervlak ontworpen hebt, kunt u de code voor een groot aantal platforms en kaders exporteren. In de volgende sectie, wordt de scaffolded code worden aangepast model functionaliteit is opgenomen. 

Deze demo begint met een Swagger JSON-instantie die u in de editor swagger.io die vervolgens worden gebruikt om code maakt gebruik van de JAX-RS voor toegang tot een REST API-eindpunt te genereren wordt plakt. Klik, wordt bewerkt u de scaffolded code om terug te keren model gegevens, een REST API gebaseerd op een gegevens permanente om simuleren.  

1. De volgende Swagger JSON-code naar het Klembord kopiëren:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Navigeer naar de [Online Swagger Editor]. Wanneer er, klikt u op het menu **Bestand -> Plakken JSON** item.

    ![Plakken JSON menu-item][paste-json]

1. Plak de contactpersonen lijst API Swagger JSON u eerder hebt gekopieerd. 

    ![JSON-code plakken in Swagger][pasted-swagger]

1. De documentatie pagina's en overzicht van de API weergegeven in de editor weergeven. 

    ![Weergave Swagger gegenereerd documenten][view-swagger-generated-docs]

1. Selecteer de menuoptie **genereren Server-JAX-RS >** aan de serverzijde-code die u later bewerken om toe te voegen model implementatie scaffold. 

    ![Menu-Item Code genereren][generate-code-menu-item]

    Zodra de code wordt gegenereerd, vindt u wordt een ZIP-bestand wilt downloaden. Dit bestand bevat de code scaffolded door de Swagger-code genereren en alle die horen bij het maken van scripts. Pak de hele bibliotheek naar een map op uw ontwikkelwerkstation. 

## <a name="edit-the-code-to-add-api-implementation"></a>De Code om toe te voegen API-implementatie bewerken

In deze sectie, moet u de Swagger gegenereerde code aan de clientzijde implementatie vervangen door uw aangepaste code. De nieuwe code retourneert een ArrayList van de contactpersonen naar de klant bellen. 

1. Open het bestand *Contact.java* model, dat zich bevindt in de map *src/generatie/java/io/swagger/model* [Visual Studio-Code] of uw favoriete teksteditor. 

    ![Open contactpersonen modelbestand][open-contact-model-file]

1. De volgende constructor toevoegen aan de klasse van de **contactpersoon** . 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Open het *ContactsApiServiceImpl.java* implementatie servicebestand, dat zich bevindt in de map *src/Hoofdgegeven/java/io/swagger/api/impl* [Visual Studio-Code] of uw favoriete teksteditor.

    ![Open contactpersonen Code servicebestand][open-contact-service-code-file]

1. De code in het bestand overschrijven met deze nieuwe code een model implementatie toevoegen aan de servicecode. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Open een opdrachtprompt en wijzig directory in de hoofdmap van uw toepassing.

1. Voer de volgende opdracht Maven de code maken en uitvoeren met de Jetty app-server lokaal. 

        mvn package jetty:run

1. Hier ziet u het opdrachtvenster doorgevoerd dat Jetty uw code op poort 8080 heeft gestart. 

    ![Open contactpersonen Code servicebestand][run-jetty-war]

1. Gebruik [Postman] om een verzoek om u aan de 'krijgt alle contactpersonen' API methode voor http://localhost:8080/api/contacten.

    ![De contactpersonen API bellen][calling-contacts-api]

1. [Postman] gebruiken om een nieuw vergaderverzoek met de API 'krijgt specifieke contactpersoon' methode vinden op http://localhost:8080/api/contactpersonen/2.

    ![De contactpersonen API bellen][calling-specific-contact-api]

1. Ten slotte het Java WAR (Web-archief)-bestand maken door het uitvoeren van de volgende opdracht uit Maven bij uw console. 

        mvn package war:war

1. Zodra het WAR-bestand is gemaakt, worden deze in **de doelmap** geplaatst. Navigeer naar **de doelmap** en wijzig het WAR-bestand in **ROOT.war**. (Zorg dat het hoofdlettergebruik deze indeling).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Ten slotte de volgende opdrachten worden uitgevoerd vanuit de hoofdmap van uw toepassing om een map **implementeren** gebruiken om te implementeren het WAR-bestand naar Azure te maken. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>De uitvoer publiceren naar Azure App-Service

In deze sectie hebt u meer informatie over het maken van een nieuwe API-App met behulp van de Azure-Portal, die API-App voorbereiden voor het hosten van Java-toepassingen en het gemaakte WAR-bestand implementeren naar Azure App Service naar uw nieuwe API-App uitvoeren. 

1. Maak een nieuwe API-app in de [portal van Azure]door te klikken op het **Web-Nieuw > + Mobile-API-app >** menu-item, de appdetails van uw invoeren en vervolgens te klikken op **maken**.

    ![Een nieuwe API-App maken][create-api-app]

1. Nadat uw API-app is gemaakt, open blade van van uw app- **Instellingen** en klik vervolgens op het menu-item **Toepassingsinstellingen** . Selecteert u de meest recente Java-versies van de beschikbare opties, selecteer de meest recente Tomcat in het menu **Web container** , en klik vervolgens op **Opslaan**.

    ![Java instellen in het blad API-App][set-up-java]

1. Klik op de menuopdracht voor instellingen van **implementatie referenties** en geef een gebruikersnaam en wachtwoord die u gebruiken wilt voor het publiceren van bestanden naar uw API-App. 

    ![Implementatie-referenties instellen][deployment-credentials]

1. Klik op de opdracht van de **bron van de implementatie** -instellingen. Wanneer er, klikt u op de knop **kiezen bron** , selecteer de optie **Lokale cijfer opslagplaats** en klik vervolgens op **OK**. Hiermee maakt u een cijfer opslagplaats in Azure wordt aangegeven, die een koppeling met de API-App wordt uitgevoerd. Elke keer dat u code op de *basispagina* -vertakking van uw bibliotheek cijfer toepast worden uw code gepubliceerd in uw live exemplaar voor doorlopende API-App. 

    ![Een nieuwe lokale cijfer opslagplaats instellen][select-git-repo]

1. De URL van de nieuwe cijfer-bibliotheek naar het Klembord kopiëren. Deze opslaan als dit belangrijk in even is. 

    ![Een nieuwe cijfer opslagplaats voor de app instellen][copy-git-repo-url]

1. Cijfer push het WAR-bestand naar de online-bibliotheek. Ga hiervoor naar de map **implementeren** die u eerder hebt gemaakt zodat u de code snel aan de bibliotheek in uw App-Service met eenvoudig kunt doorvoeren. Nadat u in het consolevenster bent en genavigeerd naar de map waarin de map webapps zich bevindt, moet u de volgende cijfer-opdrachten voor het proces te starten en een implementatie starten. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Nadat u de **push** -aanvraag verzendt, wordt u gevraagd het wachtwoord dat u eerder voor de implementatie-referentie hebt gemaakt. Nadat u uw referenties invoert, ziet u de portal weergeven dat de update is geïmplementeerd.

1. Als u nog een keer Postman te raken de zojuist geïmplementeerd API-App met App-Azure-Service gebruikt, ziet u dat het gedrag consistent is en dat nu deze contactpersonen gegevens retourneert zoals verwacht en het gebruik van eenvoudige codewijzigingen in de Swagger.io scaffolded Java-code. 

    ![Uw Java contactpersonen REST API live gebruiken in Azure wordt aangegeven][postman-calling-azure-contacts]

## <a name="next-steps"></a>Volgende stappen

In dit artikel, het is om te beginnen met een Swagger JSON-bestand en enkele scaffolded Java-code uit de editor Swagger.io opgehaald. Hierin de eenvoudige wijzigingen en een cijfer implementeren proces voor het hebben van een functionele API-app geschreven in Java het resultaat. Het volgende zelfstudie wordt getoond hoe [API-apps van JavaScript-clients, met CORS]in beslag neemt[App Service API CORS]. Het implementeren van verificatie en machtiging weergeven later zelfstudies in de reeks

In dit voorbeeld samenstellen, kunt u meer informatie over de [Opslag SDK for Java] persistent de JSON BLOB's. Of u kunt het [Document DB Java SDK] uw Contact-gegevens opslaan in Azure Document DB. 

Zie het [Java Developer Center]voor meer informatie over het gebruik van Java in Azure wordt aangegeven.

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure-portal]: https://portal.azure.com/
[DB Java document SDK]: ../documentdb/documentdb-java-application.md
[gratis proefversie]: https://azure.microsoft.com/pricing/free-trial/
[Cijfer]: http://www.git-scm.com/
[Java Developer Center]: /develop/java/
[Java Developer's Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger-Editor]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Opslag SDK for Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger-Editor]: http://editor.swagger.io/
[Visual Studio-Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
