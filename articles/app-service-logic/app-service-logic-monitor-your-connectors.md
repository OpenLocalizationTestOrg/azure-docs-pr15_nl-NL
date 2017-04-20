<properties
    pageTitle="Beheren en controleren van uw verbindingslijnen en API-Apps in de App Service | Microsoft Azure"
    description="Prestaties van de weergave van uw verbindingslijnen en API-Apps in logica Apps; microservices architectuur"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Beheren en controleren van de ingebouwde API-Apps en verbindingslijnen

>[AZURE.NOTE] Deze versie van het artikel is van toepassing op logica apps 2014-12-01-schema voorbeeldversie.

U een ingebouwde API-App hebt gemaakt. Wat nu?

In Azure wordt aangegeven is elke API-App een aparte website gehost op Azure. Hierdoor kunt u gemakkelijk zien hoeveel aanvragen zijn doorgevoerd en zien hoeveel gegevens wordt gebruikt door de verbindingslijn. U kunt ook back-up maken van uw App API, waarschuwingen maken, folie beveiliging inschakelen en gebruikers en rollen toevoegen.

In dit onderwerp worden enkele van de verschillende opties voor het beheren van uw API-App.

Als u wilt zien deze ingebouwde functies, open uw API-App in de [portal van Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). Als de API-App op uw startboard, selecteert u deze om de eigenschappen te openen. U kunt ook selecteren **Bladeren**, selecteer **API-Apps**en selecteer uw API-App:

![][browse]

## <a name="see-the-properties-you-entered"></a>Zie de eigenschappen die u hebt ingevoerd

Wanneer u de API-App opent, zijn er verschillende functies en taken beschikbaar:

![][settings]

U kunt:

- **Instellingen** wordt bepaalde informatie weergegeven over de API-App, inclusief de details van uw abonnement, en bevat de gebruikers die toegang tot de API-app hebben. U kunt ook vergroot of verkleint van het aantal exemplaren van de API-App met de functie schaal; onder andere functies.
- Gebruik de knoppen **starten** en **stoppen** om te bepalen de API-App.
- Wanneer productupdates zijn aangebracht in de onderliggende bestanden die worden gebruikt door uw API-App, kunt u klikken op **bijwerken** om de meest recente versies. Bijvoorbeeld klikken automatisch **bijwerken** als er een oplossing of een beveiligingsupdate door Microsoft, uw App API met deze correctie bijgewerkt.
- Selecteer de **Wijziging plannen** upgrade of downgrade op basis van het gegevensgebruik van de API-App. U kunt deze functie ook gebruiken om het gegevensgebruik van uw weer te geven.
- Bij het maken van een verbindingslijn tabellen bevat, zoals de SQL-connector, kunt u desgewenst verbinding maken met de naam van een tabel invoeren. Een schema op basis van de tabel worden automatisch gemaakt en beschikbaar wanneer u klikt op **Schema's downloaden**. U kunt deze gedownloade schema vervolgens gebruiken om een transformatie of een kaart te maken.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>De verbindingslijn of de waarden van de systeemconfiguratie API ingevoerde wijzigen

Nadat u geconfigureerd of uw ingebouwd-connector hebt gemaakt, kunt u de waarden die u hebt ingevoerd. Bijvoorbeeld als u de SQL-Connector geconfigureerd en u wilt wijzigen van de SQL Server-naam of tabelnaam, kunt u dit doen in het blad API-App voor de verbindingslijn.

Stappen zijn:

1. Open uw verbindingslijn of API-App. Wanneer u dat doet, wordt het blad API-App wordt geopend.
2. Klik op de hyperlink onder de eigenschap Host in **Essentials**. De hyperlink heet iets zoals *slackconnector* of *microsoftsqlconnector123*:

    ![][apiapphost]

3. Selecteer **Instellingen**in het blad API App Host. Selecteer in het blad instellingen **Toepassingsinstellingen**. De configuratiewaarden worden vermeld onder **App-instellingen**:

    ![][hostsettings]

4. Klik op de instelling die u wilt wijzigen, voert u de nieuwe waarde en **Sla** uw wijzigingen.


## <a name="install-the-hybrid-connection-manager---optional"></a>Installeren van de hybride Connection Manager - optioneel

![][hcsetup]

De hybride Connection Manager kunt u de mogelijkheid verbinding maken met een on-premises-systeem, zoals SQL Server of SAP. Deze hybride connectivity gebruik van Azure Service Bus om verbinding te en om te bepalen de beveiliging tussen uw Azure resources en uw on-premises implementatie-resources.

