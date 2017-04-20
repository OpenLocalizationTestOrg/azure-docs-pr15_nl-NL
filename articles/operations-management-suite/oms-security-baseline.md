<properties
   pageTitle="Bewerkingen Management Suite beveiligings- en controle oplossing basislijn | Microsoft Azure"
   description="In dit document wordt uitgelegd hoe u OMS beveiliging en controle van oplossing voor het uitvoeren van een basislijn beoordeling van alle gecontroleerde computers voor naleving en beveiliging doel."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Basislijn Assessment in bewerkingen Management Suite beveiligings- en controle-oplossing

In dit document helpt u bij [bewerkingen Management Suite (OMS) beveiliging en controle oplossing](operations-management-suite-overview.md) basislijn assessment mogelijkheden gebruiken voor toegang tot de beveiligde status van uw gecontroleerde resources.

## <a name="what-is-baseline-assessment"></a>Wat is een basislijn Assessment?

Microsoft, definieert samen met industriële en government organisaties overal ter wereld, u een Windows-configuratie met zeer veilige serverimplementaties. Deze configuratie is een set registersleutels staat, beleidsinstellingen voor controle en instellingen voor het beveiligingsbeleid samen met Microsoft aanbevolen waarden voor deze instellingen. Deze set regels heet beveiliging volgens de basislijn. OMS beveiligings- en controle basislijn assessment videomogelijkheden kunt al uw computers voor naleving naadloos scannen. 

Er zijn drie soorten regels:

- **Register-regels**: Controleer of registersleutels juist zijn ingesteld.
- **Beleidsregels controle**: regels over uw controlebeleid.
- **Beleidsregels voor beveiliging**: regels met betrekking tot de machtigingen van de gebruiker op de computer.

> [AZURE.NOTE] [Gebruik OMS beveiliging om te beoordelen van de basislijn van de configuratie beveiliging](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) voor een kort overzicht van deze functie begrijpen.

## <a name="security-baseline-assessment"></a>Beveiliging basislijn Assessment

U kunt uw huidige beoordeling van de basislijn veiligheid voor alle computers die met OMS beveiligings- en controle met het dashboard worden bewaakt bekijken.  De volgende stappen uit om toegang tot het dashboard van beveiliging basislijn assessment uitvoeren:

1. Klik op **beveiliging en controle** -tegel in het **Microsoft bewerkingen Management Suite** belangrijkste dashboard.
2. Klik op **Basislijn Assessment** onder **Beveiliging Domains**in het dashboard **beveiligings- en controle** . Het dashboard **Beoordeling van de basislijn veiligheid** wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:
    
    ![OMS beveiligings- en controle basislijn Assessment](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Dit dashboard is onderverdeeld in drie hoofdgebieden:

- **Computers vergeleken met basislijn**: in deze sectie wordt een overzicht van het aantal computers die zijn geopend en het percentage van computers waarop de beoordeling doorgegeven. Ook krijgt deze de bovenste 10 computers en het resultaat percentage voor de beoordeling.
- **Status van regels vereist**: deze sectie heeft de bedoeling om kennis van de mislukte regels door ernst weer te geven en is mislukt regels op type. Door te kijken in de eerste grafiek kunt u snel als de meeste de mislukte regels kritiek, al dan niet zijn vinden. Dit wordt ook een overzicht van de bovenste 10 mislukte regels en hun ernst. Het tweede diagram ziet u het type regel die is mislukt tijdens de evaluatie. 
- **Computers basislijn assessment ontbreken**: in deze sectie een lijst met de computers die niet zijn gestart vanwege besturingssysteem incompatibiliteit tussen of fouten. 

### <a name="accessing-computers-compared-to-baseline"></a>Toegang tot computers vergeleken met basislijn

Als al uw computers zijn worden compatibel is met de evaluatie van de basislijn beveiliging. Echter wordt naar verwachting dat in bepaalde dit omstandigheden niet optreden. Als onderdeel van het proces voor beveiliging is het belangrijk om op te nemen met de computers die niet kon worden slagen voor alle beveiliging assessment tests reviseren. Er is een snelle manier om te visualiseren die door de optie **Computers toegankelijk** zich bevindt in de sectie **Computers vergeleken met volgens de basislijn** . Hier ziet u het logboek zoekresultaat met een lijst met computers zoals u ziet in het volgende scherm:

![Resultaten van de computer toegankelijk](./media/oms-security-baseline/oms-security-baseline-fig2.png)

De zoekresultaten worden weergegeven in een tabelindeling, waarbij de eerste kolom heeft de naam van de computer en de tweede kleur heeft het aantal regels die is mislukt. Als u wilt ophalen van de informatie over het type regel die is mislukt, klikt u in het aantal mislukte regels naast de naam van de computer. Hier ziet u een resultaat dat vergelijkbaar is met de weergegeven in de volgende afbeelding:

![Details van de computer toegankelijk resultaten](./media/oms-security-baseline/oms-security-baseline-fig3.png)

In dit zoekresultaat moet u het totaal van de gebruikte regels, het aantal kritieke regels die is mislukt, de waarschuwingsregels en de regels informatie is mislukt.

### <a name="accessing-required-rules-status"></a>Toegang krijgen tot het vereiste regels status

Nadat de informatie over het percentage aantal computers waarop de beoordeling doorgegeven, is het raadzaam voor meer informatie over welke regels op basis van de ernst worden verbroken. Deze visualisatie helpt u bij de prioriteit te bepalen welke computers moeten eerst worden behandeld om ervoor te zorgen dat zij beschikbaar zijn in de volgende beoordeling voldoen. Plaats de muisaanwijzer op het kritieke deel van de grafiek zich bevindt in de tegel **is mislukt regels door ernst** onder **regels voor status vereist** en klikt u erop. Hier ziet u een vergelijkbaar met het volgende scherm resultaat:

![Kan geen regels door ernst details](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

In dit resultaat log ziet u het type basislijn regel die is mislukt, de beschrijving van deze regel en de algemene configuratie inventarisatie (CCE)-ID van deze regel. Deze kenmerken moet genoeg een corrigerende actie u lost dit probleem in de doelcomputer wordt uitgevoerd.

> [AZURE.NOTE] Toegang tot de [Nationale beveiligingsprobleem Database](https://nvd.nist.gov/cce/index.cfm)voor meer informatie over CCE.

### <a name="accessing-computers-missing-baseline-assessment"></a>Toegang tot computers basislijn assessment ontbreken

OMS ondersteunt basislijn gebruikersprofiel van het domein op Windows Server 2008 R2 snel aan de Windows Server 2012 R2. Windows Server 2016 volgens de basislijn is nog niet definitief en zodra deze is gepubliceerd, worden toegevoegd. Alle andere besturingssystemen gescande via OMS beveiligings- en controle basislijn assessment wordt weergegeven onder de sectie **Computers basislijn assessment ontbreken** .

## <a name="see-also"></a>Zie ook

In dit document, hebt u geleerd over OMS beveiligings- en controle basislijn assessment. Meer informatie over OMS beveiliging, raadpleegt u de volgende artikelen:

- [Overzicht van de Management Suite Kantoorbeheersysteem bewerkingen](operations-management-suite-overview.md)
- [Cmdlets voor controle en reageren op beveiligingsmeldingen in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-responding-alerts.md)
- [Resources voor controle in bewerkingen Management Suite beveiligings- en controle-oplossing](oms-security-monitoring-resources.md)

