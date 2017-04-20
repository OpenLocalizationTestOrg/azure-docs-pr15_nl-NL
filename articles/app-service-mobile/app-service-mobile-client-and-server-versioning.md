<properties
  pageTitle="Versiebeheer voor clients en servers SDK in Mobile-Apps en Services van Mobile | Azure App-Service"
  description="Lijst met client SDK's en compatibiliteit met server SDK versies voor Mobile-Services en Azure Mobile-Apps"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Versiebeheer voor clients en servers in Mobile-Apps en Services van Mobile

De nieuwste versie van Azure Mobile Services is de functie **Mobile-Apps** van Azure App-Service.

De Mobile-Apps clients en servers SDK's oorspronkelijk zijn gebaseerd op de gebruikersprofielen in Mobile-Services, maar ze zijn *niet* compatibel met elkaar.
Dat wil zeggen, moet u een *Mobile-Apps* -client SDK gebruiken met een *Mobile-Apps* -server SDK en op dezelfde manier voor *Mobile-Services*. Deze overeenkomst wordt afgedwongen via een speciale koptekst-waarde die wordt gebruikt door de client en server SDK's, `ZUMO-API-VERSION`.

Opmerking: wanneer dit document naar een backend *Mobile-Services verwijst* , deze niet per se hoeft te worden gehost op Mobile-Services. Is het nu mogelijk is om te migreren van een mobiele service uit te voeren op App Service zonder codewijzigingen, maar de service zou steeds gebruikmaken van *Mobiele Services* SDK versies.

Meer informatie over het migreren naar de App Service zonder codewijzigingen, Zie het artikel [migreren een Mobile-Service aan Azure App-Service].

## <a name="header-specification"></a>Specificatie van de koptekst

De toets `ZUMO-API-VERSION` kunnen in de HTTP-kop- of de queryreeks worden opgegeven. De waarde is een tekenreeks in het formulier **x.y.z**.

Bijvoorbeeld:

Https://service.azurewebsites.net/tables/TodoItem ophalen

KOPTEKSTEN: ZUMO-API-VERSIE: 2.0.0

BERICHT https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Het afmelden voor versie controleren

U kunt afmelden bij versie controleren door in te stellen van de waarde **waar** voor de app **MS_SkipVersionCheck**instellen. Geef dit in uw web.config of in de sectie instellingen van de Azure-portal.

> [AZURE.NOTE] Er zijn een aantal wijzigingen in gedrag tussen mobiele Services en Mobile-Apps, met name in de gebieden van offline synchroniseren, verificatie en push-meldingen. U moet alleen afmelden bij versie controleren na het volledige testen om ervoor te zorgen dat deze wijzigingen doorgevoerd het van uw app-functionaliteit niet breken.

## <a name="summary-of-compatibility-for-all-versions"></a>Overzicht van compatibiliteit voor alle versies

In onderstaande tabel ziet u de compatibiliteit tussen alle typen voor clients en servers. Een back-end is ingedeeld als mobiele **Services** of mobiele **Apps** die zijn gebaseerd op de server SDK die worden gebruikt.

|                           | **Mobiele Services** Node.js of .NET | **Mobiele Apps** Node.js of .NET |
| ----------                | -----------------------             |   ----------------              |
| [Services voor mobiele clients] | OK                                  | Fout\*                         |
| [Mobile-Apps-clients]     | Fout\*                             | OK                              |

\*Dit kan worden gecontroleerd door het opgeven van **MS_SkipVersionCheck**.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Services voor mobiele clients en servers

De client SDK's in de onderstaande tabel compatibel zijn met **Mobiele Services.**

Opmerking: de Mobile-Services-client SDK's *niet* verzenden de kopwaarde van een voor `ZUMO-API-VERSION`. Als de service deze kop- of query tekenreekswaarde ontvangt, een fout geretourneerd, tenzij u expliciet uitfaden zoals hierboven is beschreven hebt gekozen.

### <a name="MobileServicesClients"></a>Mobiele *Services* client SDK 's

| Client-platform                   | Versie                                                                   | Versie koptekst waarde |
| -------------------               | ------------------------                                                  | -------------------  |
| Beheerde client (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/b                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | n/b                  |
| Android-apparaat                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n/b                  |
| HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/b     |

### <a name="mobile-services-server-sdks"></a>Mobiele *Services* -server SDK 's

| Server-platform  | Versie                                                                                                        | Geaccepteerde versie koptekst |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* versie 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Geen koptekst versie** |
| Node.js          | (binnenkort)                        | **Geen koptekst versie** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Gedrag van mobiele Services backends

| ZUMO-API-VERSIE | Waarde van MS_SkipVersionCheck | Antwoord |
| ---------------- | ---------------------------- | -------- |
| Niet opgegeven    | Een                          | 200 - OK |
| Elke waarde        | Waar                         | 200 - OK |
| Elke waarde        | ONWAAR/niet opgegeven          | 400 - Ongeldige aanvraag |

## <a name="2.0.0"></a>Azure Mobile-Apps clients en servers

### <a name="MobileAppsClients"></a>Mobiele *Apps* client SDK 's

Versie controleren is geïntroduceerd beginnen met de volgende versies van de client SDK voor **Azure Mobile-Apps**:

| Client-platform                   | Versie                   | Versie koptekst waarde |
| -------------------               | ------------------------  | -----------------    |
| Beheerde client (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android-apparaat                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobiele *Apps* server SDK 's

Versie controleren is opgenomen in de volgende server SDK versies:

| Server-platform  | SDK                                                                                                        | Geaccepteerde versie koptekst |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure-mobile-apps)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Gedrag van de Mobile-Apps backends

| ZUMO-API-VERSIE | Waarde van MS_SkipVersionCheck | Antwoord |
| ---------------- | ---------------------------- | -------- |
| x.y.z of null-waarden    | Waar                         | 200 - OK |
| Null-waarden             | ONWAAR/niet opgegeven          | 400 - Ongeldige aanvraag |
| 1.x.y            | ONWAAR/niet opgegeven          | 400 - Ongeldige aanvraag |
| 2.0.0-2.x.y      | ONWAAR/niet opgegeven          | 200 - OK |
| 3.0.0-3.x.y      | ONWAAR/niet opgegeven          | 400 - Ongeldige aanvraag |


## <a name="next-steps"></a>Volgende stappen

- [Een mobiele Service migreren naar Azure App-Service]


[Services voor mobiele clients]: #MobileServicesClients
[Mobile-Apps-clients]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Een mobiele Service migreren naar Azure App-Service]: app-service-mobile-migrating-from-mobile-services.md

