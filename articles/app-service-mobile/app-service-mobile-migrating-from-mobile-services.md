<properties
    pageTitle="Migreren van mobiele Services naar een mobiele App Service-App"
    description="Informatie over het migreren eenvoudig uw Mobile Services-toepassing aan een App-Service Mobile-App"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Uw bestaande Azure Mobile Service migreren naar Azure App-Service

Met de [algemene beschikbaarheid van Azure App-Service], kunnen Azure Mobile Services-sites worden eenvoudig gemigreerd in-place om te profiteren van alle functies van de Azure App-Service.  In dit document wordt uitgelegd wat u kunt verwachten bij het migreren van uw site van Azure Mobile Services naar Azure App-Service.

## <a name="what-does-migration-do"></a>Wat doet migratie aan uw site

Migratie van uw Azure Mobile-Service verandert de Mobile-Service in een app [Azure App Service] handhaven de code.  De melding Hubs, SQL gegevensverbinding verificatie-instellingen, geplande taken en de domeinnaam van het blijven ongewijzigd.  Mobiele clients gebruik uw Azure Mobile Service blijven werken normaal.  Migratie opnieuw uw service wordt gestart zodra deze is overgebracht naar Azure App-Service.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Waarom moet u uw site migreren

Microsoft is aanbevelen waarop u uw Azure Mobile-Service om te profiteren van de functies van Azure App-Service, inclusief migreren:

  *  Nieuwe host-functies, zoals [WebJobs] en [aangepaste domeinnamen].
  *  Connectiviteit met uw on-premises resources met [VNet] naast [Hybride verbindingen].
  *  Cmdlets voor controle en probleemoplossing met nieuwe Relic of [Toepassing inzichten].
  *  Ingebouwde DevOps tooling, inclusief [tijdelijke sleuven], terugdraaien, en in-productieve testen.
  *  [Automatisch schalen], taakverdeling en [prestatiecontroles uit].

Zie het onderwerp [Mobile Services versus App Service] voor meer informatie over de voordelen van Azure App-Service.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint een primaire werk op uw site, moet u de [Back-up van uw Mobile-Service] scripts en SQL-database.

## <a name="migrating-site"></a>Migreren van uw sites

Het migratieproces worden gemigreerd van alle sites binnen een enkel Azure.

Migreren van uw site:

  1.  Meld u aan bij de [Portal van Azure klassieke].
  2.  Selecteer een Mobile-Service in de regio die u wilt migreren.
  3.  Klik op de knop **migreren naar App-Service** .

    ![De knop migreren][0]

  4.  Lees de migreren App Service dialoogvenster te openen.
  5.  Voer de naam van de Mobile-Service in het daarvoor bestemde vak.  Bijvoorbeeld, als uw domeinnaam contoso.azure-mobile.net is, voert _contoso_ in het daarvoor bestemde vak.
  6.  Klik op de knop maatstreepjes.

De status van de migratie in de activiteitscontrole controleren. Uw site is vermeld als *migreren* in de klassieke Azure-Portal.

  ![Migratie Activiteitscontrole][1]

Elke migratie kan duren 3 op 15 minuten per mobiele service wordt gemigreerd.  Uw site blijft beschikbaar tijdens de migratie.
Uw site opnieuw is opgestart aan het einde van het migratieproces.  De site is niet beschikbaar bij het opstarten, waarin mogen een paar seconden duren.

## <a name="finalizing-migration"></a>De migratie te voltooien

Plannen om uw site van een mobiele client aan het einde van het migratieproces te testen.  Controleer of dat u kunt alle acties in de algemene client zonder wijzigingen in de mobiele client uitvoeren.  

### <a name="update-app-service-tier"></a>Selecteer een juiste App-Service prijzen van laag

U hebt meer flexibiliteit bij prijzen na het migreren naar Azure App-Service.

  1.  Meld u aan bij de [portal van Azure].
  2.  Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3.  Het blad instellingen standaard wordt geopend.
  4.  Klik in het menu instellingen op **App-Service plannen** .
  5.  Klik op de tegel **Prijzen laag** .
  6.  Klik op de tegel die geschikt zijn voor uw vereisten en klik op **selecteren**.  Mogelijk moet u Klik op **Alles weergeven** om te zien van de beschikbare prijzen lagen.

Als uitgangspunt, raden we aan de volgende lagen:

