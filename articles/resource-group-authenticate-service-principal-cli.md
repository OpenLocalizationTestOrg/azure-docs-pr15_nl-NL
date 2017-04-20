<properties
   pageTitle="Service principal maken met Azure CLI | Microsoft Azure"
   description="Het gebruik van Azure CLI maken van een toepassing voor de Active Directory en de service principal en toegang tot bronnen via Rolgebaseerd toegangsbeheer wordt beschreven. Deze ziet hoe u het verifiëren van toepassing met een wachtwoord of het certificaat."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Azure CLI gebruiken om te maken van een service principal voor toegang tot bronnen

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wanneer u een toepassing of script die vereist zijn voor toegang tot bronnen hebt, wilt hebt u waarschijnlijk niet uitvoeren dit proces met uw eigen referenties. Mogelijk hebt u verschillende machtigingen die u wilt gebruiken voor de toepassing en u niet wilt dat de toepassing om door te gaan met uw referenties als verantwoordelijkheden wijzigen. In plaats daarvan kunt u een identiteit voor de toepassing met verificatiereferenties en roltoewijzingen maken. Telkens wanneer de app wordt uitgevoerd, deze wordt geverifieerd met deze referenties. In dit onderwerp ziet u hoe u [Azure CLI voor Mac, Linux en Windows](xplat-cli-install.md) gebruiken voor het instellen van een toepassing wilt uitvoeren onder een eigen referenties en -identiteit.

Met Azure CLI hebt u twee opties voor het verifiëren van uw AD-toepassing:

 - wachtwoord
 - certificaat

