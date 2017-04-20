<properties 
    pageTitle="Service-Bus verificatie en machtiging | Microsoft Azure"
    description="Overzicht van gedeelde Access handtekening (SA's)-verificatie."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Service Bus verificatie en machtiging

Toepassingen kunnen worden geverifieerd Azure-Service met bus voor op verificatie gedeeld Access handtekening (SA's) of via Azure Active Directory-toegangsbeheer (ook wel bekend als Access Control-Service of ACS). Access-handtekening verificatie kunt toepassingen om te verifiëren met een toegangstoets die is geconfigureerd op de naamruimte of op de entiteit waaraan specifieke rechten zijn gekoppeld bus voor Service gedeeld. U kunt deze toets vervolgens gebruiken om een Access-handtekening gedeeld token die clients gebruiken kunnen om te verifiëren bij Service Bus te genereren.

>AZURE. BELANGRIJKE SA's wordt aangeraden via ACS, aangezien deze een eenvoudige, flexibele en eenvoudig-en-klare verificatiemethode voor Service Bus biedt. Toepassingen kunnen SA's gebruiken in scenario's waarin ze hoeft niet voor het beheren van het begrip van een gemachtigde "gebruiker".

## <a name="shared-access-signature-authentication"></a>Verificatie met een gedeelde Access-handtekening

[Verificatie SA's](service-bus-sas-overview.md) kunt u een gebruikerstoegang geven tot Service Bus resources met specifieke rechten. SA's verificatie in Service Bus omvat de configuratie van een cryptografische sleutel met bijbehorende rechten op een Service Bus resource. Clients vervolgens toegang tot deze resource kunnen krijgen door het presenteren van een token SA's die bestaat uit de bron-URI wordt geopend en een verstrijken ondertekend met de geconfigureerde sleutel.

U kunt sleutels voor SA's configureren voor een naamruimte Service Bus. De sleutel is van toepassing op alle berichten entiteiten in die naamruimte. U kunt ook toetsen van Service Bus wachtrijen en onderwerpen. SA's wordt ook ondersteund op Service bevinden doorstuurt.

Als u wilt gebruiken SA's, kunt u een object [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) configureren op een naamruimte, wachtrij of onderwerp dat uit de volgende bestaat:

- *KeyName* die aangeeft van de regel.

- *PrimaryKey* is een cryptografische sleutel gebruikt Aanmeldingsadres/SA's om tokens te valideren.

- *Secundaire sleutel* is een cryptografische sleutel gebruikt Aanmeldingsadres/SA's om tokens te valideren.

- *Rights* dat staat voor de verzameling luisteren, verzenden of beheren rechten.

Voor autorisatieregels die zijn geconfigureerd op het niveau van de naamruimte Verleen toegang tot alle entiteiten in een naamruimte voor clients met tokens die zijn aangemeld met de bijbehorende sleutel. Snel aan de 12 toestemming kunnen regels worden geconfigureerd op een Service Bus naamruimte, wachtrij of onderwerp. Een [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) met alle rechten is geconfigureerd voor elke naamruimte standaard wanneer deze eerst is ingericht.

Voor toegang tot een entiteit, is de client vereist een SA's token gegenereerd met een specifieke [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Het token SA's wordt gegenereerd op basis van de HMAC-SHA256 van een resource-tekenreeks die bestaat uit de bron-URI waartoe u toegang is aangevraagd, en een verstrijken met een cryptografische sleutel die is gekoppeld aan de autorisatieregel.

SA's verificatie-ondersteuning Service is opgenomen in de SDK van Azure .NET-versie 2.0 en hoger. SA's biedt ondersteuning voor een [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Alle API's die een verbindingsreeks als een parameter accepteren bieden ondersteuning voor SA's verbindingstekenreeksen.

## <a name="acs-authentication"></a>ACS-verificatie

Service Bus-authenticatie via ACS wordt beheerd via een companion "-sb" ACS naamruimte. Als u een naamruimte companion ACS moet worden gemaakt voor een naamruimte Service Bus wilt, kunt u de naamruimte van uw Service Bus met behulp van de Azure klassieke portal; geen maken u moet de naamruimte met de PowerShell-cmdlet [New-AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) maken. Bijvoorbeeld:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Als u wilt voorkomen dat een naamruimte ACS maken, geef de volgende opdracht:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Bijvoorbeeld als u een Service Bus naamruimte **contoso.servicebus.windows.net**maakt, is een companion ACS naamruimte **contoso-sb.accesscontrol.windows.net** ingericht automatisch. Voor alle naamruimten die zijn gemaakt voor augustus 2014, is ook een bijbehorende ACS naamruimte gemaakt.

Een service standaardidentiteit "eigenaar," met alle rechten, is al dan niet standaard in deze companion ACS naamruimte ingericht. U kunt fijnmazige besturing een Service Bus entiteit tot en met ACS verkrijgen door de juiste vertrouwensrelaties configureren. U kunt extra service-identiteiten voor het beheren van toegang tot de Service Bus entiteiten configureren.

Voor toegang tot een entiteit, vraagt de client een token SWT bij ACS met de juiste claims door het presenteren van de referenties. Het token SWT moet vervolgens worden verzonden als onderdeel van de aanvraag naar Service Bus toestemming van de client voor toegang tot de entity inschakelen.

ACS verificatie-ondersteuning Service is opgenomen in de SDK van Azure .NET-versie 2.0 en hoger. Deze verificatie biedt ondersteuning voor een [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx). Alle API's die een verbindingsreeks als een parameter accepteren bieden ondersteuning voor ACS verbindingstekenreeksen.

## <a name="next-steps"></a>Volgende stappen

Gaat u verder lezen [gedeeld Access handtekening verificatie met Service Bus](service-bus-shared-access-signature-authentication.md) voor meer informatie over SA's.

Zie voor een overzicht van SA's in de Service Bus, [Gedeeld Access handtekeningen](service-bus-sas-overview.md).

U vindt meer informatie over ACS tokens in [hoe: een Token aanvraagt bij ACS via het OAuth laten TERUGLOPEN Protocol](https://msdn.microsoft.com/library/hh674475.aspx).



