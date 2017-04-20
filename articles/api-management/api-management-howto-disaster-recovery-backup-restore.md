<properties 
    pageTitle="Implementeren herstel met service back-up en herstellen in Azure API Management | Microsoft Azure" 
    description="Informatie over het gebruik van de back-up en herstellen als u wilt uitvoeren herstel in Azure API Management." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Implementeren herstel met service back-up en herstellen in Azure API Management

Door te kiezen om te publiceren en beheren van uw API's via Azure API Management profiteert u van veel fouttolerantie en infrastructuurmogelijkheden die u anders moet ontwerpen, te implementeren en te beheren. Het Azure platform vermindert een grote hoeveelheid mogelijke fouten op een deel van de kosten.

Als u wilt herstellen uit de beschikbaarheid van moeten problemen bij de regio waar uw API Management service wordt gehost worden opnieuw samenstellen van uw service in een andere regio op elk gewenst moment. Afhankelijk van uw beschikbaarheid doelstellingen en herstel tijd doelstelling wilt u mogelijk een back-service in een of meer regio's reserveren en probeert te onderhouden hun configuratie en de inhoud in synchronisatie met de actieve service. De service back-up en herstellen functie biedt de benodigde bouwsteen voor het implementeren van uw strategie voor noodgevallen herstel.

Deze handleiding ziet u hoe u de verificatie van Azure resourcemanager aanvragen en back-up maken en terugzetten van uw API Management service-exemplaren.

>[AZURE.NOTE] Het proces voor een back-up en herstellen van een exemplaar van de service API Management voor herstel kan ook worden gebruikt voor het repliceren API Management service-exemplaren voor scenario's zoals tijdelijke.
>
>Houd er rekening mee dat elke back-up na 7 dagen verloopt. Als u probeert een back-up herstellen nadat de verloopdatum van 7 dagen is verlopen, de terugzetten, mislukt met een `Cannot restore: backup expired` bericht.

## <a name="authenticating-azure-resource-manager-requests"></a>Verificatie Azure resourcemanager aanvraagt

>[AZURE.IMPORTANT] De REST API voor back-up en herstellen resourcemanager Azure gebruikt en heeft een andere verificatiemethode dan de REST API's voor het beheren van uw entiteiten API Management. De stappen in deze sectie wordt beschreven hoe Azure resourcemanager aanvragen verifiëren. Zie [Azure resourcemanager verifiëren aanvragen](http://msdn.microsoft.com/library/azure/dn790557.aspx)voor meer informatie.

Alle taken die u uitvoeren op resources met bronbeheer Azure moeten worden geverifieerd met Azure Active Directory met de volgende stappen.

-   Een toepassing toevoegen aan de tenant Azure Active Directory.
-   Machtigingen instellen voor de toepassing die u hebt toegevoegd.
-   De token ophalen voor het verifiëren van aanvragen naar Azure resourcemanager.

De eerste stap is het opzetten van een Azure Active Directory-toepassing. Meld u aan bij de [Klassieke Azure-Portal](http://manage.windowsazure.com/) met het abonnement met uw exemplaar van de service API Management en Ga naar het tabblad **toepassingen** voor uw standaard Azure Active Directory.

>[AZURE.NOTE] Als de standaardmap Azure Active Directory niet zichtbaar voor uw account is, neemt u contact op met de beheerder van het abonnement dat Azure om de vereiste machtigingen toewijzen aan uw account. Zie voor informatie over de locatie van de standaardmap [vinden het telefoonboek van uw standaard](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-portal).

![Azure Active Directory-toepassing maken][api-management-add-aad-application]

Klik op **toevoegen**, **het ontwikkelen van mijn organisatie toepassing toevoegen**, en kies **Native client-toepassing**. Voer een beschrijvende naam en klik op de pijl-rechts. Voer de URL van een tijdelijke aanduiding voor zoals `http://resources` voor de **URI omleiden**, als dit is een vereist veld, maar de waarde wordt niet later gebruikt. Klik op het selectievakje in om op te slaan van de toepassing.

Zodra de toepassing wordt opgeslagen, klikt u op **configureren**, schuif omlaag naar de sectie **machtigingen voor andere toepassingen** en klik op de **toepassing toevoegen**.

![Machtigingen toevoegen][api-management-aad-permissions-add]

**Windows** **Azure Service Management API** en klik op het selectievakje in als de toepassing wilt toevoegen.

![Machtigingen toevoegen][api-management-aad-permissions]

Klik op **Machtigingen gedelegeerde** naast de toegevoegde **Windows** **Azure Service Management API** -toepassing, schakel het selectievakje voor **Access Azure servicebeheer (preview)**en klikt u op **Opslaan**.

![Machtigingen toevoegen][api-management-aad-delegated-permissions]

Vóór het aanroepen van API's die genereren van de back-up en herstellen, is het nodig zijn om een token. Het volgende voorbeeld wordt de [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget-pakket om op te halen het token.

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;

    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }

                Console.WriteLine(result.AccessToken);

                Console.ReadLine();
            }
        }
    }

Vervang `{tentand id}`, `{application id}`, en `{redirect uri}` volgens de volgende instructies.

Vervang `{tenant id}` met de tenant-id van de Azure Active Directory-toepassing die u zojuist hebt gemaakt. U kunt de id openen door te klikken op **weergave-eindpunten**.

![Eindpunten][api-management-aad-default-directory]

![Eindpunten][api-management-endpoint]