| Mobile Service prijzen laag | App-Service prijzen laag |
| :-------------------------- | :----------------------- |
| Vrij te geven                        | Gratis F1                  |
| Eenvoudige                       | B1 Basic                 |
| Standaard                    | S1 standaard              |

Er is veel flexibiliteit bij het kiezen van de prijzen van laag voor uw toepassing rechts.  Raadpleeg [Prijzen van App-Service] voor volledige informatie over de prijzen van uw nieuwe App-Service.

> [AZURE.TIP]De App Service Standard laag bevat toegang tot veel functies die u gebruiken wilt mogelijk, inclusief [tijdelijke sleuven], automatische back-ups, en automatisch schalen.  Bekijk de nieuwe mogelijkheden in terwijl u er bent!

### <a name="review-migration-scheduler-jobs"></a>De gemigreerde Scheduler taken bekijken

Geplande taken worden niet meer zichtbaar tot ongeveer 30 minuten na de migratie.  Geplande taken gaat u verder met het op de achtergrond uitvoeren.
Uw geplande taken weergeven nadat ze opnieuw zichtbaar zijn:

  1.  Meld u aan bij de [portal van Azure].
  2.  Selecteer **Bladeren >**, voer een **planning** in het vak _Filter_ en selecteer **Scheduler siteverzamelingen**.

Er zijn een beperkt aantal gratis scheduler taken beschikbaar na de migratie.  Controleer uw gebruik en de [Abonnementen van Azure Scheduler].

### <a name="configure-cors"></a>CORS configureren indien nodig

Cross-origin resources delen is een techniek toe te staan dat een website voor toegang tot een Web API op een ander domein.  Als u Azure Mobile Services met een koppeling naar een website gebruikt, moet u CORS configureren als onderdeel van de migratie.  Als u toegang krijgt Azure Mobile Services uitsluitend op mobiele apparaten tot, klikt u vervolgens hoeft CORS niet te worden geconfigureerd, behalve in sommige gevallen.

Uw gemigreerde CORS-instellingen zijn beschikbaar als de instelling van de App **MS_CrossDomainWhitelist** .  Uw site migreren naar de App-Service CORS faciliteit:

  1.  Meld u aan bij de [portal van Azure].
  2.  Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3.  Het blad instellingen standaard wordt geopend.
  4.  Klik op **CORS** in het menu API.
  5.  Voer een oorsprong toegestaan in het daarvoor bestemde, vak na elke opdracht op Enter te drukken.
  6.  Als uw lijst met toegestane oorsprong juist is, klikt u op de knop opslaan.