Zie [werken met de hybride Connection Manager in Azure App-Service](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hybride Connection Manager is vereist alleen als u verbinding met een on-premises implementatie resource achter de firewall maakt. Als u geen verbinding met een on-premises-systeem maakt, kan de hybride Connection Manager niet worden vermeld in uw blade verbindingslijn.

## <a name="monitor-the-performance"></a>De prestaties controleren
Prestatiegegevens records zijn ingebouwde functies en opgenomen met elke API-App die u maakt. Deze gegevens zijn specifiek voor uw API-App gehost in Azure wordt aangegeven. Voorbeeld van de doelstellingen:

![][monitoring]

U kunt:

- Selecteer **aanvragen en fouten** toe te voegen verschillende prestatiegegevens, met inbegrip van de meest-bekende HTTP-foutcodes, zoals 200, 400 of 500 HTTP-statuscodes. U kunt ook Zie antwoord tijden, zien hoeveel aanvragen worden gedaan naar de API-App, en hoeveel gegevens wordt geleverd en hoeveel gegevens uitvalt. Op basis van de prestatiegegevens, kunt u e-mail waarschuwingen maken, als een meting groter is dan een drempelwaarde van uw keuze.
- **Gebruik**, kunt u zien hoeveel **processor** wordt gebruikt door de API-App, bekijk het huidige **Gebruiksquota** in MB en de maximale gegevensgebruik op basis van uw kosten laag. **Geschatte besteedt** , kunt u bepalen van de potentiële kosten van het uitvoeren van uw API-App.
- Selecteer **processen** Process Explorer openen. Hiermee worden uw web-sessies en hun eigenschappen, inclusief thread aantal en het geheugen gebruik.

Met deze hulpmiddelen, kunt u als de App-Service plannen moet worden vergroot of verkleind, op basis van uw zakelijke behoeften bepalen. Deze functies zijn ingebouwde bij de portal met geen extra functies vereist.

## <a name="control-the-security"></a>Besturingselement voor de beveiliging

API-Apps gebruiken rollen gebaseerde beveiliging. Deze functies zijn van toepassing op de hele Azure ervaring, waaronder API-Apps en andere Azure resources. De rollen opnemen:

Rol | Beschrijving
--- | ---
Eigenaar | Volledige toegang tot de ervaring management en kunt toegang geven tot andere gebruikers of groepen.
Inzender | Volledige toegang tot de management-ervaring is. Kan geen toegang geven tot andere gebruikers of groepen.
Lezer | Ziet alle resources behalve geheimen.
Beheerder van de gebruiker toegang | Alle resources, rollen maken/beheren kunt bekijken en ondersteuning voor tickets maken/beheren.

Zie [Rolgebaseerd toegangsbeheer in de portal van Microsoft Azure](../active-directory/role-based-access-control-configure.md).

U kunt eenvoudig gebruikers toevoegen en deze specifieke rollen toewijzen aan uw API-App. De portal ziet u de gebruikers die toegang en hun toegewezen rol hebben:

![][access]  

- Selecteer **gebruikers** aan een gebruiker toevoegen, een rol toewijzen en verwijderen van een gebruiker.
- Selecteer **rollen** om te zien van alle gebruikers in een bepaalde rol, een gebruiker toevoegen aan een rol en een gebruiker verwijderen uit een rol.


## <a name="more-good-stuff"></a>Meer goede spullen
- Selecteer **API definitie** het bestand Swagger automatisch gemaakt voor de specifieke API-app te openen.
- Selecteer **afhankelijkheden** om weer te geven van de bestanden de API-App vereist. Bijvoorbeeld als u de verbindingslijn SAP gebruikt, kunt u installeren extra bestanden op de on-premises implementatie hybride Connection Manager te werk gaan. Deze afhankelijkheden worden weergegeven in uw blade API-app.

>[AZURE.IMPORTANT] Wanneer u de eigenschappen van uw API-app te openen en kijk onder **Essentials**, zijn er **Host** en de **Gateway** koppelingen waarmee nieuwe bladen wordt geopend:
>
> ![][host]
>
>Deze eigenschappen zijn specifiek voor de website waarop uw API-App. Wanneer u een ingebouwde API-App of verbindingslijn, meestal deze eigenschappen echt niet van toepassing en het is raadzaam dat u deze eigenschappen niet bijwerkt. Als u uw eigen API-App hebt gemaakt in Visual Studio en geïnstalleerd op uw Azure-abonnement, kunt u de bladen Host en de Gateway gebruiken. <br/><br/>


>[AZURE.NOTE] Ga naar [Logica-App proberen](https://tryappservice.azure.com/?appservice=logic)om te beginnen met logica Apps voordat u zich registreert voor een Azure-account. U kunt een tijdelijk starter logica-app maken. Geen creditcards vereist en geen verplichtingen.

## <a name="read-more"></a>Meer weten?

[Uw Apps logica controleren](app-service-logic-monitor-your-logic-apps.md)<br/>
[Verbindingslijnen en de lijst met Apps in de App Service API](app-service-logic-connectors-list.md)<br/>
[Rolgebaseerd toegangsbeheer in de portal van Microsoft Azure](../active-directory/role-based-access-control-configure.md)<br/>
[Gebruik van de hybride Connection Manager in Azure App-Service](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