Vervang `{application id}` en `{redirect uri}` gebruik van de **Client-Id** en de URL van de sectie **Omleiden URI's** van de Azure Active Directory-toepassing **configureren** tabblad. 

![Resources][api-management-aad-resources]

Wanneer de waarden zijn opgegeven, weer in het voorbeeld een token vergelijkbaar met het volgende voorbeeld.

![Token][api-management-arm-token]

Voordat u de back-up en herstellen bewerkingen in de volgende secties wordt beschreven, stel de kop van de aanvraag autorisatie voor de REST-oproep.

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>Back-up maken van een API Management-service
Back-up maken van een probleem het volgende HTTP van API Management serviceaanvraag:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

waarbij geldt:

* `subscriptionId`-id van het abonnement met de API-Management-service die u probeert aan back-up
* `resourceGroupName`-een tekenreeks in de vorm van 'Api - standaard-{service-regio}' waar `service-region` geeft aan wat het Azure regio waar de API Management service-u probeert te back-up wordt gehost, bijvoorbeeld`North-Central-US`
* `serviceName`-de naam van de API Management-service u stapt een back-up van opgegeven op het moment van het is gemaakt
* `api-version`-vervangen`2014-02-14`

Geef in de hoofdtekst van de aanvraag, de doeltoepassing Azure-opslagaccountnaam, toegangstoets, de naam van de container blob en de naam van de back:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

De waarde van de `Content-Type` verzoek kop naar `application/json`.

Back-up is een langdurige bewerking die kan meerdere minuten duren.  Als het verzoek geslaagd is en het back-proces is gestart ontvangt u een `202 Accepted` statuscode antwoord met een `Location` koptekst.  Controleer 'GET-aanvragen voor de URL in de `Location` koptekst om vast te stellen de status van de bewerking. Terwijl de back-up uitgevoerd wordt blijft u ontvangt een statuscode '202 geaccepteerde'. Een Antwoordcode van `200 OK` wordt gemeld dat de back-up is voltooid.

**Opmerking**:

- **Container** opgegeven in het verzoek hoofdtekst **moet bestaan**.
* Terwijl de back-up wordt uitgevoerd u **beter niet alle bewerkingen van de management service proberen** zoals SKU upgraden of downgraden, wijziging van de domeinnaam, enzovoort. 
* Herstel van een **back-up is gegarandeerd alleen voor 7 dagen** sinds het moment dat het is gemaakt. 
* **Gebruiksgegevens** die worden gebruikt voor het maken van analytics-rapporten **is niet opgenomen** in de back-up. [Azure API Management REST API][] gebruiken om op te halen regelmatig analytics-Rapporten maken.
* De frequentie waarmee u de service-back-ups uitvoeren op uw herstel punt doelstelling wordt bepaald. Als u wilt minimaliseren u wordt geadviseerd regelmatige back-ups implementeren, evenals uitvoering op aanvraag back-ups na belangrijke wijzigingen aanbrengt in uw API Management-service.
* **Wijzigingen** in de configuratie van de service (bijvoorbeeld API's, beleid, developer portal uiterlijk) aangebracht terwijl de back-up is in proces **mogelijk niet zijn opgenomen in de back-up en kunnen daarom niet verloren**.

## <a name="step2"> </a>Een API-beheerservice herstellen
Als u wilt herstellen van een Management API Controleer service uit een eerder gemaakte back-up de volgende HTTP-aanvraag:

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

waarbij geldt:

* `subscriptionId`-id van het abonnement met de API-beheerservice herstelt u een back-up in
* `resourceGroupName`-een tekenreeks in de vorm van 'Api - standaard-{service-regio}' waar `service-region` geeft aan wat het Azure regio waar de API-beheerservice herstelt u een back-up in wordt gehost, bijvoorbeeld`North-Central-US`
* `serviceName`-de naam van de API Management service wordt hersteld naar op het moment van het is gemaakt opgegeven
* `api-version`-vervangen`2014-02-14`

In de hoofdtekst van de aanvraag, de locatie van de back-upbestand, dat wil zeggen Azure-opslagaccountnaam, toegangstoets blob containernaam en de naam van de back-ups opgeven:

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

De waarde van de `Content-Type` verzoek kop naar `application/json`.

Terugzetten is een langdurige bewerking die meer dan 30 minuten om uit te voeren kan duren.  Als het verzoek geslaagd is en het herstelproces is gestart ontvangt u een `202 Accepted` statuscode antwoord met een `Location` koptekst.  Controleer 'GET-aanvragen voor de URL in de `Location` koptekst om vast te stellen de status van de bewerking. Terwijl de terugzetten uitgevoerd wordt blijft u ontvangen '202 geaccepteerde' statuscode. Een Antwoordcode van `200 OK` succesvolle afronding van het terugzetten wordt aangegeven.

>[AZURE.IMPORTANT] **De SKU** van de service wordt hersteld in **moet overeenkomen met** de SKU van de back-up-service wordt hersteld.
>
>**Wijzigingen** in de configuratie van de service (bijvoorbeeld API's, beleid, developer portal uiterlijk) aangebracht terwijl de bewerking voor terugzetten bevindt zich in de voortgang **kan worden overschreven**.

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende Microsoft-blogs voor twee verschillende scenario's van het back-up/herstelproces.

-   [Azure API Management Accounts repliceren](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Hartelijk dank naar Gisela voor haar bijdrage naar dit artikel.
-   [Beheer van Azure API: Een back-Up en herstellen configuratie](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   De methode gedetailleerde door Stuart niet overeenkomen met de officiële richtlijnen maar het is heel interessant.

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 
