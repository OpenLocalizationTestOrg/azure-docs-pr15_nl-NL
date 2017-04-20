<properties
pageTitle="Azure Active Directory-toepassing en Service Principal objecten | Microsoft Azure"
description="Een discussie van de relatie tussen toepassing en service principal objecten in Azure Active Directory"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Toepassing en service principal objecten in Azure Active Directory
Als u over een Azure AD (Active Directory) "toepassing" leest, is het niet altijd wissen precies wat is waarnaar wordt verwezen door de auteur. Het doel van dit artikel is duidelijker, door te definiëren de conceptuele en concrete aspecten van de integratie van het Azure AD-toepassing, met een voorbeeld van de registratie en toestemming voor een [toepassing voor meerdere tenant](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Overzicht
Een Azure AD-toepassing is groter dan alleen een vel met software. Het is een conceptuele term, verwijzen niet alleen naar toepassingen, maar ook de registratie (ook bekend: identiteit configuratie) met Azure AD, zodat deze deel te nemen aan verificatie en machtiging "gesprekken" gedurende runtime. Een toepassing kunt per definitie werken in een [client](active-directory-dev-glossary.md#client-application) rol (die een resource), een [bronserver](active-directory-dev-glossary.md#resource-server) rol weergeeft API's (met clients) of zelfs beide. Het gesprek-protocol is gedefinieerd door een [OAuth 2.0 autorisatie verlenen stroom](active-directory-dev-glossary.md#authorization-grant), met een doel van het toestaan van de client/resource access/van een resource om gegevens te beveiligen respectievelijk. Nu een niveau lager in dit artikel leest en kijk hoe het model Azure AD-toepassing een toepassing intern vertegenwoordigt. 

## <a name="application-registration"></a>Registratie van toepassing
Wanneer u een toepassing registreren in de [portal van Azure klassieke][AZURE-Classic-Portal], twee objecten zijn gemaakt in uw Azure AD-tenant: een application-object en een service principal object.

#### <a name="application-object"></a>Application-object
Een Azure AD-toepassing is *gedefinieerd* door het adres en alleen application-object, dat zich in de Azure AD-tenant waar de toepassing is geregistreerd bevindt, genoemd 'thuis' tenant van de toepassing. Het application-object bevat gegevens die betrekking hebben van een identiteit voor een toepassing en de sjabloon waaruit de bijbehorende service principal of meer objecten zijn *afgeleid* voor gebruik tijdens runtime is. 

U kunt zien van de toepassing als de *algemene* weergave van de toepassing (voor gebruik in alle tenants) en de hoofdsom service als de *lokale* weergave (voor gebruik in een specifieke tenant). De Azure AD Graph- [Toepassingsentiteit] [ AAD-Graph-App-Entity] definieert het schema voor een application-object. Een toepassingsobject daarom heeft een relatie 1:1 met de softwaretoepassing en een 1:*n* relatie met de bijbehorende *n* service principal of meer objecten.

#### <a name="service-principal-object"></a>Service principal object
De service principal object bepaalt welke beleid en machtigingen voor een toepassing, de basis voor een beveiligings-principal om aan te geven van de toepassing bij het openen van resources tijdens runtime leveren. De Azure AD Graph [ServicePrincipal entiteit] [ AAD-Graph-Sp-Entity] het schema voor een service principal object definieert. 

Een service principal object is vereist in elke tenant waarvoor een exemplaar van het gebruik van de toepassing moet worden weergegeven, beveiligde toegang bieden tot resources eigendom van gebruikersaccounts van die tenant. Een toepassing voor de één-tenant heeft slechts één service hoofdsom (in de thuis tenant). Een meerdere tenant [webtoepassing](active-directory-dev-glossary.md#web-client) hebt ook een service principal in elke tenant waar een beheerder of de gebruiker (s) van die tenant toestemming hebt gegeven, zodat het gebruik van de bronnen. Na toestemming, wordt de service principal object worden geraadpleegd voor toekomstige autorisatieaanvragen. 

> [AZURE.NOTE] Wijzigingen die u aanbrengt in uw toepassingsobject, worden ook doorgevoerd in de service principal object in van de toepassing start tenant alleen (de tenant waar deze is geregistreerd). Wijzigingen voor meerdere tenant-toepassingen, op het object application worden niet doorgevoerd in alle consumenten-tenants service principal objecten, totdat de tenant consumenten toegang verwijderd en opnieuw toegang verleent.

## <a name="example"></a>Voorbeeld
In het volgende diagram ziet u de relatie tussen van een toepassing application-object en de bijbehorende service-principal objecten in de context van een steekproef meerdere tenant-toepassing **HR app**genoemd. Er zijn drie Azure AD-tenants in dit scenario: 

- **Adatum** - de tenant die worden gebruikt door het bedrijf dat de **HR app** ontwikkeld
- **Contoso** - de tenant die worden gebruikt door de Contoso-organisatie, dat wil een consumenten van de **HR-app zeggen**
- **Fabrikam** - de tenant die worden gebruikt door het bedrijf Fabrikam die ook de **HR-app verbruikt**

![Relatie tussen een application-object en een service principal object](./media/active-directory-application-objects/application-objects-relationship.png)

Stap 1 is in het vorige diagram, het proces van het maken van de toepassing en de service principal objecten in van de toepassing start tenant.

In stap 2 wanneer beheerders van Contoso en Fabrikam toestemming voltooit, een service principal object wordt gemaakt in van hun bedrijf Azure AD-tenant en de machtigingen die de beheerder toegekend toegewezen. Bedenk ook dat de app HR worden geconfigureerd/ontworpen kan voor toestemming door gebruikers voor afzonderlijke gebruik.

Klik in stap 3 hebt de consumenten-tenants van de HR toepassing (Contoso en Fabrikam) elke hun eigen service principal object. Elke geeft het gebruik van een exemplaar van de toepassing gedurende runtime, moet voldoen aan de machtigingen ingestemd door de desbetreffende beheerder.

## <a name="next-steps"></a>Volgende stappen
Van een toepassing application-object kan worden geraadpleegd via de API Azure AD Graph, zoals aangegeven met de OData- [Toepassingsentiteit][AAD-Graph-App-Entity]

Hoofdsom van een toepassing service-object kan worden geraadpleegd via de API Azure AD Graph, zoals aangegeven met de OData- [ServicePrincipal entiteit][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com