<properties
   pageTitle="Snelstartgids voor de Azure AD-grafiek API | Microsoft Aure"
   description="De Azure Active Directory Graph API biedt programma toegang tot Azure AD via OData REST API-eindpunten. Toepassingen kunnen de grafiek-API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen voor directory-gegevens en objecten."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Snelstartgids voor de Azure AD-grafiek API

De Azure Active Directory (AD) Graph-API biedt programma toegang tot Azure AD via OData REST API-eindpunten. Toepassingen kunnen de grafiek-API gebruiken om uit te voeren maken, lezen, bijwerken en verwijderen (CRUD)-bewerkingen voor directory-gegevens en objecten. U kunt bijvoorbeeld de API grafiek naar een nieuwe gebruiker maken, bekijken of bijwerken van gebruikersgegevens, gebruikerswachtwoord wijzigen, groepslidmaatschap voor access op basis van rollen controleren, uitschakelen of verwijderen van de gebruiker. Zie [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) en [Azure AD Graph API vereisten](https://msdn.microsoft.com/library/hh974476.aspx)voor meer informatie over de grafiek API-functies en de toepassing scenario's. 

> [AZURE.IMPORTANT] Azure AD Graph API-functionaliteit is ook beschikbaar via de [Microsoft Graph](https://graph.microsoft.io/), een geïntegreerd API die API's van andere Microsoft-services zoals Outlook, OneDrive, OneNote, teamplanner en Office Graph, toegankelijk via een enkelvoudig eindpunt en met een enkel toegangstoken bevat.

## <a name="how-to-construct-a-graph-api-url"></a>Het maken van de URL van een grafiek-API

Toegang tot directory-gegevens en objecten (met andere woorden, resources of entiteiten) die u wilt CRUD-bewerkingen uitvoeren in Graph API, kunt u URL's op basis van het gegevensbestand (OData)-Protocol gebruiken. De URL's die in de grafiek API bestaan uit vier hoofdgebieden: service-hoofdsite, tenant-id, resource-pad en tekenreeks queryopties: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Volg het voorbeeld van de volgende URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Service-hoofdsite**: In Azure AD Graph API, Ga naar de hoofdsite van de service is altijd https://graph.windows.net.
- **Tenant-id**: dit is een gecontroleerde domeinnaam (geregistreerde), in het voorbeeld hierboven contoso.com. Ook u kunt werken een tenant object-ID of de "myorganiztion" of "me" alias. Zie voor meer informatie, [adressering van entiteiten en bewerkingen in de grafiek-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Resource-pad**: dit gedeelte van een URL geeft de bron om te worden gecommuniceerd met (gebruikers, groepen, een bepaalde gebruiker of een bepaalde groep, enz.) In het bovenstaande voorbeeld is het hoogste niveau 'groepen' naar adres dat resource instellen. U kunt ook adres van een specifieke entiteit, bijvoorbeeld "gebruikers / {object-id}" of ' gebruikers/userPrincipalName'.
- **Queryparameters**:? worden gescheiden door het gedeelte van de resource pad van het gedeelte query parameters. De queryparameter 'api-versie' is vereist voor alle aanvragen in de grafiek-API. De grafiek-API ondersteunen ook de volgende opties van de OData-query: **$filter**, **$orderby** **$uitvouwen**, **$top**en **$format**. De volgende queryopties worden momenteel niet ondersteund: **$count**, **$inlinecount**en **$skip**. Zie [query's worden ondersteund, Filters, en opties voor paginering in Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options)voor meer informatie.

## <a name="graph-api-versions"></a>Graph API-versies

U geeft de versie voor een aanvraag Graph API in de queryparameter 'api-versie'. Voor versie 1.5 en hoger gebruik u de versiewaarde van een numerieke; API-versie = 1,6. Voor eerdere versies gebruikt u een datumtekenreeks die aan de notatie jjjj-MM-DD voldoet; bijvoorbeeld api-versie = 2013-11-08. Voor de preview-functies, gebruikt u de tekenreeks "beta"; bijvoorbeeld api-versie beta =. Zie voor meer informatie over de verschillen tussen versies Graph API, [Azure AD Graph API versiebeheer](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>API Graph-metagegevens

Als u wilt retourneren van het bestand API Graph-metagegevens, voegt u het segment "$metadata" na de tenant-id in het voorbeeld-URL voor, geeft de volgende URL metagegevens voor het voorbeeldbedrijf die worden gebruikt door de Verkenner Graph: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. U kunt deze URL opgeven in de adresbalk van een webbrowser om de metagegevens weer te geven. Het CSDL metagegevensdocument geretourneerd, worden de entiteiten en complexe typen, hun eigenschappen, en de functies en acties die worden aangeboden door de versie van de gewenste grafiek-API. De parameter api-versie weglaten retourneert metagegevens voor de meest recente versie.

## <a name="common-queries"></a>Algemene query 's

[Azure AD Graph API algemene query's](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) bevat algemene query's die kunnen worden gebruikt met de Azure AD Graph, met inbegrip van query's die kunnen worden gebruikt voor toegang tot op het hoogste niveau resources in uw adreslijst en query's kunt bewerkingen uitvoeren in uw adreslijst.

Bijvoorbeeld `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` geeft als resultaat de bedrijfsgegevens voor directory contoso.com.

Of `https://graph.windows.net/contoso.com/users?api-version=1.6` bevat alle gebruikersobjecten in de directory contoso.com.

## <a name="using-the-graph-explorer"></a>Via de Verkenner Graph

U kunt de Explorer Graph voor de grafiek van Azure AD-API gebruiken om query's in de directory-gegevens terwijl u uw toepassing maken.

> [AZURE.IMPORTANT] De grafiek Explorer biedt geen ondersteuning voor schrijven of de gegevens uit een map verwijderen. U kunt alleen lezen bewerkingen voor uw Azure AD-map met de Verkenner Graph uitvoeren.

Hieronder ziet u de uitvoer die u zien wilt als u wilt navigeren naar de Verkenner Graph voorbeeldbedrijf gebruiken, en voer zijn `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` om alle gebruikers in de adreslijst demo weer te geven:

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**De Verkenner Graph laden**: als u wilt het hulpmiddel hebt geladen, gaat u naar [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Klik op **Voorbeeldbedrijf gebruiken** om uit te voeren van de Verkenner Graph ten opzichte van de gegevens van een steekproef-tenant. U hoeft niet te gebruiken van het bedrijf demo referenties. U kunt ook op **aanmelden** en meld u aan met de referenties van uw Azure AD-account aan de Verkenner grafiek voor uw tenant uitgevoerd. Als u Graph Explorer op uw eigen tenant uitvoeren, moet u of uw beheerder toe te staan bij het aanmelden. Als u een Office 365-abonnement hebt, hebt u automatisch een Azure AD-tenant. De referenties die u kunt aanmelden bij Office 365 zijn, zelfs Azure AD-accounts en kunt u deze referenties met Graph Verkenner.

**Query's uitvoeren**: als u wilt een query uitvoert, typ de query in het vak van de tekst verzoek en klik op **ophalen** of klik op de code **invoeren** . De resultaten worden weergegeven in het vak antwoord. Bijvoorbeeld `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` wordt een lijst met alle objecten groeperen in de adreslijst demo.

Houd rekening met de volgende voorzieningen en beperkingen van de Verkenner Graph:
- Hiermee stelt u de mogelijkheid van de functie automatisch aanvullen op resource. Moet u dit zien op **Voorbeeldbedrijf gebruiken** en klik vervolgens op in het tekstvak aanvraag (waarin de URL van het bedrijf wordt weergegeven). U kunt een resource instellen in de vervolgkeuzelijst selecteren.

- Ondersteunt het, "me" en "MijnOrganisatie" adressering van aliassen. U kunt bijvoorbeeld `https://graph.windows.net/me?api-version=1.6` om terug te keren het gebruikersobject van de gebruiker die zijn aangemeld of `https://graph.windows.net/myorganization/users?api-version=1.6` om terug te keren van alle gebruikers in de huidige map. Houd er rekening mee dat het, "me" alias gebruiken een fout voor het bedrijf demo oplevert omdat er geen aangemelde gebruiker die het verzoek.

- Een antwoord kopteksten sectie. Dit kan worden gebruikt voor het oplossen van problemen die optreden bij het uitvoeren van query's.

- Een JSON-viewer voor het antwoord met mogelijkheden voor het uitvouwen en samenvouwen.

- Er is geen ondersteuning voor het weergeven van een miniatuurfoto.

## <a name="using-fiddler-to-write-to-the-directory"></a>Gebruik van Fiddler schrijven naar de map

Voor de toepassing van deze handleiding Snelstartgids, kunt u de Fiddler Web foutopsporing om te kunnen uitvoeren 'schrijven' bewerkingen op u van oefening Azure AD-map. Zie voor meer informatie en voor het installeren van Fiddler [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Klik in het onderstaande voorbeeld kunt u Fiddler Web foutopsporing wilt gebruiken voor het maken van een nieuwe beveiligingsgroep 'MyTestGroup' in het telefoonboek van uw Azure AD.

**Een toegangstoken verkrijgen**: toegang tot Azure AD Graph, clients worden geverifieerd Azure AD eerst zijn vereist. Zie [Verificatie-scenario's voor Azure AD](active-directory-authentication-scenarios.md)voor meer informatie.

**Opstellen en een query uit te voeren**: de volgende stappen uit.

1. Open Fiddler Web foutopsporing en Ga naar het tabblad **Composer** .
2. Aangezien u een nieuwe beveiligingsgroep maken wilt, selecteert u het **bericht** als de HTTP-methode uit het vervolgkeuzemenu. Zie voor meer informatie over bewerkingen en machtigingen op een groepsobject [groep](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) binnen de [Azure AD Graph REST API verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. Typ in het veld naast **publicatie**in de volgende handelingen uit als de aanvraag-URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] U moet vervangen door mytenantdomain met de domeinnaam van uw eigen Azure AD-map.

4. In het veld direct onder bericht keuzemenu, typt u het volgende:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] SUBSTITUEREN uw &lt;uw toegangstoken&gt; met het toegangstoken voor uw Azure AD-map.

5. Typ het volgende in het veld **aanvragen hoofdtekst** :

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Zie voor meer informatie over het maken van groepen, [Groep maken](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Zie voor meer informatie over Azure AD entiteiten en typen die worden gebruikt door de grafiek en informatie over de bewerkingen die op deze kan worden uitgevoerd met Graph, [Azure AD Graph REST API verwijzing](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [Azure AD Graph-API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Meer informatie over [bereiken van Azure AD Graph API machtiging](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