Dit onderwerp wordt uitgelegd hoe u beide opties gebruiken in Azure CLI. Als u zich wilt aanmelden bij Azure uit een programming framework (zoals Python, Ruby of Node.js), is het wachtwoordverificatie mogelijk de beste optie. Voordat u beslist of een wachtwoord of het certificaat wilt gebruiken, raadpleegt u de sectie [voorbeeldtoepassingen](#sample-applications) voor de voorbeelden in de verschillende kaders te verifiëren.

## <a name="active-directory-concepts"></a>Active Directory-concepten

In dit artikel, moet u twee objecten - de toepassing AD (Active Directory) en de service principal maken. De AD-toepassing is de algemene weergave van uw toepassing. De presentatie bevat de referenties (een toepassings-id en een wachtwoord of certificaat). De hoofdsom service is de lokale weergave van uw toepassing in een Active Directory. De presentatie bevat de roltoewijzing. In dit onderwerp is gericht op een toepassing voor de één-tenant waar de toepassing is bedoeld om uit te voeren in slechts één organisatie. Meestal gebruikt u één-tenant toepassingen voor LOB-toepassingen die worden uitgevoerd binnen uw organisatie. In een enkel-tenant-toepassing hebt u een AD-app en één service principal.

Vraagt u zich worden af - waarom heb ik nodig beide objecten? Deze benadering is handig wanneer u rekening houden met meerdere tenant-toepassingen. Meestal gebruikt u meerdere tenant toepassingen voor software-als-een-service (SaaS)-toepassingen, waar uw toepassing wordt uitgevoerd in veel verschillende abonnementen. Voor meerdere tenant-toepassingen hebt u een AD-app en meerdere service principes (één in elke Active Directory die toegang tot de app verleent). Raadpleeg [de handleiding voor ontwikkelaars naar autorisatie met de API van Azure resourcemanager](resource-manager-api-authentication.md)instellen van een toepassing voor meerdere tenant.

## <a name="required-permissions"></a>Vereiste machtigingen

Als u wilt voltooien in dit onderwerp, moet u over de machtigingen in zowel uw Azure Active Directory en uw Azure-abonnement. Specifiek, moet u kunnen maken van een app in de Active Directory en de hoofdsom service toewijzen aan een rol. 

In uw Active Directory moet uw account een beheerder (zoals **Globale beheerder** of **Beheerder van de gebruiker**). Als uw account is toegewezen aan de rol van de **gebruiker** , moet u beschikken over een beheerder uw machtigingen verhogen.

Uw abonnement, moet uw account hebben `Microsoft.Authorization/*/Write` toegang is verleend tot en met de rol van de [eigenaar](./active-directory/role-based-access-built-in-roles.md#owner) of de [gebruiker toegang tot](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) de beheerdersrol. Als uw account is toegewezen aan de rol **Inzender** , ontvangt u een fout tijdens een poging om de service-hoofdsom toewijzen aan een rol. Klik nogmaals moet de beheerder van uw abonnement u voldoende toegang verlenen.

Nu gaat u verder met een sectie voor [wachtwoord](#create-service-principal-with-password) of [certificaat](#create-service-principal-with-certificate) verificatie.

## <a name="create-service-principal-with-password"></a>Service principal maken met een wachtwoord

In deze sectie, kunt u Voer de stappen uit als u wilt de AD-toepassing maken met een wachtwoord, en de rol van de lezer toewijzen aan de service principal.

In dit artikel leest u over de volgende stappen uit.

1. Meld u aan bij uw account.

        azure login

1. U hebt twee opties voor het maken van de AD-toepassing. U kunt de AD-toepassing en de service hoofdsom in één stap maken, of ze afzonderlijk maken. Als u niet nodig hebt Geef een startpagina en id URI's voor de app, moet u deze maken in één stap. Maak deze afzonderlijk als u nodig hebt voor het instellen van deze waarden voor een web-app. Beide opties worden weergegeven in deze stap.

     - Als u wilt maken de AD-toepassing en service principal in één stap, bieden u de naam van de app en een wachtwoord, zoals wordt weergegeven in de volgende opdracht uit:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Als u wilt de AD-toepassing afzonderlijk maken, geef de naam van de app, een startpagina URI id URI's en een wachtwoord, zoals wordt weergegeven in de volgende opdracht uit:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         De voorgaande opdracht retourneert de waarde van een toepassings-id. Als u wilt maken van een service principal, bieden u die waarde als een parameter in de volgende opdracht uit:
     
            azure ad sp create -a <AppId>
     
     Als uw account heeft geen de [vereiste machtigingen](#required-permissions) op de Active Directory, wordt een foutbericht weergegeven waarin wordt aangegeven dat 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.
    
     Voor beide opties voor de nieuwe service principal geretourneerd. De **Object-Id** nodig bij het toewijzen van machtigingen. De guid weergegeven met de **Namen van de Service Principal** nodig is wanneer u zich aanmeldt. Deze guid is hetzelfde resultaat als de app-id. In de steekproef-toepassingen, worden deze waarde wordt de **Client-ID**genoemd. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Verleen de service principal machtigingen voor uw abonnement. In dit voorbeeld kunt u de service-hoofdsom toevoegen aan de functie **Reader** , die verleent leesmachtiging alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde rollen](./active-directory/role-based-access-built-in-roles.md). Geef de **object-id** die u hebt gebruikt bij het maken van de toepassing voor de parameter **ServicePrincipalName** . 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht wordt weergegeven. Het bericht geeft aan uw account **heeft geen autorisatie actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid} wordt uitgevoerd**. 

Dat is alles. Uw AD-toepassing en service principal zijn ingesteld. Het volgende gedeelte ziet u hoe u aanmelden met de referentie via Azure CLI. Als u de referentie gebruiken in uw toepassing code wilt, hoeft u niet wilt doorgaan met dit onderwerp. U kunt gaan naar de [steekproef toepassingen](#sample-applications) voor voorbeelden van aanmelden met uw toepassings-id en wachtwoord. 

### <a name="provide-credentials-through-azure-cli"></a>Referenties via Azure CLI opgeven

Nu, moet u aanmelden als de toepassing bewerkingen uit te voeren.

1. Wanneer u zich als een service principal aanmelden, moet u de tenant-id van de map opgeven voor de AD-app. Een tenant is een exemplaar van Active Directory. Als u wilt ophalen van de tenant-id voor uw abonnement op dit moment geverifieerde, gebruiken:

        azure account show

     Welke geeft als resultaat:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Als u nodig hebt om de tenant-id van een ander abonnement, gebruikt u de volgende opdracht uit:

        azure account show -s {subscription-id}

2. Als u nodig hebt om op te halen van de client-id wilt gebruiken voor het aanmelden, gebruikt:

        azure ad sp show -c exampleapp --json

     De waarde moet worden gebruikt voor logboekregistratie in is de guid weergegeven in de hoofdsom servicenamen.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Meld u aan als de service-principal.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    U wordt gevraagd om het wachtwoord. Het wachtwoord die u hebt opgegeven bij het maken van de AD-toepassing opgeven.

        info:    Executing command login
        Password: ********

U bent nu geverifieerd als de service-principal voor de service-principal die u hebt gemaakt.

## <a name="create-service-principal-with-certificate"></a>Service principal maken met certificaat

In deze sectie, kunt u de stappen voor het uitvoeren:

- een zelfondertekend certificaat maken
- de AD-toepassing maken met het certificaat en de hoofdsom service
- de rol van de lezer toewijzen aan de hoofdsom service

Als u wilt deze stappen hebt voltooid, moet u [OpenSSL](http://www.openssl.org/) is geïnstalleerd.

1. Een zelfondertekend certificaat maken.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. De gedeelde en persoonlijke sleutels combineren.

        cat privkey.pem cert.pem > examplecert.pem

3. Open het bestand **examplecert.pem** en zoek de lange reeks tekens tussen **---BEGIN certificaat---** en **---BEËINDIGEN certificaat---**. Kopieer de certificaatgegevens. U kunt deze gegevens als een parameter doorgeven bij het maken van de service principal.

1. Meld u aan bij uw account.

        azure login

1. U hebt twee opties voor het maken van de AD-toepassing. U kunt de AD-toepassing en de service hoofdsom in één stap maken, of ze afzonderlijk maken. Als u niet nodig hebt Geef een startpagina en id URI's voor de app, moet u deze maken in één stap. Maak deze afzonderlijk als u nodig hebt voor het instellen van deze waarden voor een web-app. Beide opties worden weergegeven in deze stap.

     - Als u wilt maken de AD-toepassing en service principal in één stap, bieden u de naam van de app en de certificaatgegevens, zoals wordt weergegeven in de volgende opdracht uit:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Als u wilt de AD-toepassing afzonderlijk maken, geef de naam van de app, een startpagina URI id URI's en de certificaatgegevens, zoals wordt weergegeven in de volgende opdracht uit:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         De voorgaande opdracht retourneert de waarde van een toepassings-id. Als u wilt maken van een service principal, bieden u die waarde als een parameter in de volgende opdracht uit:
     
            azure ad sp create -a <AppId>
  
     Als uw account heeft geen de [vereiste machtigingen](#required-permissions) op de Active Directory, wordt een foutbericht weergegeven waarin wordt aangegeven dat 'Authentication_Unauthorized' of 'geen abonnement gevonden in de context'.
    
     Voor beide opties voor de nieuwe service principal geretourneerd. De Object-Id nodig bij het toewijzen van machtigingen. De guid weergegeven met de **Namen van de Service Principal** nodig is wanneer u zich aanmeldt. Deze guid is hetzelfde resultaat als de app-id. In de steekproef-toepassingen, worden deze waarde wordt de **Client-ID**genoemd. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Verleen de service principal machtigingen voor uw abonnement. In dit voorbeeld kunt u de service-hoofdsom toevoegen aan de functie **Reader** , die verleent leesmachtiging alle resources in het abonnement. Zie voor andere functies, [RBAC: ingebouwde rollen](./active-directory/role-based-access-built-in-roles.md). Geef de **object-id** die u hebt gebruikt bij het maken van de toepassing voor de parameter **ServicePrincipalName** . 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Als uw account beschikt niet over voldoende machtigingen aan een rol toewijzen, ziet u een foutbericht wordt weergegeven. Het bericht geeft aan uw account **heeft geen autorisatie actie 'Microsoft.Authorization/roleAssignments/write' bereik/abonnementen / {guid} wordt uitgevoerd**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Geef certificaat tot en met geautomatiseerde Azure CLI-script

Nu, moet u aanmelden als de toepassing bewerkingen uit te voeren.

1. Wanneer u zich als een service principal aanmelden, moet u de tenant-id van de map opgeven voor de AD-app. Een tenant is een exemplaar van Active Directory. Als u wilt ophalen van de tenant-id voor uw abonnement op dit moment geverifieerde, gebruiken:

        azure account show

     Welke geeft als resultaat:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Als u nodig hebt om de tenant-id van een ander abonnement, gebruikt u de volgende opdracht uit:

        azure account show -s {subscription-id}

1. Als u wilt ophalen vingerafdruk van het certificaat en verwijder overbodige tekens, gebruiken:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Welke geeft als resultaat een vingerafdrukwaarde vergelijkbaar met:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Als u nodig hebt om op te halen van de client-id wilt gebruiken voor het aanmelden, gebruikt:

        azure ad sp show -c exampleapp

     De waarde moet worden gebruikt voor logboekregistratie in is de guid weergegeven in de hoofdsom servicenamen.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Meld u aan als de service-principal.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

U bent nu geverifieerd als de service hoofdsom voor de Active Directory-toepassing die u hebt gemaakt.

## <a name="sample-applications"></a>Voorbeeldtoepassingen

De volgende toepassingen van de steekproef hoe meld u aan als de hoofdsom service.

**.NET**

- [Een SSH implementeren VM met een sjabloon met .NET ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure resources en resourcegroepen met .NET beheren](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Aan de slag met informatiebronnen - implementeren met Azure resourcemanager sjabloon - in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Aan de slag met informatiebronnen - beheren resourcegroep - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Een SSH implementeren VM met een sjabloon in Python ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure Resource en resourcegroepen met Python beheren](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Een SSH implementeren VM met een sjabloon in Node.js ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure resources en resourcegroepen met Node.js beheren](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Een SSH implementeren VM met een sjabloon in Ruby ingeschakeld](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure Resource en resourcegroepen met Ruby beheren](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Volgende stappen
  
- Zie voor meer informatie over de integratie van een toepassing in Azure voor het beheren van resources [handleiding voor ontwikkelaars naar autorisatie met de API van Azure resourcemanager](resource-manager-api-authentication.md).
- Zie voor meer informatie over het gebruik van certificaten en Azure CLI, [certificaatverificatie met principes van Azure-Service vanaf de opdrachtregel Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