> [AZURE.TIP]Een van de voordelen van het gebruik van een Service van de App Azure is dat u uw website en mobiele service op dezelfde locatie uitvoeren kunt.  Zie het gedeelte van de [volgende stappen](#next-steps) voor meer informatie.

### <a name="download-publish-profile"></a>Een nieuwe publicatie profiel downloaden

De publicatie profiel van uw site, wordt gewijzigd wanneer u migreert naar Azure App-Service.  Als u publiceren van uw site vanuit Visual Studio wilt, moet u een nieuwe publicatie profiel.  Het nieuwe publicatie profiel downloaden:

  1.  Meld u aan bij de [portal van Azure].
  2.  Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3.  Klik op **Get publiceren profiel**.

Het bestand PublishSettings wordt gedownload naar uw computer.  Normaal gesproken wordt _sitenaam_genoemd. PublishSettings.  De publicatie-instellingen importeren in een bestaand project:

  1.  Open Visual Studio en uw Azure Mobile Service-project.
  2.  Met de rechtermuisknop op uw project in de **Oplossingverkenner** en selecteer **publiceren...**
  3.  Klik op **importeren**
  4.  Klik op **Bladeren** en selecteer uw gedownloade instellingenbestand publiceren.  Klik op **OK**
  5.  Klik op **Verbinding valideren** om ervoor te zorgen de werklast van de instellingen publiceren.
  6.  Klik op **publiceren** als u wilt publiceren van uw site.


## <a name="working-with-your-site"></a>Werken met uw na de sitemigratie

Aan de slag met de nieuwe App-Service in de [portal van Azure] na de migratie.  Hier volgen enkele notities op specifieke bewerkingen die u hebt gebruikt om uit te voeren in de [Klassieke Azure-Portal], samen met hun equivalent App Service.

### <a name="publishing-your-site"></a>Downloaden en uw gemigreerde site publiceren

Uw site is beschikbaar via een cijfer of ftp en opnieuw kan worden gepubliceerd met verschillende verschillende regelingen, inclusief WebDeploy, TFS, volgt, GitHub en FTP.  De implementatie-referenties worden gemigreerd met de rest van uw site.  Als u uw implementatie-referenties niet ingesteld of u ze niet meer weet, kunt u deze opnieuw instellen:

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3. Het blad instellingen standaard wordt geopend.
  4. Klik op **referenties implementatie** in de publicatie menu.
  5. Voer de nieuwe implementatie-referenties in de vakken en klik op de knop opslaan.

U kunt deze referenties klonen van de site met cijfer of geautomatiseerde implementaties uit GitHub, TFS of volgt in te stellen.  Zie de [documentatie over de implementatie van Azure App-Service]voor meer informatie.

### <a name="appsettings"></a>Instellingen voor toepassingen

De meeste instellingen voor een gemigreerde mobiele service zijn beschikbaar via de App-instellingen.  U kunt een lijst met instellingen voor de app krijgen van de [Azure-portal].
Om te bekijken of uw app-instellingen wijzigen:

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3. Het blad instellingen standaard wordt geopend.
  4. **Klik in het menu Algemeen.**
  5. Schuif naar de sectie instellingen van de App en de instelling van uw app zoeken.
  6. Klik op de waarde van de app-instelling om de waarde te bewerken.  Klik op **Opslaan** om op te slaan de waarde.

U kunt meerdere app-instellingen op hetzelfde moment bijwerken.

> [AZURE.TIP]Er zijn twee toepassingsinstellingen met dezelfde waarde.  Zie bijvoorbeeld de _ApplicationKey_ en _MS\_ApplicationKey_.  Beide toepassingsinstellingen op hetzelfde moment bijwerken.

### <a name="authentication"></a>Verificatie

Alle verificatie-instellingen zijn beschikbaar als de instellingen van de App op uw gemigreerde site.  Als u uw verificatie-instellingen bijwerken, moet u de juiste app-instellingen wijzigen.  De volgende tabel ziet u de juiste app-instellingen voor uw verificatieprovider:

| Provider          | Cliënt-ID                 | Geheim client                | Andere instellingen             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoft-Account | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Opmerking: **MS\_AadTenants** wordt opgeslagen als een door komma's gescheiden lijst met domeinen van de tenant (de "Tenants toegestaan" velden in de portal Mobile Services).

> [AZURE.WARNING] **Gebruik de verificatiemethoden niet in het menu instellingen**
>
> Azure App-Service biedt een afzonderlijke 'zonder code' verificatie en machtiging onder de _verificatie / autorisatie_ menu instellingen en de (afgeschaft) _Mobile verificatie_ -optie onder het menu instellingen.  Deze opties zijn niet compatibel met een gemigreerde Azure Mobile-Service.  U kunt de [upgrade van uw site](app-service-mobile-net-upgrading-from-mobile-services.md) om te profiteren van de Azure-Service voor App-verificatie.

### <a name="easytables"></a>Gegevens

Het tabblad _gegevens_ in de Mobile-Services is vervangen door _Eenvoudig tabellen_ binnen de Azure-portal.  Toegang krijgen tot eenvoudig tabellen:

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3. Het blad instellingen standaard wordt geopend.
  4. Klik op **eenvoudige tabellen** in het menu mobiele.

U kunt een tabel toevoegen door te klikken op de knop **toevoegen** of toegang tot uw bestaande tabellen door te klikken op de naam van een tabel.  Er zijn verschillende bewerkingen die u vanaf deze blade doen kunt, waaronder:

* Tabelmachtigingen wijzigen
* De operationele scripts bewerken
* Het tabelschema beheren
* De tabel te verwijderen
* De inhoud van de tabel wissen
* Bepaalde rijen van de tabel verwijderen

### <a name="easyapis"></a>API

Het tabblad _API_ in de Mobile-Services is vervangen door _Eenvoudig-API's_ binnen de Azure-portal.  Toegang krijgen tot eenvoudig API's:

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3. Het blad instellingen standaard wordt geopend.
  4. Klik op **Eenvoudige API's** in het menu mobiele.

Uw gemigreerde API's staan in het blad.  U kunt ook een API toevoegen vanaf deze blade.  Als u wilt beheren op een specifieke API, klikt u op de API.
Van het nieuwe blad, kunt u de machtigingen aanpassen en de scripts bewerken voor de API.

### <a name="on-demand-jobs"></a>Geplande taken

Alle scheduler taken zijn beschikbaar via de sectie Scheduler taak siteverzamelingen.  Toegang krijgen tot uw geplande taken:

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **Bladeren >**, voer een **planning** in het vak _Filter_ en selecteer **Scheduler siteverzamelingen**.
  3. Selecteer de taak-verzameling voor uw site.  Deze heet _sitenaam_-taken.
  4. Klik op **Instellingen**.
  5. Klik op **Scheduler taken** onder beheren.

Geplande taken worden weergegeven met de frequentie die u hebt opgegeven voor de migratie.  Taken op aanvraag zijn uitgeschakeld.  Een taak op aanvraag uitvoeren:

  1. Selecteer de taak die u wilt uitvoeren.
  2. Klik zo nodig op **inschakelen** om in te schakelen van de taak.
  3. Klik op **Instellingen**en daarna de **planning**.
  4. Selecteer een terugkeerpatroon van **één keer**en klik vervolgens op **Opslaan**

Uw taken op aanvraag zich bevinden in `App_Data/config/scripts/scheduler post-migration`.  Het is raadzaam dat u alle op aanvraag taken naar [WebJobs] of [functies converteren].  Schrijf nieuwe scheduler taken als [WebJobs] of [functies].

### <a name="notification-hubs"></a>Melding Hubs

Mobile-Services gebruikt melding Hubs voor push-meldingen.  De volgende App-instellingen worden gebruikt om de koppeling van de melding Hub naar uw Mobile-Service na de migratie:

| Toepassingsinstelling                    | Beschrijving                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | De melding Hub Namespace           |
| **MS\_NotificationHubName**             | De naam van de Hub melding                |
| **MS\_NotificationHubConnectionString** | De verbindingsreeks van de melding Hub   |
| **MS\_NamespaceName**                   | Een alias voor MS_PushEntityNamespace      |

Uw Hub melding wordt beheerd via de [portal van Azure].  Houd rekening met de naam van de melding Hub (u kunt dit vinden met de App-instellingen):

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **Bladeren**>, selecteer **Melding Hubs**
  3. Klik op de naam van de Hub van de melding dat is gekoppeld aan de mobile-service.

> [AZURE.NOTE]Als uw HUb melding is een type "Gemengde", is het niet zichtbaar.  "Gemengde" Typ melding hubs zowel melding Hubs en oudere Service Bus voorzieningen gebruiken.  [Uw gemengde naamruimten converteren] voordat u verdergaat.  Als de conversie voltooid is, wordt uw hub melding weergegeven in de [portal van Azure].

Bekijk de [Melding Hubs] documentatie voor meer informatie.

> [AZURE.TIP]Melding Hubs beheerfuncties in de [portal van Azure] zijn nog steeds in de Preview-versie.  De [Klassieke Azure-Portal] blijft beschikbaar voor alle uw melding Hubs beheren.

### <a name="legacy-push"></a>Oudere Push-instellingen

Als u van uw mobiele service vóór de inleiding op melding Hubs Push hebt geconfigureerd, gebruikt u _oudere push_.  Als u Push gebruikt en u een melding Hub weergegeven in uw configuratie niet ziet, is het waarschijnlijk dat u _oudere push_gebruikt.  Deze functie wordt gemigreerd met alle andere functies.  Echter, is het raadzaam dat u een upgrade naar melding Hubs uitvoeren zodra de migratie voltooid is.

In de tussentijd zijn de oude push-instellingen (met het aantal aanzienlijke uitzondering van het APNS-certificaat) beschikbaar in App-instellingen.  Werk de APNS-certificaat door te worden vervangen door het juiste bestand in het bestandssysteem.

### <a name="app-settings"></a>Andere instellingen van de App

De volgende aanvullende app-instellingen zijn gemigreerd van uw mobiele-Service en beschikbaar onder *Instellingen* > *App-instellingen*:

| Toepassingsinstelling              | Beschrijving                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | De naam van uw app                    |
| **MS\_MobileServiceDomainSuffix** | Het voorvoegsel domein. Internet Explorer Azure-mobile.net |
| **MS\_ApplicationKey**            | Uw toepassingstoets                    |
| **MS\_MasterKey**                 | Uw app-model-sleutel                     |

De toepassing en outmodel sleutel zijn gelijk aan de toepassingstoetsen van uw oorspronkelijke Mobile-Service.  Met name is de toepassingstoets verzonden door mobiele clients voor het valideren van het gebruik van de mobiele API.

### <a name="cliequivalents"></a>Equivalenten voor opdrachtregel

U kunt de opdracht _azure mobiele_ meer gebruiken voor het beheren van uw Azure Mobile Services-site.  In plaats daarvan zijn veel functies vervangen door de opdracht _azure-site_ .  Gebruik de volgende tabel om equivalenten voor veelgebruikte opdrachten:

| _Azure Mobile_ Opdracht                     | Equivalente _Azure Site_ -opdracht            |
| :----------------------------------------- | :----------------------------------------- |
| mobiele locaties                           | lijst met locaties van site                         |
| mobiele lijst                                | lijst met sites                                  |
| _naam_ van de mobiele weergeven                         | _naam_ van de site weergeven                           |
| _naam_ van de mobiele opnieuw starten                      | _naam_ van de site opnieuw starten                        |
| mobiele in dat geval _naam_                     | site-implementatie in dat geval _commitId_ _naam_ |
| mobiele toets instellen _ _type_ _waarde_ _       | site appsetting verwijderen _toets_ _naam_ <br/> _sleutel_toevoegen met site appsetting=_waarde_ van _naam_ |
| mobiele config _naam_                  | site appsetting _naam_                |
| mobiele config _naam_ _sleutel_ ophalen             | _toets_ _naam_ van site appsetting weergeven          |
| mobiele config instellen _naam_ _sleutel_             | site appsetting verwijderen _toets_ _naam_ <br/> _sleutel_toevoegen met site appsetting=_waarde_ van _naam_ |
| mobiele domein _naam_                  | lijst met _de naam_ van site domein                    |
| mobiele domein _naam_ _domein_ toevoegen          | site-domein toevoegen _de naam_ van het _domein_            |
| _naam_ van de mobiele domein verwijderen                | _domein_ _de naam_ van site domein verwijderen         |
| _naam_ van de mobiele schaal weergeven                   | _naam_ van de site weergeven                           |
| _naam_ van de mobiele schaal wijzigen                 | site schaal modus _modus_ _naam_ <br /> schaal exemplaren _exemplaren_ van de site _naam_ |
| mobiele appsetting _naam_              | site appsetting _naam_                |
| mobiele appsetting _ _toets_ _waarde_ _ toevoegen | _sleutel_toevoegen met site appsetting=_waarde_ van _naam_   |
| _naam_ _sleutel_ voor mobiele appsetting delete      | site appsetting verwijderen _toets_ _naam_        |
| mobiele appsetting weergeven _naam_ _sleutel_        | site appsetting verwijderen _toets_ _naam_        |

Verificatie of de instellingen voor push-meldingen bijwerken door de juiste toepassingsinstelling bij te werken.
Bewerken van bestanden en publiceren van uw site via een FTP- of cijfer.

### <a name="diagnostics"></a>Diagnostische hulpprogramma's en gegevens vastleggen

Diagnostische gegevens vastleggen is normaal uitgeschakeld in een Azure-App-Service.  Diagnostische gegevens vastleggen inschakelen:

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3. Het blad instellingen standaard wordt geopend.
  4. Selecteer de **Diagnostische logboeken** in het menu functies.
  5. Klik **op** de volgende logboekbestanden: **Toepassing logboekregistratie (bestandssysteem)**, **gedetailleerde foutberichten**en **mislukt verzoek traceren**
  6. Klik op **File System** voor logboekregistratie van webserver
  7. Klik op **Opslaan**

De logboeken weergeven:

  1. Meld u aan bij de [portal van Azure].
  2. Selecteer **alle resources** of **App Services** Klik op de naam van uw gemigreerde Mobile-Service.
  3. Klik op de knop **Extra**
  4. Selecteer **Log Stream** in het menu OBSERVE.

Logboeken worden weergegeven in het venster als ze worden gegenereerd.  U kunt ook de logboeken voor latere analyse met de referenties van uw implementatie downloaden. Zie de documentatie [logboekregistratie] voor meer informatie.

## <a name="known-issues"></a>Bekende problemen

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Een mobiele App klonen gemigreerd verwijderen Hiermee wordt een site-storing

Als u uw gemigreerde mobiele service via Azure PowerShell klonen en verwijder vervolgens het klonen, wordt de DNS-vermelding voor uw productieservice verwijderd.  Uw site is niet meer via Internet toegankelijk zijn.  

Oplossing: Als u uw site klonen wilt, doen via de portal.

### <a name="changing-webconfig-does-not-work"></a>Wijzigen van Web.config werkt niet

Als u een ASP.NET-website hebt, wordt gewijzigd in de `Web.config` bestand kan niet worden toegepast.  De App-Service van Azure genereert een geschikte `Web.config` bestand tijdens het opstarten ter ondersteuning van de runtime Mobile-Services.  U kunt bepaalde instellingen (zoals aangepaste headers) overschrijven met behulp van een XML-transformatie-bestand.  Een bestand maken in genoemd `applicationHost.xdt` -dit bestand moet terechtkomen in de `D:\home\site` map op de Azure-Service.  Upload de `applicationHost.xdt` bestand via een aangepaste implementatiescript of rechtstreeks met Kudu.  Hier volgt een voorbeelddocument:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Zie de documentatie [XDT transformeren voorbeelden] op GitHub voor meer informatie.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Gemigreerde Mobile Services kan niet worden toegevoegd aan verkeer Manager

Wanneer u een profiel verkeer Manager maakt, kunt u een gemigreerde mobiele service aan het profiel niet rechtstreeks kiezen.  Gebruik een 'externe eindpunt'.  Het externe eindpunt kan alleen worden toegevoegd via PowerShell.  Zie de [verkeer Manager zelfstudie](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Nu dat uw toepassing is gemigreerd naar App Service, zijn er nog meer functies die u kunt gebruiken:

  * Implementatie [tijdelijke sleuven] kunt u wijzigingen aan uw site van fase en uitvoeren van A / B testen.
  * [WebJobs] bieden vervanging voor op aanvraag geplande taken.
  * U kunt [continu implementeren] uw site door te GitHub, TFS of volgt koppelen aan uw site.
  * U kunt [Toepassing inzichten] gebruiken om te controleren van uw site.
  * Een website en een Mobile-API van dezelfde code fungeren.

### <a name="upgrading-your-site"></a>Een upgrade van uw mobiele Services-site naar de SDK van Azure Mobile-Apps

  * Voor Node.js gebaseerde serverprojecten voorziet de nieuwe [Mobiele Apps Node.js SDK] in diverse nieuwe functies. Bijvoorbeeld kunt u nu doen plaatselijke ontwikkeling en foutopsporing, een versie Node.js hierboven 0,10 gebruiken en aanpassen met een middleware Express.js.

  * Voor. NET gebaseerde serverprojecten, de nieuwe [mobiele Apps SDK NuGet-pakketten](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) hebben meer flexibiliteit op NuGet afhankelijkheden.  Deze pakketten ondersteuning voor de nieuwe App Service-verificatie en opstellen met een ASP.NET-project. Zie meer informatie over het upgraden, [Upgrade van uw bestaande .NET Mobile-Service met App-Service](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App-Service prijzen]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Toepassing inzichten]: ../application-insights/app-insights-overview.md
[Automatisch schalen]: ../app-service-web/web-sites-scale.md
[Azure App-Service]: ../app-service/app-service-value-prop-what-is.md
[Azure App implementatie servicedocumentatie]: ../app-service-web/web-sites-deploy.md
[Azure klassieke Portal]: https://manage.windowsazure.com
[Azure-portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler-abonnementen]: ../scheduler/scheduler-plans-billing.md
[continu implementeren]: ../app-service-web/app-service-continuous-deployment.md
[Uw gemengde naamruimten converteren]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[aangepaste domeinnamen]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[algemene beschikbaarheid van Azure App-Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybride verbindingen]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Logboekregistratie]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobiele Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Mobiele Services versus App Service]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Melding Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[prestaties controleren]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Back-up van uw Mobile-Service]: ../mobile-services/mobile-services-disaster-recovery.md
[tijdelijk opslaan sleuven]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[Voorbeelden van XDT transformeren]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Functies]: ../azure-functions/functions-overview.md
