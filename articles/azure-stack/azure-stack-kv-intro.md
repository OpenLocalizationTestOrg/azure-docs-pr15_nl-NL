<properties
    pageTitle="Azure stapel toets kluis Inleiding | Microsoft Azure"
    description="Leer hoe Azure stapel toets kluis beheert toetsen en geheimen"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Inleiding tot belangrijke kluis Azure gestapelde #

## <a name="before-you-start"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan:

- De lezer heeft toegang tot een abonnement dat Azure-toets kluis is ingeschakeld is.
- De SDK van de PowerShell Azure is geconfigureerd en beschikbaar.
- Voor de versie TP2 kunnen alle tenant-omlaag bewerkingen alleen worden uitgevoerd vanaf PowerShell.

## <a name="key-vault-basics"></a>Toets kluis basisbeginselen

Toets kluis Azure gestapelde helpt beschermen cryptografische sleutels en geheimen die cloud-toepassingen en services gebruiken. U kunt sleutels en geheimen (zoals verificatie toetsen, opslag account toetsen gegevens versleuteling toetsen, .pfx-bestanden en wachtwoorden) coderen met behulp van de sleutel kluis.

Toets kluis stroomlijnt het proces key management en kunt u het beheer van sleutels die openen en versleutelen van uw gegevens. Ontwikkelaars kunnen maken van sneltoetsen voor het ontwikkelen en testen in minuten en vervolgens naadloos migreren naar productie toetsen. Beveiligingsbeheerders van de kunnen verlenen (en intrekken) machtiging voor sleutels, indien nodig.

Iedereen met een stapel Azure-abonnement kunt maken en gebruiken van belangrijke kluizen. Hoewel toets kluis ontwikkelaars en Beveiligingsbeheerders voordelen, kan deze kan worden geïmplementeerd en beheerd door een beheerder die worden beheerd door andere services Azure stapel voor een organisatie. Deze beheerder kan zich aanmelden met een abonnement Azure stapel maken, bijvoorbeeld een kluis voor de organisatie waarin toetsen opslaan en vervolgens worden die verantwoordelijk is voor deze operationele taken:

- Maken of importeren van een sleutel of geheim
- Intrekken of een toets of geheim verwijderen
- Autoriseer gebruikers of toepassingen toegang tot de belangrijkste kluis, zodat ze vervolgens kunnen beheren of gebruik de toetsen en geheimen
- Gebruik van de sleutel configureren (bijvoorbeeld, meld u aan of versleutelen)

Deze beheerder kan vervolgens ontwikkelaars URI's om te bellen vanuit hun toepassingen, en bieden de beheerder van een waardepapier met gebruik van de sleutel logboekinformatie.

Ontwikkelaars kunnen ook beheren de toetsen rechtstreeks met behulp van API's. Zie de sleutel kluis handleiding voor ontwikkelaars voor meer informatie.

## <a name="scenarios"></a>Scenario 's

De volgende tabel ziet u enkele scenario's beschreven waar toets kluis kunnen helpen aan de behoeften van ontwikkelaars en Beveiligingsbeheerders:


### <a name="developer-for-an-azure-stack-application"></a>Ontwikkelaars voor een toepassing Azure-Stack

**Probleem**: ik wil een toepassing schrijven voor Azure-Stack die sleutels voor ondertekenen en versleutelen wordt gebruikt, maar ik wil dat deze moeten buiten mijn-toepassing, zodat de oplossing is geschikt voor een toepassing die geografisch is verdeeld.

**Overzicht**: sleutels zijn opgeslagen in een kluis en aangeroepen door URI als dat nodig is.


### <a name="developer-for-software-as-a-service-saas"></a>Ontwikkelaars voor software als service (SaaS)

**Probleem:** Ik wil niet de verantwoordelijkheid of mogelijke aansprakelijkheid voor mijn klanten tenant sleutels en geheimen.

**Instructie:** Klanten kunnen hun eigen toetsen importeren in Azure stapel en beheren. Ik wil klanten de eigenaar bent en het beheren van hun toetsen zodat ik vestigen kunt op doen wat ik doen best, waarin de core biedt software-functies.


### <a name="chief-security-officer-cso"></a>Beveiliging directeur (BBF)

**Probleem:** Ik wil om ervoor te zorgen dat mijn organisatie controle over de levenscyclus van de belangrijkste is en gebruik van de sleutel kunt controleren.

**Instructie** Toets kluis is zo ontworpen dat Microsoft niet zien of uw sleutels extraheren.  Wanneer een toepassing cryptografische bewerkingen uitvoeren moet met behulp van klanten toetsen, doet toets kluis dit namens de toepassing. De toepassing niet de klanten toetsen te zien.  Hoewel we verschillende Azure stapel services en resources gebruiken, wil ik de toetsen beheren vanaf één locatie Azure gestapelde. De kluis biedt één interface, ongeacht het aantal kluizen u Azure gestapelde, welke regio's ze ondersteuning en welke toepassingen gebruiken.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met belangrijke kluis](azure-stack-kv-getting-started.md)
